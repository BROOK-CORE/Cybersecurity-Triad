# Cybersecurity Architecture — CIA Triad

## Overview

The **CIA Triad** is a basic security model used to think about the three main goals of information security:

1. **Confidentiality** — only authorized people or systems can access information.
2. **Integrity** — information remains accurate, trustworthy, and protected from unauthorized modification.
3. **Availability** — authorized users can access systems and resources when they need them.

A secure system needs to consider all three.

---

# 1. Confidentiality

Confidentiality means preventing unauthorized access to information.

Examples of information that may require confidentiality:

- Passwords
- Personal information
- Financial records
- Company documents
- Private communications
- Database records

Two major security mechanisms used to protect confidentiality are:

- **Access control**
- **Encryption**

---

## Authentication

Authentication answers:

> **Who are you?**

It verifies the identity of a person, device, or service.

Common authentication factors include:

### Something you know

- Password
- PIN
- Security question

### Something you have

- Security token
- Smartphone
- Hardware security key

### Something you are

- Fingerprint
- Face recognition
- Other biometric characteristic

---

## Authorization

Authorization answers:

> **What are you allowed to do?**

Authentication and authorization are different.

```text
Authentication → Identify the user
Authorization  → Decide what the user can access
```

A user may successfully authenticate but still be denied access to a resource because they do not have the required permissions.

---

## Multi-Factor Authentication

**MFA** requires multiple authentication factors.

Example:

```text
Password
   +
Authenticator code
   =
MFA
```

Using multiple factors makes it harder for a stolen password alone to provide access.

---

## Role-Based Access Control

**RBAC** assigns permissions based on roles.

Example:

```text
Administrator
├── Create users
├── Delete users
├── Change settings
└── Read all records

Employee
├── Read assigned records
└── Update assigned records

Guest
└── Read public information
```

The goal is to give users only the permissions required for their job.

---

# 2. Encryption

Encryption protects information by transforming readable data into ciphertext.

```text
Plaintext
   ↓
Encryption + Key
   ↓
Ciphertext
   ↓
Decryption + Key
   ↓
Plaintext
```

Without the required key, an intercepted encrypted message should not be readable.

---

## Symmetric Encryption

Symmetric encryption uses the same secret key for encryption and decryption.

```text
              Secret Key
                 ↓
Plaintext → [Encryption] → Ciphertext
                              ↓
                         [Decryption]
                              ↑
                         Secret Key
                              ↓
                          Plaintext
```

The major challenge is securely sharing the secret key between the communicating parties.

---

# 3. Integrity

Integrity means that data remains correct and trustworthy.

A system should be able to determine whether information has been:

- Modified
- Corrupted
- Replaced
- Deleted
- Tampered with

Example:

```text
Original record
      ↓
Stored safely
      ↓
Someone modifies it
      ↓
Integrity check
      ↓
Modification detected
```

---

## Logging

Logs create a record of important system activity.

Examples:

```text
Login successful
Login failed
File changed
Account created
Transaction performed
Permission changed
```

Example log:

```text
2026-09-29 10:15:32
USER: admin
ACTION: login
RESULT: success
```

Logs can help security teams investigate unexpected activity.

---

## Cryptographic Integrity Checks

A **hash** produces a fixed-size value based on input data.

```text
Original Data
     ↓
Hash Function
     ↓
Hash Value
```

If the data changes, its hash will normally change as well.

Conceptually:

```text
File
 ↓
SHA-256
 ↓
Hash A
```

After modification:

```text
Modified File
      ↓
SHA-256
      ↓
Hash B
```

If:

```text
Hash A ≠ Hash B
```

the file contents have changed.

---

# 4. Digital Signatures

Digital signatures can provide evidence about:

- Who signed data
- Whether the data was modified after signing

A simplified model:

```text
Data
 ↓
Hash
 ↓
Digital Signature
```

The recipient can verify the signature and detect whether the signed data has been altered.

---

# 5. Immutability

An immutable system is designed so that existing records cannot simply be changed or removed without detection or authorization.

```text
Record 1
   ↓
Record 2
   ↓
Record 3
   ↓
Record 4
```

If someone attempts to modify an earlier record, integrity mechanisms can reveal the change.

---

# 6. Blockchain and Integrity

Blockchain systems use cryptographic techniques and chained records to make unauthorized modification more difficult to conceal.

```text
Block 1
  ↓
Block 2
  ↓
Block 3
  ↓
Block 4
```

The important security idea is tamper detection and a traceable record history.

---

# 7. Availability

Availability means:

> Authorized users can access required systems and resources when they are needed.

