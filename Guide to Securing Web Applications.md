# Guide to Securing Web Applications  

*Note from the Author:*  
This guide is the manifestation of my frustration with the current state of secure development practice education among junior or mid-level developers. Having turned to the web to learn most of what I know about web development, I was quick to notice the lack of discussion around secure development. Hundreds of tutorials exist to walk you through how to create a React app, or write routes in Express, or develop a Yelp clone. However, very few go into the slightest bit of detail on how to develop those applications securely. This means that young, inexperienced web developers are coming into the workforce lacking in a strong security mindset. 

This guide intended to be used as a reference for web application developers interesting in developing secure applications. Developers should refer to it as they build their applications and use it to recognize potential security flaws within their applications.

I will not claim that this guide is all encompassing or details everything that must be done to secure a web application. However, it does address some major security flaws (such as those laid out in the OWASP 2017 Top Ten). This guide can be used by anyone developing web applications. Although those developing applications in Node.js will find multiple links to resources that may be useful in confronting the web security vulnerabilities I touch on in the guide.

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



## Cross Site Request Forgery  


Forces an authenticated victim's browser to send a _forged_ HTTP request to a vulnerable web application. The vulnerable web application treats the request as legitimate, and exposes sentitive information such as session cookies and other automatically included authentication information to the attacker. 


### _CSRF Exploitations_

**State-Changing Requests**

An attacker cannot view the response to any forged request. Therefore, an attacker may attempt to trick the victim into performing actions that benefit the attacker, such as changing their email address, bank account and routing numbers, transfer funds, etc. 

This can be especially hazardous if the compromised user is an admin account. 

**Social Engineering**

Attackers often embed malicious HTML or JavaScript into an email or website that is made available to the victim. The malicious code requests a specific 'task URL' which is then executed without the user's knowledge. 

### _Preventing CSRF_

**CSRF Tokens**

A developer can generate a token that can be added to requests that mutate state (e.g. PUT, POST, and DELETE methods). 

*  The random token can be added to a hidden input field, query string, or header field.  

*  CSRF middleware will check that the token matches the generated token for the request-response pair.  


**Verify Same Origin**  

A developer can examine an HTTP request header value to prevent CSRF attacks.

*  Identify source origin using the **Origin Header** and **Referer Header**

>  The Origin header should match the target origin, Verify the hostname in the Referer header matches the target origin

*  Identify the target origin

> If your web application server is directly access by its users, use the origin in the URL 


### _Links_

[OWASP CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendations_For_Automated_CSRF_Defense)  

[Node.js CSURF](https://github.com/expressjs/csurf)


## Authentication and Session Management  

Attackers take advantage of flaws in authentication setup or session management to impersonate victim users. Attackers can use these flaws to steal passwords, secret keys, and tokens used to authenticate sessions, allowing them to take over another users account.

### _Broken Authentication_

**Flaws in session management and authentication can occur in the following places:**

* Login and logout functionality
 
* Password management  

* Token timeouts

* Remember me functionality

* Recovery questions

* Account updates

**Common Flaws in Session Management**

* Improper timeout settings 
 
> Can prevent an attacker from impersonating a user

* Improperly protected session ID's or tokens

> Vulnerable in the case of a man-in-the-middle attack

* Unprotected passwords

> Used to log in as a victim user 


### _Preventing Authentication Exploitations_

**Preventing Session Management Vulnerabilities**

* User authentication credentials should be protected when stored and in transit 

> Always hash user authentication credentials when stored and in transit   

* Session tokens should not be exposed in the URL 

* Session tokens should be timed-out after a short time, and be invalidated at logout 

* Session tokens should be recreated after a successful login

* Passwords, session IDs, and other authentication info should never be sent over a non-HTTPS connection 

> Use HTTPOnly header to ensure your session/cookies are not accessible through JavaScript 

* Don't provide more information than is necessary after a unsuccessful login attempt 

> Do not indicate what authentication data was incorrect (username was incorrect, password was incorrect), simply indicate a login error  

* Disable accounts after too many failed login attempts


### _Links_

[NPM Helmet Library](https://www.npmjs.com/package/helmet)  
[Node Passport Library (Authentication)](http://www.passportjs.org/docs/authenticate/)  
[OWASP Session Management Cheat Sheet](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet)



















