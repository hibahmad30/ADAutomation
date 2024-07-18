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
To begin, we will replicate an enterprise environment by creating a Google Sheet that has users and their data inputted. To generate the first and last name values, I used a random name generator to get 105 different random user values, and inputted them into the sheet. 
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
<p align="center">
Prior to running our Powershell script, we will navigate to our Windows Server 2019 instance in VirtualBox and create an Organizational Unit (OU) in Active Directory Users and Computers called 'Test'. This matches the defined value in our CSV file under the 'OU' column. 
<br/>
<br/>
 <img src="https://i.imgur.com/BqeTxwX.png" alt="Create Test OU"/>
  <br/>
<br/>
Next, we will run Windows PowerShell ISE as an administrator, and navigate to 'File > New' to begin writing our script.  
  <br/>
<br/>
<img src="https://i.imgur.com/3yhE4sy.png" alt="Start Powershell ISE"/>
 <br/>
 <br/>
  The script begins with the ‘Import-Module activedirectory’ line, which imports the Active Directory module. This module contains cmdlets for managing Active Directory (AD), and is necessary to run any AD-related commands.
 <br/>
 <br/>
Then, the variable ‘ADUsers’ is created, which stores the imported data from the CSV file located at ‘C:\Users\Administrator\Desktop\users.csv’ using ‘Import-Csv’. Each row in the CSV file is treated as a separate object with properties corresponding to the column headers.
 <br/>
 <br/>
The ‘foreach ($User in $ADUsers)’ line starts a ‘foreach’ loop, which iterates through each object (row) in the $ADUsers variable, which stores the imported CSV file data. 
 <br/>
 <br/>
The ‘foreach’ statement reads through the user data from each field (column) in the current row of the CSV file and assigns it to individual variables such as username, password, first name, last name, etc. For example, ‘$Username = $User.username’ assigns the ‘username’ field from the current row of the CSV to the ‘$Username’ variable. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/5L2MUKu.png" alt="Powershell Script"/>
 <br/>
 <br/>
 Now that the user data is imported and stored into values, the ‘if’ block will check to see if the user already exists in AD, and will issue a subsequent warning message. The ‘else’ block will create the user if they do not already exist in AD, using the variables that were defined earlier and the imported CSV data. 
 <br/>
 <br/>
The script is set up to not require the user to change their password at logon, however since the current password is very weak and all users share the same password, this is not a security best practice, and is only configured for this practice lab environment. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/gAIFg0z.png" alt="Powershell Script Continued"/>
 <br/>
 <br/>
Proceed by running the script and navigating to Active Directory Users and Computers. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/BlLYv4w.png" alt="Run Powershell Script"/>
 <br/>
 <br/>
After refreshing, the users that were defined in the original Google Sheet should now be present in the 'Test' OU. After clicking on 'Properties' of a sample user, the unique attributes should also be defined: 
 <br/>
 <br/>
 <img src="https://i.imgur.com/JWcy7CO.png" alt="Confirm Users"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/MKOUSd9.png" alt="Sample User Data"/>
<h2>Key takeaways:</h2>
In this project, I built a virtualized network using Oracle’s VirtualBox to simulate a realistic IT environment. The network consisted of three primary components: a Windows client host, an Active Directory (AD) server, and a Splunk server. The setup was designed to mirror a typical small to medium-sized enterprise environment. The Windows client received its IP address dynamically via DHCP, whereas the AD and Splunk servers were assigned static IPs to ensure consistent communication across the network. This architecture enabled the implementation of robust security monitoring and log management through the use of Sysmon (System Monitor) and the Splunk Universal Forwarder.
 <br/>
 <br/>
The goal of this project was to create a controlled environment where security events could be monitored and analyzed effectively. By simulating a NAT network with dedicated roles for each system, the project provided a practical setting for understanding how enterprise networks function and how security tools like Sysmon and Splunk can be used to enhance visibility into system activities. The AD server represented a central authority for user authentication and policy enforcement, which is crucial in managing and securing network resources. The Splunk server, serving as a centralized log repository and analysis platform, demonstrated how security events can be aggregated, analyzed, and acted upon in real-time.
 <br/>
 <br/>
To build this environment, I configured Oracle VM VirtualBox to create three virtual machines: one for the Windows client, one for the AD server, and one for the Splunk server. Static IP addresses were assigned to the AD and Splunk servers to maintain their connectivity within the network, while the Windows client received a dynamic IP address. Sysmon was installed on both the Windows client and the AD server to capture detailed system activity logs, such as process creations, network connections, and file modifications. These logs were then forwarded to the Splunk server using the Splunk Universal Forwarder, a lightweight agent designed for log collection and transmission. The Splunk server aggregated these logs and provided tools for real-time monitoring and analysis, allowing for the detection of potential security threats and anomalies within the network.
 <br/>
 <br/>
By installing and configuring this build, I gained practical experience in setting up and managing virtualized environments, configuring network settings, and deploying security monitoring tools. This project enhanced my understanding of how enterprise networks operate and how critical security tools can be integrated to improve network visibility and incident response capabilities. This project not only reinforced my technical skills but also provided insights into the operational and security challenges faced by modern IT infrastructures.
<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

