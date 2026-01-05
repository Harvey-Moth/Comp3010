# Comp3010
---
# Introduction (10%) - 104 words
_(Provide an overview of the SOC context, the BOTSv3 exercise, and the objectives of your
investigation. Clearly define scope and assumptions.)_

This report will cover the analysis of the dataset BOTSv3. Analysis was performed by the tool "Splunk" which allows filtering and formatting of data in large datasets, using this tool we can audit and analyse the operations performed captured in the dataset, who they were performed by, when they were performed and information about the technical specifics behind the users and tools used.

---
# SOC Roles & Incident Handling Reflection


There are three Tiers to SOC analysts, below I will highlight their roles and how they relate to the BOTSv3 exercise.
## SOC Analyst Tiers Overview

| Tier | Level        | Responsibility |
|-----:|-------------|----------------|
| 1    | Junior Level | Largely responsible for vulnerability scanning. |
| 1    | Junior Level | Will determine the severity of threats and escalate threats that require further attention. |
| 1    | Junior Level | In charge of managing the security tools used, for example, Splunk or Wireshark. |
| 1    | Junior Level | Does not proactively search for threats, only determine the severity of threats caught IDS software. |
| 2    | Mid Level    | Handle the escalated more complex threats that have been escalated by Tier One |
| 2    | Mid Level    | Use further tools to build up a more comprehensive picture. Where tier one will find unusual activity, tier two will understand what happened and how can they prevent it. |
| 3    | Senior Level | Perform active analysis to spot threats that aren't picket up automatically. |
| 3    | Senior Level | Perform Penetration tests, which consists of testing the security of a system by acting as an attacker to point out any vulnerabilities |



The main areas of Security Operation Centres consist of: <br>


## SOC Incident Handling Phases

| Phase      | Description |
|------------|-------------|
| Prevention | Using Intrusion detection systems to detect unusual activity and applying security practices, like encryption.<br>The use of automated SIEM's that are leveraging AI to detect incoming threats in real time keeps the workload minimal so technical analysts can focus on more complex threats.<br>In the case of the BOTSv3 exercise, ensuring data is protected, like in the case of the buckets by using an access control list with correct privacy would help prevent more serious data leaks. |
| Detection  | Detection can be performed using tools like Wireshark to capture network traffic, and more relevant to this exercise, Splunk can be used to index and filter large amounts of data to detect vulnerabilities, misconfigurations and other security concerns.<br>In the case of BOTSv3, using search filters a data storage bucket was found to have been set to public which is a huge security concern. |
| Response   | The incident response process consists of creating and incident response plan which details the roles and processes that would be applied if a specific incident were to take place, in the context of this exercise, the response to an analyst finding the AWS buckets ACL public was to use that to upload the text file "OPEN_BUCKET_PLEASE_FIX.txt", however, in addition to this, a good response plan would have steps on who to report this to, and any steps that should be taken by the analyst that discovered the issue, if they are qualified to do so. The consequences for this error could have been far worse so it's important to have clear steps to ensure quick and comprehensive solutions.   |
| Recovery   |As part of an incident response plan, redundancy systems and other risk mitigation processes should be outlined and implemented. After an incident has been discovered, following the incident recovery plan to implement the solution will ensure a fix is correctly implemented and does not cause any unforeseen issues. In this case, the recovery process consisted of alerting the bucket owner that of the mistake using the text file.             |










Source: https://www.connectwise.com/cybersecurity-center/glossary/tier-1-vs-tier-2-vs-tier-3-cybersecurity

---
# Installation & Data Preparation
_(Document Splunk installation, dataset ingestion, and validation steps. Provide supporting
evidence (screenshots/configs). Justify setup choices in the context of SOC infrastructure.)_
##### Installation of Splunk was performed on a VMware virtual machine running Ubuntu. Splunk Enterprise was downloaded from the official site "https://www.splunk.com/en_us/download.html".

Splunk Installation
Installation Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/615202f18d654ddf5638b6f17b9aa27cb7d89d15/Walkthrough%20Screenshots/Splunk%20installation



### 1. Downloading the dataset from the Github repository 
<img width="2559" height="1439" alt="1  Downloading the dataset" src="https://github.com/user-attachments/assets/f7a53f38-8f90-496d-be1e-adc02c1791ce" />
The BOTSv3 dataset is available from a public Github repository. Downloading it is as simple as downloading the archive to a Linux machine of virtual machine.

