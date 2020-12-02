We use webpack via GN (and `typescript.gni` - there is a description of what the target template does in that file - it can be used for typescript or javascript), e.g.:

```gn
# BUILD.gn

import("//brave/components/common/typescript.gni")

transpile_web_ui("my_script") {
  entry_points = [
    ["my_script_a", rebase_path("my_script.ts")]
  ]
  resource_name = "my_script"
}
```

This points to the entry point(s) for the script. The script uses `import` / `require` and this template uses webpack to output a bundle(s) and any other files it includes such as .jpg, .css, lazy-loaded .js, etc. In the above example, it would create the bundle at `gen/brave/web-ui-my_script/my_script_a.bundle.js`.

You don't need to put any inputs of the JS files used, it does all that automatically. It also outputs a GRD file of all the dynamic files output by this process at `gen/brave/web-ui-my_script/my_script.grd`. We then have _another_ target template which will find that GRD and pack / compress them to a PAK file: `pack_web_resources`, which is also in `typescript.gni`.

Here's an example: https://github.com/brave/brave-core/blob/1.19.x/components/brave_new_tab_ui/BUILD.gn