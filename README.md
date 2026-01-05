# Comp3010
---
# Introduction (10%)
_(Provide an overview of the SOC context, the BOTSv3 exercise, and the objectives of your
investigation. Clearly define scope and assumptions.)_

This report will cover the analysis of the dataset BOTSv3. Analysis was performed by the tool "Splunk" which allows filtering and formatting of data in large datasets, using this tool we can audit and analyse the operations performed captured in the dataset, who they were performed by, when they were performed and information about the technical specifics behind the users and tools used.

---
# SOC Roles & Incident Handling Reflection (10%) 
_(Reflect on how SOC tiers, responsibilities, and incident handling methodologies relate to
the BOTSv3 exercise. Discuss prevention, detection, response, and recovery phases.)_

There are three Tiers to SOC analysts, below I will highlight their roles and how they relate to the BOTSv3 exercise.

***Tier One*** <br>
- Largely responsible for vanuerbility scanning.
- Will determine the severity of threats and escalate threats that require further attention
- In charge of managing the security tools used, for example, splunk or wireshark








Source: https://www.connectwise.com/cybersecurity-center/glossary/tier-1-vs-tier-2-vs-tier-3-cybersecurity

---
# Installation & Data Preparation (15%)
_(Document Splunk installation, dataset ingestion, and validation steps. Provide supporting
evidence (screenshots/configs). Justify setup choices in the context of SOC infrastructure.)_
##### Installation of splunk was performed on a VMware virtual machine running Ubuntu. Splunk Enterprise was downloaded from the offical site "https://www.splunk.com/en_us/download.html".

Splunk Installation
Installlation Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/615202f18d654ddf5638b6f17b9aa27cb7d89d15/Walkthrough%20Screenshots/Splunk%20installation



### 1. Downloading the dataset from the Github repository 
<img width="2559" height="1439" alt="1  Downloading the dataset" src="https://github.com/user-attachments/assets/f7a53f38-8f90-496d-be1e-adc02c1791ce" />
The BOTSv3 dataset is avalible from a public github repository. Downloading it is as simple as downlaoding the archive to a linux machine of virtual machine.

### 2. Moving the dataset to the correct folder so splunk can access it
<img width="2559" height="1439" alt="2  Moving dataset into the correct folder" src="https://github.com/user-attachments/assets/3b8d0775-2f08-45de-8750-0c6d8f6caae7" />
Moving the dataset to the correct folder is important. We must ensure the data is stored in the same place as the splunk files for ease of indexing later on in the process.

### 3. Running splunk from the dataset directory
<img width="2559" height="1439" alt="3  Running splunk from the dataset directory" src="https://github.com/user-attachments/assets/880fe613-958d-4c07-bba7-c75229138e24" />
Once the dataset is in the correct place, we can start splunk with the "./Splunk start" command. It is important we use the superuser "Sudo" prefix in order to prevent any protected storage locations from interfearing with the software, sudo allows us to run it as a administrator.

### 4. Opening the dataset in Splunk and indexing it
<img width="2559" height="1439" alt="4  Opening the dataset in Splunk and indexing it" src="https://github.com/user-attachments/assets/5c460b53-8d07-4713-bd4d-166183c842c9" />




---
# Guided Questions (40%)
_(Choose and answer ONE SET of BOTSv3’s 200-level questions using Splunk queries and
analysis. Present answers clearly, with supporting evidence (screenshots, query outputs,
dashboards). Explain the SOC relevance of each answer.)_

### Question 1
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%201

Methodology:
- index="botsv3" sourcetype = "aws=cloudtrail"
- used the field select to filter for useridentity.userName
- used the filter to show all the values

Answer: splunk_access,web_admin,bstoll,btun

SOC Relevance:

Evidence:<img width="2878" height="1788" alt="Question 1 part 3" src="https://github.com/user-attachments/assets/d0267c5c-111d-4337-8e71-ed3f1727788a" />



### Question 2 
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%202

Methodology: Used the filter to to find all filters relating to MFA, this option shows if MFA authentication was true or false

Answer: userIdentity.sessionContext.attributes.mfaAuthenticated

SOC Relevance:

Evidence:<img width="2878" height="1799" alt="Question 2 part 2" src="https://github.com/user-attachments/assets/247938ff-ee53-4194-bb78-41097d126e44" />

### Question 3 
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%203

