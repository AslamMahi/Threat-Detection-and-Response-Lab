# Cribl Setup Guide

This guide provides instructions for setting up Cribl using Docker Compose. Cribl is a powerful log management and observability platform that helps you route, shape, and enrich your data streams.

## Installation Steps

1. **Navigate to the Directory Containing `cribl-docker-compose.yml`**

    ```bash
    cd /path/to/your/cribl-docker-compose-directory
    ```

2. **Start Cribl Using Docker Compose**

    ```bash
    docker-compose -f cribl-docker-compose.yml up -d
    ```

    This command will start the Cribl services in detached mode.

3. **Verify Cribl is Running**

    ```bash
    docker-compose -f cribl-docker-compose.yml ps
    ```

    Ensure all Cribl containers are listed and have a status of `Up`.

4. **Access Cribl**

    Open your web browser and go to [http://localhost:9000](http://localhost:9000) to access the Cribl UI.

5. **Stop Cribl**

    ```bash
    docker-compose -f cribl-docker-compose.yml down
    ```

    This command will stop and remove the Cribl containers while preserving data volumes.

6. **Remove Cribl**

    ```bash
    docker-compose -f cribl-docker-compose.yml down -v
    ```

    This will stop, remove the containers, and delete the associated volumes.

## Additional Configuration

- **Modify Configuration**: Edit the `cribl-docker-compose.yml` file if you need to adjust configuration settings.

- **Data Persistence**: Ensure data volumes are correctly set up in the Docker Compose file to persist your data.

For more details, consult the [Cribl documentation](https://docs.cribl.io/).
