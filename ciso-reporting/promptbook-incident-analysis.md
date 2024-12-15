This is the sample series of prompts for creating the promptbook dedicated to automating the Security Incident Management status analysis:

PROMPT 1
```
/CisoIncidentsStatusSummary Consider the top <TOP_INCIDENTS_NUMBER> most occurring incidents with severity <SEVERITIES> in the period of <TIME_FRAME>. 
Respond by applying ALL these 4 indications to this specific response only:
1. Start the response with the following first sentence: "This is the list of Incidents with severity <SEVERITIES> created <TIME_FRAME>". 
2. Do not return a table: return the results in paragraphs instead; highlight the incident title and write as bulleted list the numbers by status. Do not write classifications where the number is zero. Write in bold underlined the incident classified as true positive.
3. Format the ENTIRE response in HTML including the very first sentence; do not include the doctype tag and the header: write only the content of the HTML body, without the opening and closing "body" tag and DO NOT write any character before and after that HTML content. 
4. Translate this ENTIRE response in <RESPONSE_LANGUAGE> from its very first to its very last word, but the titles of the incidents.
```

PROMPT 2
```
/CisoTruePositiveIncidentsTTAnalysis Consider only the incidents occurred <TIME_FRAME> with severity <SEVERITIES>.
Respond by applying BOTH these 2 indications to this specific response only:
1. Start the response with the following first sentence: "These are the MITRE Tactics and Techniques for the Incidents closed <TIME_FRAME> as 'True Positives'". 
2. Do not return a table: return the results in paragraphs instead; highlight the incident title and write in a bullet the the number of occurrences, in a second bullet the list of tactics and in a third bullet the list of techniques.
```

PROMPT 3
```
/askGPT Respond by applying ALL these 3 indications to this specific response only: 
1. Create a copy of the list returned by the previous prompt and, in that copy, replace the value of each listed Tactic with its link to the MITRE Att&ck Framework web site describing that specific tactic. Also replace the value of each listed Techniques with its link to the MITRE Att&ck Framework web site describing that specific technique; concatenate the technique code with its title. Ensure that the links are correct. 
2. Format the ENTIRE response in HTML including the very first sentence; do not include the doctype tag and the header: write only the content of the HTML body, without the opening and closing "body" tag and DO NOT write any character before and after that HTML content. 
3. Translate this ENTIRE response in <RESPONSE_LANGUAGE> from its very first to its very last word. Keep in English only the titles of the incidents and the names of the Tactics and Techniques.
```

PROMPT 4
```
/CisoMTTR Set the start_date and end_date input parameters to the first and last day of <TIME_FRAME>. In the response, include all the content returned by the KQL call.
```

PROMPT 5
```
/askGPT Respond by applying ALL these 6 indications to this specific response only: 
1. Start the response with the following sentence: "This is the MTTR, Mean Time To Resolve, for the incidents closed <TIME_FRAME>.". 
2. After that sentence, write a table with two columns, Metric and Value, and three rows containing the first three numbers of the previous response. Write the first two numbers in minutes (with no decimal places) and in hours (in round brackets, with only 1 decimal place); write explicitly that these are minutes and hours. 
3. After that first table, write the sentence "The MTTR is defined as the average time between the creation and closure of incidents, for the incidents closed within the specified time window, as shown in the following table:". 
4. After that sentence, write explicitly, in a second table, what are the first and last days considered for both the two calculated MTTR values, as returned by the previous response. The second table should contain these three columns: Period, Start Date, End Date. 
5. Format the dates as weekday, day, month, year. Format this ENTIRE response in HTML, from the very first to the very last word; for any table, use a border 1px with color light gray, spacing 1px and width 100%; do not include the doctype tag and the header: write only the content of the HTML body, without the opening and closing "body" tag and DO NOT write any character before and after that HTML content. 
6. Translate this ENTIRE response in <RESPONSE_LANGUAGE> from its very first to its very last word. Keep in English only the acronym MTTR and the name "Mean Time To Resolve" but add its translation after that name in round brackets.
```

