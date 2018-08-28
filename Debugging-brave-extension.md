To debug the [brave-extension](https://github.com/brave/brave-extension), it is recommended to load the extension manually in dev mode.

> Note: If you'd like to use `yarn` instead of `npm` you can use `yarn import` to create a `yarn.lock` file from our `package-lock.json`.

In a new terminal, change your current working directory to `src/brave/vendor/brave-extension` and run `npm run dev`.

Back in your original terminal, start Brave without loading the extension:

```
npm start -- --disable_brave_extension
```

Then navigate to `chrome://extensions/` and turn on the checkbox for `Developer mode`.

Click on `Load unpacked extension...` and choose the directory `src/brave/vendor/brave-extension/dev`.

You'll be able to right click on the browser action icon and inspect to open developer tools for the browser action context.  You can also go to `chrome://extensions/` and open developer tools for the background page.