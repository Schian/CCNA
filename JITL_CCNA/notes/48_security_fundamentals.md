# Security Fundamentals

## Key Concepts

- The **CIA** Triad
  - Confidentiality
    - Only authorised users should be able to access data
      - Some information/data is public and can be accessed by anyone
      - Some is secret and should only be accessed by specific people
  - Integrity
    - Data should not be tampered with by unauthorised users
    - Data should be correct and authentic
  - Availability
    - The network/system should be operational and accessible to authorised users
- A **vulnerability** is any potential weakness that can compromise the CIA of a system or information
  - A *potential* weakness isn't a problem on its own
  - "A window is a vulnerability, but houses still have windows"
- An **exploit** is something that can potentially be used to exploit the vulnerability
  - Something that can *potentially* be used as an exploit isn't a problem on it's own
  - "A rock can break the window, but rocks on their own aren't a problem"
- A **threat** is the potential of a **vulnerability** to be **exploited**
  - A hacker **exploiting** a **vulnerability** in your system is a **threat**
  - "A robber using the rock to break the window is a threat"
- A **mitigation technique** is something that can protect against threats
  - Should be implemented everywhere a vulnerability can be exploited
    - client devices, servers, switches, routers, firewalls, etc
  - "Placing bars over glass can mitigate the threat of robbers breaking in"
- **`NO SYSTEM IS PERFECTLY SECURE!`**

## Common Attacks

- DoS (denial-of-service) attacks
  - Target the availability of a system so users can't access it
- Spoofing attacks
  - Involve using fraudulent source IP or MAC addresses
- Reflection or Amplification attacks
  - Involves spoofing a source IP address to cause a reflector to send lots of traffic to the target
- Man-in-the-middle attacks
  - An attacker intercepts traffic between the source and destination to eavesdrop and/or modify the traffic
- Reconnaissance attacks
  - Used to gather information on the target to perform future attacks
- Malware
  - Malicious software such as viruses, worms, and trojan horses that infect a system
- Social engineering attacks
  - attacks that use psychological manipulation to target people and make them reveal information or perform an action
- Password-related attacks
  - attacks such as dictionary attacks and brute force attacks, used to guess the target's password

## Multi-Factor Authentication

- **Multi-factor authentication** involves providing more than just a `username:password` to prove your identity
- Usually involves providing two of the following:
  - **Something you know**
    - A `username:password` combination, a PIN, etc.
  - **Something you have**
    - Pressing a notification on your phone, swiping a badge or pass, etc.
  - **Something you are**
    - Biometrics such as a face, palm, fingerprint, or retina scan
- Requiring multiple factors of authentication greatly increases security
  - Even if an attacker learns the target's password (**something you know**), they won't be able to login to the target's account

## Digital Certificates

- **Digital Certificates** are another form of authentication used to prove the identity of the holder of the certificate
- They are used for websites to verify that the website being accessed is legitimate
- Entities that want a certificate to prove their identity send a CSR (Certificate Signing Request) to a CA (Certificate Authority), which will generate and sign the certificate.

## Controlling and Monitoring Users with AAA

- **AAA** (triple-A) stands for **A**uthentication, **A**uthorisation, and **A**ccounting
  - A framework for controlling and monitoring users of a computer system
  - **Authentication** is the process of verifying a user's identity
    - Logging in = Authentication
  - **Authorisation** is the process of granting the user the appropriate access and permissions
    - Granting the user access to some files and services
    - Restricting access to other files and services
  - **Accounting** is the process of recoding the user's activities on the system
    - Logging when a user makes a change to a file
- Enterprises typically use a AAA server to provide AAA services
  - ISE (Identity Services Engine) is Cisco's AAA server
- AAA servers usually support two AAA protocols
  - RADIUS
    - An open standard protocol
    - Uses UDP ports 1812 and 1813
  - TACACS+
    - A Cisco proprietary protocol
    - Uses TCP port 49

## Security Program Elements

- **User Awareness Programs** are designed to make employees aware of potential security threats and risks
  - A company might send out false phishing emails to see if employees click a link and sign in with their credentials
  - Although the emails are harmless, employees who fall for the false emails will be informed that it is part of a user awareness program and they should be careful of phishing emails
- **User Training Programs** are more formal than the above
  - Dedicated training sessions which educate users on the corporate security policies, how to create strong passwords, and how to avoid potential threats
  - **Physical Access Control** protects equipment and data from potential attackers by only allowing authorised users into protected areas such as network closets or data centre floors
    - Multi-factor locks can protect access to restricted ares
      - A door that requires a swipe and fingerprint scan to enter
      - The permissions of the badge can easily be changed
