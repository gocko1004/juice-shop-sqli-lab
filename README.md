# Juice Shop SQL Injection Lab

This project provides a safe lab for learning and demonstrating SQL injection using OWASP Juice Shop.

Repository link:  
https://github.com/gocko1004/juice-shop-sqli-lab

## Overview

OWASP Juice Shop is a deliberately insecure e-commerce application. This repository contains a ready-to-run Docker environment for workshops, demos, and self-study. 

You will learn:
- How attackers bypass login with SQL injection.
- How attackers extract sensitive data from a database.
- Why these attacks are dangerous in the real world.
- Which defenses prevent them.

## Prerequisites

- Install Docker Desktop and make sure it is running.  
- Install Git.  
- (Optional) Install Visual Studio Code to browse and edit the files.

## Quickstart

Open a terminal (PowerShell, CMD, or Linux/Mac shell) and run:

git clone https://github.com/gocko1004/juice-shop-sqli-lab.git  
cd juice-shop-sqli-lab  
docker compose up  

When Docker finishes pulling and starting the container, open:

http://localhost:3000

You will see the Juice Shop home page.

## SQL Injection Demos

### Demo 1: Login Bypass

1. Go to Account → Login.  
2. In the Email field, enter: `' OR 1=1 --`  
3. In the Password field, type anything.  
4. Click Log in.  

Result: You are logged in without a real account. Juice Shop shows you as logged in even though the credentials were fake.

### Demo 2: User Data Extraction

1. Go to Account → Login.  
2. In the Email field, enter: `' UNION SELECT id, email, password, 1, 1 FROM users --`  
3. In the Password field, type anything.  
4. Click Log in.  

Result: Juice Shop may display leaked user data (emails and password hashes) directly in the UI, or it may show a database error that reveals internal details. Both outcomes demonstrate a successful SQL injection.

## Why These Work

- `' OR 1=1 --` ends the query early, adds a condition that always evaluates to true, and comments out the rest. The application accepts the login without valid credentials.  
- `UNION SELECT ...` combines another SELECT statement, forcing the application to output data from the users table.  

These are classic SQL injection techniques and show why it is one of the most critical issues in the OWASP Top 10.

## Stopping and Resetting the Lab

To stop the lab:  
`docker compose down`

To reset the lab (clear all data and pull the latest image):  
`docker compose down -v && docker compose pull`

## Safety Notice

This project is for educational use only. Do not use these techniques on systems you do not own or do not have explicit permission to test.

## Credits

- OWASP Juice Shop for the vulnerable application.  
- Personal learning lab by Goce Petrov. More: https://www.gocepetrov.com/security-and-it

## SQL Injection Cheat Sheet

This section gives you two ready-to-use payloads and a reminder of why they work. Use it as a quick reference during workshops.

Payload 1, login bypass: `' OR 1=1 --`  
- Works on the login form.  
- Logs you in without a real account.  

Payload 2, user data extraction: `' UNION SELECT id, email, password, 1, 1 FROM users --`  
- Works on the login form.  
- Attempts to leak user IDs, emails, and password hashes.  

Why they work:  
- `OR 1=1` makes the WHERE clause always true.  
- `UNION SELECT` pulls extra data from the users table.  

Defense reminders:  
- Always use parameterized queries.  
- Enforce strict input validation.  
- Use least-privilege database accounts.  
- Monitor queries and alerts for anomalies.

