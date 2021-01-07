# Previewing Icons

Icons can be previewed using the [Skia Icon Preview Extension](https://github.com/petemill/skia-icon-preview-extension).

![](https://github.com/rossmoody/img/blob/master/46456255-11da2980-c763-11e8-8bb4-27cde6ef1c03.png)

# Important Chromium Icon Documentation

The [Chromium Icon Documentation](https://chromium.googlesource.com/chromium/src/+/master/components/vector_icons/README.md) is latest reference place to go for updating Icon files.

# Sketch to SKIA Process

There is a process that seems pretty dependable in converting icons that are shapes to SKIA with minimal manual editing. 

1. Set the artboard size of an individual icon to the same size of the canvas in the icon you are replacing. This is very often 16, 24, or 32. 

2. Convert icons to 100% shapes and combine them. In order for this process to work, we can't have stroked elements within the svg or we will have to manually state the stroke size in Skia. 

3. Selecting the artboard (not the icon, the artboard), click Edit-> Copy SVG Code

4. Using the [SVGOMG](https://jakearchibald.github.io/svgomg/) cleaner, paste in our SVG code. I have toyed with these settings a bit and I'm unsure the absolute best combination but the one setting that is mandatory is to make sure the "Remove viewBox" setting is disabled. We need the viewbox declarations to create our canvas size in Skia.

5. Copy the compressed SVG code, bring it to [Skiafy](https://github.com/evanstade/skiafy) and paste it in. Since we are typically designing to size, I leave my scale at 1.0 and click Skiafy. Your code should have a canvas declaration and varying degrees of Move/Close declarations: 

```
CANVAS_DIMENSIONS, 32,
MOVE_TO, 27, 27,R_H_LINE_TO, -5,V_LINE_TO, 16.5f,R_ARC_TO, 1.5f, 1.5f, 0, 0, 0, -1.5f, -1.5f,R_H_LINE_TO, -8,R_ARC_TO, 1.5f, 1.5f, 0, 0, 0, -1.5f, 1.5f,V_LINE_TO, 27,H_LINE_TO, 6,V_LINE_TO, 12.83f,R_LINE_TO, 10.5f, -6.56f,LINE_TO, 27, 12.83f,V_LINE_TO, 27,
CLOSE,R_MOVE_TO, -13, 0,R_H_LINE_TO, 5,R_V_LINE_TO, -9,R_H_LINE_TO, -5,R_V_LINE_TO, 9,CLOSE,R_MOVE_TO, 14.59f, -16.71f,LINE_TO, 17.3f, 3.23f,R_ARC_TO, 1.5f, 1.5f, 0, 0, 0, -1.59f, 0,R_LINE_TO, -11.29f, 7.06f,ARC_TO, 2.98f, 2.98f, 0, 0, 0, 3, 12.83f,V_LINE_TO, 27.5f,CUBIC_TO, 3, 28.88f, 4.12f, 30, 5.5f, 30,R_H_LINE_TO, 22,R_CUBIC_TO, 1.38f, 0, 2.5f, -1.12f, 2.5f, -2.5f,V_LINE_TO, 12.83f,R_ARC_TO, 2.98f, 2.98f, 0, 0, 0, -1.41f, -2.54f,CLOSE
```

One bug that happens occasionally is an SVG will bring a `<def> </def>` bracket around the svg code. This will cause Skia to not render any coordinates. Deleting this bracket fixes the issue. 

# References

[1] https://chromium.googlesource.com/chromium/src/+/master/components/vector_icons/README.md

[2] https://github.com/evanstade/skiafy

[3] https://jakearchibald.github.io/svgomg/

[4] https://chromium.googlesource.com/chromium/src/+/master/ui/gfx/vector_icon_types.h#28

