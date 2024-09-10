# ELK Stack Setup Guide

This guide provides instructions for setting up the ELK Stack (Elasticsearch, Logstash, and Kibana) using Docker and the `elastic-container.sh` script from the [elastic-container](https://github.com/peasead/elastic-container) repository.

## Prerequisites

- **Operating System**: Linux, macOS, or Windows 10/11 with WSL 2
- **Required Tools**: Docker, Docker Compose, `jq`, `curl`, `git`

### Installation Instructions

#### macOS

1. **Install Required Tools**

    Install the tools using Homebrew:

    ```bash
    brew install jq git curl docker-compose
    brew install --cask docker
    ```

2. **Configure Docker**

    Open Docker and follow the prompts:

    ```bash
    open /Applications/Docker.app
    ```

    - Confirm opening the app.
    - Allow Docker privileged access.
    - Enter your password if prompted.
    - Close or minimize Docker after setup.

#### Ubuntu

1. **Install Required Tools**

    ```bash
    sudo apt-get update
    sudo apt-get install jq git curl
    ```

2. **Install Docker**

    Follow the [Docker installation instructions for Ubuntu](https://docs.docker.com/engine/install/ubuntu/). Ensure you install `docker-compose-plugin` instead of `docker-compose`.

#### CentOS/Fedora/Rocky/RHEL

1. **Install Required Tools**

    ```bash
    sudo dnf install jq git curl
    ```

2. **Install Docker**

    Follow the [Docker installation instructions for CentOS/Fedora/Rocky/RHEL](https://docs.docker.com/engine/install/centos/). Ensure you install `docker-compose-plugin`.

#### Other Linux Distributions

1. **Install Required Tools**

    Use your distribution’s package manager to install `jq`, `git`, and `curl`. For Arch Linux users, also install `inetutils` and adjust the script as needed.

2. **Install Docker**

    Follow the [Docker installation instructions](https://docs.docker.com/engine/install/). Ensure you install `docker-compose-plugin`.

#### Windows 10/11 with WSL 2

1. **Ensure WSL 2**

    Confirm WSL version 2 is in use:

    ```powershell
    wsl -l -v
    ```

    If necessary, set WSL 2 for Ubuntu 20.04:

    ```powershell
    wsl --set-version Ubuntu-20.04 2
    ```

2. **Install Required Tools**

    ```bash
    sudo apt-get update
    sudo apt-get install jq git curl
    ```

3. **Install Docker**

    Follow the [Docker installation instructions for WSL 2](https://docs.docker.com/desktop/install/windows-install/). Start Docker:

    ```bash
    sudo service docker start
    ```

## Setup Steps

1. **Clone the Repository**

    Clone the repository to your local machine:

    ```bash
    git clone https://github.com/peasead/elastic-container.git
    cd elastic-container
    ```

2. **Configure Environment Variables**

    - Open the `.env` file in a text editor.
    - **Change Default Passwords**: Update the default password (`changeme`) for `ELASTIC_PASSWORD` and `KIBANA_PASSWORD`. The `elastic` username should remain unchanged.
    - **Enable Detection Rules (Optional)**: To enable pre-built detection rules by OS, set the corresponding values to `1` in the `.env` file:

      ```plaintext
      LinuxDR=0
      WindowsDR=1
      MacOSDR=0
      ```

3. **Make the Script Executable**

    ```bash
    chmod +x elastic-container.sh
    ```

4. **Start the ELK Stack**

    Execute the script to start the ELK stack:

    ```bash
    ./elastic-container.sh start
    ```

5. **Access the ELK Stack**

    - Wait for the script to prompt you to browse to [https://localhost:5601](https://localhost:5601).
    - You might encounter a browser warning due to self-signed certificates. Proceed by typing `thisisnotsafe` or accepting the risk to access the Elastic login screen.

## Usage

- **Default Credentials**: Username: `elastic`, Password: `changeme`. Update these credentials in the `.env` file as needed.
- **Note**: This setup is intended for local security research and should not be exposed to the Internet or used in a production environment.

### Managing the Stack

- **Start the Stack**

    ```bash
    ./elastic-container.sh start
    ```

- **Stop the Stack**

    ```bash
    ./elastic-container.sh stop
    ```

- **Restart the Stack**

    ```bash
    ./elastic-container.sh restart
    ```

- **Destroy the Stack**

    ```bash
    ./elastic-container.sh destroy
    ```

- **Check Status**

    ```bash
    ./elastic-container.sh status
    ```

- **Clear Data**

    ```bash
    ./elastic-container.sh clear
    ```

- **Stage Images**

    ```bash
    ./elastic-container.sh stage
    ```

## Automating Agent Enrollment

To enroll an Elastic Agent:

1. **Get the Enrollment Token**

    Obtain the token from Kibana or via API:

    ```bash
    curl -k --request GET \
       --url 'https://<KIBANAHOST>:5601/api/fleet/enrollment_api_keys' \
       -u <USER>:<PASSWORD> \
       --header 'Content-Type: application/json' \
       --header 'kbn-xsrf: xx'
    ```

2. **Install the Elastic Agent**

    Use the token to install the Elastic Agent:

    ```powershell
    $ProgressPreference = 'SilentlyContinue'
    Invoke-WebRequest -Uri https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.12.2-windows-x86_64.zip -OutFile elastic-agent-8.12.2-windows-x86_64.zip
    Expand-Archive .\elastic-agent-8.12.2-windows-x86_64.zip -DestinationPath .
    cd elastic-agent-8.12.2-windows-x86_64
    .\elastic-agent.exe install --url=https://<FLEETHOST>:8220 --insecure -f --enrollment-token=<api_key>
    ```

## Additional Configuration

Edit the `.env` file for custom configurations:

- **Passwords and Versions**:

    ```plaintext
    ELASTIC_PASSWORD="changeme"
    KIBANA_PASSWORD="changeme"
    STACK_VERSION="8.14.0"
    ```

- **Change Stack Versions**: Adjust the versions of Elasticsearch, Kibana, and Elastic-Agent as needed.

## License

This project is licensed under the MIT License. See the [LICENSE](https://github.com/peasead/elastic-container/blob/main/LICENSE) file for details.

## Acknowledgments

- **ELK Stack**: Developed and maintained by Elastic.
- **Elastic-Container**: Created and maintained by [peasead](https://github.com/peasead). Special thanks for providing the `elastic-container.sh` script which simplifies the ELK stack setup process.

For further details, visit the [elastic-container GitHub repository](https://github.com/peasead/elastic-container).
