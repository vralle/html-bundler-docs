# `data`

**Type**: `object | string`

**Default**: `{}`

Defines template data accessible across all templates.

## Configuration

- `object`: Object-based data.
- `string`: Specifies the path to the template data file.

**Key Behaviors**:

- **Template-Specific Overrides**: Use [entry data](entry#entrydescriptiondata) to pass data to individual templates.
- **Merge Logic**: Template-specific data overrides matching properties in the global data object.
- **HMR Support**: `data` is configured as a reference to a data file.

## Object-Based Data

**Type**: `object`

Directly defines data as a JavaScript object.

**Limitations**:

- **HMR Support**: Requires Webpack restart to reflect changes.

**Example**:

```js
// webpack.config.js
module.exports = {
  plugins: [
    new HtmlBundlerWebpackPlugin({
      entry: {
        index: "path/to/template.eta",
      },
      data: { title: "Page Title", description: "Page Description" },
    }),
  ],
};
```

## File Reference Data

**Type**: `string`

Specifies an absolute or relative path to a JSON or JavaScript file.
For a JavaScript file, the module must export an object.

**Supported formats**:

- JavaScript
- JSON

**HMR Support**:

- Changes initiate Webpack recompilation if the file is part of the watch configuration.

**Example**:

```js
// webpack.config.js
module.exports = {
  plugins: [
    new HtmlBundlerWebpackPlugin({
      entry: {
        index: "path/to/template.eta",
      },
      data: "path/to/renderData.json",
    }),
  ],
};
```

## Use Case Comparison

| `data`   | Best For              | HMR Support |
| -------- | --------------------- | ----------- |
| `object` | Static data           | No          |
| `string` | Dynamic/editable data | Yes         |

## Notes

- Relative paths are resolved using Webpack the `context` configuration.

**References**:

- Webpack [`context`][webpack-context-url]
- Webpack [HMR][webpack-hmr-url]

[webpack-context-url]: https://webpack.js.org/configuration/entry-context/#context
[webpack-hmr-url]: https://webpack.js.org/concepts/hot-module-replacement/#get-started
