# plugin-storybook

## Install

```bash
yarn add @poi/plugin-storybook --dev

# Use React?
yarn add @storybook/react --dev
# Use Vue? 
yarn add @storybook/vue --dev
```

## Usage

The first entry will be used as storybook config file, and the second one (optional) will be used as storybook addons file.

```js
// poi.config.js
module.exports = {
  // Use your storybook config as entry
  entry: '.storybook/config.js',
  // If you want storybook addons:
  // entry: ['.storybook/config.js', '.storybook/addons.js']
  plugins: [
    require('@poi/plugin-storybook')()
  ],

  // To use with React:
  // The order matters!
  plugins: [
    require('@poi/plugin-react')(),
    require('@poi/plugin-storybook')()
  ]
}
```

Then create storybook config file and components:

Just like how the [official storybook guide](https://storybook.js.org/basics/guide-vue/) told you, but without installing storybook cli.

```js
// .storybook/config.js
import { configure } from '@storybook/vue'

function loadStories() {
  // You can require as many stories as you need.
  require('../src/stories')
}

configure(loadStories, module)
```

And your actual story `src/stories/index.js`:

```js
import { storiesOf } from '@storybook/vue'

import Mybutton from './path/to/your/lovely/Button.vue'

storiesOf('MyButton', module)
  .add('round component', () => ({
    components: { MyButton },
    template: '<my-button :rounded="true">rounded</my-button>'
  }))
  .add('jsx', () => ({
    render() {
      return <MyButton>JSX</MyButton>
    }
  }))
```


And you're all set, run `poi` to see your storybook in action:

```bash
poi
poi build
```

Here's the source code of [a complete example](https://github.com/poi-examples/storybook-vue-example).


### Using markdown file

You can import `.md` files and they will be parsed by `markdown-loader` using `marked`, however you might want to use your own loader to parse markdown files:

```js
// poi.config.js
module.exports = {
  extendWebpack(config) {
    const markdownRule = config.rules.get('markdown')
    // Use markdown-it instead
    markdownRule.replace('markdown-loader', 'markdown-it-loader', {
      /* maybe some options */
    })
  }
}
```

## Options

### managerTemplate

Type: `string`<br>
Default: [`./lib/manager.ejs`](./lib/manager.ejs)

Path to the HTML template for generated `index.html`.

### iframeTemplate

Type: `string`<br>
Default: [`./lib/iframe.ejs`](./lib/iframe.ejs)

Path to the HTML template for generated `iframe.html`.

### markdown

Type: `boolean`<br>
Default: `undefined`

Whether to support `.md` files or not.

## Gotchas

[Vue-devtools does not work for now](https://github.com/storybooks/storybook/issues/1708).

## License

[MIT](https://oss.ninja/mit/egoist) &copy; [EGOIST](https://github.com/egoist)
