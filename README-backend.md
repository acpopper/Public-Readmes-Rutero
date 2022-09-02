# Readme :mountain:

Esta aplicación corre un contenedor docker de RoR 7 con ruby 3.1

### Dependencias

- Docker (Docker version 20.10.16, build aa7e414)
- Docker-compose (Docker Compose version v2.6.0)

## Docker

Construir las imágenes (cuando cambia el Gemfile o el Dockerfile):

```bash
docker-compose build
```

Ver imágenes construidas:

```bash
docker images
```

Construir un contenedor en base a la última imagen:

```bash
docker-compose up
```

Correr un contenedor específico:

```bash
# Usar flag -d "detached" para correr en background
# O flag --rm para borrar el contenedor cuando se acabe el proceso.
docker run [imagen]
```

Ver contenedores activos:

```bash
# con flag -a muestra detenidos también
docker ps
```

Se llenó la memoria o hay muchas imágenes que no se usan?

```bash
# Para borrar una imagen
docker rmi [imagen]
# Para borrar contenedor
docker rm [contenedor]
# Para borrar todos los contenedores
docker rm -f $(docker ps -a -q)
# Para borrar todas las imágenes y contenedores que no se usan
docker system prune
# Para borrar TODO, menos volúmenes.
docker system prune -a
```

Ver logs de un contenedor:

```bash
docker logs [contenedor]
```

brir una terminal en un contenedor:

```bash
docker exec -ti [contenedor] bash
```

Iniciar bash de un "servicio" de docker-compose (en nuestro caso "web"). Útil para, por ejemplo, correr comandos de rails, rake tasks, utilizar la consola de rails, etc.

```bash
docker-compose run web bash
```

## Branches

Ver branches locales

```bash
# -a para incluir remotas
git branch
```

### Githubflow modificado:

1. (En development branch) Actualizar local y crear nueva rama

```bash
git pull
git checkout -b <feature branch>
```

2. Trabajar en la branch (se puede realizar el paso 2.1 para hacer commits frecuentes)

   2.1. Realizar commit y push a github

```bash
git add .
git commit -m"Mensaje"
# si es el primer push
git push --set-upstream origin <branch name>
# si se ha pusheado
git push
```

3. Hacer merge desde development a feature de manera remota o local para probar compatibilidad. Resolver conflictos y correr localmente.
4. Si todo ok, hacer merge a development
5. Cada release se hará un merge a main.

---

Si hay un bug o se debe realizar un cambio pequeño, se hace una rama de hotfix desde main, se resuelve el bug, se hace el merge a main y a development.

Cada merge (sin contar main y development) implica borrar la rama feature.

## Comandos Rails

Comandos de una aplicación específica se corren anteponiendo "docker-compose run". Por ejemplo:

```bash
# Si tenemos un servicio/contenedor que nombramos "web" en el docker-compose.yml
# que corre nuestra aplicación rails
docker-compose run web rails db:create
docker-compose run web rails db:migrate
docker-compose run web rails g scaffold etc...
# O, ejecutar docker-compose run web bash, y luego los comandos solos en consola.
```

## Heroku

### Setup

Añadir repositorio remoto (heroku)

_Estar en el directorio local del código (mismo que para este github)_

```bash
# Similar para agregar "app" staging en heroku
git remote add heroku https://git.heroku.com/rutero-backend.git
```

Mostrar "remotes" del directorio actual (debería mostrarse heroku):

```bash
git remote -v
```

### Deploy a heroku

_Iniciar Docker desktop_

Login al registro de contenedores de heroku:

```bash
heroku container:login
```

Push:

```bash
heroku container:push web --remote [remote]
# Reemplazar [remote] con heroku para producción o staging para staging
```

Release:

```bash
heroku container:release web --remote [remote]
# Reemplazar [remote] con heroku para producción o staging para staging
```

Down/delete container:

```bash
heroku container:rm web --remote [remote]
# Reemplazar [remote] con heroku para producción o staging para staging
```

### Comandos importantes

En el cli de heroku (consola local)

```bash
# Resetear postgresql a un backup
heroku pg:backups:restore [id backup] DATABASE_URL --app example-app
# O de una app a otra (cada una con el addon de postgres). Este reemplaza la BDD de staging por una copia de example
heroku pg:backups:restore example-app::b101 DATABASE_URL --app example-staging-app
```

### Utility AUTENTIFICATION

Como está configurada la autenticación, ahora para cada endpoint que la requiera, se cuenta con una variable automáticamente seteada, con la información del usuario que está "logeado" desde el frontend.

```ruby
@current_user
@curr_user_type
```

Disponibles para ser usadas en aquellos métodos en los controladores que ejecutan autenticación.
