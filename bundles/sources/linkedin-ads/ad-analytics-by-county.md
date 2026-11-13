---
title: "Ad Analytics by County"
description: |
  Daily performance of each campaign by the county of the members who saw the ads: impressions, clicks, cost in local currency and in USD, engagements, conversions and lead metrics. One row per `dateRangeStart`, `dateRangeEnd`, `campaign` and `county` combination. `county` is LinkedIn's URN for the location (for example `urn:li:geo:{id}`) and `countyName` its English name; `campaign` is the campaign URN, and its `{id}` is the `id` in [Campaigns](./campaigns.md).

  LinkedIn reports location one campaign at a time, so the connector makes more requests than for [Ad Analytics](./ad-analytics.md). LinkedIn drops a location with fewer than 3 events, so totals can be lower than in Ad Analytics, and location data arrives 12 to 24 hours late. LinkedIn does not break down reach, audience penetration, conversion value or the carousel card metrics by location, so this table has none of those columns.
tags: ["owox", "connector", "linkedin-ads"]
type: "OWOX Data Mart"
sources:
  - id: "owox-connector"
    resource: "https://github.com/OWOX/owox-data-marts/blob/6b104bfa5980750dc37865eae15712577f42f307/packages/connectors/src/Sources/LinkedInAds/LinkedInAdsAPIReference/LinkedInAdsFieldsSchema.js"
    title: "OWOX Data Marts LinkedIn Ads connector — Ad Analytics by County endpoint (`adAnalyticsByCounty`)"
  - id: "vendor-reference"
    resource: "https://learn.microsoft.com/en-us/linkedin/marketing/integrations/ads-reporting/ads-reporting"
    title: "LinkedIn Marketing API reference — ads-reporting"
---

## Connector endpoint

- **Endpoint:** LinkedIn Ads · `adAnalyticsByCounty`
- **Default destination table:** `linkedin_ads_ad_analytics_by_county`
- **Unique keys:** `dateRangeStart`, `dateRangeEnd`, `campaign`, `county`
- **Time series:** yes, loaded by date range

# Schema

