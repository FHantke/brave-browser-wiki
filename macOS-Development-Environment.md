## Prerequisites

You will need the following prerequisites to build Brave on macOS:

- Xcode
    - You can install Xcode through the App Store, or by running `xcode-select --install`.
- NodeJS >= 7.x
- Yarn

## Troubleshooting

As of Chromium 62, the 10.12 SDK is required for building. If you are using macOS < 10.12, you may need to:

`git clone git@github.com:phracker/MacOSX-SDKs.git`

And then before building make a symbolic link to MacOSX10.12.sdk in `Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs`