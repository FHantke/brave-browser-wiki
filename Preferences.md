Chromium's wiki entry for preferences is quite detailed but also somewhat outdated. Here's a simple overview of the few parts of how to create and access a new preference for Brave:

- The string for the preference key, usually `kMyPrefName = "brave.my_pref_name"` (either in `brave/common/pref_names.h` or, more preferrably, local to a component at `brave/components/myComponent/common/prefs.h`)
- The registration of the preference type and default value `RegisterBooleanPref(kMyPrefName, true)` (in `brave_profile_prefs.cc` or `MyComponent::RegisterProfilePrefs`)
- Getting the pref value in some code somewhere (`profile()->GetPrefs()->GetBooleanPref`)
- Allowing the user to set the pref on the settings page by adding a polymer settings control pointing at the new setting key `"brave.my_pref_name"`
- Allowing the settings page `chrome.settingsPrivate` API to access the setting (in `brave/browser/extensions/api/settings_private/brave_prefs_util.cc`)