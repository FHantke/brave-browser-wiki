# Setup
### 1. Prerequisites
1.1 Clone and build brave-browser https://github.com/brave/brave-browser/wiki

1.2 Make sure you have Ninja builder installed: `brew install ninja`

1.3 Build the application target "views_examples_with_content_exe" to preview changed icons without needing to rebuild by:

```
cd src
ninja -C out/Release views_examples_with_content_exe
```

1.4 Once the command has finished building, launch the app from `src/out/Release/views_examples_with_content_exe`

![](https://github.com/rossmoody/images/blob/master/Screenshot%202018-06-19%2017.50.57.png)

Once you begin replacing Skia code, reviewing icon using this UI will save time. 

The first text box with `Size in dip` should be set to the value specified in  `CANVAS_DIMENSIONS` line of .icon file, for instance this is 32 for `https://github.com/brave/brave-core/blob/master/vector_icons/components/vector_icons/forward_arrow.icon`. 

It works with other values also, but when we specify the value from .icon file, we get the same look in a tool as it would be in actual browser toolbar.

The second field `Color AARRGGBB` is a hexadecimal code of the color, AA-alfa RR-red GG-green BB-blue. Now Chromium draws icons with a gray of `FF777777` , so this one should be specified to correctly render. 

The 3rd field is a full path to the icon. Once all fields are filled you should be able to render icons. 

![](https://github.com/rossmoody/images/blob/master/Screenshot%202018-06-19%2018.01.20.png)

### 2. Paths conventions
In order to make conversion go smooth and have no need to adjust result file, it is required for source svg to be:

- the same size as a target icon; size of a target icon is in the line `CANVAS_DIMENSIONS, `
- to have no line objects, lines should be represented as a closed loop with fill of extremely small width.

Modified icons should be placed in the location `brave-browser/src/brave/vector_icons/<original chromium path>`.

For example, for the file

```
brave-browser/src/components/vector_icons/reload.1x.icon
```
we should create modified version at 
```
brave-browser/src/brave/vector_icons/components/vector_icons/reload.1x.icon
```
The build script will choose one from `brave/vector_icons` if it is present. 


#Converting svg into skia file 
### 3. Resizing and manipulating exising SVG icons

The process will be shown on example of converting `reload_button_48.svg`

![initial](https://github.com/AlexeyBarabash/images1/blob/master/pic01.svg), 

 to `src/components/vector_icons/reload.1x.icon`

3.1 Use https://jakearchibald.github.io/svgomg/ to simplify the source svg
```
<?xml version="1.0" encoding="UTF-8"?>
<svg width="48px" height="48px" viewBox="0 0 48 48" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    <!-- Generator: Sketch 48.2 (47327) - http://www.bohemiancoding.com/sketch -->
    <title>reload_button_48</title>
    <desc>Created with Sketch.</desc>
    <defs></defs>
    <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
        <g id="reload_button_48">
            <g id="btn_toolbar_reload" transform="translate(1.000000, 4.000000)">
                <path d="M32.4841739,36.260339 C29.12,38.6142373 24.9933913,40 20.5356522,40 L20.5078261,40 C9.1826087,40 0,31.0535593 0,20.0135593 L0,19.9891525 C0,8.94915254 9.1826087,0 20.5078261,0 L20.5356522,0 C28.3213913,0 34.0507826,4.23050847 37.5262609,10.459661 L40,16.6101695" id="Stroke-7" stroke="#606467" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"></path>
                <path d="M47,18 C47,14.688 44.312,12 41,12 C37.688,12 35,14.688 35,18 C35,21.315 37.688,24 41,24 C44.312,24 47,21.315 47,18 Z" id="Fill-9" fill="#606467"></path>
            </g>
        </g>
    </g>
</svg>
```
press `Copy as text` button and svg goes into 
```
<svg width="48" height="48" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><path d="M33.484 40.26C30.12 42.614 25.994 44 21.536 44h-.028C10.183 44 1 35.054 1 24.014v-.025C1 12.95 10.183 4 21.508 4h.028c7.785 0 13.515 4.23 16.99 10.46L41 20.61" stroke="#606467" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/><path d="M48 22c0-3.312-2.688-6-6-6s-6 2.688-6 6a6 6 0 0 0 12 0z" fill="#606467"/></g></svg>

```
3.2 The source svg has 48x48 px size, and target icon file should be 16x16 px size according to `CANVAS_DIMENSIONS, 16,` line. So we need to apply scale ratio of 16/48=0.333 to get the result of correct size.
Open https://alexeybarabash.github.io/skiafy/, the fork of https://github.com/evanstade/skiafy with custom scale support.  Put simplified svg into clonned skiafy/index.html
Specify ratios for x and y of 0.333
Press `Skiafy!` button
You will get this output 
```
MOVE_TO, 11.15f, 13.41f,
CUBIC_TO, 10.03f, 14.19f, 8.66f, 14.65f, 7.17f, 14.65f,
R_H_LINE_TO, -0.01f,
CUBIC_TO, 3.39f, 14.65f, 0.33f, 11.67f, 0.33f, 8,
R_V_LINE_TO, -0.01f,
CUBIC_TO, 0.33f, 4.31f, 3.39f, 1.33f, 7.16f, 1.33f,
R_H_LINE_TO, 0.01f,
R_CUBIC_TO, 2.59f, 0, 4.5f, 1.41f, 5.66f, 3.48f,
LINE_TO, 13.65f, 6.86f,
CLOSE,
MOVE_TO, 15.98f, 7.33f,
R_CUBIC_TO, 0, -1.1f, -0.9f, -2, -2, -2,
R_CUBIC_TO, -1.1f, 0, -2, 0.9f, -2, 2,
R_ARC_TO, 2, 2, 0, 0, 0, 4, 0,
CLOSE
``` 
add comma after `CLOSE`

3.3 Replaced icon files goes inside of `src/brave/vector_icons/<corresponding chromium dir>`. `src/components/vector_icons/reload.1x.icon` goes to `src/brave/vector_icons/components/vector_icons/reload.1x.icon'.

3.4 Create `src/brave/vector_icons/components/vector_icons/reload.1x.icon`. 
Copy lines of `CANVAS_DIMENSIONS` and `END` from the original chromium icon. 
Put the result from 3.2 replacing text inside:
```
CANVAS_DIMENSIONS, 16,
// <Text from 2.2 here>
END
```
Put copyright text of the Firefox license. You will get
```
/* This Source Code Form is subject to the terms of the Mozilla Public
 * License, v. 2.0. If a copy of the MPL was not distributed with this file,
 * You can obtain one at http://mozilla.org/MPL/2.0/. */

CANVAS_DIMENSIONS, 16,
MOVE_TO, 11.15f, 13.41f,
CUBIC_TO, 10.03f, 14.19f, 8.66f, 14.65f, 7.17f, 14.65f,
R_H_LINE_TO, -0.01f,
CUBIC_TO, 3.39f, 14.65f, 0.33f, 11.67f, 0.33f, 8,
R_V_LINE_TO, -0.01f,
CUBIC_TO, 0.33f, 4.31f, 3.39f, 1.33f, 7.16f, 1.33f,
R_H_LINE_TO, 0.01f,
R_CUBIC_TO, 2.59f, 0, 4.5f, 1.41f, 5.66f, 3.48f,
LINE_TO, 13.65f, 6.86f,
CLOSE,
MOVE_TO, 15.98f, 7.33f,
R_CUBIC_TO, 0, -1.1f, -0.9f, -2, -2, -2,
R_CUBIC_TO, -1.1f, 0, -2, 0.9f, -2, 2,
R_ARC_TO, 2, 2, 0, 0, 0, 4, 0,
CLOSE
END
```

3.5 Verify the result

Run `src/out/Release/views_examples_with_content_exe`
Choose `Vector Icon` in dropdown list above
Put the full path to your modified icon file
Specify the color required, FF777777 for the gray
Press `Render`
You will see not the thing you have expected: 

![step 2](https://github.com/AlexeyBarabash/images1/blob/master/pic02.png)


3.6 Manually adjust the result
Unexpected rendering happened because [4] `By default, the path will be filled. This changes the paint action to stroke at the given width.`
So before the first circle we should set mode to STROKE with a width 1:
```
CANVAS_DIMENSIONS, 16,
 STROKE, 1, //<---
 MOVE_TO, 11.15f, 13.41f,
 CUBIC_TO, 10.03f, 14.19f, 8.66f, 14.65f, 7.17f, 14.65f,
 R_H_LINE_TO, -0.01f,
 CUBIC_TO, 3.39f, 14.65f, 0.33f, 11.67f, 0.33f, 8,
 R_V_LINE_TO, -0.01f,
 CUBIC_TO, 0.33f, 4.31f, 3.39f, 1.33f, 7.16f, 1.33f,
 R_H_LINE_TO, 0.01f,
 R_CUBIC_TO, 2.59f, 0, 4.5f, 1.41f, 5.66f, 3.48f,
 LINE_TO, 13.65f, 6.86f,
 CLOSE,
 MOVE_TO, 15.98f, 7.33f,
 R_CUBIC_TO, 0, -1.1f, -0.9f, -2, -2, -2,
 R_CUBIC_TO, -1.1f, 0, -2, 0.9f, -2, 2,
 R_ARC_TO, 2, 2, 0, 0, 0, 4, 0,
 CLOSE,
 END
```

![step 3](https://github.com/AlexeyBarabash/images1/blob/master/pic03.png)


the rendered icon still has two issues:
* a. the ends of large circle are connected with a line
* b. the small circle is not filled, but stroked now.

(a) happened because of the first `CLOSE` command, will comment it out

(b) happened because we need to switch line mode from stroke to fill. Will do that by starting new path with `NEW_PATH` command

The new code will be 
```
CANVAS_DIMENSIONS, 16,
STROKE, 1,
MOVE_TO, 11.15f, 13.41f,
CUBIC_TO, 10.03f, 14.19f, 8.66f, 14.65f, 7.17f, 14.65f,
R_H_LINE_TO, -0.01f,
CUBIC_TO, 3.39f, 14.65f, 0.33f, 11.67f, 0.33f, 8,
R_V_LINE_TO, -0.01f,
CUBIC_TO, 0.33f, 4.31f, 3.39f, 1.33f, 7.16f, 1.33f,
R_H_LINE_TO, 0.01f,
R_CUBIC_TO, 2.59f, 0, 4.5f, 1.41f, 5.66f, 3.48f,
LINE_TO, 13.65f, 6.86f,
//CLOSE,  //fix for (a)
NEW_PATH, //fix for (b)
MOVE_TO, 15.98f, 7.33f,
R_CUBIC_TO, 0, -1.1f, -0.9f, -2, -2, -2,
R_CUBIC_TO, -1.1f, 0, -2, 0.9f, -2, 2,
R_ARC_TO, 2, 2, 0, 0, 0, 4, 0,
CLOSE,
END
```
 
The result will be 

![step 4](https://github.com/AlexeyBarabash/images1/blob/master/pic04.png).


And how it looks on the toolbar:

![on toolbar](https://github.com/AlexeyBarabash/images1/blob/master/pic05.png).

If svg consists of closed loops only, than no need to make fixes.

### 4. Sketch to SKIA Process

There is a process that seems pretty dependable in converting icons that are shapes to SKIA with minimal manual editing. 

4.1 Set the artboard size of an individual icon to the same size of the canvas in the icon you are replacing. This is very often 16, 24, or 32. 

![](https://github.com/rossmoody/images/blob/master/Screenshot%202018-06-19%2018.06.17.png)

4.1 Convert icons to 100% shapes and combine them. In order for this process to work, we can't have stroked elements within the svg or we will have to manually state the stroke size in Skia. 

4.2 Selecting the artboard (not the icon, the artboard), click Edit-> Copy SVG Code

4.3 Using the [SVGOMG](https://jakearchibald.github.io/svgomg/) cleaner, paste in our SVG code. I have toyed with these settings a bit and I'm unsure the absolute best combination but the one setting that is mandatory is to make sure the "Remove viewBox" setting is disabled. We need the viewbox declarations to create our canvas size in Skia.

4.4 Copy the compressed SVG code, bring it to [Skiafy](https://alexeybarabash.github.io/skiafy/) and paste it in. Since we are typically designing to size, I leave my scale at 1.0 and click Skiafy. Your code should have a canvas declaration and varying degrees of Move/Close declarations: 

```
CANVAS_DIMENSIONS, 32,MOVE_TO, 27, 27,R_H_LINE_TO, -5,V_LINE_TO, 16.5f,R_ARC_TO, 1.5f, 1.5f, 0, 0, 0, -1.5f, -1.5f,R_H_LINE_TO, -8,R_ARC_TO, 1.5f, 1.5f, 0, 0, 0, -1.5f, 1.5f,V_LINE_TO, 27,H_LINE_TO, 6,V_LINE_TO, 12.83f,R_LINE_TO, 10.5f, -6.56f,LINE_TO, 27, 12.83f,V_LINE_TO, 27,CLOSE,R_MOVE_TO, -13, 0,R_H_LINE_TO, 5,R_V_LINE_TO, -9,R_H_LINE_TO, -5,R_V_LINE_TO, 9,CLOSE,R_MOVE_TO, 14.59f, -16.71f,LINE_TO, 17.3f, 3.23f,R_ARC_TO, 1.5f, 1.5f, 0, 0, 0, -1.59f, 0,R_LINE_TO, -11.29f, 7.06f,ARC_TO, 2.98f, 2.98f, 0, 0, 0, 3, 12.83f,V_LINE_TO, 27.5f,CUBIC_TO, 3, 28.88f, 4.12f, 30, 5.5f, 30,R_H_LINE_TO, 22,R_CUBIC_TO, 1.38f, 0, 2.5f, -1.12f, 2.5f, -2.5f,V_LINE_TO, 12.83f,R_ARC_TO, 2.98f, 2.98f, 0, 0, 0, -1.41f, -2.54f,CLOSE
```

One bug I run into occasionally is an SVG will bring a `<def> </def>`  bracket around the svg code. This will cause Skia to not render any coordinates. Deleting this bracket fixes the issue. 

### 5. About rebuild of browser 

There is an issue now, brave icon files are not specified in gn scripts, so ninja tool cannot detect they were changed. 
For now it is required to remove manually all `vector_icons.cc` files from the disk inside of `brave-browser/src/out/Release/gen/` . This will be fixed, https://github.com/brave/brave-browser/issues/491.

### 6. References

[1] https://chromium.googlesource.com/chromium/src/+/master/components/vector_icons/README.md

[2] https://github.com/evanstade/skiafy

[3] https://jakearchibald.github.io/svgomg/

[4] https://chromium.googlesource.com/chromium/src/+/master/ui/gfx/vector_icon_types.h#28

