
# Azure Logic App ARM Template for Scheduling Security Copilot SCU Provisioning

The `template.json` file is an ARM template for deploying an Azure Logic App that automatically adjusts the Security Copilot capacity (SCUs) based on the time of day and whether the day is a workday, weekend, or national holiday.  
The Logic App also allows configuration of overage settings—**No Overage**, **Unlimited**, or **Limited Overage** (with a specified SCU limit).

---

### Prerequisites

- Azure credit for consuming SCUs  
- Credentials with permission to deploy this Logic App  
- Credentials that, at runtime, have permission to create or delete capacity resources in the specified Resource Group  

---

### Description

This Logic App sets the specified number of Security Compute Units (SCUs) for Security Copilot—but only on working days.  
The desired SCU count is defined in the Logic App parameters. If capacity already exists, the Logic App updates it (again, only on working days) to match the specified SCU count.  
If the SCU count is set to zero, the Logic App deletes the capacity regardless of the day.  
Working days are defined by excluding weekends and listing national and local holidays.

---

### Instructions

1. Deploy the template using the button below.  
2. Assign the **Contributor** role to the Logic App’s Managed Identity on the Resource Group where capacity will be created, modified, or deleted.  
3. Set the desired execution schedule in the Logic App trigger (typically once or more per day at specific times).  
4. Configure the Logic App parameters (e.g., list of holidays, SCU count, capacity name, resource group).  
5. In the action titled **"Initialize National Holidays"**, update the array with your country’s national and local holidays.  
6. Enable the Logic App.

---

### Deployment Button

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2Fcfs-set-X-SCUs%2Ftemplate.json)


### Additional Instructions

Once configured, you can clone the Logic App (via the **Clone** button on its Overview page) to create additional instances scheduled at different times, each setting a different SCU count.

For example, in a complex scenario, your organization might use:

* One instance, named "Baseline", firing at 7 am and 6 pm that set 2 SCUs
* A second instance (a clone of the baseline), named "Level 1", firing at 6 am and 12 pm, that set 4 SCUs
* A third instance (another clone of the baseline), named "Level 2", firing at 8 am, that set 5 SCUs
* A fourth instance (another clone of the baseline), named "Level 3", firing at 5 am and 5 pm, that set 6 SCUs

![Img1](./timing_sample.png)


> **Note:** You can also deploy additional instances by redeploying the template. However, cloning an existing Logic App is recommended to minimize setup steps and reduce errors (e.g., mismatched capacity names).  
> Cloned Logic Apps retain most parameter values; you only need to:
> - Grant the Managed Identity access to the target Resource Group  
> - Set the recurrence schedule  
> - Define the desired SCU count  
>  
> When cloning, it’s best to initially disable the Logic App to prevent it from running before permissions are assigned.

In smaller organizations with limited security staff and no 24/7 automation readiness, we’ve seen successful use of just two Logic App instances, as shown below:


![Img2](./timing_sample2.png)

### Additional resources
The following article contains a detailed description of how the Logic App works. It also contains setup instructions that are no longer accurate because most of the actions described are now automated during the template deployment:
[Scheduling the Provisioning of Capacity (SCUs) for Security Copilot - Logic App Template](https://www.linkedin.com/pulse/scheduling-provisioning-capacity-scus-copilot-logic-pescosolido-ku8ef/?trackingId=vhZAZBr9Snqoj%2FkSaFdqJQ%3D%3D)

**For lab environments where you simply need to provision a few SCUs for a limited number of minutes, I recommend using that further simplified Logic App: [cfs-set-X-SCUs-for-Y-minutes](https://github.com/stefanpems/cfs/tree/main/cfs-set-X-SCUs-for-Y-minutes)**