Methodology: Used the hardware search filter

Answer: Intel(R) Xeon(R) CPU E5-2676 @2.40Ghz

SOC Relevance:

Evidence:<img width="2878" height="1786" alt="Question 3 part 2" src="https://github.com/user-attachments/assets/a505a8b5-5981-4e32-8543-a0c21a7bb576" />

### Question 4 
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%204-6/4
MAKE SURE TO REDO SCREENSHOT TO SHOW THE CONTENTS OF THE REQUEST
Methodology: **Remember this one was timestamped as earlier so its this one when they made it public**
- Added the eventID filter
- Added the bucketname parameter to gather all bucket api calls
- Added the eventname parameter to search for the PutBucketAcl name as that is always the type of api call performed.

Answer: ab45689d-69cd-41e7-8705-5350402cf7ac

SOC Relevance:

Evidence:<img width="2878" height="1799" alt="Question 4 Part 4" src="https://github.com/user-attachments/assets/514b59d7-5808-4845-b3b2-fb1f5e94af1c" />

### Question 5 
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%204-6/5

Methodology: Using the requestParameters.AccessControlPolicy.Owner.DisplayName we can see the owner of the access control policy and their username which is

Answer: bstoll

SOC Relevance:

Evidence:<img width="2878" height="1795" alt="Question 5 part 1" src="https://github.com/user-attachments/assets/fc3c0d04-c31b-4f81-8ae7-00653f6df003" />

### Question 6 
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%204-6/6

Methodology: The bucketName search parameter proved useful for a different question. What it displays is not the username of the user but instead the name of the whole bucket.

Answer: frothlywebcode

SOC Relevance:

Evidence:<img width="2878" height="1799" alt="Question 6 part 1" src="https://github.com/user-attachments/assets/09bdc06f-13ac-42bf-8152-ee3f4e2610c4" />


### Question 7 
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%207

Methodology: Using the sourcetyoe "aws:s3:accesslogs" and the known hostname "splunk.froth.ly" I searched for anything using "Put" as that would be used in a PutObject operation. As we know the uploaded file is a text file, i searched for listings containing "docx" firstly but no results appeared, I then tried "txt" and got a match, to confirm, i made a note of the timestamp and compared it to the time we know the bucket was open for. The bucket was mad public at 2:01:46.000 PM and the file was uploaded at 2:02:44.000 PM meaning after the bucket was made public.

Answer: OPEN_BUCKET_PLEASE_FIX.txt

SOC Relevance:

Evidence:<img width="2878" height="1799" alt="Question 7 Part 3" src="https://github.com/user-attachments/assets/8aae9aa7-4b0e-4f35-af3a-fe289bd2cb2b" />

Additional timeline evidence: https://github.com/Harvey-Moth/Comp3010/blob/eb221c0a2b4c732cc0fc1ff56804d47a6f902d26/Walkthrough%20Screenshots/Questions/Question%207/Timeline%20evidence.png

### Question 8 
Walkthrough Screenshots: https://github.com/Harvey-Moth/Comp3010/tree/cb6aa285ecee3e2b082eff977680b7f7e895c7a1/Walkthrough%20Screenshots/Questions/Question%208

Methodology: Firstly, I searched driver providers to see which ones were Microsoft drivers but that did not give me much. After attempting that method, I found the "OperatingSystem" filter and when I applied it, i found there were 2 operating systems used by the hosts, Windows 10 Pro (Which all but one used) and windows 10 enterprice which was the odd one out of the list. From there i saw the hostname is BSTOLL-L but it did not give me the Fully Qualified Domain Name. After trying all the filters I could to attempt to find the domain name and looking at external sources like domain lookup tools with no luck, I found that if the host name is searched generally right after indexing, information about the host shows up including the computer name, which, in this case was the FQDN.

Answer: BSTOLL-L.froth.ly

SOC Relevance:

Evidence:<img width="2878" height="1799" alt="Question 8 Part 5" src="https://github.com/user-attachments/assets/f803480c-398c-4830-97fd-d73f0c3ee260" />

---
# Conclusion, References and Presentation (5%)
_(Summarise findings, key lessons, and SOC strategy implications. Highlight improvements
for detection and response.
• Professionally structured, correctly referenced (IEEE style), and clearly written. You may
use Zotero or EndNote to help you.)_




	
