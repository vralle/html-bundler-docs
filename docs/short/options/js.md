# `js`

```ts
type JsOptions = {
  filename?: FilenameTemplate;
  chunkFilename?: FilenameTemplate;
  outputPath?: string;
  inline?: "auto" | boolean | JsInlineOptions;
};
```

**Default**:

```js
{
  filename: '[name].js',
  chunkFilename: '[id].js',
  outputPath: null,
  inline: false,
}
```

**`JsOptions` Object properties**:

- **`filename`**: Specifies the output filename template for JavaScript files. For details,
  see the [`filename`](filename#filename) option.
- **`chunkFilename`**: Defines the filename template for non-initial chunks.
- **`outputPath`**: Sets the output directory for JavaScript files. Refer to the [`outputPath`](outputpath) option.
- **`inline`**: Controls JavaScript inlining behavior in HTML.\
  **Valid values**:
  - `false`: Outputs JavaScript to external files (default).
  - `true`: Inlines JavaScript via `<script>` tags.
  - `'auto'`: Inlines in `development` mode; outputs files in `production`.
  - `JsInlineOptions`: Advanced Inline Configuration

## Advanced Inline Configuration

Type:

```ts
type JsInlineOptions = {
  enabled?: "auto" | boolean;
  chunk?: RegExp | Array<RegExp>;
  source?: RegExp | Array<RegExp>;
  attributeFilter?: (props: {
    attribute: string;
    value: string;
    attributes: { [attributeName: string]: string; };
  }) => boolean | void;
};
```

**`JsInlineOptions` Object Properties**:

- **`enabled`**: Overrides the base inlining behavior. Default: `true`.
- **`chunk`**: Inlines chunks matching specified regular expression(s).
- **`source`**: Inlines all chunks from source files matching regular expression(s).
- **`attributeFilter`**: Filters attributes for inlined `<script>` tags. **Returns**:
  - `true` to retain an attribute.
  - `false` or `undefined` to remove it.

## Examples

### Attribute Filtering

To preserve specific attributes during inlining:

**Source HTML**:

```html
<script id="js-main" src="./main.js" defer></script>
```

**Configuration**:

```js
new HtmlBundlerPlugin({
  js: {
    inline: {
      attributeFilter: ({ attribute }) => attribute === "id",
    },
  },
});
```

**Output HTML**:

```html
<script id="js-main">
  // Inlined JavaScript
</script>
```

### Filename template

```js
new HtmlBundlerPlugin({
  js: {
    filename: "js/[name].[contenthash:8].js",
  },
});
```

### Inline runtime chunks

**Configuration part**:

```js
new HtmlBundlerPlugin({
  js: {
    filename: "js/[name].[contenthash:8].js",
    inline: {
      chunk: [/runtime.+[.]js/],
    },
  },
});
```

**Output Behavior**:

```text
- `js/app.xxxxxxxx.js`  → Saved as file.
- `runtime.xxxxxxxx.js` → Inlined into HTML.
```

### Chunk Splitting

```js
optimization: {
  splitChunks: {
    test: /\.(js|mjs)$/, // Required to avoid invalid outputs
  },
},
```

> [!WARNING]
> Omitting the `test` regex in `splitChunks` may cause Webpack to generate invalid files. Always restrict splitting
> to JavaScript/TypeScript files.

---

## Related Resources

- Webpack's [filename templates][webpack-filename-url]
- [Split chunks from **node_modules**][recipes-split-chunks-url]

**References**:

- Webpack's [`output.chunkFilename`][webpack-chunkfilename-url].

[recipes-split-chunks-url]: recipes#split-chunks-keep-module-name
[webpack-chunkfilename-url]: https://webpack.js.org/configuration/output/#outputchunkfilename
[webpack-filename-url]: https://webpack.js.org/configuration/output/#outputfilename
