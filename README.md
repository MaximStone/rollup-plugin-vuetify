# rollup-plugin-vuetify

The plugin is a missing autoloader of vuetify components.

It eliminates the pain of manually importing all of the Vuetify components you use. It also allows you to use tree shaking as efficiently as possible.

This is a must if you decide to write your vuetify-based library.

## Installation

Install the plugin with npm:

```shell
npm install --save-dev rollup-plugin-vuetify
```

## Configuration

There is a configuration example for building a simple js library:

```javascript
const { rollup } = require("rollup");
const vue = require("rollup-plugin-vue");
const postcss = require("rollup-plugin-postcss");
const vuetify = require("rollup-plugin-vuetify");

const build = async () => {
  try {
    const bundleTs = await rollup({
      input: "src/index.js",
      external: ["vue", "vuetify/lib"],
      plugins: [postcss(), vue(), vuetify()],
    });

    bundleTs.write({
      format: "esm",
      file: resolve(__dirname, "dist/bundle.js"),
    });
  } catch (e) {
    console.error(e);
  }
};

build();
```

You can also find the typescript example here [test/rollup.js](https://github.com/vatson/rollup-plugin-vuetify/blob/master/test/rollup.js)

## Demo

The [`test/`](https://github.com/vatson/rollup-plugin-vuetify/tree/master/test/) folder was created as an example of how a project can be configured and what types of components can be used.

```
.
├── dist
│   ├── ts.js                    <- bundled with @rollup/plugin-typescript
│   └── ts2.js                   <- bundled with rollup-typescript-2
├── rollup.js                    <- rollup configuration
├── src
│   ├── Complex.vue              <- component with partial manual import
│   ├── Decorated.vue            <- component decorated with vue-class-component
│   ├── Extended.vue             <- component created with Vue.extend()
│   ├── External                 <- component splitted into separate files
│   │   ├── Component.vue
│   │   ├── index.js
│   │   ├── script.js
│   │   └── template.html
│   ├── Simple.vue               <- simple generic component
│   └── index.js
└── tsconfig.json
```

## Supported Feartures

- Typescript >2.x and vue-property-decorator ([docs](https://class-component.vuejs.org/));
- Components created with Vue.extend() ([docs](https://ru.vuejs.org/v2/api/#Vue-extend));
- Components separated on the external files ([docs](https://vuejs.org/v2/guide/single-file-components.html#What-About-Separation-of-Concerns), [example](https://github.com/vatson/rollup-plugin-vuetify/tree/master/test/src/External));
- Autoloading of **Vuetify 1.x** and **2.x** components;
- Partial import. All missing components will be added to the final bundle. This option is great for existing projects;

## Known Caveats

- Plugin doesn't allow overwriting the built-in vuetify components with your own (knows as [Custom Dynamic imports](https://vuetifyjs.com/ru/customization/a-la-carte#custom-dynamic-imports));
- All known limitations of the original vuetify-loader (see more here [https://vuetifyjs.com/ru/customization/a-la-carte#limitations](https://vuetifyjs.com/ru/customization/a-la-carte#limitations));

## Under The Hood

It works similarly to the [vuetify-loader](https://github.com/vuetifyjs/vuetify-loader). But plugin uses AST transformation under the hood. It allows you to get rid of extra code and optimize your final bundle even more.
