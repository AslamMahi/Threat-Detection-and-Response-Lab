# Installing Splunk Universal Forwarder on Windows

Splunk Universal Forwarder (UF) is a lightweight version of Splunk Enterprise used to collect and forward log data to a Splunk indexer or a central Splunk deployment. This guide provides the steps to install, configure, and verify the Splunk Universal Forwarder on a Windows machine.

## Prerequisites

- A Splunk Enterprise or Splunk Cloud instance should be running and accessible.
- Administrative privileges on the Windows machine.

## Log In to Splunk Website

1. **Access the Splunk Website**

    - Go to the [Splunk login page](https://www.splunk.com/en_us/account.html).

2. **Log In**

    - Enter your Splunk account credentials (username and password).
    - Click `Sign In` to access your account.

## Download Splunk Universal Forwarder

1. **Go to the Splunk Downloads Page**

    - Visit the [Splunk Universal Forwarder download page](https://www.splunk.com/en_us/download/universal-forwarder.html).

2. **Download the Installer**

    - Select the version appropriate for Windows (e.g., `SplunkUniversalForwarder-<version>-x64-release.msi`).
    - Download the `.msi` installer file.

## Install Splunk Universal Forwarder

1. **Run the Installer**

    - Locate the downloaded `.msi` file.
    - Double-click on the `.msi` file to start the installation process.

2. **Follow the Installation Wizard**

    - Click `Next` on the welcome screen.
    - Accept the license agreement and click `Next`.
    - Choose the installation directory or leave the default and click `Next`.
    - Select the components to install and click `Next`.
    - For the `Splunkd` user credentials, choose `Local System` or provide specific user credentials as needed.
    - Click `Install` to start the installation.
    - Once the installation is complete, click `Finish`.

3. **Start the Splunk Universal Forwarder**

    - The Splunk Universal Forwarder service should start automatically. If it does not, you can start it manually:
      ```cmd
      net start SplunkForwarder
      ```

## Configure Splunk Universal Forwarder

1. **Open Command Prompt as Administrator**

    - Right-click on the Command Prompt icon and select "Run as administrator."

2. **Configure Forwarding to Your Splunk Instance**

    Use the following command to set up the forwarding to your Splunk indexer or central deployment:

    ```cmd
    "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add forward-server <SPLUNK_INDEXER>:9997
    ```

    Replace `<SPLUNK_INDEXER>` with the hostname or IP address of your Splunk indexer.

3. **Set Up Data Inputs**

    You may need to specify what data to collect. Here’s an example of how to monitor a directory:

    ```cmd
    "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add monitor C:\path\to\directory
    ```

    Replace `C:\path\to\directory` with the path of the directory you want to monitor.

4. **Restart the Splunk Universal Forwarder**

    After making configuration changes, restart the Splunk Universal Forwarder:

    ```cmd
    "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" restart
    ```

## Add the Forwarder to Splunk Enterprise

1. **Log In to Splunk Enterprise**

    - Open your web browser and navigate to your Splunk Enterprise instance (e.g., `https://<SPLUNK_ENTERPRISE>:8000`).
    - Log in with your Splunk credentials (default username: `admin`, password: `changeme`).

2. **Navigate to Data Inputs**

    - From the Splunk Enterprise home screen, go to `Settings` in the top menu.
    - Click on `Data Inputs` under the `Data` category.

3. **Add a New Data Input**

    - Click on `Add Data`.
    - Choose `Forwarded Data` from the available options.

4. **Configure the Forwarded Data**

    - In the `Add Data` screen, specify the details for the forwarder.
    - You can set up monitoring and indexing settings according to your needs.
    - Click `Next` and configure the source type, index, and any other required settings.

5. **Review and Save**

    - Review your configuration settings.
    - Click `Review` and then `Submit` to complete the process.

6. **Verify Data Collection**

    - Go to the `Search & Reporting` app in Splunk Enterprise.
    - Run a search query to check for data coming from the forwarder:
      ```spl
      index=* source="*"
      ```

    You should see data being indexed from your Universal Forwarder.

## Verify the Installation

1. **Check the Status**

    Verify that the Splunk Universal Forwarder is running correctly:

    ```cmd
    "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" status
    ```

    You should see output indicating that the Splunk Universal Forwarder service is running.

2. **Check the Logs**

    Review the log files to ensure that the Universal Forwarder is working as expected:

    - Navigate to the logs directory:
      ```cmd
      cd "C:\Program Files\SplunkUniversalForwarder\var\log\splunk"
      ```

    - View the latest log file (`splunkd.log`):
      ```cmd
      type splunkd.log
      ```

    Look for entries indicating successful connections to the indexer and data collection.

## Uninstall Splunk Universal Forwarder (if needed)

To uninstall the Splunk Universal Forwarder:

1. **Open Control Panel**

    - Go to `Control Panel` > `Programs and Features`.

2. **Uninstall the Program**

    - Find `Splunk Universal Forwarder` in the list.
    - Select it and click `Uninstall`.

3. **Follow the Uninstallation Wizard**

    - Follow the prompts to complete the uninstallation process.

For more information, visit the [Splunk Universal Forwarder documentation](https://docs.splunk.com/Documentation/Forwarding).

Summary of Commands:

Install: Run the .msi installer and follow the installation wizard.
Configure Forwarding: "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add forward-server <SPLUNK_INDEXER>:9997
Add Data Inputs: "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add monitor C:\path\to\directory
Restart: "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" restart
Check Status: "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" status
View Logs: type splunkd.log

This guide includes all necessary steps for installing and configuring the Splunk Universal Forwarder, as well as integrating it with Splunk Enterprise.