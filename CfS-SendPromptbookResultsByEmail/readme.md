# Send the results of Security Copilot's Promptbook by email

This Logic App executes the specified Promptbook in Security Copilot (Copilot for Security), then it converts into HTML each prompt's response written in the simplified markdown language of Security Copilot. With those HTML excerpts or plain text responses, it constructs the HTML body of an email and finally sends that email to the specified recipients.
It is possible to exclude - by position index (1, 2, 3, etc...) - one or more responses from the email sent to the recipients.

### Prerequisites:
* This Logic App is useful when you have active Security Compute Units (SCUs) for Security Copilot


### Post deployment actions:
1. Set the credentials for the OAuth API connection to Security Copilot and Office 365 (the connection to O365 is used to send the email).
2. Edit the workflow and set the promptbook name and related parameters in the run-prompt action (after the deployment, they are empty).
3. In the workflow editor, if needed, modify the trigger type and/or parameters (e.g., change the recurrence date and time or replace the trigger with the Microsoft Sentinel Incident trigger).
4. In the workflow editor, open the parameters and set the desired email title, subject, and recipients.
5. In the workflow editor, in the parameters list, if needed, specify the indexes of the prompts that should not be included in the email (use the SkipPromptsInOutput parameter).
6. In the workflow editor, if needed, further customize the email subject (e.g., if you have changed the trigger to MS Sentinel Incident, you may want to include the incident ID at the end of the email subject).

## Deployment button
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fstefanpems%2Fcfs%2Frefs%2Fheads%2Fmain%2FCfS-SendPromptbookResultsByEmail%2FCfS-SendPromptbookResultsByEmail.json)
