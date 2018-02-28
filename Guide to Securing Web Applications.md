# Guide to Securing Web Applications 
## Injection Flaws
Untrusted data is sent to a server as part of a command or query, causing the system to perform in an unintended way (from the perspective of the developer) 

### Types of Injection Flaws

*Server Side JavaScript Injection* - an attacker sends malicious JavaScript code to be executed on the server, can be exploited when unvalidated user input is unwittingly executed by the following JavaScript code:
	* eval()
	* setTimeout()
	* setInterval()
	* Function()

*SQL Injection/Database Injection* - hacker injects code into a database query that will be executed on the database, causing unintended consequences such as sensitive data being returned to the hacker or data loss 

*NoSQL Injection* - hacker injects code into a database query that will be executed on the database, takes advantage of built in NoSQL commands 

### Preventing Injection Flaws

*Preventing Injection Flaws*
	* Always validate/sanitize user data before processing 
	* Don’t use forbidden JavaScript functions to process user data 
		* use JSON.parse or another alternative 
	* Include ‘user strict’ declaration at beginning of JavaScript file or function 

*Prevent SQL Injection/Database Injection*
	* Always validate/sanitize user data before processing 
	* Use prepared statements
		* Database query is precompiled, user data is fed into precompiled query and treated as pure data (not as a potential executable portions of a SQL command) 
	* Never concatenate strings to form database query 

*Prevent NoSQL Injection*
	* Always validate/sanitize user data before processing
	* Validate types provided by user input in MongoDB queries 
	* Use the principle of least privilege 
		* limit access rights for users to the bare minimum necessary for them to perform their work/tasks 
