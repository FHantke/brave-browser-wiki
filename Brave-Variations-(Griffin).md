Griffin is Brave's version of Google's Finch - A backend for Chromium's variation service. The variations service is a browser component to progressively roll-out and test new features in Chromium-based browsers. Griffin provides the following capabilities to improve security, reliability and user experience:

1. Staged Rollouts
2. Private Statistical Experiments
3. Parameter Updates

## Staged Rollouts
Staged rollouts are a way to test feature changes on a progressively increasing fraction of the user population. That way product risk is reduced by detecting unintended side effects, like web compatibility issues, early on before rolling them out to the whole user base.

## Private Statistical Experiments
Statistical experiments or A/B tests are a form of randomized controlled trials (RCTs) and a popular method to compare different variations of a product. The key mechanism hereby is the random assignment of browsers into groups, each of which then executes variant-conditional code. The goal is to detect a meaningful difference between the variations, e.g. a significant drop in the number of crashes.

## Parameter Updates
In its simplest form the variations service can be used as a parameter server to control the configuration of features remotely. This is crucial for example if we want to immediately disable a vulnerable feature without having to rush a hotfix.

# Variations are private and transparent
With these benefits in mind, Brave’s privacy and security promises always trump any potential upside of new features like the use of variations. This is why we impose the following restrictions on the service, following our usual privacy-first principles:

* Unchanged data collection policy
* Everything is open source
* Privacy reviews

## Unchanged Data Collection Policy
There are two ways that variations could affect data collection. The first are HTTP logs on the variations server since each browser has to periodically request the configuration file. Like with all Brave services, the file is being served through a CDN and request logs are never persisted.

In the case of experiments one has to either observe existing downstream metrics like crash report numbers, or implement a dedicated response measurement to be able to analyse the effect of variations. For the latter, we don’t allow anything but our proven mechanism for [privacy preserving product analytics](https://brave.com/privacy-preserving-product-analytics-p3a/) (P3A). Until we have implemented an [opt-out mechanism](https://github.com/brave/brave-browser/issues/15711), A/B tests are limited to users who have opted-in to ads.

## Everything is Open Source
To verify and audit the workings of the variations service there are several options. As always the entire codebase is open source:

* The variations service browser component in [Chromium](https://source.chromium.org/chromium/chromium/src/+/master:components/variations/service/variations_service.h).
* All resources to generate, sign and serve variations in the [brave-variations](https://github.com/brave/brave-variations) repository.
* All feature implementations in the [brave-core](https://github.com/brave/brave-core) repository (search for [base::Feature](https://github.com/brave/brave-core/search?q=base%3A%3Afeature)).

To inspect active studies in the Brave browser (as opposed to Chrome), navigate to brave://version. On top of that we have built a dashboard, listing all active studies at https://griffin.brave.com/.

## Privacy Reviews
Privacy reviews scrutinise any new study with respect to its impact on the identifiability and addressability of users. In the cases where Brave acts as a data controller (e.g. Brave ads), this could mean only allowing experiments to run in sufficiently large markets like the US. This is to ensure that groups are always large enough to prevent the identification of individual users by Brave. To prevent  publishers and websites from telling individual users apart, this could mean ensuring that page-visible changes don’t increase the fingerprinting surface, as exposed through the JavaScript API.

# Variations under the Hood
The variations service consists of Chromium’s browser component and a server, which hosts a configuration file - also called seed file - containing definitions for all variations. Variations break down into studies, each containing a list of experiments. Each experiment can be thought of as a set of enabled and/or disabled features with parameters. As described in the previous section, there are strict rules around which studies can run in the browser.

The browser pulls a new version of the seed file, once on startup and subsequently every 30 minutes. The seed file is then parsed internally by Chromium’s [variation service](https://source.chromium.org/chromium/chromium/src/+/master:components/variations/service/variations_service.h).

## Studies
A study is a set of experiments conducted on clients according to filter rules concerning country, platform and version. In the case of an A/B test the experiment can be thought of as the group that the browser signs-up to with a defined probability, e.g. "Group A" and "Group B" with both a 50% chance of being selected.

## Experiments
An experiment (think “group”) is a set of features and parameters that is enabled or disabled with a given sign-up probability. Each client independently “flips a biased coin” to determine whether it is eligible for an experiment. “Coin flips” are implemented via Chromium’s [field trials](https://source.chromium.org/chromium/chromium/src/+/master:base/metrics/field_trial.h).

## Features
A feature is the underlying abstraction that links code on the client to studies from the variations service. It is Chromium’s implementation of [feature flags](https://en.wikipedia.org/wiki/Feature_toggle). A feature can have an arbitrary number of associated parameters in the form of key/value pairs.