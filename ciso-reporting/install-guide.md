

# Prerequisites

1. The approach and logic used in this solution can be applied with Security Copilot connected to any source of data but the sample custom plugins and promptbooks provided
   are designed to work in an environment where the security is managed in the Unified SOC portal (Defender + Sentinel).

2. This procedure assumes that you have a certain usage of Security Copilot and, so, an existing baseline compute capacity already deployed.
   If this is not the case, create a minimum capacity of at least 1 SCU: it will be needed to access and configure Security Copilot.   

3. The custom Logic Apps that invoke Security Copilot have access to Security Copilot through an OAuth connection authenticated with user credentials.
   Security Copilot acceesses the target source systems through an impersonated access model; the credentials used in the OAuth connectuon from the Logic App, then, must have the
   right to read the security data in the target source systems (in the promptbooks provided in this solution, the sources are Defender, Entra and Sentinel).
   In my knowledge, at the time of this writing, for this connection from the Logic Apps to Security Copilot it is not possible to specify credentials of type "Managed Identities":
   it is required to have "user credentials", despite the fact that these credentials will (generally) not be used by humans but they will be used only by this Logic App. 
   As a prerequisite for proceeding in this installation, it is then necessary to create user credentials with the following minimum "active" permissions ("eligible" permissions in PIM
   are not adequate to be used from an automation):
   - Security Reader at tenant level
   - Microsoft Sentinel Reader at the level of the Sentinel's resource group
   - Contributor on Microsoft Security Copilot
   These credentials require also to have assigned an Exchange Online license because they will be used to send the report email. 
   It is recommended to use a dedicated user, with a clear name (like "**Security Copilot Automation Account**") and to monitor its usage to ensure that any anomalous logon made with
   these credentials generate an alert.
   Before starting this setup procedure, it is highly recommended to prepare a browser session authenticated with the credentials of this new Security Copilot Automation
   Account, where you have already successully accessed:
   - The user's mailbox with Outlook web access
   - The Defender Portal
   - The Sentinel blade in Azure
   - The Security Copilot portal (at the very first access, it requires you to acknowledge some introductory topics)

