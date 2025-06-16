# 🚀 Automated Customer Onboarding Workflow

## 🧭 Workflow Overview

### 1. Webhook Trigger
- **Source:** `customer.created` events from **Stripe**
- **Action:** Extracts customer data, subscription info, and plan details

### 2. Multi-System Account Creation
- **HubSpot CRM:** Creates contact with customer details, lifecycle stage, and lead source  
- **Zendesk Support:** Creates end-user account with custom fields  
- **Mixpanel Analytics:** Creates user profile for product analytics  
- **Amplitude Analytics:** Sets up user tracking with properties  

### 3. Intelligent Welcome Email Sequence
- **Plan-based branching:** Different emails for Premium vs Standard customers  
- **Premium customers:** Enhanced welcome with premium benefits & account manager info  
- **Standard customers:** Standard welcome with getting started resources  

### 4. Service Provisioning
- Creates user account in your main application  
- **Feature-based access:** Premium customers get advanced features automatically  
- Sets appropriate user roles and permissions  

### 5. Marketing Segmentation
- **Mailchimp:** Adds to mailing list with appropriate tags (premium/standard, new customer)  
- **HubSpot Workflows:** Enrolls in automated marketing sequences  
- **Smart tagging:** Based on plan type for targeted campaigns  

### 6. Team Notifications & Tasks
- **Slack Notification:** Alerts customer success team with full customer details  
- **Premium Task Creation:** High-priority welcome call task for premium customers  
- **Status Tracking:** Shows completion of all onboarding steps  

---

## 🎯 Key Features

### ✅ Conditional Logic
- Different treatment for Premium vs Standard customers  
- Plan-based feature provisioning  
- Tiered support and communication  

### 🚨 Error Handling
- Parallel processing of account creation  
- Merge results before proceeding  
- Fallback values for missing data  

### 🔌 Comprehensive Integration
- **CRM:** HubSpot  
- **Support:** Zendesk  
- **Analytics:** Mixpanel, Amplitude  
- **Email Marketing:** Mailchimp  
- **Team Communication:** Slack  
- **Task Management:** Todoist  

---

## ⚙️ Setup Instructions

### 1. Configure Stripe Webhook
- **Endpoint URL:**  
  `https://your-n8n-instance.com/webhook/stripe-customer-created`
- **Events to Listen:**  
  `customer.created`

### 2. Replace API Credentials
```env
YOUR_HUBSPOT_ACCESS_TOKEN
YOUR_ZENDESK_TOKEN
YOUR_MIXPANEL_TOKEN
YOUR_AMPLITUDE_API_KEY
YOUR_MAILCHIMP_API_KEY
YOUR_APP_API_TOKEN
