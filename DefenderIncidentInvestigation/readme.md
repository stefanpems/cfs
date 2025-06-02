# Description  

This is a Logic App that can be executed as a playbook in the Microsoft unfied SOC operations portal.

The Logic App reads the prompts to be executed from a dedicated JSON parameter. The part of the Logic App that read and executes the prompts applies the following logic: 
* For minimizing the cost of execution, whenever possible each prompt references directly a skill with the related parameters. 
* Each prompt can contain placeholder that are replaced by the actual values retrieved in the preceeding prompts. 
* Where necessary, the Logic App creates multiple instance of the same prompt to ensure that all the correct entity identifiers are used one by one to replace the placehoder. 
* The Logic App also allows to set placeholders for multiple comma-separated entity identifiers.

Please refer to ... for further details.

# Prerequisites

* Microsoft Sentinel integrated with Microsoft Defender into the unified SOC portal
* Capacity (SCUs) in Security Copilot 


# Deployment

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2FDefenderIncidentInvestigation%2Flogicapp-azuredeploy.json)

* To understand the meaning of each individual parameter, read the corresponding tooltip.
* For the "Prompts" parameter, copy and paste the contents of https://raw.githubusercontent.com/stefanpems/cfs/refs/heads/main/DefenderIncidentInvestigation/example%20of%20prompts%20JSON%20parameter.json, or provide your own JSON using the same schema as the shared example. Be aware of the logic and naming convention for positionating the placeholders.


# Post-Deployment

In the Logic App:
* Set the parameters, including the prompts JSON parameter
* Set the credentials for the connections to Security Copilot and to Office 365 (if required)
* Assign the permission to its Managed Identity to write back the comments or tags in Sentinel (if required)

In Microsoft Sentinel:
* Authorize Sentinel on the Resource Group of the Logic App
