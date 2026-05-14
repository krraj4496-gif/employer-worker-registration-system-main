<div align="center">

<br/>

```
███████╗███╗   ███╗██████╗ ██╗      ██████╗ ██╗   ██╗███████╗██████╗
██╔════╝████╗ ████║██╔══██╗██║     ██╔═══██╗╚██╗ ██╔╝██╔════╝██╔══██╗
█████╗  ██╔████╔██║██████╔╝██║     ██║   ██║ ╚████╔╝ █████╗  ██████╔╝
██╔══╝  ██║╚██╔╝██║██╔═══╝ ██║     ██║   ██║  ╚██╔╝  ██╔══╝  ██╔══██╗
███████╗██║ ╚═╝ ██║██║     ███████╗╚██████╔╝   ██║   ███████╗██║  ██║
╚══════╝╚═╝     ╚═╝╚═╝     ╚══════╝ ╚═════╝    ╚═╝   ╚══════╝╚═╝  ╚═╝
```

# 🏢 Employer–Worker Registration System

**A Java desktop accounting application for managing employer–worker relationships, wages, and payment records.**

<br/>

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Swing](https://img.shields.io/badge/Java%20Swing-GUI-007396?style=for-the-badge&logo=java&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

<br/>

</div>

---

## 📖 Overview

The **Employer–Worker Registration System** is a desktop accounting application built entirely in Java. It bridges the gap between employers and workers by maintaining structured records of their professional relationships — including daily work logs, wage tracking, and payment histories — all backed by a PostgreSQL relational database.

Whether you're managing a small construction crew, a seasonal workforce, or any labor-intensive operation, this system provides a clean GUI to keep everything organized and auditable.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔐 **Admin Login** | Secure login screen to protect access to all records |
| 🏭 **Employer Registration** | Add and manage employer profiles with business and contact info |
| 👷 **Worker Registration** | Register workers with personal and contact details |
| 📋 **Work Record Logging** | Log daily work entries linking workers to employers with dates and wages |
| 💰 **Payment Tracking** | Track payments made to workers and received from employers separately |
| 🔍 **Search Box** | Quickly look up workers and employers from the registry |
| 📄 **Registration Documents** | Generate record summaries for work relationships |

---

## 🖥️ Screenshots

### 🔑 Login Screen
The entry point — secure admin authentication before accessing any data.

> *(Login screen screenshot)*

### 🏠 Homepage & Dashboard
Central navigation hub to access all modules of the system.

| Homepage | Employer Registration | Worker Registration |
|---|---|---|
| *Home dashboard* | *New employer form* | *New worker form* |

### 🔍 Search & Records

| Search Box | Registration Document |
|---|---|
| *Worker/employer search* | *Work record option pane* |

---

## 🗄️ Database Schema

The system uses **PostgreSQL** with the following relational tables:

```sql
-- Admin authentication
CREATE TABLE admin (
    id          SMALLSERIAL PRIMARY KEY NOT NULL,
    username    VARCHAR,
    password    VARCHAR
);

-- Employer profiles
CREATE TABLE employer (
    employer_id SERIAL PRIMARY KEY NOT NULL,
    name        VARCHAR NOT NULL,
    surname     VARCHAR NOT NULL,
    business    VARCHAR,
    phonenumber VARCHAR
);

-- Worker profiles
CREATE TABLE worker (
    worker_id    SERIAL PRIMARY KEY NOT NULL,
    name         VARCHAR NOT NULL,
    surname      VARCHAR NOT NULL,
    phone_number VARCHAR
);

-- Work records linking workers to employers
CREATE TABLE worker_record (
    worker_record_id SERIAL PRIMARY KEY NOT NULL,
    worker_id        INTEGER REFERENCES worker(worker_id),
    employer_id      INTEGER REFERENCES employer(employer_id),
    date             VARCHAR(10) NOT NULL,
    wage             SMALLINT NOT NULL
);

-- Employer-side work logs
CREATE TABLE employer_record (
    employer_record_id SERIAL PRIMARY KEY NOT NULL,
    employer_id        INTEGER REFERENCES employer(employer_id),
    date               VARCHAR(10) NOT NULL,
    note               VARCHAR(255),
    number_worker      SMALLINT NOT NULL,
    wage               SMALLINT NOT NULL
);

-- Payment records: worker payments
CREATE TABLE worker_payment (
    worker_payment_id SERIAL PRIMARY KEY NOT NULL,
    worker_id         INTEGER REFERENCES worker(worker_id),
    employer_id       INTEGER REFERENCES employer(employer_id),
    date              VARCHAR(10) NOT NULL,
    paid              INTEGER NOT NULL
);

-- Payment records: employer payments
CREATE TABLE employer_payment (
    employer_payment_id SERIAL PRIMARY KEY NOT NULL,
    employer_id         INTEGER REFERENCES employer(employer_id),
    date                VARCHAR(10) NOT NULL,
    paid                INTEGER NOT NULL
);
```

---

## 🚀 Getting Started

### Prerequisites

- **Java JDK** 8 or higher
- **PostgreSQL** installed and running
- **PostgreSQL JDBC Driver** — download from [jdbc.postgresql.org](https://jdbc.postgresql.org/download.html)
- An IDE such as Eclipse or IntelliJ IDEA

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/krraj4496-gif/employer-worker-registration-system-main.git
cd employer-worker-registration-system-main
```

**2. Set up PostgreSQL**

Create a new database and run all the SQL statements from the [Database Schema](#-database-schema) section above.

Then add at least one admin record manually:
```sql
INSERT INTO admin (username, password) VALUES ('admin', 'yourpassword');
```

**3. Configure the JDBC connection**

Open the database connection file in the source and update the connection string:
```java
// For PostgreSQL
DriverManager.getConnection(
    "jdbc:postgresql://localhost:5432/your_db_name",
    "postgres",
    "your_password"
);
```

> 💡 Want to use a different database? Just change the connection string to match your JDBC driver (e.g., `jdbc:mysql://...`).

**4. Add the JDBC driver to your classpath**

Download the PostgreSQL JAR from [jdbc.postgresql.org](https://jdbc.postgresql.org/download.html) and add it to your project's build path in your IDE.

**5. Build and run**

Run the main application class from your IDE, or compile from the command line:
```bash
javac -cp .;postgresql-xx.x.x.jar src/**/*.java
java  -cp .;postgresql-xx.x.x.jar Main
```

---

## 🏗️ Project Structure

```
employer-worker-registration-system-main/
├── src/
│   ├── component/       # Reusable UI components
│   ├── db/              # Database connection & query helpers
│   ├── entity/          # Data model classes (Employer, Worker, etc.)
│   ├── form/            # Swing form windows (Login, Home, Registration...)
│   └── Main.java        # Application entry point
├── bin/                 # Compiled class files
├── .classpath
├── .project
└── README.md
```

---

## 🛠️ Tech Stack

| Technology | Role |
|---|---|
| **Java** | Core language (100% of codebase) |
| **Java Swing** | Desktop GUI framework |
| **PostgreSQL** | Relational database |
| **JDBC** | Java–database connectivity layer |
| **Eclipse / IntelliJ** | Recommended IDEs |

---

## 🤝 Contributing

Contributions are welcome! Here's how to get involved:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/your-feature-name`
3. **Commit** your changes: `git commit -m "feat: add your feature"`
4. **Push** to your branch: `git push origin feature/your-feature-name`
5. **Open** a Pull Request — describe what you changed and why

Please keep code clean, comment meaningfully, and test your changes with a live PostgreSQL instance before submitting.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- Original project concept by [cbozan](https://github.com/cbozan/employer-worker-registration-system)
- PostgreSQL JDBC Driver — [jdbc.postgresql.org](https://jdbc.postgresql.org)
- Java Swing documentation — [docs.oracle.com](https://docs.oracle.com/javase/8/docs/technotes/guides/swing/)

---

<div align="center">

Made with ☕ Java and 🐘 PostgreSQL

*If this project helped you, consider giving it a ⭐ on GitHub!*

</div>
