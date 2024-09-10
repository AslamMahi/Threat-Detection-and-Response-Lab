# Grafana Installation Guide

Grafana is a popular open-source tool for visualizing time-series data and creating dashboards. This guide provides steps to install Grafana using Docker.

## Prerequisites

- Docker and Docker Compose should be installed and running on your system.

## Installation Steps

1. **Create a `docker-compose.yml` File**

    Create a directory for your Grafana setup, and within that directory, create a `docker-compose.yml` file with the following content:

    ```yaml
    version: '3'

    services:
      grafana:
        image: grafana/grafana:latest
        container_name: grafana
        ports:
          - "3000:3000"
        volumes:
          - grafana-data:/var/lib/grafana
        environment:
          - GF_SECURITY_ADMIN_PASSWORD=admin

    volumes:
      grafana-data:
    ```

    This configuration sets up Grafana and exposes it on port 3000. It also uses a Docker volume to persist Grafana data.

2. **Start Grafana**

    Navigate to the directory containing your `docker-compose.yml` file and run:

    ```bash
    docker-compose up -d
    ```

    This command will start Grafana in detached mode.

3. **Verify Grafana is Running**

    Check the status of the Grafana container to ensure it is running:

    ```bash
    docker-compose ps
    ```

    Ensure the Grafana container is listed and its status is `Up`.

4. **Access Grafana**

    Open your web browser and navigate to [http://localhost:3000](http://localhost:3000). You should see the Grafana login page.

    - **Username**: `admin`
    - **Password**: `admin` (or the password you set in the `GF_SECURITY_ADMIN_PASSWORD` environment variable)

    Log in to access the Grafana dashboard.

5. **Change Default Admin Password**

    For security reasons, it's recommended to change the default admin password:

    - After logging in, click on your user profile in the lower-left corner of the Grafana UI.
    - Select `Change Password` and follow the prompts to set a new password.

6. **Stop Grafana**

    To stop the Grafana container, use:

    ```bash
    docker-compose down
    ```

    This command will stop and remove the Grafana container while preserving the data volume.

7. **Remove Grafana**

    If you need to remove Grafana completely, including the data volume:

    ```bash
    docker-compose down -v
    ```

    This command stops and removes the container and deletes the associated volume.

## Additional Configuration

- **Configuration Changes**: You can further customize Grafana by modifying its configuration file or environment variables. Refer to the [Grafana Docker documentation](https://grafana.com/docs/grafana/latest/installation/docker/) for more details.

- **Data Persistence**: The `grafana-data` volume in the `docker-compose.yml` file ensures that your Grafana data is persistent across container restarts.


