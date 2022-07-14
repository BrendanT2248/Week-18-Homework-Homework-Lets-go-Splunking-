# Week-18-Homework-Lets-go-Splunking

## Vandalay Industries Monitoring Activity Instructions

## Step 1: The Need for Speed

Background: As the worldwide leader of importing and exporting, Vandalay Industries has been the target of many adversaries attempting to disrupt their online business. Recently, Vandaly has been experiencing DDOS attacks against their web servers.

Not only were web servers taken offline by a DDOS attack, but upload and download speed were also significantly impacted after the outage. Your networking team provided results of a network speed run around the time of the latest DDOS attack.

### **Task: Create a report to determine the impact that the DDOS attack had on download and upload speed. Additionally, create an additional field to calculate the ratio of the upload speed to the download speed.**

### **1. Upload the following file of the system speeds around the time of the attack.**

[Speed Test File](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Files/server_speedtest.csv)

Screenshot of uploaded 'server_speedtest.csv' file into Splunk:

![Speed Test File in Splunk](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Speed%20Test%20File.PNG)

### **2. Using the `eval` command, create a field called ratio that shows the ratio between the upload and download speeds.**

_Hint: The format for creating a ratio is: `| eval new_field_name = 'fieldA'  / 'fieldB'`_

Query used in Splunk to generate a ratio between upload and download speeds - `source="server_speedtest.csv" | eval ratio = DOWNLOAD_MEGABITS/UPLOAD_MEGABITS`

![Ratio Download Upload](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Ratio%20Download%20Upload.PNG)

We can see the ratio of downloads and uploads from this query. 

### **3. Create a report using the Splunk's table command to display the following fields in a statistics report:**

- `_time`
- `IP_ADDRESS`
- `DOWNLOAD_MEGABITS`
- `UPLOAD_MEGABITS`
- `ratio`

_Hint: Use the following format when for the table command: | table fieldA  fieldB fieldC_

Query used in Splunk to generate a ratio between upload and download speeds - `source="server_speedtest.csv" | eval ratio = DOWNLOAD_MEGABITS/UPLOAD_MEGABITS | table _time, IP_ADDRESS, DOWNLOAD_MEGABITS, UPLOAD_MEGABITS, ratio`

![Table 1 Download Upload](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Table%201%20Download%20Upload.PNG)

From the above screenshot, we can see the total event timeline of when downloads and uploads took place. 

![Table 2 Download Upload](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Table%202%20Download%20Upload.PNG)

From the above screenshot, we can see the time in which downloads and uploads took place as well as the source IP address that was taking this action. 

![Table 3 Download Upload](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Table%203%20Download%20Upload%20Column%20Graph.PNG)

From the above screenshot, we can see a visual representation of these download and upload events in a column graph format. The time is displayed on the bottom and this makes our data much more readable. 

### **Answer the following questions:**

- **Based on the report created, what is the approximate date and time of the attack?**

Based on our findings in the above queries, we can see download and upload speed was drastically dropped at approximately 2:30PM on the 23rd of February. We can see this in the below screenshot: 

![Downloads Uploads Dropped 1430](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Downloads%20Uploads%20Dropped%201430.PNG)

We can se the downloads dropped to 7.87 at 2:30PM. This is a drastic drop from 109.16 megabits that was the previous event. It's fair to assume then that this is around when the attack started.

- **When did the system begin to recover?**

![Downloads Uploads Recovering](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Downloads%20Uploads%20Recovering.PNG)

We can see from the above screenshot that downloaded megabits increased from 17.56 to 65.34, which is a drastic increase. Therefore, it's safe to assume this is around when the system started to recover and get back to normal network traffic flow. 

- **When did the network traffic flow appear to be normal?**

![Downloads Uploads Fully Recovered](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/Downloads%20Uploads%20Recovered%20Fully.PNG)

We can see from the above screenshot that downloaded megabits jumped to 123.91 from 78.34 at approximately 11:30PM on the 23rd of Feb. It is safe to assume that this is about the time where network traffic flow has stabilised and returned to normal. 

Submit a screen shot of your report and the answer to the questions above.

## Step 2: Are We Vulnerable?

Background:  Due to the frequency of attacks, your manager needs to be sure that sensitive customer data on their servers is not vulnerable. Since Vandalay uses Nessus vulnerability scanners, you have pulled the last 24 hours of scans to see if there are any critical vulnerabilities.

For more information on Nessus, read the following link: https://www.tenable.com/products/nessus

### **Task: Create a report determining how many critical vulnerabilities exist on the customer data server. Then, build an alert to notify your team if a critical vulnerability reappears on this server.**

### **1. Upload the following file from the Nessus vulnerability scan.**

[Nessus Scan Results](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Files/nessus_logs.csv)

Screenshot of nessus scan results uploaded into Splunk:

![Nessus Logs Uploaded](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20logs%20uploaded.PNG)

### **2. Create a report that shows the count of critical vulnerabilities from the customer database server.**

- **The database server IP is `10.11.36.23`.**

Use `dest_ip="10.11.36.23"` in the query

