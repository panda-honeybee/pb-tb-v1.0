---
layout: post
title:  "CAP Theorem: A Fundamental Principle of Distributed Systems"
date:   2025-02-25 18:52:34 -0800
categories: System-Design
comments: 1
---

**CAP Theorem** is a fundamental concept in distributed systems. It states that a distributed system can only provide **two out of three** guarantees among **Consistency (C)**, **Availability (A)**, and **Partition Tolerance (P)** at the same time.  

In most real-world systems, **network failures can and do happen**, meaning that the system must **either prioritize Consistency (CP) or Availability (AP), but not both at the same time**.  

The **only** scenario where a system can achieve **both Consistency (C) and Availability (A)** is if **network partitions never occur**—which is unrealistic in distributed environments.  

---

##### Understanding CAP Theorem with a Bank Example  

Imagine a **banking system** with multiple branches, where each branch represents a **server in a distributed system**.  

###### 1️⃣ Consistency (C) – Every branch sees the same account balance  
- If a customer withdraws **$500 from Branch A**, the system **must instantly update** so that **Branch B also sees the correct new balance**.  
- The system maintains **one single source of truth**, ensuring that all branches display the same data at all times.  

###### 2️⃣ Availability (A) – Every branch can always process transactions  
- Even if one branch **loses connection** to other branches, it **must still allow withdrawals and deposits** without downtime.  
- Customers should never see a "Bank Offline" error.  

###### 3️⃣ Partition Tolerance (P) – The system keeps running even if branches lose connection  
- If the **network connection between branches fails**, the banking system **must continue to operate** independently.  
- A branch should not shut down just because it **cannot communicate** with other branches.  

---

##### What Happens When a System Chooses CP or AP?  

###### 📌 If the System Chooses CP (Consistency + Partition Tolerance)  
- The **bank prioritizes accuracy** and ensures that all branches always have the **correct balance**, even during a network failure.  
- However, this means that if a branch **cannot communicate with others**, **transactions will be blocked** until the network is restored.  

**Example:**  
- A customer tries to withdraw money at **Branch B**, but the system hasn't received updates from **Branch A** due to a network issue.  
- **Result:** The transaction is **blocked** to ensure data consistency, but customers **get frustrated because they can't access their money**.  

###### 📌 If the System Chooses AP (Availability + Partition Tolerance)  
- The **bank allows transactions** at all branches, even during a network failure.  
- However, **this sacrifices consistency**, meaning some branches may **show outdated or incorrect balances**.  

**Example:**  
- A customer **withdraws $500 at Branch A** while another customer **withdraws $500 at Branch B**, even though the account only had **$600**.  
- **Result:** When the network is restored, the system realizes that **more money was withdrawn than existed**, causing an **inconsistent state**.  

---

##### Why CA (Consistency + Availability) Is Impossible in Distributed Systems  
If a bank system **tries to guarantee both C (Consistency) and A (Availability), it means:**  
✅ Every branch **always has the correct account balance** (Consistency)  
✅ Every branch **always processes transactions, even during failures** (Availability)  

🔴 **But this is only possible if there are no network failures.**  
- In reality, **networks can fail** (Partition Tolerance is needed).  
- The **only way to achieve CA** is to **run the entire bank from a single branch (one server)** so that **no network communication is required**—which means it's no longer a distributed system.  

---

##### Conclusion  
- **Distributed systems must handle network failures (P), so they must choose between CP (strong consistency) or AP (high availability).**  
- **Banks typically choose CP (Consistency + Partition Tolerance)** because ensuring correct balances is more important than always allowing transactions.  
- **E-commerce sites or messaging apps might choose AP (Availability + Partition Tolerance)** so that users can continue to interact even if some data is temporarily inconsistent.  

Thus, **CAP Theorem helps engineers make trade-offs** based on what matters most for their system. 
