---
title: "Ad Analytics by Country"
description: |
  Daily performance of each campaign by the country of the members who saw the ads: impressions, clicks, cost in local currency and in USD, engagements, conversions and lead metrics. One row per `dateRangeStart`, `dateRangeEnd`, `campaign` and `country` combination. `country` is LinkedIn's URN for the location (for example `urn:li:geo:{id}`) and `countryName` its English name; `campaign` is the campaign URN, and its `{id}` is the `id` in [Campaigns](./campaigns.md).

  LinkedIn reports location one campaign at a time, so the connector makes more requests than for [Ad Analytics](./ad-analytics.md). LinkedIn drops a location with fewer than 3 events, so totals can be lower than in Ad Analytics, and location data arrives 12 to 24 hours late. LinkedIn does not break down reach, audience penetration, conversion value or the carousel card metrics by location, so this table has none of those columns.
tags: ["owox", "connector", "linkedin-ads"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://github.com/OWOX/owox-data-marts/blob/6b104bfa5980750dc37865eae15712577f42f307/packages/connectors/src/Sources/LinkedInAds/LinkedInAdsAPIReference/LinkedInAdsFieldsSchema.js"
    title: "OWOX Data Marts LinkedIn Ads connector — Ad Analytics by Country endpoint (`adAnalyticsByCountry`)"
  - id: "vendor-reference"
    resource: "https://learn.microsoft.com/en-us/linkedin/marketing/integrations/ads-reporting/ads-reporting"
    title: "LinkedIn Marketing API reference — ads-reporting"
---

## Connector endpoint

- **Endpoint:** LinkedIn Ads · `adAnalyticsByCountry`
- **Default destination table:** `linkedin_ads_ad_analytics_by_country`
- **Unique keys:** `dateRangeStart`, `dateRangeEnd`, `campaign`, `country`
- **Time series:** yes, loaded by date range

# Schema

| Column | Type | Description |
|--------|------|-------------|
| `account` | STRING | URN of the advertising account, for example urn:li:sponsoredAccount:{id}. |
| `campaign` | STRING | PK. URN of the campaign, for example urn:li:sponsoredCampaign:{id}. Its {id} is the id in Campaigns. |
| `country` | STRING | PK. URN of the member country the metrics belong to, for example urn:li:geo:{id}. |
| `countryName` | STRING | Name of the member country in English, as LinkedIn spells it, for example "United States" or "California, United States". Empty when LinkedIn returns no name for the location. |
| `actionClicks` | NUMERIC | The count of clicks on the action button of the Sponsored Messaging ad. |
| `adUnitClicks` | NUMERIC | The count of clicks on the ad unit displayed alongside the Sponsored Messaging ad. |
| `averageDwellTime` | NUMERIC | Average user dwell time (in seconds). It measures the duration for which more than 50% of the ad's pixels remain visible in the viewport. |
| `clicks` | NUMERIC | The count of chargeable clicks. Despite not charging for clicks for CPM campaigns, this field still represents those clicks for which we would otherwise charge advertisers based on objective (for example, clicks to view the landing page or company page). |
| `commentLikes` | NUMERIC | The count of likes of a comment. Sponsored Content only. |
| `comments` | NUMERIC | The count of comments. Sponsored Content only. |
| `companyPageClicks` | NUMERIC | The count of clicks to view the company page. |
| `costInLocalCurrency` | NUMERIC | Cost in the account's local currency based on the pivot and timeGranularity. For example, this would be spend by member company size per month if the pivot is MEMBER_COMPANY_SIZE and timeGranularity is MONTHLY. Cost is not adjusted for over delivery when a member professional demographic pivot is specified in the request. |
| `costInUsd` | NUMERIC | Cost in USD based on the pivot and timeGranularity. For example, this would be spend by campaign on a given day if the pivot is CAMPAIGN and timeGranularity is DAILY. Cost is not adjusted for over delivery when a member professional demographic pivot is specified in the request. |
| `costPerQualifiedLead` | NUMERIC | How much money was spent per qualified lead. Ratio is costInLocalCurrency / qualifiedLeads. |
| `dateRangeStart` | DATE | PK. Start date of the report data point. Date is specified in UTC format (YYYY-MM-DD). |
| `dateRangeEnd` | DATE | PK. End date of the report data point. Date is specified in UTC format (YYYY-MM-DD). |
| `documentCompletions` | NUMERIC | The number of times users reached 100% of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `documentFirstQuartileCompletions` | NUMERIC | The number of times users reached the first quartile of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `documentMidpointCompletions` | NUMERIC | The number of times users reached the second quartile of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `documentThirdQuartileCompletions` | NUMERIC | The number of times users reached the third quartile of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `downloadClicks` | NUMERIC | The number of times users have indicated the intent to download the media in an ad by clicking the download icon. This may or may not result in an actual download (e.g. if the user rejects a browser download prompt). This metric is only available for ad formats supporting media downloads. |
| `externalWebsiteConversions` | NUMERIC | Total number of times users took a desired action after clicking on or seeing your ad. When conversions cannot be attributed to individual users, group level attribution or estimation may be used. |
| `externalWebsitePostClickConversions` | NUMERIC | Total number of times users took a desired action after clicking on your ad. When conversions cannot be attributed to individual users, group level attribution or estimation may be used. |
| `externalWebsitePostViewConversions` | NUMERIC | Total number of times users took a desired action after seeing your ad. When conversions cannot be attributed to individual users, group level attribution or estimation may be used. |
| `follows` | NUMERIC | The count of follows. Sponsored Content and Follower ads only. |
| `fullScreenPlays` | NUMERIC | Number of times members click on the full screen button or on the video(mobile only) to go into full screen mode. |
| `headlineClicks` | NUMERIC | The number of times members clicked on the headline of conversation ads. |
| `headlineImpressions` | NUMERIC | The number of times members were shown the headline of conversation ads. |
| `impressions` | NUMERIC | This is the count of impressions for Sponsored Content and sends for Sponsored Messaging. |
| `jobApplications` | NUMERIC | The number of times a member completed a job application after viewing or clicking on an ad. Currently, this metric is broken down into postViewJobApplications (if the member applied after viewing the ad) and postClickJobApplications (if the member applied after clicking the ad). |
| `jobApplyClicks` | NUMERIC | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after viewing or clicking on an ad which has a LinkedIn job landing page. Currently, this metric is broken down into postViewJobApplyClicks (if the member performed the action after viewing the ad) and postClickJobApplyClicks (if the member performed the action after clicking the ad). |
| `landingPageClicks` | NUMERIC | The count of clicks which take the user to the creative landing page. |
| `leadGenerationMailContactInfoShares` | NUMERIC | The number of times users shared contact info through the One Click Lead Gen for Sponsored Messaging ads. |
| `leadGenerationMailInterestedClicks` | NUMERIC | The count of Sponsored Messaging ad recipients who clicked to demonstrate interest. |
| `likes` | NUMERIC | The count of likes. Sponsored Content only. |
| `oneClickLeadFormOpens` | NUMERIC | The count of times users opened the lead form for a One Click Lead Gen campaign. |
| `oneClickLeads` | NUMERIC | The count of leads generated through One Click Lead Gen. |
| `opens` | NUMERIC | The count of opens of Sponsored Messaging ads. |
| `otherEngagements` | NUMERIC | The count of user interactions with the ad unit that do not fit into any other more specific category. |
| `postClickJobApplications` | NUMERIC | The number of times a member completed a job application after clicking on an ad. See also jobApplications. |
| `postClickJobApplyClicks` | NUMERIC | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after clicking on an ad which has a LinkedIn job landing page. See also jobApplyClicks. |
| `postClickRegistrations` | NUMERIC | The number of times a member has registered for an event or seminar after clicking on an ad which has a LinkedIn landing page. This includes gross registrations and does not account for a user unregistering. |
| `postViewJobApplications` | NUMERIC | The number of times a member completed a job application after viewing an ad. See also jobApplications. |
| `postViewJobApplyClicks` | NUMERIC | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after clicking an ad which has a LinkedIn job landing page. See also jobApplyClicks. |
| `postViewRegistrations` | NUMERIC | The number of times a member has registered for an event or seminar after viewing an ad which has a LinkedIn event landing page. This includes gross registrations and does not account for a user unregistering. See also registrations. |
| `qualifiedLeads` | NUMERIC | The count of qualified leads shared by the advertiser. Qualified lead is a lead that has been deemed more likely to become a customer compared to other leads, based on their engagement and fit. |
| `reactions` | NUMERIC | The count of positive reactions on Sponsored Content which can capture, like, interest, praise, and other responses. |
| `registrations` | NUMERIC | The number of times a member has registered for an event or seminar after viewing or clicking on an ad which has a LinkedIn event landing page. This includes gross registrations and does not account for a user unregistering. |
| `sends` | NUMERIC | The count of sends of Sponsored Messaging ads. |
| `shares` | NUMERIC | The count of shares. Sponsored Content only. |
| `subscriptionClicks` | NUMERIC | The count of clicks to subscribe to a series, such as a Newsletter. |
| `talentLeads` | NUMERIC | Number of leads captured through a talent media campaign. |
| `textUrlClicks` | NUMERIC | The count of clicks on any links (anchor tags) that were included in the body of the Sponsored Messaging ad. |
| `totalEngagements` | NUMERIC | The count of all user interactions with the ad unit. |
| `validWorkEmailLeads` | NUMERIC | The count of leads with a valid work email that does not use an established free or personal email domain. |
| `videoCompletions` | NUMERIC | The count of video ads that played 97-100% of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoFirstQuartileCompletions` | NUMERIC | The count of video ads that played through the first quartile of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoMidpointCompletions` | NUMERIC | The count of video ads that played through the midpoint of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoStarts` | NUMERIC | The count of video ads that were started by users. |
| `videoThirdQuartileCompletions` | NUMERIC | The count of video ads that played through the third quartile of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoViews` | INTEGER | A video ad playing for at least 2 continuous seconds 50% in-view, or a click on the CTA, whichever comes first. An interaction with the video (like going to fullscreen mode) does not count as a view. |
| `viralClicks` | NUMERIC | The count of clicks on viral impressions. See viral impressions definition. Sponsored Content only. |
| `viralCommentLikes` | NUMERIC | The count of likes on comments from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralComments` | NUMERIC | The count of comments from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralCompanyPageClicks` | NUMERIC | The count of clicks to view the company page from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralDocumentCompletions` | NUMERIC | The number of times users reached 100% of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDocumentFirstQuartileCompletions` | NUMERIC | The number of times users reached the first quartile of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDocumentMidpointCompletions` | NUMERIC | The number of times users reached the second quartile of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDocumentThirdQuartileCompletions` | NUMERIC | The number of times users reached the third quartile of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDownloadClicks` | NUMERIC | The number of times users have indicated the intent to download the media in a viral ad by clicking the download icon. This may or may not result in an actual download (e.g. if the user rejects a browser download prompt). Only available for ads supporting media downloads. |
| `viralExternalWebsiteConversions` | NUMERIC | The count of conversions that are attributed to your ads driven by a viral event. See viral impressions definition. |
| `viralExternalWebsitePostClickConversions` | NUMERIC | The count of post-click conversions that are attributed to your ads driven by a viral click. See viral impressions definition. |
| `viralExternalWebsitePostViewConversions` | NUMERIC | The count of post-view conversions that are attributed to your ads driven by a viral impression. See viral impressions definition. |
| `viralFollows` | NUMERIC | The count of follows from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralFullScreenPlays` | NUMERIC | Number of times members click on the full screen button or on the video(mobile only) to go into full screen mode. See viralImpressions definition. |
| `viralImpressions` | NUMERIC | The count of viral impressions for this activity. Viral impressions are those resulting from users sharing sponsored content to their own network of connections. Viral impressions are not counted as regular impressions. Sponsored Content only. |
| `viralJobApplications` | NUMERIC | The number of times a member completed a job application after viewing or clicking on a viral ad. Currently, this metric is broken down into viralPostViewJobApplications (if the member performed the action after viewing the viral ad) and viralPostClickJobApplications (if the member performed the action after clicking the viral ad). |
| `viralJobApplyClicks` | NUMERIC | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after viewing or clicking on a viral ad which has a LinkedIn job landing page during the date range.Currently, this metric is broken down into viralPostViewJobApplyClicks (if the member performed the action after viewing the viral ad) and viralPostClickJobApplyClicks (if the member performed the action after clicking the viral ad). |
| `viralLandingPageClicks` | NUMERIC | The count of clicks on viral impressions to take the user to the creative landing page. See viral impressions definition. Sponsored Content only. |
| `viralLikes` | NUMERIC | The count of likes from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralOneClickLeadFormOpens` | NUMERIC | The count of times users opened the lead form for viral impressions from a Lead Gen campaign. See viral impressions definition. |
| `viralOneClickLeads` | NUMERIC | The count of leads generated through One Click Lead Gen from viral impressions for this activity. See viral impressions definition. |
| `viralOtherEngagements` | NUMERIC | The count of user interactions with viral impressions that do not fit into any other more specific category. See viral impressions definition. Sponsored Content only. |
| `viralPostClickJobApplications` | NUMERIC | The number of times a member completed a job application after clicking on a viral ad. |
| `viralPostClickJobApplyClicks` | NUMERIC | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after clicking on a viral ad which has a LinkedIn job landing page. |
| `viralPostClickRegistrations` | NUMERIC | The number of times a member has registered for an event or seminar after clicking on a viral ad which has a LinkedIn landing page. See viralImpressions definition. This includes gross registrations and does not account for a user unregistering. |
| `viralPostViewJobApplications` | NUMERIC | The number of times a member completed a job application after viewing a viral ad. |
| `viralPostViewJobApplyClicks` | NUMERIC | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after viewing a viral ad which has a LinkedIn job landing page. |
| `viralPostViewRegistrations` | NUMERIC | The number of times a member has registered for an event or seminar after viewing a viral ad which has a LinkedIn event landing page. See viralImpressions definition. This includes gross registrations and does not account for a user unregistering. |
| `viralReactions` | NUMERIC | The count of positive reactions on viral Sponsored Content which can capture like, interest, praise, and other responses. See viral impressions definition for details on viral engagements. |
| `viralRegistrations` | NUMERIC | The number of times a member has registered for an event or seminar after viewing or clicking on a viral ad which has a LinkedIn event landing page. See viralImpressions definition. This includes gross registrations and does not account for a user unregistering. |
| `viralShares` | NUMERIC | The count of shares from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralSubscriptionClicks` | NUMERIC | The count of viral clicks to subscribe to a series, such as a Newsletter. |
| `viralTotalEngagements` | NUMERIC | The count of all user interactions with a viral ad unit. See viral impressions definition. Sponsored Content only. |
| `viralVideoCompletions` | NUMERIC | The count of viral video ads that played 97-100% of the video. This includes watches that skipped to this point. See viralImpressions definition. |
| `viralVideoFirstQuartileCompletions` | NUMERIC | The count of viral video ads that played through the first quartile of the video. This includes watches that skipped to this point. See viralImpressions definition. |
| `viralVideoMidpointCompletions` | NUMERIC | The count of viral video ads that played through the midpoint of the video. This includes watches that skipped to this point. See viralImpressions definition. |
| `viralVideoStarts` | NUMERIC | The count of viral video ads that were started by users. See viralImpressions definition. Since viral videos are automatically played for ON_SITE, this will be the same as viralImpressions if the servingLocation is ON_SITE. |
| `viralVideoThirdQuartileCompletions` | NUMERIC | The count of viral video ads that played through the third quartile of the video. This includes watches that skipped to this point. See viralImpressions definition |
| `viralVideoViews` | NUMERIC | A viral video ad playing for at least 2 continuous seconds 50% in-view, or a click on the CTA, whichever comes first. An interaction with the video (like going to full screen mode) does not count as a view. See viralImpressions definition. |

# Example Questions

- How much did each campaign spend in each country last month?
- Which countries produced the most external website conversions, and at what cost?
- How do impressions and clicks per country compare day by day for one campaign?
