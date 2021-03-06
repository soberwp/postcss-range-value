# postcss-range-value

PostCSS plugin to add support for **`range()`**, allowing for a responsive range value between two screen sizes.

<img src=".github/demo.gif" width="600px" height="auto">

### Installation

```shell
yarn add postcss-range-value --dev
```

Require `postcssRange` at the top of Webpack or Mix:
```js
const postcssRange = require('postcss-range-value');
```

#### Using Webpack

```js
postcss([postcssRange]);
```

#### Using Mix Sass (Sage 10)

```js
mix
    .sass('resources/assets/styles/editor.scss', 'styles')
    .options({
        postCss: [postcssRange]
    });
```

### Config

Some config can be passed into `postcssRange()` in Webpack or Mix. 

Example below with the current defaults.

```js
postcssRange({
    rootRem: 16,
    prefix: 'range',
    screenMin: '48rem', // 768
    screenMax: '100rem', // 1600
    clamp: true,
})
```

### Usage

Range values can be passed to any CSS property that supports sizing.

`range($min-value, $max-value, $screen-min, $screen-max)`

The range value will responsively scale from `$min-value` to `$max-value` between `$screen-min` and `$screen-max`.

#### Basic

```scss
.range-value {
    font-size: range(2rem, 6rem, 48rem, 87.5rem);
}
```

You can omit `$screen-min` and `$screen-max` if passed in your `postcssRange` config.

```scss
.range-value {
    font-size: range(2rem, 6rem);
}
```

#### Using ratios

You can also use ratios to calculate the `$min-value` or `$max-value`. For ratio suggestions use [modularscale.com](https://www.modularscale.com/).

Scaling up using the golden ratio (`1.618`) from `2rem`.

```scss
.range-value {
    margin-bottom: range(2rem, 1.618);
}
```

Scaling down using the golden ratio (`1.618`) from `6rem`.

```scss
.range-value {
    margin-bottom: range(1.618, 6rem);
}
```

In some edge-cases, you may want to reverse the scaling logic, which can be done by passing in a negative ratio.

For the example below, `$min-value` will be `1.618`x larger than `$max-value`.

```scss
.range-value {
    margin-bottom: range(-1.618, 2rem);
}
```

#### Shorthand support

There is shorthand support for `margin`, `margin-inline`, `margin-block` and `padding`, `padding-inline`, `padding-block`.

```scss
.range-value {
    margin: range(2rem, 6rem) auto;
}

.range-value {
    margin: range(1.618, 10rem) range(1.618, 6rem) 2rem 2rem;
}
```

### In/Out

```scss
.range-value {
    font-size: range(2rem, 6rem, 48rem, 87.5rem);
}

.range-value {
    font-size: clamp(2rem, 2rem + (6 - 2) * ((100vw - 48rem) / (87.5 - 48)), 6rem);
}
```

Please note by default postcss-range-vaue uses `clamp()`.  Browsers that do not support CSS `clamp()` will not work. You disable the use of `clamp()` by setting `clamp: false` in the config, which will revert back to media queries for older browser support.
