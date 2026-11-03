# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
The spoofing vunerability in insecure.ts leaves cookies exposed for hackers to steal. It doesn't have proper safety that doesn't let the javascript scripts read the cookies and it also writes the secret in plaintext "SOMESECRET", so hackers can base their scripts off of the sourcecode and real secrets. The vulnerabilities overall allows the hackers to login to a user's session and act as that user using the cookies.  

2. Briefly explain different ways in which vulnerability can be exploited.
The vunerabiity can be exploited by stealing the cookies, which the mal-steal-cookie file does. When I clicked a link in the html file it takes me to http://localhost:8001/malhome. There it logs the connect.sid cookie which the malicious guy can then use to log in as me. 
Another way it can be exploited is CSRF which brings you to http://localhost:8000/sensitive, which is connected to the original site hosted on localhost:8000, it then submits a post request and returns either {"message":"Operation successful"} or {"message":"Unauthorized Access"} depending on if its the admin. 

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
secure.ts doesn't have the spoofing vunerability because in the middleware section it added     
cookie: {
        httpOnly: true,
        sameSite: true,
    }
This means that the javascript can't read the cookie and the browser sends requests that started from the site itself and not random files. 
It also doesn't hardcode the secret as "SOMESECRET" and rather uses 
const secret: string = process.argv[2]; which means the secret is not written in plaintext if you get the source code.  