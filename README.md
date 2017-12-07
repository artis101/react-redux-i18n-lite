# react-redux-i18n-lite

A binding library for redux to react-i18nify-lite

## Usage

First install the package.

Using `yarn`:
```
yarn add react-redux-i18n-lite
```

Using `npm`:
```
npm i react-redux-i18n-lite --save
```

`redux-thunk` is an implicit dependency, so you need it installed and included in your project.

To learn more about `redux-thunk`, refer to it's GitHub page:
https://github.com/gaearon/redux-thunk

Next, load the translations to be used, for example in `app.js` or in your apps entry point:

```javascript
import { createStore, combineReducers, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import { loadTranslations, setLocale, syncTranslationWithStore, i18nReducer } from 'react-redux-i18n-lite';
import reducers from './reducers';

const translationsObject = {
  en: {
    application: {
      title: 'Awesome app with i18n!',
      hello: 'Hello, %{name}!'
    },
    date: {
      long: 'MMMM Do, YYYY'
    },
    export: 'Export %{count} items',
    export_0: 'Nothing to export',
    export_1: 'Export %{count} item',
    two_lines: 'Line 1<br />Line 2',
    literal_two_lines: 'Line 1\
Line 2'
  },
  nl: {
    application: {
      title: 'Toffe app met i18n!',
      hello: 'Hallo, %{name}!'
    },
    date: {
      long: 'D MMMM YYYY'
    },
    export: 'Exporteer %{count} dingen',
    export_0: 'Niks te exporteren',
    export_1: 'Exporteer %{count} ding',
    two_lines: 'Regel 1<br />Regel 2',
    literal_two_lines: 'Regel 1\
Regel 2'
  }
};

const store =  createStore(
  combineReducers({
    ...reducers,
    i18n: i18nReducer
  }),
  applyMiddleware(thunk)
);
syncTranslationWithStore(store)
store.dispatch(loadTranslations(translationsObject));
store.dispatch(setLocale('en'));

```

**NB!** Please note that reducer's name must be `i18n`! Make sure your project's reducers don't clash with it.


## Components

The easiest way to translate copies in your React components is by using the `Translate` component,
directly exported from `react-i18nify-lite` package:

```javascript
import React from 'react';
import { Translate } from 'react-redux-i18n-lite';

const AwesomeComponent = () => (
  <div>
    <Translate value="application.title"/>
      // => returns '<span>Toffe app met i18n!</span>' for locale 'nl'
    <Translate value="application.title" style={{ fontWeight: 'bold', fontSize: '14px' }} />
    // => returns '<span style="font-weight:bold;font-size:14px;">Toffe app met i18n!</span>' for locale 'nl'
    <Translate value="application.hello" name="Aad"/>
      // => returns '<span>Hallo, Aad!</span>' for locale 'nl'
    <Translate value="export" count={1} />
      // => returns '<span>Exporteer 1 ding</span> for locale 'nl'
    <Translate value="export" count={2} />
      // => returns '<span>Exporteer 2 dingen</span> for locale 'nl'
    <Translate value="two_lines" dangerousHTML />
      // => returns '<span>Regel 1<br />Regel 2</span>'
  </div>
);
```

## Keeping components updated

When the translation object or locale values are set, all instances of `Translate` will be re-rendered to
reflect the latest state. If you choose to use `I18n.t` then it is up to you to handle any state changes.

If you'd rather not re-render components after setting locale or translations object, then pass `false` as a second
argument to `setLocale` and/or `setTranslations` method call.

## Helpers

If for some reason, you cannot use the components, you can use the `I18n.t` helper instead:

```javascript
import { I18n } from 'react-redux-i18n-lite';

I18n.t('application.title'); // => returns 'Toffe app met i18n!' for locale 'nl'
I18n.t('application.hello', {name: 'Aad'}); // => returns 'Hallo, Aad!' for locale 'nl'
I18n.t('export', {count: 0}); // => returns 'Niks te exporteren' for locale 'nl'
I18n.t('application.weird_key'); // => returns 'Weird key' as translation is missing
I18n.t('application', {name: 'Aad'}); // => returns {hello: "Hallo, Aad!", title: "Toffe app met i18n!"} for locale 'nl'
```

[downloads-image]: http://img.shields.io/npm/dm/react-redux-i18n-lite.svg

[npm-url]: https://npmjs.org/package/react-redux-i18n-lite
[npm-image]: http://img.shields.io/npm/v/react-redux-i18n-lite.svg
