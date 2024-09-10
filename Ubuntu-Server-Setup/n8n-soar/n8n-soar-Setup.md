# n8n SOAR Installation Guide

This guide will help you set up **n8n SOAR** using Docker and Docker Compose. Follow the steps below to install and run n8n using the provided `docker-compose.yml` file.

---

## Prerequisites

Make sure the following tools are installed on your system:

- **Docker**: Install from [Docker's official website](https://docs.docker.com/get-docker/).
- **Docker Compose**: Install from [Docker Compose installation guide](https://docs.docker.com/compose/install/).


### Step 1: Clone the Repository

Clone the repository containing the docker-compose.yml file for n8n setup:

```bash
git clone <repository-url>
cd <repository-folder>
```

### Step 2: Review and Modify docker-compose.yml

Make sure the docker-compose.yml file is properly configured. It should look something like this:

```yml
version: "3"

services:
  n8n:
    image: n8nio/n8n
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=sqlite
      - DB_SQLITE_DATABASE=/root/n8n/database.sqlite
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=<username>
      - N8N_BASIC_AUTH_PASSWORD=<password>
    volumes:
      - ./n8n:/root/n8n
    restart: always
```
Ports: Ensure port 5678 is open on your machine to access n8n.
Environment variables:

    Set your preferred username and password for basic authentication.
    Change the database location if necessary.

### Step 3: Run Docker Compose

Once you're in the directory with the docker-compose.yml file, run the following command to start n8n:
```bash
docker-compose up -d
```
-d runs the containers in the background (detached mode).

### Step 4: Access n8n SOAR

Once the containers are up, you can access the n8n dashboard by navigating to the following address in your browser:

```bash
http://localhost:5678
```
Log in with the username and password you set in the docker-compose.yml file.

### Step 5: Verify the Installation

To check the status of your n8n container, run:

```bash
docker ps
```
You should see the n8n container listed and running.

### Step 6: Stopping the Containers

To stop the n8n SOAR instance, run:

```bash
docker-compose down
```
This will stop and remove the running containers. If you wish to remove persistent data as well, manually delete the ./n8n folder that holds the database files.

### Troubleshooting

n8n is not accessible: Ensure that port 5678 is open and not blocked by any firewall or other service.

Check container logs: Run the following to view logs for troubleshooting:

```bash
docker-compose logs
```
