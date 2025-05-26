# Description  

This is a prototype of a solution composed of two artifacts: 
* A Logic App that can be executed as a playbook on a Microsoft Defender / Sentinel incident. 
* A custom KQL plugin - NOTE: the custom plugin Incident-Account-Investigation-Helper is configured to use Microsoft Sentinel in order to enable a wider time interval when querying the SecurityAlerts table. Modify it to use Microsoft Defender if needed.

In combination with the custom KQL plugin Incident-Account-Investigation-Helper, the Logic App retrieves information about the accounts involved in the incident of interest to help determine whether they are compromised. In its current version, the Logic App sends this information via email and/or adds it as a comment to the original Sentinel incident. If you prefer a different behavior, please modify the Logic App accordingly.

The prototype of the Logic App is optimized for minimal SCU consumption. 

Please refer to https://www.linkedin.com/pulse/boosting-your-soc-operations-optimized-automation-stefano-pescosolido-d4mwf/ for further details.

# Prerequisites

* Microsoft Sentinel or Microsoft Defender
* Capacity (SCUs) in Security Copilot 


# Deployment

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2FAccountCompromiseInvestigation%2Flogicapp_azuredeploy.json)

* To understand the meaning of each individual parameter, read the corresponding tooltip.
* For the "Prompts" parameter, copy and paste the contents of https://raw.githubusercontent.com/stefanpems/cfs/refs/heads/main/AccountCompromiseInvestigation/example%20of%20JSON%20of%20prompts.json, or provide your own JSON using the same schema as the shared example


# PostDeployment

In the Logic App:
* Set the parameters
* Set the credentials for the connections to Security Copilot and to Office 365
* Assign the permission to its Managed Identity to write back the comments or tags in Sentinel (if required)
