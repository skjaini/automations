Workflow Overview
1. Webhook Trigger

Receives customer.created events from Stripe
Extracts customer data, subscription info, and plan details

2. Multi-System Account Creation

HubSpot CRM: Creates contact with customer details, lifecycle stage, and lead source
Zendesk Support: Creates end-user account with custom fields
Mixpanel Analytics: Creates user profile for product analytics
Amplitude Analytics: Sets up user tracking with properties

3. Intelligent Welcome Email Sequence

Plan-based branching: Different emails for Premium vs Standard customers
Premium customers: Get enhanced welcome with premium benefits, account manager info
Standard customers: Get standard welcome with getting started resources

4. Service Provisioning

Creates user account in your main application
Feature-based access: Premium customers get advanced features automatically
Sets appropriate user roles and permissions

5. Marketing Segmentation

Mailchimp: Adds to mailing list with appropriate tags (premium/standard, new customer)
HubSpot Workflows: Enrolls in automated marketing sequences
Smart tagging: Based on plan type for targeted campaigns

6. Team Notifications & Tasks

Slack notification: Alerts customer success team with full customer details
Premium task creation: Creates high-priority welcome call task for premium customers
Status tracking: Shows completion of all onboarding steps

🎯 Key Features
Conditional Logic

Different treatment for Premium vs Standard customers
Plan-based feature provisioning
Tiered support and communication

Error Handling

Parallel processing of account creation
Merge results before proceeding
Fallback values for missing data

Comprehensive Integration

CRM (HubSpot)
Support (Zendesk)
Analytics (Mixpanel, Amplitude)
Email Marketing (Mailchimp)
Team Communication (Slack)
Task Management (Todoist)

⚙️ Setup Instructions

Configure Stripe Webhook:
Endpoint URL: https://your-n8n-instance.com/webhook/stripe-customer-created
Events: customer.created

Replace API Credentials:

YOUR_HUBSPOT_ACCESS_TOKEN
YOUR_ZENDESK_TOKEN
YOUR_MIXPANEL_TOKEN
YOUR_AMPLITUDE_API_KEY
YOUR_MAILCHIMP_API_KEY
YOUR_APP_API_TOKEN


Update System-Specific IDs:

YOUR_LIST_ID (Mailchimp)
YOUR_FLOW_ID (HubSpot)
YOUR_PREMIUM_ACCOUNT_MANAGER_ID (Todoist)


Customize Content:

Replace [Company Name] in emails
Update app URLs and support contacts
Modify feature lists based on your plans



💡 Benefits

Instant onboarding: New customers are set up across all systems immediately
Personalized experience: Different treatment based on subscription tier