4. Before starting this setup procedure, be sure to have these info and files available:
   - Credentials for the "Security Copilot Automation Account"
   - Identification parameters for Sentinel (Subscroption ID, Resource Group Name, Sentinel Workspace Name)
   - File [ciso-incidents-summary-man.yaml](https://github.com/stefanpems/cfs/blob/main/ciso-reporting/ciso-incidents-summary-man.yaml)
   - File [ciso-incidents-summary-api-spec.yaml](https://github.com/stefanpems/cfs/blob/main/ciso-reporting/ciso-incidents-summary-api-spec.yaml)


# Setup Procedure

## On the Security Copilot portal, accessed with credentials with the Owner role:

Create the promptbooks:
- [promptbook-incident-analysis](https://github.com/stefanpems/cfs/blob/main/ciso-reporting/promptbook-incident-analysis.md)
- [promptbook-posture-analysis](https://github.com/stefanpems/cfs/blob/main/ciso-reporting/promptbook-posture-analysis.md)

## On the Azure portal, accessed with credentials with an administrative role:

- Deploy the Logic App [ciso-get-users-status.json](https://github.com/stefanpems/cfs/blob/main/ciso-reporting/ciso-get-users-status.json)
  - NOTE: It has no "Deploy in Azure" button; it is necessary to perform a manual deployment.
  - Edit its connection to Security Copilot and authenticate with the credentials of the Security Copilot Automation Account

- Deploy the Logic App [CfS-SendPromptbookResultsByEmail](https://github.com/stefanpems/cfs/tree/main/CfS-SendPromptbookResultsByEmail) to create the
  "**CISO Incident Analysis**" automation 
  - Use the "Deploy in Azure" button
  - Edit its connection to Security Copilot and authenticate with the credentials of the Security Copilot Automation Account
  - Edit its connection to Office 365 and authenticate with the credentials of the Security Copilot Automation Account
  - Chose the promptbook "promptbook-incident-analysis" and specify its parameters. Typically you may want to specify:
    - TOP_INCIDENTS_NUMBER = 10
    - SEVERITIES = High and Medium
    - TIME_FRAME = last week (NOTE: if you change this value, you have to modify the content of the ReplacePromptsInOutput workflow paramter accordingly)
    - RESPONSE_LANGUAGE = set a language supported by Security Copilot
    - RESPONSE_LANGUAGE = 10
    - TOP_IMPACTED_USERS_NUMBER = 5 (the value of this number has an important impact on the consumption - and, then, costs - of the solution.)
  - Set all the Logic App parameters. Specifically, ensure that you have set:
    - The Email Title and Email Subject
    - The EmailRecipients
    - The SkipPromptsInOutput and ReplacePromptsInOutput as specified at the end of the file
      [promptbook-incident-analysis](https://github.com/stefanpems/cfs/blob/main/ciso-reporting/promptbook-incident-analysis.md)
  - Set the recurrence. Typically it has to be set to be run once per week, like on Monday morning at 6 am.
  - For the time being, keep the Logic App disabled (we will enable it when all the required configurations are in place) 
 

- [OPTIONAL] Deploy the Logic App [CfS-SendPromptbookResultsByEmail](https://github.com/stefanpems/cfs/tree/main/CfS-SendPromptbookResultsByEmail)
  to create the "**CISO Posture Analysis**" automation. 
  NOTE: this automation and related promptbook are in a very preliminary draft state.
  Not only are these prompts not optimized in terms of compute capacity consumption, but the overall promptbook is also very incomplete.
  Please refer to [this article](https://www.linkedin.com/pulse/periodic-reporting-security-managers-cisos-using-stefano-pescosolido-fm80f/)
  for further details on how this promptbook should be improved.
  - Use the "Deploy in Azure" button
  - Edit its connection to Security Copilot and authenticate with the credentials of the Security Copilot Automation Account
  - Edit its connection to Office 365 and authenticate with the credentials of the Security Copilot Automation Account
  - Chose the promptbook "promptbook-posture-analysis" and specify its parameters. Typically you may want to specify:
    - WORDS_NUMBER = 50
    - NUMBER_OF_DAYS = 30
    - SEVERITIES = High and Medium 
  - Set all the Logic App parameters. Specifically, ensure that you have set:
    - The HtmlBodyHeader to change the color shade of the email
      (see [promptbook-posture-analysis](https://github.com/stefanpems/cfs/blob/main/ciso-reporting/promptbook-posture-analysis.md))
    - The Email Title and Email Subject
    - The EmailRecipients
  - Set the recurrence. Typically it has to be set to be run once per week, like on Monday morning at 6 am.
  - For the time being, keep the Logic App disabled (we will enable it when all the required configurations are in place)

- Prepare and deploy the ciso-incidents-summary-man custom plugin (used by the "**CISO Incident Analysis**" automation)

- [OPTIONAL] Prepare and deploy the ciso-posture-summary-man custom plugin (used by the "**CISO Posture Analysis**" automation)
  
- [OPTIONAL] Prepare and deploy the Sentinel-SOC-Optimization-CustomPlugin custom plugin (used by the "**CISO Posture Analysis**" automation)


## On the Security Copilot portal, accessed with credentials with the Security Copilot Automation Account:

- Ensure that all the custom plugins just installed are enabled


## On the Security Copilot portal, accessed with credentials with the Owner role:

- Increment the number of SCUs

  
## On the Azure portal, accessed with credentials with an administrative role:

- Enable the "**CISO Incident Analysis**" automation nd wait its first automatic execution
  
- Enable the "**CISO Posture Analysis**" automation nd wait its first automatic execution

- For the "**CISO Incident Analysis**" automation, set the recurrence

- For the "**CISO Posture Analysis**" automation, set the recurrence

- Deploy N instances of the [cfs-set-X-SCUs](https://github.com/stefanpems/cfs/tree/main/cfs-set-X-SCUs) to ensure that there is enough
  capacity everytime that the CISO Incident Analysis and CISO Posture Analysis are started



--- 
WORK IN PROGRESS... TO BE CONTINUED...
