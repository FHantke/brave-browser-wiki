# WebUI Resources Wiki
## Chromium WebUI Pages
Chromium HTML will refer to resources either _relatively_ (e.g. `../my_module.html`) or via _full url_ (e.g. `chrome://history/my_module.html`). It is important that these can be accessed **both** via HTTP and via FileSystem. The corresponding filesystem path is defined in the `opmize_web_ui` build action as the `input` param. For example `chrome://settings/` corresponds to `$target_gen_dir/settings_resources.unpak/`.

This is why most of the GN build files for a chromium webui will perform the following steps:
1. Closure compiler - performs type checking
2. Use grit to pack the files via a GRD file to a single PAK file
3. Unpack the PAK file to an output directory
4. optimize_web_ui: combine to single file and minify
5. Use grit again to pack the optimized files to a single PAK file

### How does optimize_web_ui know which FileSystem path corresponds to a Url (chrome://x/y/z) path?

If the path is chrome://**resources** or chrome://**brave-resources**, the FileSystem location for the path is defined in optimize_web_ui.py script, as this is common to all WebUIs:
```python
_URL_MAPPINGS = [
    ('chrome://resources/cr_components/', _CR_COMPONENTS_PATH),
    ('chrome://resources/cr_elements/', _CR_ELEMENTS_PATH),
    ('chrome://resources/css/', _CSS_RESOURCES_PATH),
    ('chrome://resources/html/', _HTML_RESOURCES_PATH),
    ('chrome://resources/js/', _JS_RESOURCES_PATH),
    ('chrome://resources/polymer/v1_0/', _POLYMER_PATH),
    ('chrome://brave-resources/', _BR_RESOURCES_PATH)
]
```

If the path is chrome://**anything-else**, the Filesystem location is defined in the "optimize_webui" GN definition, e.g:
```
optimize_webui("build") {
  host = "history"
  input = rebase_path(“.”, root_build_dir)
  …
}
```
_This lets the script know that the current directory that the `BUILD.gn` file is to be used as the base path for all chrome://history/x URL includes._

### Debug builds
Only Release builds are optimized in this way. For Debug builds, usually all the source files are bundled in to a PAK file directly and served individually from a UrlDataSource. That is, the chrome://settings WebUI will serve chrome://settings/module1 and chrome://settings/module2 directly, and without flattening or minifying.
It is important to test any additions or changes to module loading with the `optimize_web_ui` build flag set on and off.

## Brave-specific WebUI Pages
WebUIs which are created wholly for Brave do not use optimize_webui. Instead, they use Webpack, which handles type checking, module resolution, module concatentation or chunking, and minifying. It also generates the GRD file dynamically based on Javascript module imports and optimization settings.
