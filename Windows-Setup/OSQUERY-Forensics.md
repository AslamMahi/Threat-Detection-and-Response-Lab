# OSQUERY-Forensics Installation Guide for Windows

## Introduction

This guide will walk you through the process of installing and configuring OSQUERY on a Windows machine for forensic purposes. OSQUERY is a powerful tool that allows you to perform SQL-based queries on your system to retrieve information about the state of the operating system and installed applications.

## Prerequisites

Before you begin, ensure that you have the following:

- A Windows machine with administrative privileges
- Internet access for downloading OSQUERY and dependencies
- Basic knowledge of command-line operations

## Installation Steps

### 1. Download OSQUERY

1. Visit the [OSQUERY releases page on GitHub](https://github.com/osquery/osquery/releases).
2. Locate the latest release for Windows.
3. Download the `.msi` installer package for the release.

### 2. Install OSQUERY

1. Run the downloaded `.msi` installer package.
2. Follow the prompts in the installation wizard.
   - Choose the installation directory (default is usually fine).
   - Select additional components if needed (e.g., OSQUERY daemon).

### 3. Verify the Installation

1. Open Command Prompt as an administrator.
2. Run the following command to check if OSQUERY is installed correctly:
```bash
osqueryi --version
```
3. You should see the OSQUERY version information displayed.

### 4. Configure OSQUERY

1. OSQUERY configuration files are located in the installation directory, typically under C:\Program Files\osquery\.
2. Edit the configuration file osquery.conf to suit your forensic needs. You can set up queries, logging, and other parameters.
3. For example, to enable logging, you can configure the following:

```json
{
  "options": {
    "log_results": true
  }
}
```
### 5. Run OSQUERY

1. To start OSQUERY, open Command Prompt as an administrator.
2. Run the OSQUERY interactive shell with the following command:

```bash
osqueryi
```
3. You can now run SQL queries to collect forensic data. For example:

```sql
SELECT * FROM processes;
```
### Useful Queries

Here are some example queries that can be useful for forensic analysis:

List all running processes:

```sql
SELECT name, pid, path FROM processes;
```
Retrieve information about installed software:

```sql
SELECT name, version FROM programs;
```
Check recent logon events:

```sql
SELECT * FROM logon_events;
```

## Troubleshooting

If you encounter any issues during installation or configuration:

Ensure that you have administrative privileges.
Verify that the OSQUERY service is running.
Check the OSQUERY logs for any error messages.

## Conclusion

You have successfully installed and configured OSQUERY on your Windows machine. You can now use it to perform detailed forensic analysis and retrieve system information using SQL queries.

For further information and advanced usage, refer to the official OSQUERY documentation.