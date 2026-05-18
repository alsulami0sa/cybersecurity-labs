# SQL Injection Security Lab

## Overview
This lab focused on understanding how SQL injection vulnerabilities affect web applications and how they can be identified and tested in a controlled academic environment.
The lab included practical exercises using SQLmap and OWASP ZAP to identify SQL injection vulnerabilities, enumerate database information, and analyze security findings.

## Objectives
- Understand how SQL injection vulnerabilities occur
- Identify vulnerable web application inputs
- Perform SQL injection testing in a controlled environment
- Enumerate database information using SQLmap
- Detect SQL injection vulnerabilities using OWASP ZAP
- Review vulnerability findings and security identifiers

## Tools Used
- SQLmap
- OWASP ZAP
- ShellGPT
- Web Browser
- Windows Server 2019
- Parrot Security

## Lab Activities

### 1. SQL Injection Testing

Used SQLmap against a vulnerable web application in the lab environment to:
- Test the application for SQL injection vulnerabilities
- Enumerate available databases
- Enumerate database tables
- Review data exposed through vulnerable database queries
- Explore the impact of successful SQL injection

### 2. Vulnerability Detection with OWASP ZAP
Used OWASP ZAP to perform an automated security scan of the target web application.

The scan helped identify:
- SQL injection vulnerabilities
- Vulnerability details
- CWE identifiers
- WASC identifiers

### 3. SQL Injection with AI Assistance
Used ShellGPT in the controlled lab environment to assist with SQL injection testing and database enumeration.
This exercise demonstrated how AI-assisted tools can support security testing workflows.

## What I Learned
- How SQL injection can expose sensitive database information
- How SQLmap automates SQL injection testing and database enumeration
- How OWASP ZAP can identify web application vulnerabilities
- How vulnerability findings can be mapped to identifiers such as CWE and WASC
- The importance of securing database-backed web applications
- The role of input validation and secure database queries in preventing SQL injection

## Security Recommendations
- Use parameterized queries and prepared statements
- Validate and sanitize user input
- Apply least-privilege permissions to database accounts
- Avoid exposing detailed database error messages
- Regularly test web applications for injection vulnerabilities
- Keep web application frameworks and dependencies updated

## Disclaimer
All activities documented in this lab were performed in an authorized and controlled academic environment for educational purposes only.
