This is the sample series of prompts for creating the promptbook dedicated to automating the Security Posture status analysis:

PROMPT 1
```
Which threats should I focus on based on their exposure scores? For each returned threat, include a brief summary (no more that <WORDS_NUMBER> words), the Exposure Score, the Last Updated date, the number of related Alerts, the number of Misconfigured Devices and the number of Vulnerable Devices.
```

PROMPT 2
```
/GetSentinelSOCOptimizationRecommendations Is my Sentinel environment configured appropriatelt with reference to threat coverage and data utilization? Summarize the results in 3 paragraphs: Threat Coverage Recommendations (for each threat, show a bulleted list of: Threat Name, Recommended Action, Number of Active Detections, Number of Recommended Detections), Data Utilization Recommendations (for each table, show a bulleted list of: Table Name, Recommendations, Number of Suggested Rules) and Detection Tuning Recommendations (for each detection, show a bulleted list of: Detection ID, Recommended Action, Recommended Entities). For Data Utilization, show only the recommendations with 1 or more Suggested Rules
```

PROMPT 3
```
/CisoRecommendationsBySeverity List the top recurring 10 Recommendations created in the last <NUMBER_OF_DAYS> days with severity <SEVERITIES>
```
