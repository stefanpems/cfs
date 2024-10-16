# Azure Logic App ARM Template for scheduling Copilot for Security SCU provisioning. 

The file template.json is an ARM template for creating an Azure Logic App that is capable of automatically changing the capacity (SCUs) of Copilot for Security based on the time of the day and wheater the day is a workday, weekend of national holiday.

### Prerequisites:
* Credit in Azure for consuming SCUs

### Description and Instructions: 
The following article contains a detailed description of how the Logic App works and how to set it up:
[Scheduling the Provisioning of Capacity (SCUs) for Copilot for Security - Logic App Template](https://www.linkedin.com/pulse/scheduling-provisioning-capacity-scus-copilot-logic-pescosolido-ku8ef/?trackingId=vhZAZBr9Snqoj%2FkSaFdqJQ%3D%3D)

## Deployment button
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2Fcfs-set-X-SCUs%2Ftemplate.json)
