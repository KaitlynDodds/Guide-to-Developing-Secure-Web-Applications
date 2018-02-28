# Guide to Securing Web Applications  

*Note from the Author:*  
I have several reasons for deciding to write this guide. Foremost among them is that the security of public facing web applications has never been more importnant. News feeds are peppered with reports of the latest security breaches and data loss. It seems the number of victims of these attacks grows with every passing day. This has prompted a surge in security concerns for any company that handle sensitive user information. While no web application will ever be completely immune to cybersecurity attacks, this does not mean that developers should ignore or skimp on security practices within their own development.

This guide is the manifestation of my frustration with the current state of secure development practice education among junior or mid-level developers. Having turned to the web to learn most of what I know about web development, I was quick to notice the lack of discussion around secure development. Hundreds of tutorials exist to walk you through how to create a React app, or write routes in Express, or develop a Yelp clone. However, very few (none that I have found) go into the slightest bit of detail on how to develop those applications securely. This means that young, inexperienced web developers are coming into the workforce, devoid of a strong security mindset. 

This guide intended to be used as a reference for web application developers interesting in developing secure applications. Developers should refer to it as they build their applications and use it to recognize potential security flaws within their applications.

I will not claim that this guide is all encompassing or details everything that must be done to secure a web application. However, it does address some major security flaws (such as those laid out in the OWASP 2017 Top Ten). This guide can be used by anyone developing web applications, although those developing applications in Node.js will find multiple links to resources that may be useful in confronting the web security vulnerabilities I touch on in the guide. 

****

## Injection Flaws
Untrusted data is sent to a server as part of a command or query, causing the system to perform in an unintended way (from the perspective of the developer) 


### _Types of Injection Flaws_

**Server Side JavaScript Injection**  
An attacker sends malicious JavaScript code to be executed on the server, can be exploited when unvalidated user input is unwittingly executed by the following JavaScript code:

* eval()
* setTimeout()
* setInterval()
* Function()


**SQL Injection/Database Injection**  
A hacker injects code into a database query that will be executed on the database, causing unintended consequences such as sensitive data being returned to the hacker or data loss 


**NoSQL Injection**  
A hacker injects code into a database query that will be executed on the database, takes advantage of built in NoSQL commands 


### _Preventing Injection Flaws_

**Preventing Injection Flaws**
	* Always validate/sanitize user data before processing 
	* Don’t use forbidden JavaScript functions to process user data 
		* use JSON.parse or another alternative 
	* Include ‘user strict’ declaration at beginning of JavaScript file or function 

**Prevent SQL Injection/Database Injection**
	* Always validate/sanitize user data before processing 
	* Use prepared statements
		* Database query is precompiled, user data is fed into precompiled query and treated as pure data (not as a potential executable portions of a SQL command) 
	* Never concatenate strings to form database query 

**Prevent NoSQL Injection**
	* Always validate/sanitize user data before processing
	* Validate types provided by user input in MongoDB queries 
	* Use the principle of least privilege 
		* limit access rights for users to the bare minimum necessary for them to perform their work/tasks 


## Cross-Site Scripting


Occurs when a web application takes untrusted data and sends it to the web browser without proper data validation or escaping. Allows attackers to execute scripts in the victims browser, exposing sensitive data that is typically retained by the browser. 

### _Common Types of XSS_

**Reflected XSS:**  
Malicious data is "reflected" back at the user by an immediate HTTP request from the victim. 
	
> Malicious code is executed immediately in the users browser.

**Stored XSS**  
Malicious data is stored on the server or browser and embedded in HTML pages provided later.
	
> Malicious code is executed after is has been stored for a period of time.


### _Preventing XSS_

**How to Prevent XSS Attacks**  

* Always sanitize and validate all user input

* Encode output for correct context

> Prevents JavaScript from running in that context (encode and escape all ASCI characters)

* Set the HTTPOnly flag on cookies

> ensures cookies cant be accessed by JavaScript on the client side, thus cant be stolen by an attackers XSS

* Apply encoding on both the client and server-side

> mitigates DOM based XSS as well, untrusted data never leaves the clients browser 

*  Set the Content-Security-Policy header 

> allows developers to create whitelists for client-side resources, header ensures that the browser only renders/executes resources from the specified sources


























