# Marketing Campaign Analytics Workflow

This n8n workflow automates the weekly collection, processing, and reporting of marketing campaign data from various platforms, including Google Analytics, Facebook Ads, Google Ads, and Twitter. The generated report is then saved to Google Drive, emailed to the marketing team, and a summary notification is sent to Slack.

## Workflow Overview

The workflow is triggered every Monday at 9 AM and performs the following steps:

1.  **Weekly Report Trigger**: Initiates the workflow weekly.
2.  **Set Date Range**: Defines the start and end dates for the data collection, typically covering the past week.
3.  **Get Google Analytics Data**: Fetches campaign data such as sessions, users, pageviews, bounce rate, and average session duration from Google Analytics.
4.  **Get Facebook Ads Data**: Retrieves performance metrics like impressions, clicks, CTR, CPM, CPP, spend, and conversions from Facebook Ads.
5.  **Get Google Ads Data**: Collects campaign performance data including impressions, clicks, CTR, cost, and conversions from Google Ads.
6.  **Get Twitter Analytics**: Gathers engagement metrics (likes, retweets, replies, quotes) for tweets from a specified Twitter handle.
7.  **Process GA Data**: Parses and structures the raw Google Analytics data into a standardized format.
8.  **Process FB Data**: Parses and structures the raw Facebook Ads data, extracting relevant metrics and calculating conversions.
9.  **Process Google Ads Data**: Parses and structures the raw Google Ads data, converting cost from micros to actual currency.
10. **Process Twitter Data**: Aggregates Twitter engagement metrics and calculates overall engagement rates.
11. **Merge All Data**: Combines the processed data from all platforms into a single dataset.
12. **Generate Marketing Report**: Creates a comprehensive marketing report, including a summary of key metrics (total ad spend, clicks, impressions, conversions, sessions, overall CTR, cost per conversion, conversion rate), a breakdown by platform, and top-performing campaigns by conversions and ROAS.
13. **Save Report to Google Drive**: Uploads the generated report to Google Drive.
14. **Email Marketing Report**: Sends the full marketing report via email to the marketing team and management.
15. **Slack Notification**: Posts a brief summary of the report to a designated Slack channel.

## Nodes

Here's a detailed breakdown of each node in the workflow:

### 1. Weekly Report Trigger
* **Type**: `n8n-nodes-base.cron`
* **Notes**: Triggers every Monday at 9 AM.

### 2. Set Date Range
* **Type**: `n8n-nodes-base.set`
* **Description**: Sets the `weekStart` and `weekEnd` variables for the reporting period. These variables are used in subsequent HTTP requests.

### 3. Get Google Analytics Data
* **Type**: `n8n-nodes-base.httpRequest`
* **URL**: `https://www.googleapis.com/analytics/v3/data/ga?ids=ga:YOUR_VIEW_ID&start-date={{ $('Set Date Range').item.json.weekStart }}&end-date={{ $('Set Date Range').item.json.weekEnd }}&metrics=ga:sessions,ga:users,ga:pageviews,ga:bounceRate,ga:avgSessionDuration&dimensions=ga:campaign,ga:source,ga:medium&filters=ga:campaign!=(not set)`
* **Authentication**: `googleAnalyticsOAuth2`
* **Notes**: Fetches campaign data from Google Analytics, filtering out campaigns with `(not set)` names.

### 4. Get Facebook Ads Data
* **Type**: `n8n-nodes-base.httpRequest`
* **URL**: `https://graph.facebook.com/v18.0/act_YOUR_AD_ACCOUNT_ID/insights?fields=campaign_name,impressions,clicks,ctr,cpm,cpp,spend,actions&time_range={'since':'{{ $('Set Date Range').item.json.weekStart }}','until':'{{ $('Set Date Range').item.json.weekEnd }}'}`
* **Authentication**: `facebookGraphApi`
* **Notes**: Fetches Facebook Ads campaign performance, including conversions from 'purchase' actions.

### 5. Get Google Ads Data
* **Type**: `n8n-nodes-base.httpRequest`
* **URL**: `https://googleads.googleapis.com/v14/customers/YOUR_CUSTOMER_ID/googleAds:searchStream`
* **Authentication**: `googleAds`
* **Body**: Google Ads Query Language (GAQL) query to retrieve campaign name, impressions, clicks, CTR, cost, and conversions for the specified date range.
* **Notes**: Fetches Google Ads campaign performance.

