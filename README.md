# v-rich-text-editor

A rich text editor developed to use with Vuetify.

### How to use
Make sure you have Vue 2 project with Vuetify already added to it.

```bash
npm install --save v-rich-text-editor
or
yarn add v-rich-text-editor
```

In your main.js, or a similar entry point, just import the library using:

```javascript
import 'v-rich-text-editor';
```

Now the editor will be globally available to use anywhere in your project.

```html
<v-rich-text-editor v-model="htmlText" />
```
