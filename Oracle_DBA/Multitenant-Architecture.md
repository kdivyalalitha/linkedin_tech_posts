# Oracle Multitenant: Understanding CDB & PDB Architecture

With **Oracle 12c onwards**, the Multitenant Architecture introduced a game-changing way of managing databases.

Instead of handling multiple independent databases, Oracle allows database consolidation using **CDB (Container Database)** and **PDB (Pluggable Database)**.

---

# ✨ What is a CDB (Container Database)?

A **CDB (Container Database)** acts as the central structure that hosts one or more PDBs.

It:

* Acts as the central container structure.
* Contains the root container and Oracle system metadata.
* Provides the common infrastructure for PDBs.
* Manages resources and operations across PDBs.

---

# ✨ What is a PDB (Pluggable Database)?

A **PDB (Pluggable Database)** is a portable database that plugs into a CDB.

It contains application-related database objects such as:

* User schemas
* Tables
* Indexes
* Application data

PDBs provide flexibility because they can be:

* Created
* Cloned
* Unplugged
* Plugged into another CDB
* Moved between compatible environments

---

# 🏗️ CDB & PDB Architecture

A simplified view of the architecture:

```text
                    CDB
             Container Database
                     │
          ┌──────────┼──────────┐
          │          │          │
          ▼          ▼          ▼
       CDB$ROOT    PDB1       PDB2
          │          │          │
          │          │          │
      Common       App Data   App Data
      Metadata     Schemas    Schemas
```

### Simple way to remember:

**CDB = Container / Infrastructure**

**PDB = Pluggable Database / Application Data**

---

# ✅ Why Multitenant?

## 1️⃣ Simplified Management

Multiple PDBs can be managed within a single CDB.

This provides a consolidated architecture instead of maintaining many completely independent database instances.

---

## 2️⃣ Resource Efficiency

PDBs share common CDB infrastructure, including database instance resources.

This can reduce the overhead associated with running multiple independent database environments.

---

## 3️⃣ High Availability

Multitenant architecture provides capabilities that can simplify database operations such as:

* Backup and recovery
* Database maintenance
* PDB-level operations
* Resource management

However, the exact operational benefits depend on the Oracle version, architecture, and configuration.

---

## 4️⃣ Agility

PDBs provide flexibility for:

* Rapid database provisioning
* Cloning
* Migration
* Testing
* Development environments
* Database consolidation

This makes Multitenant particularly useful in large enterprise environments.

---

# 🔍 Key DBA Views to Monitor

Oracle DBAs can use several views to understand the CDB/PDB environment.

### `V$CONTAINERS`

Lists information about containers in the CDB.

Useful for identifying:

* CDB root
* PDBs
* Container IDs
* Container names

---

### `V$PDBS`

Displays information about PDBs.

Useful for checking:

* PDB name
* Open mode
* Restricted status
* PDB status

---

### `DBA_PDBS`

Provides administrative information about PDBs.

Useful for checking information about PDB configuration and status.

---

# 🛠️ Useful DBA Checks

### Check current container

```sql
SHOW CON_NAME;
```

### List containers

```sql
SELECT CON_ID, NAME, OPEN_MODE
FROM V$CONTAINERS;
```

### Check PDB status

```sql
SELECT CON_ID, NAME, OPEN_MODE
FROM V$PDBS;
```

These commands help a DBA quickly understand the current Multitenant environment.

---

# 🎯 Oracle DBA Perspective

Multitenant architecture changes the way DBAs think about database administration.

Instead of managing every database as an isolated entity, DBAs can manage multiple PDBs within a common CDB architecture.

A DBA should understand:

**CDB → Root → PDBs → Application Schemas → Data**

This becomes especially important when working with:

* Database provisioning
* Cloning
* Backup and recovery
* PDB management
* Resource management
* Database migration
* High availability

---

# 💡 Easy Way to Remember

```text
CDB
│
├── CDB$ROOT
│      ↓
│   Common Infrastructure
│
├── PDB1
│      ↓
│   Application Data
│
├── PDB2
│      ↓
│   Application Data
│
└── PDB3
       ↓
    Application Data
```

### In short:

**CDB = The backbone / container infrastructure**

**PDBs = The pluggable databases containing application data**


