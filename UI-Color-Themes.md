Brave has two built-in color themes: dark and light

The default theme is light for Release and Beta channels, and dark for Dev and Development.

WebUI pages can read, change and observe the theme value:

`type ThemeType = 'Light' | 'Dark'`

## Get
`chrome.braveTheme.getBraveThemeType((themeType: ThemeType) => void): void`

## Set
`chrome.braveTheme.setBraveThemeType(themeType: ThemeType): void`

## Observe
`chrome.braveTheme.onBraveThemeTypeChanged.addListener((themeType: ThemeType) => void): void`
