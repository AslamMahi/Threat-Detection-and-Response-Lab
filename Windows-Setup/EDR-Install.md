# Installing Elastic Agent on Windows

Elastic Agent is a unified agent for data collection in your Elastic Stack environment. This guide provides the steps to install Elastic Agent on a Windows machine, including how to obtain the necessary enrollment token from Kibana and use the `--insecure` flag.

## Prerequisites

- An Elastic Stack (Elasticsearch and Kibana) instance should be running and accessible.
- Access to Kibana with appropriate permissions to enroll agents.

## Obtain Enrollment Token from Kibana

1. **Log in to Kibana**

    - Open your web browser and navigate to [http://localhost:5601](http://localhost:5601) (or your Kibana URL).
    - Log in with your credentials.

2. **Navigate to Fleet**

    - In the Kibana sidebar, go to `Fleet`. If you don’t see it, you might need to enable Fleet in Kibana or ensure you have the necessary permissions.

3. **Access Enrollment Tokens**

    - In the Fleet section, go to `Agents`.
    - Click on `Add Agent` or `Enroll Agent` to generate an enrollment token.
    - You will be presented with a command similar to the following:
      ```bash
      ./elastic-agent enroll --url=https://<FLEET_HOST>:8220 --enrollment-token=<ENROLLMENT_TOKEN> --insecure
      ```

    - Copy the `enrollment-token` and `FLEET_HOST` from the Kibana interface. 

## Installation Steps on Windows

1. **Download Elastic Agent**

    - Go to the [Elastic Agent downloads page](https://www.elastic.co/downloads/elastic-agent).
    - Download the appropriate version for Windows, e.g., `elastic-agent-<version>-windows-x86_64.zip`.

2. **Extract the Elastic Agent**

    - Open the downloaded `.zip` file.
    - Extract the contents to a directory of your choice, e.g., `C:\Program Files\Elastic\Agent`.

3. **Open Command Prompt as Administrator**

    - Right-click on the Command Prompt icon and select "Run as administrator."

4. **Navigate to the Elastic Agent Directory**

    ```cmd
    cd "C:\Program Files\Elastic\Agent"
    ```

5. **Install the Elastic Agent**

    Use the enrollment command obtained from Kibana, including the `--insecure` flag:

    ```cmd
    .\elastic-agent.exe install --url=https://<FLEET_HOST>:8220 --enrollment-token=<ENROLLMENT_TOKEN> --insecure
    ```

    Replace `<FLEET_HOST>` with your Kibana hostname and `<ENROLLMENT_TOKEN>` with the token obtained from Kibana.

6. **Verify the Elastic Agent Status**

    Check the status of the Elastic Agent to ensure it is running correctly:

    ```cmd
    .\elastic-agent.exe status
    ```

    You should see output indicating that the Elastic Agent is active and connected.

7. **Access Elastic Agent in Kibana**

    - Go back to Kibana.
    - Navigate to `Fleet` and then `Agents`.
    - You should see the new agent listed and its status.

8. **Uninstall the Elastic Agent (if needed)**

    To uninstall the Elastic Agent:

    ```cmd
    .\elastic-agent.exe uninstall
    ```

## Additional Configuration

- **Fleet Configuration**: Elastic Agent configuration is managed through the Fleet interface in Kibana. You can configure policies, integrations, and other settings from Kibana's Fleet section.
- **Logs**: Logs for the Elastic Agent can be found in the `logs` directory within the installation path. These logs are useful for troubleshooting.

## License

Elastic Agent is licensed under the [Elastic License](https://www.elastic.co/legal/elastic-license).

## Acknowledgments

- **Elastic Stack**: Developed and maintained by Elastic.
- **Elastic Agent**: Used for unified monitoring and security.

For more information, visit the [Elastic Agent documentation](https://www.elastic.co/guide/en/fleet/current/elastic-agent.html) and the [Elastic downloads page](https://www.elastic.co/downloads/elastic-agent).

Summary of Commands:

    Download: Get the latest Elastic Agent from the Elastic website.
    Extract: Unzip the file to a desired directory.
    Install: .\elastic-agent.exe install --url=https://<FLEET_HOST>:8220 --enrollment-token=<ENROLLMENT_TOKEN> --insecure
    Verify Status: .\elastic-agent.exe status
    Uninstall: .\elastic-agent.exe uninstall

This guide reflects the current management of Elastic Agent, focusing on using Fleet in Kibana for configuration and managing settings directly from the Kibana interface. 