- **The field that identifies the level of vulnerabilities is severity.**

It's worth noting there are 5 types of severity levels these logs look for, as shown below:

![Severity Levels](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20logs%20severity%20levels.PNG)

We want to filter for any vulnerabilities whereby the severity level is a 'crtiical' value. We can use `severity="crtiical"` in our query.

The entire query is `source="nessus_logs.csv" dest_ip="10.11.36.23" severity="critical"`

See below screenshot for entire query that displays the count of critical vulnerabilities from the customer database server from this log file:

![Severity Level Critical Filtered](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20logs%20severity%20level%20filtered.PNG)

There were a total of 49 critical vulnerabilities found when the destination IP was 10.11.36.23 (which is the database server).

### **3. Build an alert that monitors every day to see if this server has any critical vulnerabilities. If a vulnerability exists, have an alert emailed to `soc@vandalay.com`.**

Create the alert by clicking 'save as' and then 'alert' after we've generated our results from the query above:

![Save as nessus alert](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20save%20alert.PNG)

Enter details to generate alerts whenever Nessus detects a vulnerability on the database server. Enter name, description and any other relative information:

![Nessus Alert 1](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20alert%201.PNG)

Enter email details to send the generated alert to - `soc@vandalay.com` and enter message details to be sent with the email:

![Nessus Alert 2](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20alert%2022.PNG)

Contiune entering details of alert and then click 'save':

![Nessus Alert 3](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20alert%203.PNG)

Once saved we should be able to see the alert and edit if we wish:

![Nessus Alert 4](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/nessus%20alert%204.PNG)

Submit a screenshot of your report and a screenshot of proof that the alert has been created.

## Step 3: Drawing the (base)line

Background:  A Vandalay server is also experiencing brute force attacks into their administrator account. Management would like you to set up monitoring to notify the SOC team if a brute force attack occurs again.

### **Task: Analyze administrator logs that document a brute force attack. Then, create a baseline of the ordinary amount of administrator bad logins and determine a threshold to indicate if a brute force attack is occurring.**


### **1. Upload the administrator login logs.**

[Admin Logins](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Files/Administrator_logs.csv)

Screenshot of uploaded 'Administrator_logs.csv' file into Splunk:

![Admin Logs Image](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/administrator_logs.PNG)

### **2. When did the brute force attack occur?**

_Hints:_

- _Look for the name field to find failed logins._
  
- _Note the attack lasted several hours._

Because we want to look for any indiactors of a brute force attack, we want to drill down the logs into whereby accounts failed to log on. If we navigate to the field 'name' we can see the values this holds in the logs. In this case, we can see a count of 1004 "An account failed to log on". This is a good indactor that a brute force attack has occured. See screenshot: 

![Name field account failed to log on](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/admin%20logs%20name%20field.PNG)

Now if we add this to our search we can see the total events of when this occurred. We can then see when the majority of these events took place, which is a good indicator as to when this occurred. See screenshot:

![Brute force attack strarting](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/admin%20logs%20brute%20force%20start%20time.PNG)

As we can see, there is quite a large spike in the events whereby "An account failed to log on" at around 9am on the 21st of Febraury. We can see of the total 1004 events that occurred, at 9am there was 124 of these events with similar numbers appearing in the hours after. Thereby, I believe the attack started around this time - 9:00AM on the 21st of Febraury 2020. 

### **3. Determine a baseline of normal activity and a threshold that would alert if a brute force attack is occurring.**

We can see from the timeline of these events that 124 is quite a spike. In the preceeding hours to this event, the most amount of bad logins was around 23. We can consider this normal behaviour. See screenshot: 

![Normal bad log ons](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/admin%20logs%20normal%20bad%20log%20ons.PNG)

With this in mind and taking into account there were around 124-135 log on attempts when the brute force attack occurred, I would consider a baseline of about 40 bad log ons per hour. I would have the ranges for bad log ons per hour as follows:

- 0-25 is normal behaviour
- 25-50 is worth investigating
- 50+ is considered crtiical and events should be investigated as soon as possible. 

### **4. Design an alert to check the threshold every hour and email the SOC team at `SOC@vandalay.com` if triggered.**

To create an alert for this type of event, I went through the following steps:

Click 'save as' and then 'alert':

![Save as alert](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/brute%20force%20attempt%20alert%201.PNG)

Fill in details of alert, including when to trigger it (anything above 25 attempts per hour):

![Alert details](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/brute%20force%20attempt%20alert%202.PNG)

Enter details of an action. In this case we will be sending an email to `SOC@vandalay.com`:

![Alert email setup](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/brute%20force%20attempt%20alert%203.PNG)

Save the alert and we can then view it:

![Alert saved](https://github.com/BrendanT2248/Week-18-Homework-Lets-go-Splunking/blob/main/Images/brute%20force%20attempt%20alert%204.PNG)

We have now successfully created an alert that will trigger when there are over 25 events of an account failing to log in.

Submit the answers to the questions about the brute force timing, baseline and threshold. Additionally, provide a screenshot as proof that the alert has been created.