### 6. Get Twitter Analytics
* **Type**: `n8n-nodes-base.httpRequest`
* **URL**: `https://api.twitter.com/2/tweets/search/recent?query=from:YOUR_TWITTER_HANDLE&start_time={{ $('Set Date Range').item.json.weekStart }}T00:00:00Z&end_time={{ $('Set Date Range').item.json.weekEnd }}T23:59:59Z&tweet.fields=public_metrics,created_at`
* **Authentication**: `twitterOAuth2`
* **Notes**: Fetches Twitter engagement metrics (likes, retweets, replies, quotes) for a specific Twitter handle within the date range.

### 7. Process GA Data
* **Type**: `n8n-nodes-base.code`
* **Description**: Transforms the raw Google Analytics data into a structured JSON format, extracting relevant metrics and parsing them into appropriate data types (integers, floats).

### 8. Process FB Data
* **Type**: `n8n-nodes-base.code`
* **Description**: Processes the raw Facebook Ads data, extracting campaign details and performance metrics. It specifically looks for 'purchase' actions to record conversions.

### 9. Process Google Ads Data
* **Type**: `n8n-nodes-base.code`
* **Description**: Transforms the raw Google Ads data, parsing metrics and converting `cost_micros` to a standard cost format.

### 10. Process Twitter Data
* **Type**: `n8n-nodes-base.code`
* **Description**: Aggregates engagement metrics (likes, retweets, replies, quotes) from individual tweets and calculates an overall engagement rate for the Twitter handle.

### 11. Merge All Data
* **Type**: `n8n-nodes-base.merge`
* **Mode**: `combine`
* **Combination Mode**: `mergeByIndex`
* **Description**: Combines the processed data from Google Analytics, Facebook Ads, Google Ads, and Twitter into a single collection, allowing for unified reporting.

### 12. Generate Marketing Report
* **Type**: `n8n-nodes-base.code`
* **Description**: This crucial node takes all the merged data and generates a comprehensive marketing report. It calculates:
    * **Summary Metrics**: Total ad spend, total clicks, total impressions, total conversions, total sessions, overall CTR, cost per conversion, and conversion rate.
    * **Platform Breakdown**: Detailed metrics for Google Analytics, Facebook Ads, Google Ads, and Twitter.
    * **Top Performing Campaigns**: Identifies the top 5 campaigns by conversions and by Return on Ad Spend (ROAS), assuming a $50 average order value for ROAS calculation.

### 13. Save Report to Google Drive
* **Type**: `n8n-nodes-base.googleDrive`
* **Operation**: `create`
* **Description**: Saves the generated marketing report as a file in Google Drive.

### 14. Email Marketing Report
* **Type**: `n8n-nodes-base.emailSend`
* **From Email**: `reports@yourcompany.com`
* **To Email**: `marketing-team@yourcompany.com,management@yourcompany.com`
* **Subject**: `Weekly Marketing Performance Report - {{ $('Set Date Range').item.json.weekStart }} to {{ $('Set Date Range').item.json.weekEnd }}`
* **Description**: Sends the full marketing report via email to the specified recipients.

### 15. Slack Notification
* **Type**: `n8n-nodes-base.slack`
* **Text**: A formatted Slack message containing key summary statistics from the generated report.
* **Description**: Sends a quick summary of the weekly marketing report to a Slack channel for immediate awareness.

## Setup and Configuration

Before running this workflow, you will need to:

1.  **Replace Placeholders**: Update `YOUR_VIEW_ID`, `YOUR_AD_ACCOUNT_ID`, `YOUR_CUSTOMER_ID`, `YOUR_TWITTER_HANDLE`, `YOUR_GA_ACCESS_TOKEN`, `YOUR_FB_ACCESS_TOKEN`, `YOUR_GOOGLE_ADS_TOKEN`, and `YOUR_TWITTER_BEARER_TOKEN` with your actual credentials and IDs.
2.  **Configure Credentials**: Ensure you have configured the following credentials in your n8n instance:
    * Google Analytics OAuth2
    * Facebook Graph API
    * Google Ads
    * Twitter OAuth2
    * Google Drive
    * Email (SMTP or other configured email service)
    * Slack Webhook
3.  **Adjust Date Range (Optional)**: The "Set Date Range" node can be modified if you need a different reporting period (e.g., monthly, daily).
4.  **ROAS Calculation (Generate Marketing Report Node)**: The `roas` calculation in the "Generate Marketing Report" node assumes a `$50 average order value`. Adjust this value (`item.conversions * 50`) to match your business's average order value for accurate ROAS reporting.
5.  **Email Recipients and Slack Channel**: Update the "To Email" addresses in the "Email Marketing Report" node and ensure the Slack webhook in the "Slack Notification" node points to your desired channel.

## Usage

Once configured, activate the workflow in n8n. It will automatically run every Monday at 9 AM, providing timely marketing performance reports. You can also manually trigger the workflow for ad-hoc reports.
