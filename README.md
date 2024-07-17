<h1>Automating AD Bulk User Creation with Powershell</h1>
  
<h2>Description</h2>
For this project, I developed a solution to streamline user management in Active Directory by creating a PowerShell script that automates the bulk creation of user accounts. By compiling user attributes and information for over 100 users into an Excel sheet and saving it as a CSV file, the script reads and processes this data to efficiently create the accounts in Active Directory. This approach not only saves time and reduces manual errors but also enhances scalability for managing large volumes of user accounts in enterprise environments. 
<br /> 
<br />
The AD users for this project will be added to the existing Active Directory server that I previously set up, which includes a Windows 10 client system that also forwards logs to a Splunk server; for more details, refer to my <a href="https://github.com/hibahmad30/SplunkConfig">'Centralized Log Management Solution with Splunk Enterprise'</a> build. 

<h2>Environments Used </h2>

- <b>Oracle VM VirtualBox</b>
- <b>Windows Server 2019</b>
- <b>Active Directory</b>
- <b>Windows PowerShell ISE</b>
- <b>Google Sheets</b>

<h2>Generate CSV File with User Attributes</h2> 

<p align="center">
To begin, we will replicate an enterprise environment by creating a Google Sheet that has users and their data inputted. To generate the first name, middle initial, and last name values, I used a random name and letter generator to get 105 different random user values, and inputted them into the sheet. 
<br/>
<br/>  
For the ‘Display Name’ column, I used the following Excel formula to concatenate the first and last name together- ‘=A2&" "&C2’. For the 'Email' and 'UPN' columns, I used the following formula to take the lowercase first and last name values followed by '@domain.local' - ‘=LOWER(A2&"."&C2&"@mydomain.local")’.
<img src="https://i.imgur.com/ZJDh0m6.png" alt="User Attributes" height=85% width=85%/>
 <br/>
 <br/>  
I assigned each user a default password, and specifed the OU and DC values for the Powershell script to read. As depicted below, more specific user information such as address, telephone number, and job title can be added as well for automated user provisioning. 
<br/>
 <br/> 
<img src="https://i.imgur.com/0Eb9u6n.png" alt="More User Attributes"/>
<br/>
 <br/> 
Once the desired values are entered, naviagate to 'File > Download > Comma Separated Values (.csv)' to download the sheet as a CSV file. 
 <br/>
 <br/>
<img src="https://i.imgur.com/PEX0fcI.png" alt="Download CSV File" height=85% width=85%/> 
<br/>
<br/>
CSV files are plaintext files that are stored in a table format. Here is what the downloaded CSV file should look like: 
<br/> 
<br/>
<img src="https://i.imgur.com/xbLvncU.png" alt="CSV File"/>

<h2>Parse the CSV File with Powershell Script</h2> 
In Progress
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

