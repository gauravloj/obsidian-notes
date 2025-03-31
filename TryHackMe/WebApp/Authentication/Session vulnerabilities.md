## Session Creation

### Weak Session Values

It is less common to see weak session values in modern times as frameworks are consistently used. However, with the rise of LLMs and other AI code-assistant solutions, you would be surprised at how often these old-school vulnerabilities are creeping back in. 

If a custom session creation mechanism has been implemented, there is a good chance that the session values may be guessable. A good example of this is a mechanism that simply base64 encodes the username as the session value. If a threat actor can reverse engineer the session creation process, they can generate or guess session values to hijack the accounts of legitimate users.  

### **Controllable Session Values**

In certain tokens, such as JWTs, all the relevant information to both create and verify the JWT's validity is provided. If security measures are not enforced, such as verifying the token's signature or ensuring that the signature itself was created securely, a threat actor would be able to generate their own token. These types of attacks will be discussed in more detail in a future room.  

### **Session Fixation**

Remember the web application that already gave you a session before authentication? These web applications can be vulnerable to something called session fixation. If your session value is not adequately rotated once you authenticate, a suitably positioned threat actor could record it when you are still unauthenticated and wait for you to authenticate to gain access to your session.   

### **Insecure Session Transmission**

In modern environments, it is common for the authentication server and the application servers to be distinct. Think about things like Single Sign-On (SSO) solutions. One application is used for authentication to several other web applications. In order for this process to work, your session material must be transferred from the authentication server to the application server via your browser. In this transmission, however, certain issues can creep in that would expose your session information to a threat actor. The most common is an insecure redirect where the threat actor can control the URL where you will be redirected to post-authentication. This could allow the threat actor to hijack your session. This isn't just with custom implementations, Oracle's SSO solution had a [massive bug that allowed for this to happen](https://krbtgt.pw/oracle-oam-10g-session-hijacking/).  

## Session Tracking

Session tracking is the second largest culprit of vulnerabilities. Let's take a look.

### **Authorisation Bypass**

Authorisation bypasses occur when there aren't sufficient checks being performed on whether a user is allowed to perform the action they requested. In essence, this fails to track the user's session and its associated rights correctly. It is also worth talking about the two types of authorisation bypasses:

- Vertical bypass - You can perform an action that is reserved for a more privileged user
- Horizontal bypass - You can perform an action you are allowed to perform, but on a dataset that you should not be allowed to perform the action on  
    

In most applications, vertical bypasses are easy to defend against since function decorators and path-based access control configurations are used. However, with horizontal bypasses, the user is performing an action that they should be allowed to perform. The issue is that they are performing it on someone else's data. To remedy this, actual code is required to verify who the user is (extracted from their session), which data they are requesting, and if they are allowed to request or modify the dataset.  

### **Insufficient Logging**

A key issue during incidents is not having sufficient information to piece together an attack. While a lot of logging will occur at an infrastructure level, application logging can be crucial to understanding what went wrong. In the event that the actions performed by a specific session and the ability to retrace that session to a user do not exist, it can leave gaps in the investigation that cannot be filled. It is also worth making sure that logs cover both accepted and rejected actions. In the event of a session hijacking attack, the actions would appear legitimate. Therefore, simply logging rejected actions is not sufficient to paint the picture.  

## Session Expiry

Session expiry only has a single vulnerability, which is when the expiry time for sessions are excessive. A session should be seen as a ticket to a movie. Each night, the same movie is shown, but we don't want someone to be able to use the same ticket to watch the movie again. The same counts for sessions, we need to make sure that our session expiry time takes into consideration our specific application's use case. A banking application should have a shorter session lifetime than your webmail client. 

Furthermore, in the event of long-life sessions, such as those for a webmail client, the session itself should attest to the location where it is used. If this location changes (which could be an indication of session hijacking), the session should be terminated.  

## Session Termination

For session termination, the key issue is when sessions are not properly terminated server-side when the logout action is performed. Suppose a threat actor were to hijack a user's session. In that case, even if the user became aware of the issue, without the ability to invalidate the session server-side, there isn't a method for the user to flush the access of the threat actor. However, this can be quite an issue for tokens where the lifetime of the token is embedded in the token itself. In these cases, the token can be added to a blocklist to be verified against. Some applications also take this further where all the sessions of the user can be viewed and terminated. Furthermore, upon a successful password reset, it is also recommended that all sessions are terminated to allow a user to regain full control of their account.