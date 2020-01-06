This wiki intended to track the problem(s) that folks are experiencing and what steps we've already done / need to do. Will fill this in with more details

**For folks having this problem, it's recommended that you update all your video card drivers to the latest official supported version (ex: download latest official drivers from [intel.com](https://downloadcenter.intel.com/product/80939/Graphics-Drivers), [nvidia.com](https://www.nvidia.com/Download/index.aspx?lang=en-us), or [amd.com](https://www.amd.com/en/support/kb/faq/gpu-driver-autodetect)). That has resolved many folks**

Also worth noting: the driver may expose settings relating to switching between the integrated and discrete video cards in the control panel that is installed. On Windows, you can also right click Brave to check the context menu for an entry like: `Run with graphics processor -> High performance NVIDIA processor`

<!-- MarkdownTOC -->

- [Reported customer incidents](#reported-customer-incidents)
  - [GPU taking too much power \(using discrete not integrated\)](#gpu-taking-too-much-power-using-discrete-not-integrated)
  - [Bad Web GL performance](#bad-web-gl-performance)
  - [GPU related artifacts](#gpu-related-artifacts)
- [Things that have been done which may help](#things-that-have-been-done-which-may-help)
  - [Field trial config was changed in 0.68.x](#field-trial-config-was-changed-in-068x)
- [Untriaged list of links / occurrences / etc that should be looked at](#untriaged-list-of-links--occurrences--etc-that-should-be-looked-at)
- [Actions we can take \(help needed\)](#actions-we-can-take-help-needed)

<!-- /MarkdownTOC -->

## Reported customer incidents
### GPU taking too much power (using discrete not integrated)
Work-around suggested by support for these folks has been to disable hardware acceleration, which seems to address the problem.
-  https://twitter.com/brendaneich/status/1146191407725441025?s=12 **(macOS / 0.65.121)**
    > Can you please make Brave not eat so much battery on my MacBook Pro? Seriously, just eats battery like a hungry thief, I need to use Safari instead.

- https://github.com/brave/brave-browser/issues/4858 **(macOS / 0.65.118)**
    > When GPU hardware acceleration is enabled (which is the default) it occurs that some browser window is enabling dedicated GPU (if you have an MBP 15 for example). Which is fine, a lot of modern web pages need that extra power for fancy things. But if you close that window or tab, Brave doesn't switch back to the integrated GPU. So you stay at using the dedicated GPU, which burns your battery. Chrome or other Chromium based browsers do switch back after some time, Brave doesn't.

- https://community.brave.com/t/brave-wont-change-back-to-integrated-gpu-and-high-battery-usage/47295 **(macOS / 0.60.47)**
    > I have a problem with GPU usage in macos Mojave.  When a webpage activates the dedicated GPU it will never go back to the integrated GPU when the webpage is closed. This then reflects in a much higher battery usage. I checked on chrome using the same webpages to trigger the dedicated GPU, and when I close them, always revert automatically to the integrated.
- https://www.reddit.com/r/brave_browser/comments/c1fyal/absurd_cpu_and_gpu_performance/ **(macOS / ??)**
    > Is there a reason it is using my radeon gpu chip instead of my intel chip? Some games I run do not even use the high performance chip; I just do not understand why it requires high performance when certain game do not (mac from used 2016), let alone a browser

- https://twitter.com/samwightt/status/1213668945678475265
    > Only thing that's keeping me from using it is that on my Surface Book 2 it defaults to using my GTX 1060 instead of integrated graphics, which DRAINS the battery. Using Chrome for now :(

### Bad Web GL performance
- https://github.com/brave/brave-browser/issues/4279 **(Windows 10 x64 / 0.69.24)**


> Whilst playing around with some WebGL 2 code I noticed that fps seemed to be roughly half of what Chrome was chucking out. Rendering 2 million cubes produced around 29fps whilst in Chrome it easily hit 60fps.

Still bad after the field trial updates ☹️ 

> Hi @bsclifton, unfortunately performance is the same. Still down at around 10fps (I've changed the test a bit so fps has dropped but it's same on Stable) on my test whereas Chrome is getting about 45fps.
>
> Tested on Version 0.69.24 Chromium: 75.0.3770.100 (Official Build) nightly (64-bit) that I just installed.

- https://www.reddit.com/r/brave_browser/comments/c8oe0b/webgl_is_very_slow_in_brave_compared_to_chrome/ **(macOS / 0.66.99)**
> A while ago when it first released, I tried playing [Minecraft Classic](https://classic.minecraft.net/) in Brave but it was very laggy and basically unplayable. I thought it was an issue with that specific game until last night when I released a [simple game](https://gamejolt.com/games/super-ball-dodge/423602) I made with a WebGL version and I tried to play it in Brave but it was extremely slow, especially when going into fullscreen mode

### GPU related artifacts
Basically rendering looks wrong (or not at all). Extremely slow performance. Work-around has been for users to disabled hardware acceleration (either in UI or via `--disable-gpu` command line flag).
- https://github.com/brave/brave-browser/issues/3969 **(macOS / ?)**
> My Brave experience has become so horrible. A new tab takes 3-5 secs, typing in the url bar has a lag of multiple seconds. While typing, the window turns to garbage.  I deleted ~/Library/Application Support/Brave* and reinstalled and it's the same.
> ...
> It's only a brave issue and for me it didn't start until the Dev release w/ ads (0.63.x). 

- https://github.com/brave/brave-browser/issues/3471 **(Mac OSX 10.10.5 / 0.62.7)**
> Search suggestion panel is not fully visible while typing in omnibox.
Moreover, sometimes there are some graphical artifacts (see screenshots in "Actual result").

- https://github.com/brave/brave-browser/issues/3582 **(Windows / 0.60.47)**
> Screen text garbled unreadable.

- https://github.com/brave/brave-browser/issues/3469 **(Windows 10 / 0.60.45)**
> totally blank user interface. only visible is popup menu (when right clicking)
all other browsers are working correctly (chrome, opera, firefox)

- https://github.com/brave/brave-browser/issues/3071 **(Windows / 0.58.21)**
> Whenever I open the browser, strange text fragmentation can be seen. It sometimes happens when I open the site, sometimes only after I interact with it. This isn't contained to website, also Brave intern sites like the settings or the DevTools are affected. Also on scrolling the text flickers into different fragmentations.

- https://github.com/brave/brave-browser/issues/2824 **(Windows / 0.58.18)**
> On many sites, text is showing as glitched/half showing. I've had this issue since moving to muon

- https://community.brave.com/t/the-ui-is-disintegrating/68684 **(Windows / 0.66.99, also on Nightly)**
> The ui is disintegrating. it keeps on disintegraating. i tried uninstalling and installing the browser. also i tried ll the releases like beta, dev, nightly but in all there’s a same problem. can you help me solve it?

## Things that have been done which may help
### Field trial config was changed in 0.68.x
This would ship to release channel on August 20th. Work was done in the following PRs:
- [Issue 4283: Disabling field trials](https://github.com/brave/brave-browser/pull/4551)
- [Issue 4552: Restore features to correct state after disabling field trials](https://github.com/brave/brave-core/pull/2471)


## Untriaged list of links / occurrences / etc that should be looked at
- https://www.reddit.com/r/brave_browser/comments/bpv26f/flickering_when_watching_full_screen_video/
- https://www.reddit.com/r/brave_browser/comments/btku7u/hardware_acceleration/
- https://www.reddit.com/r/brave_browser/comments/c0dhbb/screen_tearing_in_youtube_with_hardware/
- https://www.reddit.com/r/brave_browser/comments/bf4dtx/minor_issues_with_brave_pc_browser/

### Reported links that don't seem to be associated
- https://www.reddit.com/r/brave_browser/comments/c7nuhw/please_someone_tell_me_why_hardware_acceleration/
- https://www.reddit.com/r/brave_browser/comments/aifpob/slow_scroll_performamce_on_sites_with_a_lot_of/
- https://www.reddit.com/r/brave_browser/comments/c1sufr/brave_performance_issues/

## Actions we can take (help needed)
- [ ] Values that were changed with the field trials should be examined. This config change will make us more closely match Chrome/Chromium. We may have had a flag enabled which made Brave prefer the discrete GPU - or perhaps we had a flag on which bypassed the driver blacklist
- [ ] Triage the above occurrences and break down by OS / Brave version
- [ ] Find more occurrences and triage
- [x] Verify on macOS that we have added the `NSSupportsAutomaticGraphicsSwitching` key to Brave's info.plist _**(UPDATE: @bsclifton verified it is there and set to `true`)**_
- [ ] Join https://groups.google.com/a/chromium.org/d/forum/graphics-dev and share occurrences / details