# Introduction
One of the main needs of advertisers is to be able to effectively measure and optimize the ads that they serve through Brave Ads. 

Advertisers have the ability to target their ads to different categories in the Brave Ads Catalog. Advertisers often experiment by serving several different ads to different content categories, in order to determine which messages deliver the best results. The advertiser can then review the results, remove categories that do not perform well and spend more of their budget serving their ads to categories that deliver the best results. 

Within the advertising industry, ads are often referred to as creatives. Brave Ads also refer to individual ads that serve as Creatives. This document refers to each ad that an advertiser sets up to run in their Brave Ads campaign as a Creative.  

Brave Ads provides advertisers with anonymous-but-accountable reporting for their campaigns, including hourly and daily information about how each Creative (ad) performed for each operating system, and the categories that the Creative was assigned to. 

In addition to Brave Ads reporting, advertisers will often also check their own first-party server logs for landing page visits from their Brave Ads to make sure that the amount of site visits align with Brave Ads reporting. 

Advertisers often assign parameters to click-through URLs to distinguish between Creatives and categories that their ads are served to. **These values are not unique per user, and do not track individual users**. 

To better standardize and provide additional transparency to users and advertisers, a set of click-through parameters are reserved by Brave Ads. This document outlines each of these reserved parameters, how they work, and the types of values that pass into them. 

# Brave Ads Reserved Parameter Values 
## Click-through URLs, and URL parameters
The web address that a Brave Ads user visits after they click on an ad is known as a click-through URL. 

Click-through URLs are often appended with optional query string parameters to differentiate sources of visits to a web page. 

For example, a user that clicks on either of the web addresses below will visit the same page: 

* `https://brave.com` 
* `https://brave.com/?parameter=value`

Advertisers often use parameter values to distinguish traffic from different advertising and marketing platforms, and to measure and report campaign performance. 

For example, an advertiser launching a campaign for new headphones may setup clickthrough creatives with the following parameters: 

* `https://headphones.com/?source=braveads&campaign=march&ad=save10promo`
* `https://headphones.com/?source=facebook&campaign=march&ad=save10promo`
* `https://headphones.com/?source=google&campaign=march&ad=save10promo`

All of the URLs above visit the same page. With the parameters added, the advertiser can check their logs for visits from Brave Ads, Facebook or Google, for the campaign month, and for the specific promotion. 

**Brave Ads do not allow advertisers to track individual users**. Advertisers often use URL parameters in conjunction with third-party trackers to uniquely identify and attribute parameter values to individual users. **By default, Brave blocks third-party scripts and trackers to prevent this**. In comparison, both Facebook and Google Ads permit query parameter user tracking through their fbclid (Facebook) and gclid (Google) parameters, which Brave removes as part of our privacy protection.   

# Reserved Click-through URL Parameters
Brave Ads reserves the following click-through URL parameters: 

* `brave-campaign-id`
* `brave-creative-set-id`
* `brave-creative-id`

`brave-campaign-id`
* The Brave Ads server generates and assigns a unique `campaignId` for every Brave Ads campaign. 
* Example of a Brave Ads `campaignId`: `04ec84e2-6afd-4432-8703-aa70faf7c50b`

`brave-creative-set-id`
* The Brave Ads server generates and assigns a unique `creativeSetId` for Creative Sets that are assigned to Brave Ads campaigns. 
* Creative Sets may include one or more Creatives, and include the Category and Operating System that the Creative Set is targeted to serve to. 
* Example of a Brave Ads `creativeSetId`: `ae7a3556-8414-4444-a584-3f4eb9abd18e`

`brave-creative-id`
* The Brave Ads server generates and assigns a unique `creativeInstanceId` for every creative that is assigned to a Creative Set in Brave Ads Campaigns.
* The `creativeInstanceId` provides the advertiser to determine how the same creative performed for each Creative Set it has been assigned to. 
* Example of a Brave Ads `creativeInstanceId`: `878d82ed-4af6-4f73-b555-f7d64306025a`

An example of a Brave Ads click-through URL containing the reserved parameters and parameter values: 

`https://foo.com/?brave-campaign-id=04ec84e2-6afd-4432-8703-aa70faf7c50b&brave-creative-set-id=ae7a3556-8414-4444-a584-3f4eb9abd18e&brave-creative-id=878d82ed-4af6-4f73-b555-f7d64306025a`

With the appended Brave Ads reserved click-through URL parameters, the advertiser can reference their first-party server logs against their Brave Ads reporting to evaluate the volume of overall traffic served for Brave Ads campaign served, the Creative Set and the Creative assigned to each Creative Set. 


# Reserved Parameter Table
The reserved parameters **are intentionally not unique to each Brave Ads user**, and **are not used to identify or attribute page visits to an individual user**. 
<table>
<tr>
<td>Reserved Parameter</td>
<td>Passes this value</td>
<td>Example Pair</td>
<td>Example URL</td>
</tr>
<tr>
<td>brave-campaign-id, mtm_campaign, utm_campaign</td>
<td>campaignId</td>
<td>brave-campaign-id=04ec84e2-6afd-4432-8703-aa70faf7c50b</td>
<td>https://foo.com/?brave-campaign-id=04ec84e2-6afd-4432-8703-aa70faf7c50b</td>
</tr>
<tr>
<td>brave-creative-set-id</td>
<td>creativeSetId</td>
<td>brave-creative-set-id=ae7a3556-8414-4444-a584-3f4eb9abd18e</td>
<td>https://foo.com/?brave-creative-set-id=ae7a3556-8414-4444-a584-3f4eb9abd18e</td>
</tr>
<tr>
<td>brave-creative-id, mtm_content</td>
<td>creativeInstanceId</td>
<td>brave-creative-id=878d82ed-4af6-4f73-b555-f7d64306025a</td>
<td>https://foo.com/?brave-creative-id=878d82ed-4af6-4f73-b555-f7d64306025a</td>
</tr>
</table>


Note: The table above only covers the URL parameters that are reserved by Brave Ads, and the Brave Ads server. Brave Ads click-through URLs may include additional URL parameters and values assigned by the advertiser for their campaign. Similar to Brave Ads reserved URL parameters, additional URL parameters included by the advertiser **are the same for all Brave Ads users**, and **are not uniquely identifiable per user**. 
