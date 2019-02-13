## Context

In the past (~2016), Brave accepted many suggestions from the community to add search engine providers as options for the default search engine in Brave. There was no formal vetting process for these providers; we basically added search engines if it seemed like there was any user demand for them. As a result, we ended up bundling search providers that were objectionable/offensive to some users.

In 2019, we opened an issue to vastly reduce the bundled search engines list, given that most users only use one of a small set of search engines and others can be easily added in `brave://settings/searchEngines` (at least on Brave Desktop): https://github.com/brave/brave-browser/issues/3316. 

## Vetting new search providers

Bundled search providers are akin to bundled extensions in Brave: they are often seen as vetted/approved by Brave staff, are presented to all users in the UI, and thus should go through a review process instead of being accepted just because someone asked for it or opened a pull request for it.

### Proposed guidelines

Any search engine which is included in the list of available search providers in Brave should meet all of the following criteria:

* Be popular enough that not including it by default would make a significant percentage of people stop using Brave OR be a Brave partner / prospective partner OR offer privacy as a distinguishing feature (ex: not blocking Tor).
* The existence of the search engine in the Brave search providers list is not likely to be objectionable to users.
* The provider should not actively promote or publish content that is harmful, hateful, deliberately misleading, or likely to be offensive.

### Proposed enforcement

Before a search engine is added to any platform, a ticket must be opened in brave/internal to review it. A panel of reviewers, TBD, will do their best effort to review it according to the guidelines above. If there are complaints about the search engine being included, we will re-review.