A system can have strong confidentiality and integrity while still failing security requirements if legitimate users cannot access it.

Examples of availability requirements:

- Website remains reachable
- Database remains operational
- Authentication service remains available
- Business applications remain accessible
- Critical network services continue operating

---

# 8. Denial-of-Service Attacks

A **Denial-of-Service (DoS)** attack attempts to make a service unavailable to legitimate users.

Conceptually:

```text
Legitimate users
       ↓
     Server
       ↑
   Huge volume
   of malicious
     traffic
```

The attacker attempts to consume resources such as:

- CPU
- Memory
- Network bandwidth
- Connection tables
- Application capacity

The result can be degraded performance or complete service disruption.

---

# 9. Distributed Denial-of-Service

A **DDoS** attack uses multiple systems to generate traffic toward a target.

```text
Compromised Device ──┐
Compromised Device ──┤
Compromised Device ──┼──→ Target Server
Compromised Device ──┤
Compromised Device ──┘
```

The distributed nature makes the traffic larger and can make filtering more difficult.

---

# 10. Botnets

A **botnet** is a collection of compromised devices controlled by an attacker.

```text
                Attacker
                   ↓
             Control System
            ↙      ↓      ↘
         Bot 1    Bot 2    Bot 3
           ↓        ↓        ↓
           └────────┴────────┘
                    ↓
               Target System
```

Botnets can be used to generate large quantities of unwanted traffic.

---

# 11. SYN Flood

A SYN flood targets the TCP connection setup process.

A normal TCP connection uses a three-step handshake:

```text
Client                    Server

  SYN  -------------------->

       <-------------------- SYN-ACK

  ACK  -------------------->
```

In a SYN flood, an attacker sends many SYN requests without completing the connection process.

```text
SYN ─────────→
SYN ─────────→
SYN ─────────→
SYN ─────────→
SYN ─────────→
              Server
```

The server may keep resources reserved for incomplete connections.

A sufficiently large volume can consume resources and interfere with legitimate connections.

---

# 12. Protecting Availability

## Redundancy

Use multiple systems so failure of one system does not stop the entire service.

```text
          Load Balancer
          /    |    \
       Server Server Server
```

## Rate Limiting

Limit how many requests can be accepted from a source or identity within a period.

```text
Request 1 → Allow
Request 2 → Allow
Request 3 → Allow
Request 4 → Limit
Request 5 → Limit
```

## Traffic Filtering

Detect and block suspicious traffic before it reaches critical systems.

## Network Protection

Firewalls, filtering systems, and DDoS protection services can help reduce malicious traffic.

## Monitoring

Monitor:

- Traffic volume
- Connection rates
- Error rates
- CPU usage
- Memory usage
- Service health

---

# 13. CIA Triad Security Checklist

## Confidentiality

```text
Who should be able to access the data?
How is identity verified?
What permissions should each role have?
Is sensitive data encrypted?
```

## Integrity

```text
How do we know the data has not been changed?
Are important actions logged?
Can unauthorized modifications be detected?
Can records be verified?
```

## Availability

```text
Who needs the system?
When does it need to be available?
What happens if a component fails?
How will the system handle excessive traffic?
Is there redundancy?
```

---

# 14. CIA Triad Summary

| Principle | Main Goal | Example Controls | Example Threat |
|---|---|---|---|
| Confidentiality | Prevent unauthorized disclosure | Authentication, authorization, MFA, encryption, RBAC | Data breach |
| Integrity | Prevent or detect unauthorized changes | Hashes, logging, digital signatures, integrity checks | Data tampering |
| Availability | Keep resources accessible | Redundancy, monitoring, rate limiting, traffic filtering | DoS/DDoS |

---

# 15. Easy Memory Trick

```text
C = Confidentiality
    "Can the wrong person see it?"

I = Integrity
    "Has someone changed it?"

A = Availability
    "Can the right person use it?"
```

Or:

```text
CIA

C → Keep it SECRET
I → Keep it CORRECT
A → Keep it AVAILABLE
```

---

# Final Checklist

- [ ] Authentication is implemented correctly.
- [ ] Authorization restricts access appropriately.
- [ ] MFA is used where appropriate.
- [ ] Least privilege is enforced.
- [ ] Sensitive information is protected with encryption.
- [ ] Important actions are logged.
- [ ] Data integrity can be checked.
- [ ] Unauthorized modification can be detected.
- [ ] Critical services have availability requirements.
- [ ] Systems have appropriate redundancy.
- [ ] DoS/DDoS risks have been considered.
- [ ] Monitoring and incident response are available.
