# Phishing Analysis with Security Copilot
**Author:** Craig Freyman - Version modified by Stefano Pescosolido (see final paragraph for details on the changes)

An automated solution for phishing email analysis using Azure Logic Apps, Function Apps, and Security Copilot. The system monitors a shared Office 365 mailbox, analyzes submitted emails for security threats, and generates detailed reports. Optional integration with Microsoft Sentinel for incident tracking and management.

## Prerequisites

1. **Azure Resources:**
   - Azure subscription
   - Contributor rights to the deployment target resource group
   - [Security Copilot Security Compute Units](https://learn.microsoft.com/en-us/copilot/security/get-started-security-copilot)
   - Microsoft Sentinel workspace (optional)

2. **Email Configuration:**
   - [Office 365 shared mailbox for monitoring](https://learn.microsoft.com/en-us/microsoft-365/admin/email/create-a-shared-mailbox?view=o365-worldwide)
   - Permissions to manage email configurations

## Deployment Steps

### 1. Deploy Function App

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FCopilot-For-Security%2Fmain%2FLogic%20Apps%2FSecCopilot-UserReportedPhishing-FuncApp_parsingV2%2Ffunctionapp_azuredeploy.json)
(Original version)

Required Parameters:
- FunctionAppName (name is prepended with "phish" and random characters are appended - Note: if changed, this custom part of the name must be very short)
- FunctionAppResourceGroup

Wait for the Function App to fully deploy before moving on to step 2.

### 2. Deploy Logic App

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2FSecCopilot-UserReportedPhishing-FuncApp_parsingV2%2Flogicapp_azuredeploy.json)
(Modified version)

Required Parameters:
- SubscriptionId
- LogicAppName
- LogAnalyticsWorkspaceName
- LogAnalyticsWorkspaceId
- LogAnalyticsResourceGroup
- FunctionAppName (step 1)
- FunctionAppResourceGroup (step 1)
- AnalyzeOnlyDefenderIncident (Set to true if you want this Logic App to analyze emails arriving at the shared SecOps mailbox only if there is a corresponding incident in Sentinel. Set to false if you want any email arriving at the shared SecOps mailbox to be analyzed.)
- GetUrlsReputationOnMDTI (Set to true if you want this Logic App to request MDTI for the reputation of the links listed in the body of the email.)
- WriteCommentInIncident (Set to true if you want this Logic App to write the result of the analysis as comment in the Sentinel incident.)
- SendReportByEmail (Set to true if you want this Logic App to send the result of this analysis to the email here specified in the ReportRecipientsEmailAddresses parameter.)
- SkipFirstDelay (Set to true if you want this Logic App to skip the first delay before querying Log Analytics to search for the alert related to the analyzed email. Skipping this initial delay is not recommended unless the logic app is used for testing on an email with an already existing alert and incident in Sentinel.)
- SecOpsMailboxEmailAddress (Enter the email address of the shared mailbox receiving the user reported phishing.)
- ReportRecipientsEmailAddresses (Enter the email address of the shared mailbox where this Logic App should send the report of the analysis. Leave an empty space if the SendReportByEmail is set to false.)
- SubscriptionId (Enter the ID of the Azure Subscription hosting the Log Analytics workspace for Sentinel.)
- SentinelRG (Enter the name of the Resource Group hosting the Log Analytics workspace for Sentinel.)
- SentinelWorkspaceName (Enter the name of the Log Analytics workspace for Sentinel.)
- SentinelWorkspaceId (Enter the ID of the Log Analytics workspace for Sentinel.) 
- TagIntent (Set the value for the tag that should be added to the incident with the result of the analysis. Use <INTENT> as a placeholder for the result. Leave blank or set a white space if you don't want' to have a tag added to the incident for this purpose.)
- TagMdtiResult (Set the value for the tag that should be added to the incident when a match is found in MDTI on any of the URLs included in the email. Leave blank or set a white space if you don't want' to have a tag added to the incident for this purpose.)


### 3. Configure API Connections

1. Open the Logic App in Azure Portal
2. Authorize these connections:
   - Office 365 (shared mailbox access)
   - Security Copilot
   - Azure Monitor Logs (if using Sentinel)
   - Sentinel (if using Sentinel)

### 4. Configure Logic App Permissions

If using Sentinel integration, assign these roles to the Logic App's managed identity:

1. "Log Analytics Reader" role (provides Microsoft.OperationalInsights/workspaces/read)
2. "Microsoft Sentinel Responder" role (provides Microsoft.SecurityInsights/incidents/comments/write)

Assign Permissions Step by Step

1. Open the Log Analytics workspace and go to "Access control (IAM)"

![alt text](image.png)

2. Select "Log Analytics Reader"

![alt text](image-1.png)

3. Select "Managed Identity" and select the name of your Logic App

![alt text](image-2.png)

4. Click next and assign permissions.
5. Repeat steps 1-4 for the "Microsoft Sentinel Responder" role.

### 5. Enable the Logic App

The Logic App deploys in a disabled state. Enable it in the Logic App Overview to begin operation.

## Features

- Automated user submitted phishing analysis
- Classification of emails (Phishing, Spam, Legitimate, Suspicious)
- Can process PDF & Excel file attachments
- HTML report generation
- Optional Sentinel incident integration

## Security Notes

- The solution uses system-managed identities for secure access
- Review and configure storage account security settings per your company policy
- Ensure minimum required permissions for all connections
- Regularly update Function App dependencies

## Changes made in this last version (by Stefano Pescosolido)
- If there the reported email has the title with the expected structure (Phishing:[MessageId]|[sender]|[subject] [datetime]) but no incidents are found in Defender, the original phishing email is interpreted as created by a MDO AST simulation campaign. In that case the execution is cancelled before calling Security Copilot
- It has been added the possibility to investigate the links found in the body of the in MDTI (currently, the investigation is not done on the links found in the attachments) 
- A few parameters have been added to easily modify the behavior of the app. For example, it is possible to decide if the app should analyze only incidents found in Defender, write in the incident comments, tag the incident with the result of the MDTI investigation and intent investigation, send the report by email. Please refer to the description of the parameters listed above. 
