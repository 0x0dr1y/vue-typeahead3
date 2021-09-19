# ⌨️ vue-typeahead3 ![Vue version](https://img.shields.io/badge/vue-3.2.6-brightgreen.svg) [![NPM version](https://img.shields.io/npm/v/vue-typeahead3.svg)](https://npmjs.com/package/vue-typeahead3) [![NPM downloads](https://img.shields.io/npm/dm/vue-typeahead3.svg)](https://npmjs.com/package/vue-typeahead3) ![size](https://img.shields.io/bundlephobia/min/vue-typeahead3)

<div align="center">
<img src="https://s9.gifyu.com/images/typeahead.gif" width="350">
</div>

## Table of Contents

- [About](#sectionAbout)
- [Installation](#sectionInstall)
- [Usage](#sectionUsage)
  - [Properties](#sectionConfiguration)

## About

<a name="sectionAbout"/>

**vue-typeahead3** is a super lightweight _(only 3.65Kb zipped)_ typeahead / autocompletion component for Vue.js 3. It's written in TypeScript using Vue.js 3 Composition API.

### Installation

<a name="sectionInstall"/>
Currently the plugin is available via NPM and Yarn. To install it use one of the two package managers.

```javascript
// NPM
$ npm install vue-typeahead3

// Yarn
$ yarn add vue-typeahead3
```

After the package got installed things are straight forward. Simply import and register the component:

```javascript
// src/main.js
import { createApp } from 'vue'
...
import typeahead from 'vue-typeahead3'
import "vue-typeahead3/dist/vue-typeahead.css";

const app = createApp(App)
...
app.use(typeahead) // register typeahead component
...
app.mount('#app')
```

## Usage

<a name="sectionUsage"/>

Once the plugin is registered you can straight up use it in your app.

**Basic example**:

```jsx
<template>
    <typeahead :data="suggestions" v-model="food" />
    <small>You selected: {{ food }}</small>
</template>

<script>
...
data() {
  return {
    suggestions: [
    {
      value: "Banana",
      category: "Fruit",
    },
    {
      value: "Shallots",
      category: "Vegetable",
    },
    {
      value: "Ananas",
      category: "Fruit",
    },
    {
      value: "Avocado",
      category: "Fruit",
    },
    {
      value: "Garlic",
      category: "Vegetable",
    },
  ],
  food: "",
 }
}
...
</script
```

### Configuration

<a name="sectionConfiguration"/>
The example above only shows the base usage of the component. You can customize the overall component by using the provided props listed below:

| property        | type                                          | required           | default             | description                                                                                             |
| --------------- | --------------------------------------------- | ------------------ | ------------------- | ------------------------------------------------------------------------------------------------------- |
| **v-model**     |                                               | :heavy_check_mark: |                     | Specifies where the selected item is stored in the parent component.                                    |
| **suggestions** | `Record<string, string>[]` (Array of Objects) | :heavy_check_mark: |                     | Suggestions to filter                                                                                   |
| **placeholder** | `string`                                      | :x:                | `Type to search...` | Placeholder for search input                                                                            |
| **searchKey**   | `string`                                      | :x:                | `value`             | Specifies the key to use for the actual search. <br/> **Must either equal `categoryKey` or `valueKey`** |
| **categoryKey** | `string`                                      | :x:                | `category`          | Specifies the key to use for the categories.                                                            |
| **valueKey**    | `string`                                      | :x:                | `value`             | Specifies the key to use for the value.                                                                 |
| **maxResults**  | `number`                                      | :x:                | `5`                 | Specifies the max amount of suggestions displayed within the dropdown.                                  |
