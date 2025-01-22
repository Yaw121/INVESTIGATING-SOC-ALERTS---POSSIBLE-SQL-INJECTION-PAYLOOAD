## INVESTIGATING SOC ALERTS POSSIBLE SQL INJECTION PAYLOAD ##


## Objective
This repository documents a project focused on investigating Security Operations Center (SOC) alerts related to possible SQL injection payloads. The goal is to analyze and identify malicious patterns, validate the presence of SQL injection attempts, assess potential impacts, and recommend mitigation strategies to enhance application and database security.


## Key Activities
1. **Alert Investigation**
   - Review and analyze SOC alerts to identify potential SQL injection patterns.
   - Cross-reference alerts with logs to verify suspicious activity.

2. **Log Analysis**
   - Extract and analyze HTTP requests and responses from application logs.
   - Identify SQL injection payloads and associated parameters.

3. **Impact Assessment**
   - Evaluate the potential impact of detected SQL injection attempts on the database and application.
   - Determine whether the payload could exploit vulnerabilities or compromise sensitive data.

4. **Mitigation and Recommendations**
   - Propose actionable recommendations to prevent SQL injection attacks.
   - Suggest updates to application security controls, such as input validation, parameterized queries, and web application firewalls (WAF).

5. **Reporting and Documentation**
   - Document findings, including examples of SQL injection payloads and their impact.
   - Generate a report summarizing investigation steps, results, and mitigation measures.



In our SIEM platform, we can see an aleret triggered for a Possible SQL Injection Payload. 
![image](https://github.com/user-attachments/assets/53de9975-1408-4713-81df-4fff9ce9a3ab)


First, when we have a look at the website URL, there is a sql payload " OR" which means there is a SQL injection attack so now, I have to take action on the alert "Ownership" and start analyzing further activities on the alert
![image](https://github.com/user-attachments/assets/fc43aa59-84e7-471b-853a-8669c823bb0e)

2. Now, I have created a case and so that I can be able to take action on the case.
   ![image](https://github.com/user-attachments/assets/12f4c337-0b60-4e0f-9f54-be16a83b308c)

3. I used the Log management to analyze the activity of the the source IP address which is <b>167.99.169.17</b>
   ![image](https://github.com/user-attachments/assets/840374a3-6697-495a-bc9e-561d400a5d98)
   
. There are several SQL injection attack attempted including;
- Request URL: https://172.16.17.18/search/?q=%22%20OR%201%20%3D%201%20--%20-
- Request URL: https://172.16.17.18/search/?q=1%27%20ORDER%20BY%203--%2B
- Request URL: https://172.16.17.18/search/?q=%27%20OR%20%27x%27%3D%27x
- Request URL: https://172.16.17.18/search/?q=%27%20OR%20%271
- Request URL: https://172.16.17.18/search/?q=%27

Even though the attacker attempted several times, the HTTP response size and status were the same each time, which means the attacks were unsuccessful.
![image](https://github.com/user-attachments/assets/a9d2cd23-a971-4f33-bbe9-85573fd923a0)

4. After looking for activities on the source IP address, I looked for activities on the destination IP address <b>172.16.17.18</b>. 
![image](https://github.com/user-attachments/assets/4933ca8b-621f-47a7-beee-76e67023ab28)

Even though there are other IP addresses none of them were attempted except for the malicious Source IP address  <b>167.99.169.17</b>

5. I then moved to Endpoint Security and analyzed the Browser History, Terminal History, Network Connections and Processes however there was nothing unsual
![image](https://github.com/user-attachments/assets/cee729ec-5ca8-4902-b6b9-c80ef4699a63)


6. Then I searched the Source IP in VirusTotal and the results were not positive for the Source IP. About 5/94 Security vendors flagged this IP address as malicious
 ![image](https://github.com/user-attachments/assets/76f45703-510d-4cb6-b699-8d62aa95c0af)

7. I moved to Case management and started the playbook. A playbook is a documented or automated set of procedures and workflows designed to help security teams detect, investigate, and respond to security incidents systematically and efficiently.
   ![image](https://github.com/user-attachments/assets/5dac4aaf-f3f6-43ef-a19e-d9dbd1e7d7fe)

8. The Traffic looks malicious
   ![image](https://github.com/user-attachments/assets/5a5901d3-63af-4a86-bb91-7170e50ad2f8)


9. The attack type was SQL injection
![image](https://github.com/user-attachments/assets/31219212-4b8e-475b-948d-68e2515a52fe)


10. Also it’s not planned attack (part of the pentest process) because there was no pre-inform mail about it. So our IOC in this case was attacker’s IP address. Additionally, it’s true positive because there was unplanned attack attempt.
![image](https://github.com/user-attachments/assets/a6965de8-693a-47a3-a04b-17f3e2732604)


11. Finally, Destination IP is Private IP of Class B as it starts with (172.16.x.x) but Source IP is not, so it’s from internet to company network.
    ![image](https://github.com/user-attachments/assets/474ce27c-99e8-4644-a8eb-08f9c232c5cf)


12. And as attack wasn’t successful, we don’t need Tier 2 Escalation either.
   ![image](https://github.com/user-attachments/assets/c81637bc-a55b-43ec-bda2-48b6df63a435)






  

