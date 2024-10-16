# Azure Logic App (ARM Template) for creating Capacity on demand for a short period of time, for testing / lab purposes. 

This Logic App creates X Security Compute Units (SCUs) of capacity for Security Copilot and deletes them after Y minutes. The values of X and Y can be specified as parameters. The Logic App can be run manually or scheduled periodically.
If you want to see how to deploy and use this logic app, watch this short video: https://youtu.be/iwwQMiXbW9Q

Please consider that:
* This Logic App is intended to help those who need to deploy capacity in lab environments for minimal testing purposes. It is not meant to be used in a production environment, where the capacity for Security Copilot should be kept active.
* This Logic App remains running during the delay of Y minutes. If you need to deploy or remove X SCUs periodically, I recommend using my other Logic App [cfs-set-X-SCUs](https://github.com/stefanpems/cfs/blob/main/cfs-set-X-SCUs/README.md). 
* SCUs are billed on an hourly basis. Keeping them active for 1 or 59 minutes does not change the cost.

### Prerequisites:
* Credit in Azure for consuming SCUs

### Post deployment: 
* Authorize the System Assigned Managed Identity of the Logic App with the Contributor role on the Resource Group where the capacity will be created.

## Deployment button
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2Fcfs-set-X-SCUs-for-Y-minutes%2Ftemplate.json)
