
Third-party frames which get or set navigation cookies, use `document.cookie`,
and DOM storage will be given a special ephemeral storage area. This ephemeral
storage area is only given to third-party frames which otherwise have cookies
or storage blocked on them. This determination is made based all other checks,
including those by Chromium content settings and Brave Shields. If any of these
systems block third-party storage, then that storage is a candidate for
ephemeral storage.

## Partitioning and Lifetime of Ephemeral Storage Area

The ephemeral storage area is unique for every top-level frame origin. When
the last top-level frame with a particular origin is closed or navigated to a new
origin, the ephemeral storage area for that origin is destroyed. Navigating
back to the original top-level origin creates a new, empty ephemeral storage
area.  Although an ephemeral storage area is unique to a top-level origin,
cookies and DOM storage in the ephemeral storage still follow typical
same-origin restrictions within that area. This means that two third-party
iframes from different origins will not share cookies or DOM storage even
though they exist in the same ephemeral storage area.

## Implementation for DOM Storage

DOM Storage is composed of both `localStorage` and `sessionStorage`. These
two APIs have different scopes and lifetimes, but are provided via an
identical API to JavaScript.

### `sessionStorage`

`sessionStorage` is a per-origin and per-tab transient storage database. In
Brave, each tab has a unique storage area id which ensures that normal
`sessionStorage` is not shared between tabs. The implementation of ephemeral
storage creates a special ephemeral version of this id when a situation
requiring ephemeral storage is detected. This storage area is cleared when the
top-level frame navigates to a new origin.

### `localStorage`

localStorage is a persistent storage database that is partitioned per-origin.
The implementation of ephemeral `localStorage` detects when a situation requiring
ephemeral `localStorage` is required and provides the JavaScript environment with
a specially keyed `sessionStorage` database instead of the typical `localStorage`
one. This `sessionStorage` database (acting as `localStorage`) is shared between
all third-party frames using ephemeral storage under the same top-frame origin.
This storage area is cleared when no more top-frames are navigated to
pages with that origin.

## Implementation of Cookies

Cookies in Brave are ultimately stored in the `CookieMonster`. To implement cookie
support for ephemeral storage, we create a new subclass class of CookieMonster
which contains a map of ephemeral `CookieMonster`s in additional to the default
storage provided by the parent class. When the `CookieMonster` gets or sets a
cookie with a `top_frame_url_` set in the `CookieOptions` argument, it search for the
appropriate ephemeral `CookieMonster` for that URL's origin. RestrictedCookieManager
(used for `document.cookie`) and URLRequestHttpJob (used for navigation cookies)
set the `top_frame_url_` CookieOption member when detecting a situation needing
ephemeral storage.

## Flag

Ephemeral storage is only enabled when the corresponding `Feature` flag is turned
on.

## Implementation Plan

Note: The initial PRS for ephemeral storage are not dependent on whether or not
storage is blocked. Instead ephemeral storage is used for *all* third-party
storage that is not blocked. This changes in PR #4, which brings ephemeral
storage in line with the final design.

* Stage 1: Add ephemeral storage support for DOM Storage
* Stage 2: Add ephemeral storage support for `document.cookie`
* Stage 3: Add ephemeral storage support for navigation cookies
* Stage 4: For all types of ephemeral storage, only use ephemeral storage when a cookie or storage is otherwise blocked.
* Stage 5: Enable ephemeral storage by default
