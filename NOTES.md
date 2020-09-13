# Vue Custom Component
Create normal vue app with vue-cli 
```bash 
vue create my-component-name
# cd my-component-name
# if you need to add vuetify then
vue add vuetify
```

Create your component in components folder as usual. Then create ./src/index.js file add the following in that file.

```javascript
import Component from './components/MyComponentName.vue';
export default Vue => Vue.component(Component.name, Component);
```

Then add the following in package.json

```javascript
"build-component": "vue-cli-service build --target lib --name foo ./src/index.js"
```

```bash
yarn build-component
```
This will create ./dist folder with all the files of your component (e.g common, umd and css).

Make sure your `main` attribute in package.json is correctly pointing towards your output file.

```javascript
"main": "./dist/foo.common.js"
```

DONE!

Resources:

https://medium.com/justfrontendthings/how-to-create-and-publish-your-own-vuejs-component-library-on-npm-using-vue-cli-28e60943eed3