| Column | Type | Alias | Description |
|--------|------|-------|-------------|
| `account` | STRING | Account | URN of the advertising account, for example urn:li:sponsoredAccount:{id}. |
| `campaign` | STRING | Campaign | PK. URN of the campaign, for example urn:li:sponsoredCampaign:{id}. Its {id} is the id in Campaigns. |
| `county` | STRING | County | PK. URN of the member county the metrics belong to, for example urn:li:geo:{id}. |
| `countyName` | STRING | County Name | Name of the member county in English, as LinkedIn spells it, for example "United States" or "California, United States". Empty when LinkedIn returns no name for the location. |
| `actionClicks` | FLOAT | Action Clicks | The count of clicks on the action button of the Sponsored Messaging ad. |
| `adUnitClicks` | FLOAT | Ad Unit Clicks | The count of clicks on the ad unit displayed alongside the Sponsored Messaging ad. |
| `averageDwellTime` | FLOAT | Average Dwell Time | Average user dwell time (in seconds). It measures the duration for which more than 50% of the ad's pixels remain visible in the viewport. |
| `clicks` | FLOAT | Clicks | The count of chargeable clicks. Despite not charging for clicks for CPM campaigns, this field still represents those clicks for which we would otherwise charge advertisers based on objective (for example, clicks to view the landing page or company page). |
| `commentLikes` | FLOAT | Comment Likes | The count of likes of a comment. Sponsored Content only. |
| `comments` | FLOAT | Comments | The count of comments. Sponsored Content only. |
| `companyPageClicks` | FLOAT | Company Page Clicks | The count of clicks to view the company page. |
| `costInLocalCurrency` | FLOAT | Cost In Local Currency | Cost in the account's local currency based on the pivot and timeGranularity. For example, this would be spend by member company size per month if the pivot is MEMBER_COMPANY_SIZE and timeGranularity is MONTHLY. Cost is not adjusted for over delivery when a member professional demographic pivot is specified in the request. |
| `costInUsd` | FLOAT | Cost In USD | Cost in USD based on the pivot and timeGranularity. For example, this would be spend by campaign on a given day if the pivot is CAMPAIGN and timeGranularity is DAILY. Cost is not adjusted for over delivery when a member professional demographic pivot is specified in the request. |
| `costPerQualifiedLead` | FLOAT | Cost Per Qualified Lead | How much money was spent per qualified lead. Ratio is costInLocalCurrency / qualifiedLeads. |
| `dateRangeStart` | DATE | Date Range Start | PK. Start date of the report data point. Date is specified in UTC format (YYYY-MM-DD). |
| `dateRangeEnd` | DATE | Date Range End | PK. End date of the report data point. Date is specified in UTC format (YYYY-MM-DD). |
| `documentCompletions` | FLOAT | Document Completions | The number of times users reached 100% of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `documentFirstQuartileCompletions` | FLOAT | Document First Quartile Completions | The number of times users reached the first quartile of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `documentMidpointCompletions` | FLOAT | Document Midpoint Completions | The number of times users reached the second quartile of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `documentThirdQuartileCompletions` | FLOAT | Document Third Quartile Completions | The number of times users reached the third quartile of the document's length, including those that skipped to this point. This metric is only available for document ads and not all dimensions. |
| `downloadClicks` | FLOAT | Download Clicks | The number of times users have indicated the intent to download the media in an ad by clicking the download icon. This may or may not result in an actual download (e.g. if the user rejects a browser download prompt). This metric is only available for ad formats supporting media downloads. |
| `externalWebsiteConversions` | FLOAT | External Website Conversions | Total number of times users took a desired action after clicking on or seeing your ad. When conversions cannot be attributed to individual users, group level attribution or estimation may be used. |
| `externalWebsitePostClickConversions` | FLOAT | External Website Post Click Conversions | Total number of times users took a desired action after clicking on your ad. When conversions cannot be attributed to individual users, group level attribution or estimation may be used. |
| `externalWebsitePostViewConversions` | FLOAT | External Website Post View Conversions | Total number of times users took a desired action after seeing your ad. When conversions cannot be attributed to individual users, group level attribution or estimation may be used. |
| `follows` | FLOAT | Follows | The count of follows. Sponsored Content and Follower ads only. |
| `fullScreenPlays` | FLOAT | Full Screen Plays | Number of times members click on the full screen button or on the video(mobile only) to go into full screen mode. |
| `headlineClicks` | FLOAT | Headline Clicks | The number of times members clicked on the headline of conversation ads. |
| `headlineImpressions` | FLOAT | Headline Impressions | The number of times members were shown the headline of conversation ads. |
| `impressions` | FLOAT | Impressions | This is the count of impressions for Sponsored Content and sends for Sponsored Messaging. |
| `jobApplications` | FLOAT | Job Applications | The number of times a member completed a job application after viewing or clicking on an ad. Currently, this metric is broken down into postViewJobApplications (if the member applied after viewing the ad) and postClickJobApplications (if the member applied after clicking the ad). |
| `jobApplyClicks` | FLOAT | Job Apply Clicks | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after viewing or clicking on an ad which has a LinkedIn job landing page. Currently, this metric is broken down into postViewJobApplyClicks (if the member performed the action after viewing the ad) and postClickJobApplyClicks (if the member performed the action after clicking the ad). |
| `landingPageClicks` | FLOAT | Landing Page Clicks | The count of clicks which take the user to the creative landing page. |
| `leadGenerationMailContactInfoShares` | FLOAT | Lead Generation Mail Contact Info Shares | The number of times users shared contact info through the One Click Lead Gen for Sponsored Messaging ads. |
| `leadGenerationMailInterestedClicks` | FLOAT | Lead Generation Mail Interested Clicks | The count of Sponsored Messaging ad recipients who clicked to demonstrate interest. |
| `likes` | FLOAT | Likes | The count of likes. Sponsored Content only. |
| `oneClickLeadFormOpens` | FLOAT | One Click Lead Form Opens | The count of times users opened the lead form for a One Click Lead Gen campaign. |
| `oneClickLeads` | FLOAT | One Click Leads | The count of leads generated through One Click Lead Gen. |
| `opens` | FLOAT | Opens | The count of opens of Sponsored Messaging ads. |
| `otherEngagements` | FLOAT | Other Engagements | The count of user interactions with the ad unit that do not fit into any other more specific category. |
| `postClickJobApplications` | FLOAT | Post Click Job Applications | The number of times a member completed a job application after clicking on an ad. See also jobApplications. |
| `postClickJobApplyClicks` | FLOAT | Post Click Job Apply Clicks | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after clicking on an ad which has a LinkedIn job landing page. See also jobApplyClicks. |
| `postClickRegistrations` | FLOAT | Post Click Registrations | The number of times a member has registered for an event or seminar after clicking on an ad which has a LinkedIn landing page. This includes gross registrations and does not account for a user unregistering. |
| `postViewJobApplications` | FLOAT | Post View Job Applications | The number of times a member completed a job application after viewing an ad. See also jobApplications. |
| `postViewJobApplyClicks` | FLOAT | Post View Job Apply Clicks | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after clicking an ad which has a LinkedIn job landing page. See also jobApplyClicks. |
| `postViewRegistrations` | FLOAT | Post View Registrations | The number of times a member has registered for an event or seminar after viewing an ad which has a LinkedIn event landing page. This includes gross registrations and does not account for a user unregistering. See also registrations. |
| `qualifiedLeads` | FLOAT | Qualified Leads | The count of qualified leads shared by the advertiser. Qualified lead is a lead that has been deemed more likely to become a customer compared to other leads, based on their engagement and fit. |
| `reactions` | FLOAT | Reactions | The count of positive reactions on Sponsored Content which can capture, like, interest, praise, and other responses. |
| `registrations` | FLOAT | Registrations | The number of times a member has registered for an event or seminar after viewing or clicking on an ad which has a LinkedIn event landing page. This includes gross registrations and does not account for a user unregistering. |
| `sends` | FLOAT | Sends | The count of sends of Sponsored Messaging ads. |
| `shares` | FLOAT | Shares | The count of shares. Sponsored Content only. |
| `subscriptionClicks` | FLOAT | Subscription Clicks | The count of clicks to subscribe to a series, such as a Newsletter. |
| `talentLeads` | FLOAT | Talent Leads | Number of leads captured through a talent media campaign. |
| `textUrlClicks` | FLOAT | Text URL Clicks | The count of clicks on any links (anchor tags) that were included in the body of the Sponsored Messaging ad. |
| `totalEngagements` | FLOAT | Total Engagements | The count of all user interactions with the ad unit. |
| `validWorkEmailLeads` | FLOAT | Valid Work Email Leads | The count of leads with a valid work email that does not use an established free or personal email domain. |
| `videoCompletions` | FLOAT | Video Completions | The count of video ads that played 97-100% of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoFirstQuartileCompletions` | FLOAT | Video First Quartile Completions | The count of video ads that played through the first quartile of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoMidpointCompletions` | FLOAT | Video Midpoint Completions | The count of video ads that played through the midpoint of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoStarts` | FLOAT | Video Starts | The count of video ads that were started by users. |
| `videoThirdQuartileCompletions` | FLOAT | Video Third Quartile Completions | The count of video ads that played through the third quartile of the video. This includes watches that skipped to this point if the serving location is ON_SITE. |
| `videoViews` | INTEGER | Video Views | A video ad playing for at least 2 continuous seconds 50% in-view, or a click on the CTA, whichever comes first. An interaction with the video (like going to fullscreen mode) does not count as a view. |
| `viralClicks` | FLOAT | Viral Clicks | The count of clicks on viral impressions. See viral impressions definition. Sponsored Content only. |
| `viralCommentLikes` | FLOAT | Viral Comment Likes | The count of likes on comments from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralComments` | FLOAT | Viral Comments | The count of comments from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralCompanyPageClicks` | FLOAT | Viral Company Page Clicks | The count of clicks to view the company page from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralDocumentCompletions` | FLOAT | Viral Document Completions | The number of times users reached 100% of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDocumentFirstQuartileCompletions` | FLOAT | Viral Document First Quartile Completions | The number of times users reached the first quartile of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDocumentMidpointCompletions` | FLOAT | Viral Document Midpoint Completions | The number of times users reached the second quartile of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDocumentThirdQuartileCompletions` | FLOAT | Viral Document Third Quartile Completions | The number of times users reached the third quartile of the document's length on a viral post, including those that skipped to this point. This metric is only available for document ads. |
| `viralDownloadClicks` | FLOAT | Viral Download Clicks | The number of times users have indicated the intent to download the media in a viral ad by clicking the download icon. This may or may not result in an actual download (e.g. if the user rejects a browser download prompt). Only available for ads supporting media downloads. |
| `viralExternalWebsiteConversions` | FLOAT | Viral External Website Conversions | The count of conversions that are attributed to your ads driven by a viral event. See viral impressions definition. |
| `viralExternalWebsitePostClickConversions` | FLOAT | Viral External Website Post Click Conversions | The count of post-click conversions that are attributed to your ads driven by a viral click. See viral impressions definition. |
| `viralExternalWebsitePostViewConversions` | FLOAT | Viral External Website Post View Conversions | The count of post-view conversions that are attributed to your ads driven by a viral impression. See viral impressions definition. |
| `viralFollows` | FLOAT | Viral Follows | The count of follows from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralFullScreenPlays` | FLOAT | Viral Full Screen Plays | Number of times members click on the full screen button or on the video(mobile only) to go into full screen mode. See viralImpressions definition. |
| `viralImpressions` | FLOAT | Viral Impressions | The count of viral impressions for this activity. Viral impressions are those resulting from users sharing sponsored content to their own network of connections. Viral impressions are not counted as regular impressions. Sponsored Content only. |
| `viralJobApplications` | FLOAT | Viral Job Applications | The number of times a member completed a job application after viewing or clicking on a viral ad. Currently, this metric is broken down into viralPostViewJobApplications (if the member performed the action after viewing the viral ad) and viralPostClickJobApplications (if the member performed the action after clicking the viral ad). |
| `viralJobApplyClicks` | FLOAT | Viral Job Apply Clicks | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after viewing or clicking on a viral ad which has a LinkedIn job landing page during the date range.Currently, this metric is broken down into viralPostViewJobApplyClicks (if the member performed the action after viewing the viral ad) and viralPostClickJobApplyClicks (if the member performed the action after clicking the viral ad). |
| `viralLandingPageClicks` | FLOAT | Viral Landing Page Clicks | The count of clicks on viral impressions to take the user to the creative landing page. See viral impressions definition. Sponsored Content only. |
| `viralLikes` | FLOAT | Viral Likes | The count of likes from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralOneClickLeadFormOpens` | FLOAT | Viral One Click Lead Form Opens | The count of times users opened the lead form for viral impressions from a Lead Gen campaign. See viral impressions definition. |
| `viralOneClickLeads` | FLOAT | Viral One Click Leads | The count of leads generated through One Click Lead Gen from viral impressions for this activity. See viral impressions definition. |
| `viralOtherEngagements` | FLOAT | Viral Other Engagements | The count of user interactions with viral impressions that do not fit into any other more specific category. See viral impressions definition. Sponsored Content only. |
| `viralPostClickJobApplications` | FLOAT | Viral Post Click Job Applications | The number of times a member completed a job application after clicking on a viral ad. |
| `viralPostClickJobApplyClicks` | FLOAT | Viral Post Click Job Apply Clicks | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after clicking on a viral ad which has a LinkedIn job landing page. |
| `viralPostClickRegistrations` | FLOAT | Viral Post Click Registrations | The number of times a member has registered for an event or seminar after clicking on a viral ad which has a LinkedIn landing page. See viralImpressions definition. This includes gross registrations and does not account for a user unregistering. |
| `viralPostViewJobApplications` | FLOAT | Viral Post View Job Applications | The number of times a member completed a job application after viewing a viral ad. |
| `viralPostViewJobApplyClicks` | FLOAT | Viral Post View Job Apply Clicks | The number of times a member clicked on the job's apply button on an LinkedIn jobs page after viewing a viral ad which has a LinkedIn job landing page. |
| `viralPostViewRegistrations` | FLOAT | Viral Post View Registrations | The number of times a member has registered for an event or seminar after viewing a viral ad which has a LinkedIn event landing page. See viralImpressions definition. This includes gross registrations and does not account for a user unregistering. |
| `viralReactions` | FLOAT | Viral Reactions | The count of positive reactions on viral Sponsored Content which can capture like, interest, praise, and other responses. See viral impressions definition for details on viral engagements. |
| `viralRegistrations` | FLOAT | Viral Registrations | The number of times a member has registered for an event or seminar after viewing or clicking on a viral ad which has a LinkedIn event landing page. See viralImpressions definition. This includes gross registrations and does not account for a user unregistering. |
| `viralShares` | FLOAT | Viral Shares | The count of shares from viral impressions for this activity. See viral impressions definition. Sponsored Content only. |
| `viralSubscriptionClicks` | FLOAT | Viral Subscription Clicks | The count of viral clicks to subscribe to a series, such as a Newsletter. |
| `viralTotalEngagements` | FLOAT | Viral Total Engagements | The count of all user interactions with a viral ad unit. See viral impressions definition. Sponsored Content only. |
| `viralVideoCompletions` | FLOAT | Viral Video Completions | The count of viral video ads that played 97-100% of the video. This includes watches that skipped to this point. See viralImpressions definition. |
| `viralVideoFirstQuartileCompletions` | FLOAT | Viral Video First Quartile Completions | The count of viral video ads that played through the first quartile of the video. This includes watches that skipped to this point. See viralImpressions definition. |
| `viralVideoMidpointCompletions` | FLOAT | Viral Video Midpoint Completions | The count of viral video ads that played through the midpoint of the video. This includes watches that skipped to this point. See viralImpressions definition. |
| `viralVideoStarts` | FLOAT | Viral Video Starts | The count of viral video ads that were started by users. See viralImpressions definition. Since viral videos are automatically played for ON_SITE, this will be the same as viralImpressions if the servingLocation is ON_SITE. |
| `viralVideoThirdQuartileCompletions` | FLOAT | Viral Video Third Quartile Completions | The count of viral video ads that played through the third quartile of the video. This includes watches that skipped to this point. See viralImpressions definition |
| `viralVideoViews` | FLOAT | Viral Video Views | A viral video ad playing for at least 2 continuous seconds 50% in-view, or a click on the CTA, whichever comes first. An interaction with the video (like going to full screen mode) does not count as a view. See viralImpressions definition. |

# Example Questions

- Which counties did a campaign spend the most in?
- How many landing page clicks did each county produce per day?
- Which counties show engagements but no external website conversions?

## Joins

- [Campaigns](./campaigns.md) — `campaign = id` [N:1] — The campaign the row reports on; the value is a URN such as urn:li:sponsoredCampaign:{id}. The column holds the URN, so it matches the id inside it, not the whole string.
  - Description: The campaign the row reports on; the value is a URN such as urn:li:sponsoredCampaign:{id}. The column holds the URN, so it matches the id inside it, not the whole string.
- [Ad Accounts](./ad-accounts.md) — `account = id` [N:1] — The ad account the row reports on; the value is a URN such as urn:li:sponsoredAccount:{id}. The column holds the URN, so it matches the id inside it, not the whole string.
  - Description: The ad account the row reports on; the value is a URN such as urn:li:sponsoredAccount:{id}. The column holds the URN, so it matches the id inside it, not the whole string.
