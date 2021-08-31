# Coarse-Grained Timing Information for Federated Learning

## Informal intuition

Are there patterns in when Brave users run their browser? For example, are some users using Brave only during the work day and not on the weekend? Are some people using it only in the morning or only in the evening?

These are the kinds of questions we are seeking to answer as a first step toward improving our on-device recommendation engine for opt-in (user consent before activation) Brave News (formerly [Brave Today](https://brave.com/announcing-brave-today/)) and [privacy-preserving ads](https://brave.com/brave-rewards/). The technical term for what we are collecting is "operational patterns," but what that really means is this: 

- Each participant in this study will be assigned a random identifier, the collection ID, which will be stored in browser local storage.
- Every half-hour that the browser is in use, the browser will send Brave servers a ping to say that this participant's browser was in active use during that time.
- After 30 days, all of these pings will be aggregated and Brave will delete the collection ID from the server data and from the browser.

## Why is Brave recording this information?

Brave is recording this operational pattern data as part of a larger project for [building privacy-preserving, federated learning systems](https://brave.com/federated-learning/), so that Brave can improve the quality of machine learning for private on-device recommendations in the browser. Specifically, we hope to understand how browser usage varies throughout the day.

## Small-scale operational pattern example 

Below is an example of what operational patterns look like. We depict patterns from just two browsers over 24 hours with 2-hour collection slots. A blue cell indicates that a browser was online during a 2-hour slot.

| Hour<br>(Slot) | 0<br>(0) | 2<br>(1) | 4<br>(2) | 6<br>(3) | 8<br>(4) | 10<br>(5) | 12<br>(6) | 14<br>(7) | 16<br>(8) | 18<br>(9) | 20<br>(10) | 22<br>(11) |
| ----------- | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| Browser 1   | &bull; | &bull; | |  | | | &bull; | &bull; | &bull; | | &bull; |
| Browser 2   | | | | | &bull; | &bull; | &bull; | | &bull; | &bull; | &bull; |

In practice, we are running this collection for 3 weeks and with 30-minute collection slots for a random subset of US-based users.

## Can usage patterns be used to fingerprint me?

The short answer is no. Like usage data recordings, operational patterns do not contain any personally-identifying information, information about the sites you visit, or any information other than the minimal and anonymous usage data described below. However, your usage pattern could be unique amongst all of the participants and that is why we are ensuring that this information is not tied to anything else about you,  other than your operating system.

In particular, all server pings are sent through a TCP proxy which hides your IP address from Brave. We will also send that ping without User-Agent or Accept-Language headers. Note that we implicitly know your country since this study is only active for users in the US.

We are also excluding our smallest operating system (Linux) in order to ensure that each participant belongs to a large pool of eligible users.

## Who is selected to be a participant?

In order to be randomly selected to participate in this study, you must be:

- located in the USA;
- using an operating system other than Linux;
- have the [privacy-preserving analytics](https://brave.com/privacy-preserving-product-analytics-p3a/) setting enabled in Brave (default is ON).

From those eligible, 10% are randomly self-elected as participants via a decentralised client-side die roll. If you want to know whether you are part of this experiment, open brave://version/?show-variations in your browser and look for the _OperationalPatternsStudy_ under Variations. If the study is not listed, then you’re not a participant in this study.

All other Brave users will not be included or affected by the collection at all. Further, users can opt out of the measurements described here, along with all other analytics measurements, by going to brave://settings/privacy and switching off _Allow privacy-preserving product analytics (P3A)_.

![Screenshot of P3A controls in Settings](https://user-images.githubusercontent.com/815158/131560706-ae18adc1-454d-4058-a841-0795a52392d8.png)

Once you do this, the browser will send a delete request to the server with your collection ID. The server will then remove any usage reports it has received for that participant (see [https://github.com/brave/brave-browser/issues/17502](https://github.com/brave/brave-browser/issues/17502) for the status of the opt-out ping). After that, your collection ID will be removed from your browser. Opting out this way will also opt you out of any follow-up studies.

## Technical Details 

### What information is contained in operational pattern recordings?

The ID that was assigned to the participant (called "collection ID"), the timeslot that the ping relates to (e.g. 356 would correspond to the 10am to 10:30am timeslot of the 7th day of the month), and the operating system (e.g. Android).

Here is an example of a ping sent to our server:

```json
{
    "collection_id": "AXR6842CER27463NWQL",
    "collection_slot": 356,
    "platform": "android-bc",
    "wiki": "https://github.com/brave/brave-browser/wiki/Operational_Patterns"
}
```

### What information is stored in our database?

This shows what we construct on the server from the individual pings and stored in our database on a per-browser basis after 30 days:

| Index | Platform | Pattern |
| ----- | -------- | ------- |
| 1 | osx-bc | `[0, 2, 12, 14, 16, 20, ...]` |
| 2 | osx-bc | `[38, 42, 51, 78, 120, 121, ...]` |
| 3 | android-bc | `[2, 3, 22, 24, 26, 50, ...]` |

Notice that the collection ID is not present in the database and a sequential index is used instead. 