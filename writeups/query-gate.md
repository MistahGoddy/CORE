# Query Gate — Write-up

**Category:** Warmups / Practical Exercise
**Difficulty:** Basic
**Target service:** MySQL (RDBMS)

> **Note:** This write-up covers general methodology only. Specific data recovered from the target (database/table names, query results, and answers submitted to the platform) has been withheld so this repo doesn't function as an answer key for others attempting the same exercise.

## Objective

Query Gate is a live target machine running MySQL. The goal is to enumerate the service, gain access, and extract data — practicing the fundamentals of database reconnaissance and exploitation of misconfigurations.

## Approach

**1. Reconnaissance**
Ran a port scan against the target to identify exposed services:
```bash
nmap <target_ip>
```
This surfaced the relevant MySQL port and confirmed the service running on it.

**2. Access attempt**
Since MySQL was exposed directly, the next step was testing whether the most privileged account (`root`) could connect without additional setup:
```bash
mysql -u root -h <target_ip>
```

**3. Enumeration**
Once connected, standard MySQL enumeration commands were used to explore what was accessible:
```sql
SHOW DATABASES;
USE <database>;
SHOW TABLES;
SELECT * FROM <table>;
```

## Key Takeaway

This exercise demonstrates a common real-world misconfiguration pattern: databases exposed to the network without proper authentication controls. A basic port scan plus a default-account connection attempt was enough to begin enumerating the target — underscoring why databases should never be directly internet-facing without strict access controls, strong credentials, and network segmentation.

## Tools Used

- `nmap` — port scanning / service discovery
- `mysql` CLI — database connection and enumeration
