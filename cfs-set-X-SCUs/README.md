# Azure Logic App ARM Template for scheduling Copilot for Security SCU provisioning. 

The file template.json is an ARM template for creating an Azure Logic App that is capable of automatically changing the capacity (SCUs) of Copilot for Security based on the time of the day and wheater the day is a workday, weekend of national holiday.

### Prerequisites:
* Credit in Azure for consuming SCUs

### Description 
This Logic App sets the specified number of Security Compute Units (SCUs) for Security Copilot, but only if the current day is a working day. The number of desired SCUs is specified in the Logic App parameters. If the capacity already exists, the Logic App modifies its capacity - again, only during working days - according to the number of SCUs specified as a parameter. If the specified number of SCUs is zero, the Logic App deletes the capacity regardless of the day. Working days are defined by excluding non-working weekdays and listing national and local holidays.

### Instructions: 
1. Deploy the template with the deployment button below
1. Assign to the Managed Identity of the Logic App the Contributor role on the Resource Group where the Capacity has to be created or modified or deleted.
2. et the desired time for the execution recurrence in the Logic App trigger (tipically every day at specified hours)
3. Set the Logic App paramters (SCUs number, capacity name, etc...)
4. Enable the Logic App

## Deployment button
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2Fcfs-set-X-SCUs%2Ftemplate.json)

### Additional Instructions: 
Once correctly configured, clone the Logic App (use the 'Clone' button on the Overview page of the Logic App) to create additional instances, scheduled at different times of the day, that set a different number of SCUs. 
For example, you can have:
* One instance at 8 am that set 6 SCUs
* One instance at 2 pm that set 3 SCUs
* One instance at 7 pm that set 1 SCU
* One instance at 1 am that set 0 SCUs

NOTE: you can create additional instances of this Logic App by redeploying the template published in this page. However, we recommend "cloning" already deployed Logic Apps to minimize the setup steps. The cloned Logic App already has most of the parameters correctly set; you only need to authorize its Managed Identity on the Resource Group where the capacity has to be created / modified / deleted, set the recurrence, and specify the desired number of SCUs.
When cloning a Logic App, it is always advisable to set it initially to disabled.

## Additional resources
The following article contains a detailed description of how the Logic App works. It also contains setup instructions that are no longer accurate because most of the actions described are now automated during the template deployment:
[Scheduling the Provisioning of Capacity (SCUs) for Copilot for Security - Logic App Template](https://www.linkedin.com/pulse/scheduling-provisioning-capacity-scus-copilot-logic-pescosolido-ku8ef/?trackingId=vhZAZBr9Snqoj%2FkSaFdqJQ%3D%3D)