PROMPT 6
```
/CisoMTTA Set the start_date and end_date input parameters to the first and last day of <TIME_FRAME>. In the response, include all the content returned by the KQL call. 
```

PROMPT 7
```
/askGPT Respond by applying ALL these 6 indications to this specific response only: 
1. Start the response with the following sentence: "This is the MTTA, Mean Time To Acknowledge, for the incidents created <TIME_FRAME>.". 
2. After that sentence, write a table with two columns, Metric and Value, and three rows containing the first three numbers of the previous response. Write the first two numbers in minutes (with no decimal places) and in hours (in round brackets, with only 1 decimal place); write explicitly that these are minutes and hours. 
3. After that first table, write the sentence "The MTTA is defined as the average time between the creation and first modification of incidents, for the incidents created within the specified time window, as shown in the following table:". 
4. After that sentence, write explicitly, in a second table, what are the first and last days considered for both the two calculated MTTA values, as returned by the previous response. The second table should contain these three columns: Period, Start Date, End Date. 
5. Format the dates as weekday, day, month, year. Format this ENTIRE response in HTML, from the very first to the very last word; for any table, use a border 1px with color light gray, spacing 1px and width 100%; do not include the doctype tag and the header: write only the content of the HTML body, without the opening and closing "body" tag and DO NOT write any character before and after that HTML content. 
6. Translate this ENTIRE response in <RESPONSE_LANGUAGE> from its very first to its very last word. Keep in English only the acronym MTTA and the name "Mean Time To Acknowledge" but add its translation after that name in round brackets.
```

PROMPT 8
```
/CisoIncidentsOwners Set the start_date and end_date input parameters to the first and last day of <TIME_FRAME>. Set the top_owners_number input parameter to <TOP_OWNERS_NUMBER>. 
Respond by applying ALL these 4 indications to this specific response only: 
1. Start the response with the following sentence: "This is the list of owners of incidents with the biggest number of assigned incidents considering only the incidents created <TIME_FRAME>:". 
2. Return the list in bullets of "user principal names" with the number of assigned incidents after the name, in round brackets. 
3. Format the ENTIRE response in HTML including the very first sentence; do not include the doctype tag and the header: write only the content of the HTML body, without the opening and closing "body" tag and DO NOT write any character before and after that HTML content. 
4. Translate this ENTIRE response in <RESPONSE_LANGUAGE> from its very first to its very last word. 
```

PROMPT 9
```
/CisoMostImpactedUsers Set the start_date and end_date input parameters to the first and last day of <TIME_FRAME>. Set the top_impacted_users_number input parameter to <TOP_IMPACTED_USERS_NUMBER> impacted users. 
Respond by returning the list of "user principal names" of the identified impacted users, with the related number of associated alerts
```

PROMPT 10
```
/askGPT extract in a single string the comma separated list of User Principal Names included in the previous response. Replace "contoso.com" with "giustizia.it"
```

PROMPT 11
```
Call the CisoGetUsersStatus skill of the CisoIncidentsSummaryApiHelper plugin. Pass these values to the input parameters: api-version=2016-10-01 , sp=/triggers/AnalyzeUPNsCall/run , sv=1.0. Pass to the users_upn input parameter the comma separated list of UPNs that you returned in the previous response. Read in the body of the response of this call and return the values of EntraStatus and RiskStatus for these users.
```

PROMPT 12
```
/askGPT Consolidate in a single table the number of incidents, the EntraStatus and the RiskStatus for ALL the impacted users identified in the previous responses. Respond by applying ALL these 4 indications to this specific response only: 1. Start the response with the following sentence: "This is the list of the users most impacted by incidents created <TIME_FRAME> and not already closed as 'false positive'. The list includes the number of their incidents and their status and risk level in Entra.". 2. Return the list as table. 3. Format the ENTIRE response in HTML including the very first sentence; for the HTML table, use a border 1px with color light gray, spacing 1px and width 100%; do not include the doctype tag and the header: write only the content of the HTML body, without the opening and closing "body" tag and DO NOT write any character before and after that HTML content. 
4. Translate this ENTIRE response in <RESPONSE_LANGUAGE> from its very first to its very last word.
```
