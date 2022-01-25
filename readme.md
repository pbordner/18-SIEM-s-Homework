## Unit 18 Homework: Lets go Splunking!

### Scenario

You have just been hired as an SOC Analyst by Vandalay Industries, an importing and exporting company.
 
- Vandalay Industries uses Splunk for their security monitoring and have been experiencing a variety of security issues against their online systems over the past few months. 
 
- You are tasked with developing searches, custom reports and alerts to monitor Vandalay's security environment in order to protect them from future attacks.


### System Requirements 

You will be using the Splunk app located in the Ubuntu VM.


### Your Objective 

Utilize your Splunk skills to design a powerful monitoring solution to protect Vandaly from security attacks.

After you complete the assignment you are asked to provide the following:

- Screen shots where indicated.
- Custom report results where indicated.

### Topics Covered in This Assignment

- Researching and adding new apps
- Installing new apps
- Uploading files
- Splunk searching
- Using fields
- Custom reports
- Custom alerts

Let's get started!

---

## Vandalay Industries Monitoring Activity Instructions


### Step 1: The Need for Speed 

**Background**: As the worldwide leader of importing and exporting, Vandalay Industries has been the target of many adversaries attempting to disrupt their online business. Recently, Vandaly has been experiencing DDOS attacks against their web servers.

Not only were web servers taken offline by a DDOS attack, but upload and download speed were also significantly impacted after the outage. Your networking team provided results of a network speed run around the time of the latest DDOS attack.

**Task:** Create a report to determine the impact that the DDOS attack had on download and upload speed. Additionally, create an additional field to calculate the ratio of the upload speed to the download speed.


1.  Upload the following file of the system speeds around the time of the attack.
    - [Speed Test File](resources/server_speedtest.csv)

2. Using the `eval` command, create a field called `ratio` that shows the ratio between the upload and download speeds.
   - Hint: The format for creating a ratio is: `| eval new_field_name = 'fieldA'  / 'fieldB'`
- source="server_speedtest.csv" host="server_speedtest" sourcetype="csv" | eval ratio='UPLOAD_MEGABITS' / 'DOWNLOAD_MEGABITS'
  
![Speed Test File Eval](https://user-images.githubusercontent.com/90003359/151040129-e48307dd-aa0c-4a4c-b4c0-b04db9b0d2db.png)

3. Create a report using the Splunk's `table` command to display the following fields in a statistics report:
    - `_time`
    - `IP_ADDRESS`
    - `DOWNLOAD_MEGABITS`
    - `UPLOAD_MEGABITS`
    - `ratio`
  
   Hint: Use the following format when for the `table` command: `| table fieldA  fieldB fieldC`
- source="server_speedtest.csv" host="server_speedtest" sourcetype="csv" | eval ratio='UPLOAD_MEGABITS' / 'DOWNLOAD_MEGABITS' | table _time IP_ADDRESS DOWNLOAD_MEGABITS UPLOAD_MEGABITS ratio

4. Answer the following questions:

    - Based on the report created, what is the approximate date and time of the attack? 
      A. 2/23/2020 11:30:00.000 PM Is the approximate date and time.
    - How long did it take your systems to recover?
      B. It took approximatley 9 hours for the system to recover.
      
![Speed Test File eval and tables](https://user-images.githubusercontent.com/90003359/151040190-2f5690a9-95f9-4fd9-b1c5-ed62371262a9.png)

Submit a screen shot of your report and the answer to the questions above.
 
### Step 2: Are We Vulnerable? 

**Background:**  Due to the frequency of attacks, your manager needs to be sure that sensitive customer data on their servers is not vulnerable. Since Vandalay uses Nessus vulnerability scanners, you have pulled the last 24 hours of scans to see if there are any critical vulnerabilities.

  - For more information on Nessus, read the following link: https://www.tenable.com/products/nessus

**Task:** Create a report determining how many critical vulnerabilities exist on the customer data server. Then, build an alert to notify your team if a critical vulnerability reappears on this server.

1. Upload the following file from the Nessus vulnerability scan.
   - [Nessus Scan Results](resources/nessus_logs.csv)

2. Create a report that shows the `count` of critical vulnerabilities from the customer database server.
   - The database server IP is `10.11.36.23`.
   - The field that identifies the level of vulnerabilities is `severity`.
- source="nessus_logs.csv" host="nessus_logs" sourcetype="csv" dest_ip="10.11.36.23" severity=critical | stats count by severity
      
3. Build an alert that monitors every day to see if this server has any critical vulnerabilities. If a vulnerability exists, have an alert emailed to `soc@vandalay.com`.

Submit a screenshot of your report and a screenshot of proof that the alert has been created.
![Nessus Scan Results Count Severity Report](https://user-images.githubusercontent.com/90003359/151046762-51dab79d-81f4-40b8-bb28-b64e09a0813a.png)
![Nessus Scan Result Alert](https://user-images.githubusercontent.com/90003359/151046796-3fc50f99-f1c7-4bbc-8353-0d10de0282e5.png)


### Step 3: Drawing the (base)line

**Background:**  A Vandaly server is also experiencing brute force attacks into their administrator account. Management would like you to set up monitoring to notify the SOC team if a brute force attack occurs again.


**Task:** Analyze administrator logs that document a brute force attack. Then, create a baseline of the ordinary amount of administrator bad logins and determine a threshold to indicate if a brute force attack is occurring.

1. Upload the administrator login logs.
   - [Admin Logins](resources/Administrator_logs.csv)

2. When did the brute force attack occur?
A. The attack started a 9:00 AM on Friday February 21st, 2020 and lasted for 5 hours.
   - Hints:
     - Look for the `name` field to find failed logins.
     - Note the attack lasted several hours.

      
3. Determine a baseline of normal activity and a threshold that would alert if a brute force attack is occurring.
A. The baseline of normal activity is around 20 and I set my threshold at 50

4. Design an alert to check the threshold every hour and email the SOC team at SOC@vandalay.com if triggered. 

Submit the answers to the questions about the brute force timing, baseline and threshold. Additionally, provide a screenshot as proof that the alert has been created.
 - source="Administrator_logs.csv" host="Admin Logins" sourcetype="csv" name="An account failed to log on"
 
 ![Administrators Failed Logins](https://user-images.githubusercontent.com/90003359/151052312-988f461f-1ded-40e2-9a7e-174810e2d564.png)
 ![Brute Force Attack Alert on Administrators](https://user-images.githubusercontent.com/90003359/151052341-29071121-d013-41d3-9fb6-729a9687d315.png)

### Your Submission
  
In a word document, provide the following:
  - Answers to all questions where indicated. 
  - Screenshots where indicated.

---

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.