### 2. Moving the dataset to the correct folder so Splunk can access it
<img width="2559" height="1439" alt="2  Moving dataset into the correct folder" src="https://github.com/user-attachments/assets/3b8d0775-2f08-45de-8750-0c6d8f6caae7" />
Moving the dataset to the correct folder is important. We must ensure the data is stored in the same place as the Splunk files for ease of indexing later on in the process.

### 3. Running Splunk from the dataset directory
<img width="2559" height="1439" alt="3  Running splunk from the dataset directory" src="https://github.com/user-attachments/assets/880fe613-958d-4c07-bba7-c75229138e24" />
Once the dataset is in the correct place, we can start Splunk with the "./Splunk start" command. It is important we use the superuser "Sudo" prefix in order to prevent any protected storage locations from interfering with the software, "Sudo" allows us to run it as a administrator.

### 4. Opening the dataset in Splunk and indexing it
<img width="2559" height="1439" alt="4  Opening the dataset in Splunk and indexing it" src="https://github.com/user-attachments/assets/5c460b53-8d07-4713-bd4d-166183c842c9" />
By using 'Index = "BOTSv3"' we can load the dataset into Splunk and begin applying filters to manage the data.





---
# Guided Questions
_(Choose and answer ONE SET of BOTSv3’s 200-level questions using Splunk queries and
analysis. Present answers clearly, with supporting evidence (screenshots, query outputs,
dashboards). Explain the SOC relevance of each answer.)_

### Question 1 - Identify all users that accessed the AWS service using Frothly's AWS Environment  
Walkthrough Screenshots: <br>https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%201

Methodology:
- index="botsv3" sourcetype = "aws=cloudtrail"
- used the field select to filter for useridentity.userName
- used the filter to show all the values.

Answer: splunk_access, web_admin, bstoll, btun

By finding out all the users that accessed the AWS environment, successfully or unsuccessfully, we can begin to piece together if this is a simple mistake made by a team member or something more malicious, caused by an attacker

Evidence:<img width="2878" height="1788" alt="Question 1 part 3" src="https://github.com/user-attachments/assets/d0267c5c-111d-4337-8e71-ed3f1727788a" />



### Question 2 - What field would you use to alert that AWS API activity has occurred without MFA (multi-factor authentication)? 
Walkthrough Screenshots:<br> https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%202

Methodology: Used the filter to to find all filters relating to MFA, this option shows if MFA authentication was true or false

Answer: userIdentity.sessionContext.attributes.mfaAuthenticated

This field is useful for analysis as it helps to further determine the credibility of a user and API activity. It is much more unlikely that API activity that was performed by a user without MFA authentication is from outside the company but it is not a sure tell. It is a useful fact to have to continue analysis and to know whether to prioritise and escalate to a more senior team member or not.

Evidence:<img width="2878" height="1799" alt="Question 2 part 2" src="https://github.com/user-attachments/assets/247938ff-ee53-4194-bb78-41097d126e44" />

### Question 3 - What is the processor number used on the web servers? 
Walkthrough Screenshots:<br> https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%203

Methodology: Used the hardware search filter

Answer: Intel(R) Xeon(R) CPU E5-2676 @2.40Ghz

Understanding details about the hardware for the server helps us determine if systems are running as usual, as slow performance can be a sign of an issue. Knowing the hardware being used by the server, especially one you do not have physical access too like a cloud server, helps stay on top of hardware vulnerabilities.

Evidence:<img width="2878" height="1786" alt="Question 3 part 2" src="https://github.com/user-attachments/assets/a505a8b5-5981-4e32-8543-a0c21a7bb576" />

### Question 4 - Bud accidentally makes an S3 bucket publicly accessible. What is the event ID of the API call that enabled public access? 
Walkthrough Screenshots:<br> https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%204-6/4
MAKE SURE TO REDO SCREENSHOT TO SHOW THE CONTENTS OF THE REQUEST
Methodology: **Remember this one was timestamped as earlier so its this one when they made it public**
- Added the eventID filter
- Added the bucketname parameter to gather all bucket api calls
- Added the eventname parameter to search for the PutBucketAcl name as that is always the type of api call performed.

Answer: ab45689d-69cd-41e7-8705-5350402cf7ac

Using the event ID, we can flag the specific API call that caused the issue, this specific event ID is the API call "PutBucketAcl" that made the bucket public and created the vulnerability.

