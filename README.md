@'
# Writeups

Write-ups from labs and wargames while learning offensive security.
Method and reasoning only. No flags or passwords are published.

## Index
- [OverTheWire Bandit](./overthewire-bandit/) — Linux and shell fundamentals
'@ | Set-Content -Path README.md -Encoding utf8


# 3 Lab: User role controlled by request parameter

## Vulnerability
Access control flaw — the server trusted a forgeable "Admin" cookie 
to decide who is an administrator.

## Steps
1. Logged in as wiener. Saw the server set: Admin=false
2. Sent GET /admin to Repeater. Access denied with Admin=false.
3. Changed the cookie to Admin=true and re-sent → got the admin panel.
4. Found the delete link and sent GET /admin/delete?username=carlos
5. carlos deleted → lab solved.

## Mistakes I made (and fixed)
- Used "Set-Cookie" instead of "Cookie" → 400 error
- Sent to /my-account instead of /admin
- Sent to a dead host by mistake → 0 bytes, "Stream failed"

## Lesson
1. Never trust client-side data (cookies, parameters) for security decisions.
2. Never, Never give up.
