# Sysmon Installation and Log Forwarding to Elasticsearch and Splunk

This guide covers the installation of Sysmon on Windows, configuring it to send logs to Elasticsearch and Splunk. 

## Prerequisites

- Administrative privileges on the Windows machine.
- Sysmon executable from the [Sysinternals website](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon).
- `sysmonconfig-research.xml` configuration file from the [Sysmon Modular GitHub repository](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig-research.xml).
- Splunk Universal Forwarder installed and configured.
- Elastic Agent installed and configured (for Elasticsearch).

## Step 1: Install Sysmon

1. **Download Sysmon**

    - Visit the [Sysinternals Sysmon download page](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon).
    - Download the Sysinternals Suite or the standalone Sysmon executable.
    - Extract the downloaded ZIP file to a directory on your machine.

2. **Open Command Prompt as Administrator**

    - Right-click on the Command Prompt icon and select "Run as administrator."

3. **Install Sysmon with Configuration File**

    - Change to the directory where you extracted Sysmon:
      ```cmd
      cd path\to\sysinternals
      ```

    - Install Sysmon using the following command:
      ```cmd
      sysmon -accepteula -c path\to\sysmonconfig-research.xml
      ```

    - Replace `path\to\sysmonconfig-research.xml` with the path to the configuration file you downloaded.

4. **Verify Sysmon Installation**

    - Check if Sysmon is running:
      ```cmd
      sc query sysmon
      ```

    - Verify Sysmon logs by checking the Event Viewer:
      - Open Event Viewer (Run `eventvwr.msc`).
      - Navigate to `Applications and Services Logs` > `Microsoft` > `Windows` > `Sysmon` > `Operational`.

## Step 2: Forward Sysmon Logs to Elasticsearch

1. **Configure Elastic Agent for Sysmon**

    - Ensure Elastic Agent is installed on your machine. If not, follow the [Elastic Agent installation guide](https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html).

    - Open the `elastic-agent.yml` configuration file, typically located at:
      ```plaintext
      C:\Program Files\Elastic Agent\elastic-agent.yml
      ```

    - Add the following configuration to collect Windows event logs, including Sysmon:
      ```yaml
      inputs:
        - type: winlog
          paths:
            - C:\Windows\System32\winevt\Logs\Microsoft-Windows-Sysmon%4Operational.evtx
      ```

    - Save and close the `elastic-agent.yml` file.

2. **Restart Elastic Agent**

    - Restart the Elastic Agent service to apply changes:
      ```cmd
      net stop elastic-agent
      net start elastic-agent
      ```

## Step 3: Forward Sysmon Logs to Splunk

1. **Configure Splunk Universal Forwarder**

    - Open the `inputs.conf` file for the Splunk Universal Forwarder, typically located at:
      ```plaintext
      C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
      ```

    - Add the following configuration to collect Sysmon logs:
      ```ini
      [WinEventLog://Microsoft-Windows-Sysmon/Operational]
      disabled = 0
      index = main
      ```

    - Save and close the `inputs.conf` file.

2. **Restart Splunk Universal Forwarder**

    - Restart the Splunk Universal Forwarder service to apply changes:
      ```cmd
      net stop splunkforwarder
      net start splunkforwarder
      ```

## Step 4: Verify Log Forwarding

### Verify in Elasticsearch

1. **Log in to Kibana**

    - Open your web browser and navigate to Kibana (e.g., `http://localhost:5601`).

2. **Check Sysmon Logs**

    - Go to `Discover` and search for Sysmon logs:
      ```plaintext
      @timestamp: * AND event.module: sysmon
      ```

    - Verify that Sysmon logs are appearing as expected.

### Verify in Splunk

1. **Log in to Splunk Enterprise**

    - Open your web browser and navigate to your Splunk Enterprise instance (e.g., `http://<SPLUNK_HOST>:8000`).

2. **Check Sysmon Logs**

    - Go to `Search & Reporting` and run a search query:
      ```spl
      index="main" sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational"
      ```

    - Verify that Sysmon logs are appearing in the search results.

## Summary

1. **Install Sysmon**: Download, install, and configure Sysmon with `sysmonconfig-research.xml`.
2. **Forward Logs to Elasticsearch**: Configure Elastic Agent to collect and send Sysmon logs.
3. **Forward Logs to Splunk**: Configure Splunk Universal Forwarder to collect and send Sysmon logs.
4. **Verify**: Ensure Sysmon logs are being collected and displayed in Elasticsearch and Splunk.

For more details, refer to the [Sysmon documentation](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon), [Elastic Agent documentation](https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html), and [Splunk Universal Forwarder documentation](https://docs.splunk.com/Documentation/Forwarder/latest/Forwarding/).


