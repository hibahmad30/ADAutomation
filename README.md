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
To begin, we will replicate an enterprise environment by creating a Google Sheet with user data. I used a random name generator to create 105 unique names, which I then inputted into the sheet.
<br/>
<br/>  
For the 'Display Name' column, I used the Excel formula =A2&" "&C2 to concatenate the first and last names. For the 'Email' and 'UPN' columns, I used the formula =LOWER(A2&"."&C2&"@mydomain.local") to generate the email addresses by combining the lowercase first and last names followed by '@domain.local'.
   <br/>
 <br/>  
<img src="https://i.imgur.com/ZJDh0m6.png" alt="User Attributes" height=85% width=85%/>
 <br/>
 <br/>  
I assigned each user a default password and specified the 'OU' and 'DC' values for the PowerShell script to read. Additionally, more specific user information such as address, telephone number, and job title can be added for automated user provisioning.
<br/>
 <br/> 
<img src="https://i.imgur.com/0Eb9u6n.png" alt="More User Attributes"/>
<br/>
 <br/> 
Once the desired values are entered, navigate to 'File > Download > Comma Separated Values (.csv)' to download the sheet as a CSV file.
 <br/>
 <br/>
<img src="https://i.imgur.com/PEX0fcI.png" alt="Download CSV File" height=85% width=85%/> 
<br/>
<br/>
CSV files are plaintext files that store data in a table format, with each line representing a row and each value separated by a comma. Here is what the downloaded CSV file should look like:
<br/> 
<br/>
<img src="https://i.imgur.com/xbLvncU.png" alt="CSV File"/>

<h2>Parse the CSV File with Powershell Script</h2> 
<p align="center">
Before running our PowerShell script, we'll access our Windows Server 2019 instance in VirtualBox and establish an Organizational Unit (OU) named 'Test' within Active Directory Users and Computers. This corresponds to the specified value in our CSV file under the 'OU' column.
<br/>
<br/>
 <img src="https://i.imgur.com/BqeTxwX.png" alt="Create Test OU"/>
  <br/>
<br/>
Next, we will launch Windows PowerShell ISE with administrator privileges and proceed to 'File > New' to begin writing the script.  
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
The script is configured not to prompt users to change their password at logon. However, it's important to note that using a weak password shared among all users is not a security best practice. This configuration is intended only for this practice lab environment.
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
After refreshing, the users defined in the original Google Sheet should now appear in the 'Test' OU. Clicking on the 'Properties' of a sample user will display their unique attributes as well.
 <br/>
 <br/>
 <img src="https://i.imgur.com/JWcy7CO.png" alt="Confirm Users"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/MKOUSd9.png" alt="Sample User Data"/>
<h2>Key takeaways:</h2>
In this project, I developed a PowerShell script to automate the bulk creation of user accounts in Active Directory (AD). By compiling user attributes and information for over 100 users into an Excel sheet, which was then saved as a CSV file, the script efficiently read and processed this data to create the accounts in AD. This solution not only saved time and reduced manual errors but also improved scalability for managing large volumes of user accounts in an enterprise environment.
 <br/>
 <br/>
The automation of repetitive tasks significantly reduced the manual effort required, particularly beneficial in large organizations where hundreds or thousands of accounts need to be managed. By automating the user creation process, the project saved considerable time compared to manually entering each user into AD, allowing IT administrators to focus on more strategic tasks. Manual data entry is prone to errors, leading to issues such as incorrect user information or duplicate accounts. Automating the process ensured consistency and accuracy, minimizing the likelihood of errors. The script was designed to handle large volumes of user data efficiently, a crucial feature for enterprises that regularly onboard large groups of employees or need to update user information in bulk.
 <br/>
 <br/>
By using a centralized Google Sheet for data input, it became easier to manage and update user information, which could then be easily exported as a CSV file for the script to process. The project demonstrated how additional user attributes, such as address, telephone number, and job title, could be included in the CSV file and automatically populated in AD, ensuring comprehensive user profiles. The users were added to an existing Active Directory server setup, which included a Windows 10 client system forwarding logs to a Splunk server. This integration highlights the script's compatibility with existing infrastructure, ensuring seamless operation within the current environment. The creation of a specific Organizational Unit (OU) in AD to store the new users illustrated how user accounts could be organized and managed effectively within AD.
 <br/>
 <br/>
Although the script was configured not to require users to change their password at logon for the lab environment, it highlighted the importance of implementing strong password policies in a production environment. This aspect underscores the need for balancing convenience with security best practices. Overall, this project showcased the powerful capabilities of PowerShell in managing Active Directory environments. It provided a practical solution for automating user account creation, thereby enhancing efficiency, accuracy, and scalability in enterprise user management.
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

