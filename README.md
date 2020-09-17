# postcss-plugin-px2em


## Features

A plugin for PostCSS that generates em units from pixel units.

## Installation

```bash
$ npm i --save postcss-plugin-px2em
```

## Usage

### input and output

```css
// input
h1 {
  margin: 0 0 20px;
  font-size: 32px;
  line-height: 1.2;
  letter-spacing: 1px;
}

// output
h1 {
  margin: 0 0 0.2em;
  font-size: 0.32em;
  line-height: 1.2;
  letter-spacing: 0.01em;
}
```

### original

```javascript
import { writeFile, readFileSync } from 'fs';
import postcss from 'postcss';
import pxtorem from 'postcss-plugin-px2em';

const css = readFileSync('/path/to/test.css', 'utf8');
const options = {
  replace: false,
};
const processedCss = postcss(pxtorem(options)).process(css).css;

writeFile('/path/to/test.rem.css', processedCss, err => {
  if (err) throw err;
  console.log('Rem file written.');
});
```

### with webpack

```javascript
import px2em from 'postcss-plugin-px2em';
const px2emOpts = {
  ......
};
 
export default {
  module: {
    loaders: [
      {
        test: /\.css$/,
        loader: 'style-loader!css-loader!postcss-loader',
      },
    ],
  },
  postcss: [px2em(px2emOpts)],
}
```

### with [atool-build](https://github.com/ant-tool/atool-build)

`webpack.connfig.js`

```javascript
import webpack from 'atool-build/lib/webpack';
import px2em from 'postcss-plugin-px2em';

export default webpackConfig => {
  const px2emOpts = {
    ......
  };
  webpackConfig.postcss.push(px2em(px2emOpts));

  return webpackConfig;
};
```

## Configuration

Default:
```js
{
  rootValue: 100,
  unitPrecision: 5,
  propWhiteList: [],
  propBlackList: [],
  exclude:false,
  selectorBlackList: [],
  ignoreIdentifier: false,
  replace: true,
  mediaQuery: false,
  minPixelValue: 0
}
```

- `rootValue` (Number|Object) The root element font size. Default is 100.
    - If rootValue is an object, for example `{ px: 50, rpx: 100 }`, it will
    replace rpx to 1/100 rem , and px to 1/50 rem.
- `unitPrecision` (Number) The decimal numbers to allow the REM units to grow to.
- `propWhiteList` (Array) The properties that can change from px to rem.
    - Default is an empty array that means disable the white list and enable all properties.
    - Values need to be exact matches.
- `propBlackList` (Array) The properties that should not change from px to rem.
    - Values need to be exact matches.
- `exclude` (Reg)  a way to exclude some folder,eg. /(node_module)/.
- `selectorBlackList` (Array) The selectors to ignore and leave as px.
    - If value is string, it checks to see if selector contains the string.
        - `['body']` will match `.body-class`
    - If value is regexp, it checks to see if the selector matches the regexp.
        - `[/^body$/]` will match `body` but not `.body`
- `ignoreIdentifier` (Boolean/String)  a way to have a single property ignored, when ignoreIdentifier enabled, then `replace` would be set to `true` automatically.
- `replace` (Boolean) replaces rules containing rems instead of adding fallbacks.
- `mediaQuery` (Boolean) Allow px to be converted in media queries.
- `minPixelValue` (Number) Set the minimum pixel value to replace.

### License
MIT
