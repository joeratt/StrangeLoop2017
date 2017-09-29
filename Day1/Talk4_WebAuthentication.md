# Web Authentication
Talk: https://thestrangeloop.com/2017/it-me-under-the-hood-of-web-authentication.html

Node.js: Passport
- 36k downloads / day
- Example shows using plaintext passwords in the DB :sad:

2013: Adobe password db leak
- 38million users leaked

## Encryption != Hashing
- Encryption is invertable, given secret key!
- Hashing is a one-way function (not intertable)
- Salting -- prevents similar passowrds



# (Secure) Session management

## User sessions in web Applicaitons
1. Session absraction: provided by framework
  - "Stateless" sessions send all session data to the web client
  - server-side: store everything on the server
2. Persistedon the client
  - HTTP Cookies
    - Flags: HttpOnly (JS can't modify cookie, stops cross-site scripting), Secure (Stops sending cookie on non https connections)
  - {local,session}Storage
    - Vulnerable
3. Client returns auth in subsequent callbacks

## User sessions: prevent tampering

Message Authentication Codes (MACs)
- message and MAC tag are transferred to server, server confirms MACs match
- Gotcha: Sessions aren't encrypted
- Issue: Comparing MACs gives info by comparison timeout
  - If just using string comparison, most languages just stop comparing when mismatch
  - if they don't match at all, comes back faster than if it's close to matching
- Thoughts to fix:
  1. You can XOR all of the characters in both strings
    - Sometimes the JIT and Compiler will try to "speed" things up for you
  2. Use a pseudo-random function and apply the same function to both strings
    - combined with character comparison makes sure the compiler doesn't try to help you out


# Multi-Factor Authentication

1. Knowledge Factors
2. Possession Factors
3. "Something you are"


## SMS One-Time Password (OTP)

Vulnerable since goes through Phone Service Providers (example, Verizon)

## {HMAC,Time}-Based OTP

1. Physical Device generates number (HMAC)
2. Webpage OTP (Time-based)


# U2F

![https://www.yubico.com/wp-content/uploads/2015/03/U2F.png](https://www.yubico.com/wp-content/uploads/2015/03/U2F.png)

https://en.wikipedia.org/wiki/Universal_2nd_Factor

A more secure 2-Factor. You have to physically plug / NFC to use.

[Reviews to physical devices](https://github.com/hillbrad/U2FReviews)
[Github's Digital Version for MacOS](https://github.com/github/SoftU2F)

# Password-less Logins
