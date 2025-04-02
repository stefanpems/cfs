# Phishing Analysis with Security Copilot

## Orginal version

Please refer to https://github.com/Azure/Security-Copilot/edit/main/Logic%20Apps/SecCopilot-UserReportedPhishing-FuncApp_parsingV2


## Changes in this version:
1. Instead of waiting for 15 minutes to find the alert ID in Log Analytics, the query is done every 2 minutes for max 8 times or 20 minutes
2. When the email has the subject with the structure created by the report button (e.g.:Phishing:<original-message-id>>|<original-sender-address>|(<orginal-subject>) <original-date-time>) and the incident is not found in Defender/Sentinel, the suspect email is considered to be created by MDO AST (Defender for Office 365 - Attack Simulation Training). In this case, Security Copilot is not invoked.


## Deployment Steps

### 1. Deploy Function App (from the original version)

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FCopilot-For-Security%2Fmain%2FLogic%20Apps%2FSecCopilot-UserReportedPhishing-FuncApp_parsingV2%2Ffunctionapp_azuredeploy.json)

Required Parameters:
- FunctionAppName (name is prepended with "phish" and random characters are appended)
- FunctionAppResourceGroup

Wait for the Function App to fully deploy before moving on to step 2.

### 2. Deploy Logic App (modified version)

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https%3A//github.com/stefanpems/cfs/raw/refs/heads/main/UserReportedPhishing-v3/azuredeploy.json)

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

Please refer to the readme of the original version for details

### 5. Enable the Logic App

The Logic App deploys in a disabled state. Enable it in the Logic App Overview to begin operation.