Evidence:<img width="2878" height="1799" alt="Question 4 Part 4" src="https://github.com/user-attachments/assets/514b59d7-5808-4845-b3b2-fb1f5e94af1c" />

### Question 5 - What is Bud's username? 
Walkthrough Screenshots:<br> https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%204-6/5

Methodology: Using the requestParameters.AccessControlPolicy.Owner.DisplayName we can see the owner of the access control policy and their username.

Answer: bstoll

This helps us identify the user that made the bucket public, at this point we can determine whether it was a mistake caused by an expected user, an unknown user, or even an expected user that should not have permissions to do what they did.

Evidence:<img width="2878" height="1795" alt="Question 5 part 1" src="https://github.com/user-attachments/assets/fc3c0d04-c31b-4f81-8ae7-00653f6df003" />

### Question 6 - What is the name of the S3 bucket that was made publicly accessible?
Walkthrough Screenshots:<br> https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%204-6/6

Methodology: Use the filter requestParameters.bucketName on the event ID we found before.

Answer: frothlywebcode

Knowing the name of the bucket in question is essential to moving forward with a recovery plan. We can determine the extent of possible damage, how important the data inside the bucket is as if it's sensitive data it could cause GDPR breaches and timeframe for how long the vulnerability was unnoticed and unfixed. 



Evidence:<img width="2878" height="1799" alt="Question 6 part 1" src="https://github.com/user-attachments/assets/09bdc06f-13ac-42bf-8152-ee3f4e2610c4" />


### Question 7 - What is the name of the text file that was successfully uploaded into the S3 bucket while it was publicly accessible?
Walkthrough Screenshots:<br> https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%207

Methodology: Using the sourcetype "aws:s3:accesslogs" and the known hostname "splunk.froth.ly" I searched for anything using "Put" as that would be used in a PutObject operation. As we know the uploaded file is a text file, I searched for listings containing "docx" firstly but no results appeared, I then tried "txt" and got a match, to confirm, I made a note of the timestamp and compared it to the time we know the bucket was open for. The bucket was mad public at 2:01:46.000 PM and the file was uploaded at 2:02:44.000 PM meaning after the bucket was made public.

Answer: OPEN_BUCKET_PLEASE_FIX.txt

Being able to see the names of files within the bucket means we were able to see the API calls made to the open bucket, and even the contents of the "PUT" call which shows the lack of security for this specific bucket and on a more serious note, would allow us to see if anything malicious was uploaded during the vulnerability period.


Evidence:<img width="2878" height="1799" alt="Question 7 Part 3" src="https://github.com/user-attachments/assets/8aae9aa7-4b0e-4f35-af3a-fe289bd2cb2b" />

Additional timeline evidence:<br> https://github.com/Harvey-Moth/Comp3010/blob/eb221c0a2b4c732cc0fc1ff56804d47a6f902d26/Walkthrough%20Screenshots/Questions/Question%207/Timeline%20evidence.png

### Question 8 - What is the FQDN of the endpoint that is running a different Windows operating system edition than the others?
Walkthrough Screenshots:<br> https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%208

Methodology: Firstly, I searched driver providers to see which ones were Microsoft drivers but that did not give me much. After attempting that method, I found the "OperatingSystem" filter and when I applied it, I found there were 2 operating systems used by the hosts, Windows 10 Pro (Which all but one used) and windows 10 enterprise which was the odd one out of the list. From there I saw the hostname is BSTOLL-L but it did not give me the Fully Qualified Domain Name. After trying all the filters I could to attempt to find the domain name and looking at external sources like domain lookup tools with no luck, I found that if the host name is searched generally right after indexing, information about the host shows up including the computer name, which, in this case was the FQDN.

Answer: BSTOLL-L.froth.ly

Knowing the FQDN we can determine the source of the endpoint running a different operating system. If the server is uniform and only one operating system is the norm for this system, an endpoint on a different operating system and from an unknown domain could be very suspect.


Evidence:<img width="2878" height="1799" alt="Question 8 Part 5" src="https://github.com/user-attachments/assets/f803480c-398c-4830-97fd-d73f0c3ee260" />

---
# Conclusion, References and Presentation (5%) - 29 words
_(Summarise findings, key lessons, and SOC strategy implications. Highlight improvements
for detection and response.
• Professionally structured, correctly referenced (IEEE style), and clearly written. You may
use Zotero or EndNote to help you.)_


Bucket was opened, ACL was edited



	
