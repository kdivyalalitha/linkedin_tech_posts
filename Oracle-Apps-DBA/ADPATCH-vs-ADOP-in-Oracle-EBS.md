# 🔧 Oracle Apps DBA Tip: ADPATCH vs ADOP – Syntax, Use Cases & Maintenance Mode

## 📖 Overview

As an Oracle Apps DBA, understanding the difference between **ADPATCH** and **ADOP** is essential for successful patch management. The patching approach differs significantly between Oracle EBS R12.1 and R12.2 due to the introduction of **Online Patching** in R12.2.

Knowing the correct utility, prerequisites, and patching lifecycle helps minimize downtime and ensures a smooth patching process.

---

# 🛠️ ADPATCH – Oracle EBS 11i / R12.1

**ADPATCH** is the traditional patching utility used in Oracle EBS 11i and R12.1.

### Key Features

- Applies patches directly to the production file system.
- Requires application downtime.
- Maintenance Mode must be enabled before patching.
- Users should not access the application during patching.

---

## 📋 Important Prerequisites

Before applying any patch:

### ✅ Read the Patch README

Always review the README document provided with the patch downloaded from Oracle Support.

The README contains:

- Prerequisites
- Patch dependencies
- Pre-patch steps
- Post-patch steps
- Known issues

Following the README is one of the most important steps before applying any Oracle EBS patch.

---

### ✅ Verify Whether the Patch is Already Applied

Before applying a patch, verify that it has not already been applied.

```sql
SELECT *
FROM AD_BUGS
WHERE BUG_NUMBER='Patch_Number';
```

This prevents duplicate patch application.

---

### ✅ Check for Invalid Objects

Checking invalid objects before patching is a critical health check.

```sql
SELECT COUNT(*)
FROM DBA_OBJECTS
WHERE STATUS='INVALID';
```

Resolving invalid objects before patching helps:

- Improve patch success rate.
- Prevent compilation failures.
- Reduce post-patch issues.
- Maintain application stability.

---

## 📦 ADPATCH Syntax

Download the patch from **Oracle Support**, unzip it, navigate to the patch directory, and execute:

```bash
adpatch
```

The utility prompts for the required patching details during execution.

---

## 🔒 Why Maintenance Mode is Important

Before running **adpatch**, Maintenance Mode should be enabled using **adadmin**.

This prevents users from accessing Oracle EBS during patching and avoids issues such as:

- Partial patch application
- Data inconsistency
- Patch failures
- Database corruption

### Enable Maintenance Mode

```bash
adadmin
```

Select:

```
Enable Maintenance Mode
```

---

### Disable Maintenance Mode

After successful patching:

```bash
adadmin
```

Select:

```
Disable Maintenance Mode
```

---

# 🚀 ADOP – Oracle EBS R12.2+

Oracle EBS R12.2 introduced **ADOP (Application DBA Online Patching)** using **Edition-Based Redefinition (EBR)**.

Unlike ADPATCH, ADOP applies patches to the **Patch Edition**, allowing users to continue working with minimal downtime.

### Key Features

- Supports Online Patching.
- Uses Edition-Based Redefinition (EBR).
- Minimal downtime during Cutover phase.
- No Maintenance Mode required.
- Business users can continue working during most patching phases.

---

## 📦 ADOP Syntax

```bash
adop phase=apply patches=12345678 workers=4
```

---

## 🔄 Typical ADOP Lifecycle

```bash
adop phase=prepare

adop phase=apply patches=12345678

adop phase=finalize

adop phase=cutover

adop phase=cleanup
```

Each phase has a specific purpose:

- **Prepare** – Creates the Patch Edition.
- **Apply** – Applies patches to the Patch File System.
- **Finalize** – Performs final validation before Cutover.
- **Cutover** – Switches users to the patched environment.
- **Cleanup** – Removes obsolete patch editions.

---

# ADPATCH vs ADOP

| Feature | ADPATCH | ADOP |
|----------|----------|------|
| EBS Version | 11i / R12.1 | R12.2+ |
| Downtime | Required | Minimal (Cutover Only) |
| Maintenance Mode | Required | Not Required |
| Online Patching | No | Yes |
| Edition-Based Redefinition | No | Yes |
| File System | Production File System | Patch File System |

---

# Real-Time Scenario

A security patch needed to be applied to an Oracle EBS Production environment.

For an **R12.1** instance, the DBA enabled Maintenance Mode, verified the patch prerequisites, checked for invalid objects, confirmed the patch was not already applied, and successfully applied the patch using **ADPATCH** during the scheduled downtime.

For an **R12.2** environment, the same activity was performed using **ADOP**, allowing users to continue working throughout the Prepare, Apply, and Finalize phases. Only the Cutover phase required a brief downtime.

---

# Best Practices

- Always read the Patch README before starting.
- Verify patch prerequisites and dependencies.
- Check whether the patch is already applied.
- Resolve invalid database objects before patching.
- Take a complete backup before applying any patch.
- Review patch logs after completion.
- Follow Oracle's recommended patching sequence.

---

# Key Takeaways

- **ADPATCH** is used for Oracle EBS 11i and R12.1 and requires Maintenance Mode.
- **ADOP** is used in Oracle EBS R12.2 and supports Online Patching using EBR.
- Always verify prerequisites, invalid objects, and patch history before patching.
- Proper planning and validation are essential for successful Oracle EBS patching.

---

# Conclusion

Oracle EBS patching is more than simply running a command. A successful patch depends on careful planning, understanding the correct patching utility, validating prerequisites, checking system health, and following Oracle's recommended procedures.

Whether you're maintaining a legacy R12.1 environment with **ADPATCH** or managing an R12.2 environment using **ADOP**, following best practices ensures reliable, secure, and efficient patch management.
