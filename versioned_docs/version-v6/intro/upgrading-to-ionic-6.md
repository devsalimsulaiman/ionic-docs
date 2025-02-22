# Upgrading to Ionic 6

Follow this guide to upgrade your Ionic 5 apps to Ionic 6.

## Getting Started

### Angular

1. Ionic 6 supports Angular 12+. Update to the latest version of Angular by following the [Angular Update Guide](https://update.angular.io/).
2. Update to the latest version of Ionic 6:

```shell
npm install @ionic/angular@6
```

If you are using Ionic Angular Server, be sure to update that as well:

```shell
npm install @ionic/angular@6 @ionic/angular-server@6
```

3. Remove any usage of `Config.set()`. Instead, set your config in `IonicModule.forRoot()`. See the [Config Documentation](../developing/config) for more examples.
4. Remove any usage of the `setupConfig` function previously exported from `@ionic/angular`. Set your config in `IonicModule.forRoot()` instead.

### React

1. Ionic 6 supports React 17+. Update to the latest version of React:

```shell
npm install react@latest react-dom@latest
```

2. Update to the latest version of Ionic 6:

```shell
npm install @ionic/react@6 @ionic/react-router@6
```

3. Update the `test` field in the `scripts` object of your `package.json` to include `transformIgnorePatterns`:

```json
"scripts": {
  "test": "react-scripts test --transformIgnorePatterns 'node_modules/(?!(@ionic/react|@ionic/react-router|@ionic/core|@stencil/core|ionicons)/)'",
  ...
}
```

4. Import and call `setupIonicReact` in your `App` component file. If you are also using `setupConfig`, pass your config to `setupIonicReact` instead:

**Before**
```tsx title="App.tsx"
import { setupConfig } from '@ionic/react';

...

setupConfig({
  mode: 'md'
});
```

**After**
```tsx title="App.tsx"
import { setupIonicReact } from '@ionic/react';

...

setupIonicReact({
  mode: 'md'
});
```

:::note
Developers must import and call `setupIonicReact` even if they are not setting custom config.
:::

See the [Config Documentation](../developing/config) for more examples.

5. Update all controller imports from `@ionic/core` to `@ionic/core/components`. As an example, here is a migration for `menuController`:

**Before**
```tsx
import { menuController } from '@ionic/core';
```

**After**
```tsx
import { menuController } from '@ionic/core/components';
```

### Vue

1. Ionic 6 supports Vue 3.0.6+. Update to the latest version of Vue:

```shell
npm install vue@3 vue-router@4
```

2. For apps that use the Vue CLI, install Vue CLI 5:

```shell
npm install -g @vue/cli@next
```

Then, upgrade all Vue CLI plugins:

```shell
vue upgrade --next
```

3. Update to the latest version of Ionic 6:

```shell
npm install @ionic/vue@6 @ionic/vue-router@6
```

4. Add the following `transformIgnorePatterns` to either `jest.config.js` or the `jest` field in `package.json`:

```js title="jest.config.js"
module.exports = {
  ...
  transformIgnorePatterns: ['/node_modules/(?!@ionic/vue|@ionic/vue-router|@ionic/core|@stencil/core|ionicons)']
}
```

```json title="package.json"
  {
    ...
    "jest": {
      "transformIgnorePatterns": ["/node_modules/(?!@ionic/vue|@ionic/vue-router|@ionic/core|@stencil/core|ionicons)"]
    }
  }
```

See the [Testing section below](#testing) for more information.

5. Remove any usage of the `setupConfig` function previously exported from `@ionic/vue`. Set your config when installing the `IonicVue` plugin instead. See the [Config Documentation](../developing/config) for more examples.

6. Rename the `IonRouter` type for `useIonRouter` to `UseIonRouterResult`.

7. Rename the `IonKeyboardRef` type for `useKeyboard` to `UseKeyboardResult`.

8. Rename any overlay event listeners to use the new format:

**Before**
```html
<ion-modal
  :is-open="modalOpenRef"
  @onWillPresent="onModalWillPresentHandler"
  @onDidPresent="onModalDidPresentHandler"
  @onWillDismiss="onModalWillDismissHandler"
  @onDidDismiss="onModalDidDismissHandler"
>
  ...
</ion-modal>
```

**After**
```html
<ion-modal
  :is-open="modalOpenRef"
  @willPresent="onModalWillPresentHandler"
  @didPresent="onModalDidPresentHandler"
  @willDismiss="onModalWillDismissHandler"
  @didDismiss="onModalDidDismissHandler"
>
  ...
</ion-modal>
```

:::note
This applies to `ion-action-sheet`, `ion-alert`, `ion-loading`, `ion-modal`, `ion-picker`, `ion-popover`, and `ion-toast`.
:::

9. Pass in an `ion-router-outlet` into any `ion-tabs` that are being used:

**Before**
```html
<ion-tabs>
  <ion-tab-bar slot="bottom">
    ...
  </ion-tab-bar>
</ion-tabs>

<script>
  import { IonTabs, IonTabBar } from '@ionic/vue';
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    components: { IonTabs, IonTabBar }
  });
</script>
```

**After**
```html
<ion-tabs>
  <ion-router-outlet></ion-router-outlet>
  <ion-tab-bar slot="bottom">
    ...
  </ion-tab-bar>
</ion-tabs>

<script>
  import { IonTabs, IonTabBar, IonRouterOutlet } from '@ionic/vue';
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    components: { IonTabs, IonTabBar, IonRouterOutlet }
  });
</script>
```

10. Additional routes inside of tabs should be re-written as sibling routes instead of child routes:

**Before**
```ts
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    redirect: '/tabs/tab1'
  },
  {
    path: '/tabs/',
    component: Tabs,
    children: [
      {
        path: '',
        redirect: 'tab1'
      },
      {
        path: 'tab1',
        component: () => import('@/views/Tab1.vue'),
        children: {
          {
            path: 'view',
            component: () => import('@/views/Tab1View.vue')
          }
        }
      },
      {
        path: 'tab2',
        component: () => import('@/views/Tab2.vue')
      },
      {
        path: 'tab3',
        component: () => import('@/views/Tab3.vue')
      }
    ]
  }
]
```

**After**
```ts
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    redirect: '/tabs/tab1'
  },
  {
    path: '/tabs/',
    component: Tabs,
    children: [
      {
        path: '',
        redirect: 'tab1'
      },
      {
        path: 'tab1',
        component: () => import('@/views/Tab1.vue')
      },
      {
        path: 'tab1/view',
        component: () => import('@/views/Tab1View.vue')
      },
      {
        path: 'tab2',
        component: () => import('@/views/Tab2.vue')
      },
      {
        path: 'tab3',
        component: () => import('@/views/Tab3.vue')
      }
    ]
  }
]
```

### Core

1. Update to the latest version of Ionic 6:

```shell
npm install @ionic/core@6
```

## Updating Your Code

### Datetime

1. Remove any usages of the `placeholder`, `pickerOptions`, `pickerFormat`, `monthNames`, `monthShortNames`, `dayNames`, and `dayShortNames` properties. `ion-datetime` now automatically formats the month names, day names, and time displayed inside of the component according to the language and region set on the device. See the [ion-datetime Localization Documentation](../api/datetime#localization) for more information.

2. Remove any usages of the `text` and `placeholder` CSS Shadow Parts.

3. Remove any usages of the `--padding-bottom`, `--padding-end`, `--padding-start`, `--padding-top`, and `--placeholder-color` CSS Variables. To customize the padding on `ion-datetime`, you can use any of the `padding` CSS properties.

4. Remove any usage of the `open` method. To present the datetime in an overlay, place it inside of an `ion-modal` or an `ion-popover` component. See the [ion-datetime Usage Examples](../api/datetime#usage) for more information.

5. Remove any usage of the `displayFormat` or `displayTimezone` properties. To parse the UTC string provided in the payload of the `ionChange` event, we recommend using [date-fns](https://date-fns.org/). See the [ion-datetime Parsing Dates Documentation](../api/datetime#parsing-dates) for examples.

:::note
See the [Datetime Migration Sample Application](https://github.com/ionic-team/datetime-migration-samples) for more migration examples.
:::

### Icon

Ionic 6 now ships with Ionicons 6. Review the [Ionicons 6 Breaking Changes Guide](https://github.com/ionic-team/ionicons/releases/tag/v6.0.0) and make any necessary changes.

### Input

Ensure `null` is not passed in as a value to the `placeholder` property. We recommend using `undefined` instead.

### Modal

`ion-modal` now uses the Shadow DOM. Update any styles targeting the internals of `ion-modal` to use either the [ion-modal CSS Variables](../api/modal#css-custom-properties) or the [ion-modal CSS Shadow Parts](../api/modal#css-shadow-parts):

**Before**
```css
ion-modal .modal-wrapper {
  ...
}

ion-modal ion-backdrop {
  ...
}
```

**After**
```css
ion-modal::part(content) {
  ...
}

ion-modal::part(backdrop) {
  ...
}
```

### Popover

`ion-popover` now uses the Shadow DOM. Update any styles targeting the internals of `ion-popover` to use either [ion-popover CSS Variables](../api/popover#css-custom-properties) or the [ion-popover CSS Shadow Parts](../api/popover#css-shadow-parts):


**Before**
```css
ion-popover .popover-arrow {
  ...
}

ion-popover ion-backdrop {
  ...
}

ion-popover .popover-content {
  ...
}
```

**After**
```css
ion-popover::part(arrow) {
  ...
}

ion-popover::part(backdrop) {
  ...
}

ion-popover::part(content) {
  ...
}
```

### Radio

Remove any usage of the `RadioChangeEventDetail` interface.

### Select

Ensure `null` is not passed in as a value to the `placeholder` property. We recommend using `undefined` instead.

### Textarea

Ensure `null` is not passed in as a value to the `placeholder` property. We recommend using `undefined` instead.

### Browser Support

The list of browsers that Ionic supports has changed. Review the [Browser Support Guide](../reference/browser-support) to ensure you are deploying apps to supported browsers.

If you have a `browserslist` or `.browserslistrc` file, update it with the following content:

```
Chrome >=60
Firefox >=63
Edge >=79
Safari >=13
iOS >=13
```


### Testing

Ionic 6 now ships as ES Modules. ES Modules are supported in all major browsers and bring developer experience and code maintenance improvements. Developers testing with Jest will need to update their Jest configuration as Jest does not have full support for ES Modules as of Jest 27.

This update involves using Babel to compile Ionic's ES Modules down to the CommonJS (CJS) format, a format that Jest can understand. Once Jest ships support for ES Modules, this change will no longer be necessary. See https://github.com/facebook/jest/issues/9430 for updates on ES Modules support in Jest.

If you are starting fresh with a new Ionic app, this configuration is done for you in our starter applications. For those with existing Ionic apps, follow the steps below to get Jest working with Ionic 6:

1. Add a `transformIgnorePatterns` field to your Jest config that includes the relevant Ionic packages. This is typically found in `jest.config.js` or the `jest` field in `package.json`:

```js title="jest.config.js"
module.exports = {
  ...
  transformIgnorePatterns: ['/node_modules/(?!@ionic/core|@stencil/core|ionicons)']
}
```

```json title="package.json"
  {
    ...
    "jest": {
      "transformIgnorePatterns": ["/node_modules/(?!@ionic/core|@stencil/core|ionicons)"]
    }
  }
```

:::note
If you are using Ionic React or Ionic Vue, be sure to add the appropriate packages to the `transformIgnorePatterns` array. For Ionic React this includes `@ionic/react` and `@ionic/react-router`. For Ionic Vue this includes `@ionic/vue` and `@ionic/vue-router`.
:::

For developers using Create React App (CRA), there is currently no way to update the `transformIgnorePatterns` in a Jest config file. This is a CRA restriction and not something Ionic has control over. We can, however, pass the `transformIgnorePatterns` directly into the `react-scripts test` command:

```json title="package.json"
"scripts": {
  "test": "react-scripts test --transformIgnorePatterns 'node_modules/(?!(@ionic/react|@ionic/react-router|@ionic/core|@stencil/core|ionicons)/)'",
  ...
}
```

If you are still running into issues, here are a couple things to try:

1. Verify that `@babel/preset-env` is included in your [project-wide configuration](https://babeljs.io/docs/en/config-files#project-wide-configuration) instead of your [file-relative configuration](https://babeljs.io/docs/en/config-files#file-relative-configuration). This typically means defining the Babel configuration in `<project-root>/babel.config.json`.

2. If you have a `browserslist/test` field in `package.json` file, make sure it is set to `current node`.

## Need Help Upgrading?

Be sure to look at the [Ionic 6 Breaking Changes Guide](https://github.com/ionic-team/ionic-framework/blob/main/BREAKING.md). There were several changes to default property and CSS Variable values that developers may need to be aware of. Only the breaking changes that required user action are listed on this page.

If you need help upgrading, please post a thread on the [Ionic Forum](https://forum.ionicframework.com/).

