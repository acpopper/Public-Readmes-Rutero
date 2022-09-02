# rutero-front :earth_americas:

VSCode recommended extensions for rutero-front:

- Vetur
- Vue Peek
- Vue VSCode Snippets
- Palenight Theme

Dependencias:

- npm 8.5.0
- node 16.14.2

Si bien nuxt instala fácilmente la última versión con npm install nuxt, para especificar, este proyecto funciona correctamente con la versión 2.15.8

Un listado concreto de dependencias y sus versiones se encuentra en el archivo package.json del repositorio.

(https://www.geeksforgeeks.org/how-to-update-node-js-and-npm-to-next-version/)

## Build Setup

_primero git pull, borrar node-modules y package-lock.json. Después:_

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project (no utilizaremos este por el momento)
$ npm run generate
```

## Deploy

```bash
# log in to heroku:

heroku login
git push heroku development:master

# Para deployar una rama feature:
git push -f heroku feature:master
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

### Utility

Actualmente está seteado nuxt/auth para contar con lo siguiente en las secciones de <script>

```javascript
this.$auth.user;
this.$auth.loggedIn;
this.$auth.strategy.token.get();
```

Así como también se pueden usar en las secciones de <template>

```html
$auth.user $auth.loggedIn
```

De ello pueden obtener toda la info necesaria para cada usuario o admin
