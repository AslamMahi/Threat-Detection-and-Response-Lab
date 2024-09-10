# Splunk Enterprise Setup Guide

This guide provides instructions for setting up Splunk Enterprise using Docker Compose. Splunk Enterprise is a powerful data analysis and visualization tool for searching, monitoring, and analyzing machine data.

## Prerequisites

- Docker and Docker Compose should be installed and running on your system.

## Installation Steps

1. **Clone the `splunk-compose` Repository**

    Clone the repository that contains the `splunk-compose` Docker Compose file:

    ```bash
    git clone https://github.com/splunk/splunk-compose.git
    cd splunk-compose
    ```

2. **Review the `docker-compose.yml` File**

    Open the `docker-compose.yml` file to review or modify the configuration as needed:

    ```bash
    nano docker-compose.yml
    ```

    Make any necessary changes to environment variables or settings, such as the `SPLUNK_PASSWORD` for setting your desired admin password.

3. **Start Splunk Enterprise**

    Use Docker Compose to start Splunk Enterprise and any associated services:

    ```bash
    docker-compose up -d
    ```

    This command starts Splunk Enterprise in detached mode.

4. **Verify Splunk Enterprise is Running**

    Check the status of the Splunk Enterprise containers to ensure they are running:

    ```bash
    docker-compose ps
    ```

    Ensure the Splunk containers are listed and their status is `Up`.

5. **Access Splunk Enterprise**

    Open your web browser and navigate to [http://localhost:8000](http://localhost:8000) to access the Splunk Web UI. Log in with the default username and password set in your `docker-compose.yml` file:

    - **Username**: `admin`
    - **Password**: (Configured in `docker-compose.yml`)

6. **Navigate to Splunk Enterprise Dashboard**

    Once logged in, you will see the Splunk Enterprise dashboard. 

7. **Check Daily Usage Limits**

    Navigate to `Settings` > `Licensing` in the Splunk Web UI. Here you will see the daily usage limit, which includes up to 500 MB of data per day with the free license. This allows you to explore and test Splunk Enterprise without a full commercial license.

8. **Install Splunk Universal Forwarder**

    To set up the Splunk Universal Forwarder on a Windows machine, refer to our separate guide for installation steps. This will involve:

    - Downloading the Splunk Universal Forwarder from the Splunk website.
    - Installing the forwarder on the Windows machine.
    - Configuring it to send data to your Splunk Enterprise instance.

## Additional Configuration

- **Configuration Changes**: Edit the `docker-compose.yml` file to adjust environment variables or other settings as needed. For instance, you can update `SPLUNK_PASSWORD` to set your admin password.

- **Data Persistence**: Ensure that Docker volumes are set up in the `docker-compose.yml` file to persist your Splunk data.

