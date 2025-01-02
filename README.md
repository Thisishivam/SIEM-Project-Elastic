# A Simple Elastic SIEM Lab
This project demonstrates a simple SIEM (Security Information and Event Management) setup using an AWS EC2 Linux instance. The setup utilizes the Elastic Stack to monitor and analyze system logs in real-time, providing valuable insights into system activity and potential security incidents.

In this project I have leveraged AWS for generating and collecting the logs but I could have also used VirtualBox or VMware. However most of the tech companies rely on the cloud so this is more focused for cloud security.

## Features

- AWS EC2 Instance: Hosts a Linux environment for generating and collecting logs.
- Elastic Stack: Processes and visualizes logs, including Elasticsearch for indexing and Kibana for analysis.
- Log Monitoring: Real-time tracking of system events and activities.

## Prerequisites

- VirtualBox/VMware or Cloud platform like AWS, GCP, Azure
- Basic knowledge of Linux and virtualization software.

## Overview of the tasks
- Set up a free Elastic account.
- Set up AWS EC2 instance or you can use Linux VM
- Configure the Elastic Agent on the AWS instance to collect the logs and forward it to the SIEM.
- Generate security events on the Linux instance.
- Query to find the security events in the Elastic SIEM.
- Create a Dashboard to visualize security events.
- Create alerts for security events.

## Task 1: Set up an Elastic Account
Before we get started, we need to create a free account to set up a cloud Elastic instance that we can run the SIEM on. To do that, follow these steps:

- Sign up for a free trial to use Elastic Cloud at https://cloud.elastic.co/registration
- Once you have an Elastic account, log in to the Elastic Cloud console at https://cloud.elastic.co.
- Click on “Start your free trial.”
- Click on the “Create Deployment” button and select “Elasticsearch” as the deployment type.
- Choose a region and deployment size that fits your needs and click on “Create Deployment.”
- Wait for the configuration to complete.
- Once the deployment is ready, click “continue.”
  
![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Create-Deployment.png)

## Task 2: Setting up the Agent to Collect Logs
An agent is a software program that is installed on a device, such as a server or endpoint, to collect and send data to a centralized system for analysis and monitoring. In the context of Elastic SIEM, an agent is used to collect and forward security-related events from your endpoints to your Elastic SIEM instance.

1. Log in to your Elastic SIEM instance and navigate to the Integrations page by: clicking on the Kibana main menu bar at the top left, then selecting “Integrations” at the bottom left from Management dropdown menu.

2. Search for “Elastic Defend” and click on it to open the integration page.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Add-Integration.png)

3. Click on “Install Elastic Defend” and follow the instructions provided on the integration page to install the agent on your Linux instance.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Download-Elastic-agent.png)

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/ElasticAgent-Command.png)

4. Paste that command into the terminal (command line).

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/cloud-elastic-install.png)

5. Once the agent is installed, which can take a few minutes, you’ll see a message that says “Elastic Agent has been successfully installed.” It will automatically start collecting and forwarding logs to your Elastic SIEM instance, although it might take a few minutes for the logs to appear in the SIEM.

6. You can verify that the agent has been installed correctly by running this command: sudo systemctl status elastic-agent.service

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/elastic-agent-running.png)

## Task 3: Generating Security Events on the instance

To verify that the agent is working correctly, you can generate some security-related events on your Linux instance. To do this, we can install any package on the instance like apache2 and start the service onto the machine so that we can monitor the incoming logs in the Elastic.
- To install and start Apache2 on the machine simple run.
  ```bash
    sudo apt install apache2 -y
    sudo systemctl start apache2
  ```

  ![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/installing-apache.png)

  ![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/start-apache.png)

  ## Task 4: Querying for Security Events in the Elastic SIEM
  
  Now that we have forwarded data from the Linux instance to the SIEM, we can start querying and analyzing the logs in the SIEM.
  
- To do this, follow these steps:
1. Inside your Elastic Deployment, click on the menu icon at the top-left with the three horizontal lines and then click on the “Discover” tab under “Observability” to view the logs from the Linux instance.
 
  ![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/logs-captured.png)

2. You can add filters like "process.command_line" on the left side of the logs to view what activity was logged by the Elastic i.e what command was runned from the Linux instance.
 
  ![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/log-apache2.png)

  By generating and analyzing different types of security events in Elastic SIEM like the one above, or generating authentication failures by typing in the wrong password for a user or attempting SSH logins an incorrect password, you can gain a better understanding of how security incidents are detected, investigated, and responded to in real-world environments.

## Task 5: Create a Dashboard to Visualize the Events
You can also use the visualizations and dashboards in the SIEM app to analyze the logs and identify patterns or anomalies in the data. For example, you can create a simple dashboard that shows a count of security events over time.

- Here’s how you can do that:

1. Navigate to the Elastic web portal at https://cloud.elastic.co/.
2. Click on the menu icon on the top-left, below “Discover” click on “Dashboards.”

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/creating-dashboard.png)

3. Click on the “Create dashboard” button on the top right to create a new dashboard.
4. Click on the “Create Visualization” button to add a new visualization to the dashboard.
5. Select “Area” or “Line” as the visualization type, depending on your preference. This will create a chart that shows the count of events over time.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Dashboard-parameters.png)

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Dashboard-param1.png)

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Dashboard-param2.png)

6. Click on the “Save” button to save the visualization and then complete the rest of the settings.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Dashboard.png)

## Task 6: Create an Alert

In a SIEM, alerts are a crucial feature for detecting security incidents and responding to them in a timely manner. Alerts are created based on predefined rules or custom queries, and can be configured to trigger specific actions when certain conditions are met. In this task, we will walk through the steps of creating an alert in the Elastic SIEM instance to detect Nmap scans. By following these steps, you can create an alert that will monitor your logs for Nmap scan events and then notify you when they are detected.

- Here are the steps:

1. Click on the menu icon on the top-left, below “Dashboard” click on “Alerts.”
2. Click on “Manage rules” at the top right.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/creating-alerts.png)

3. Click on the “Create new rule” button at the top right.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/create-rule.png)

4. Select the rule type that you want to specify “here I have selected custom threshold to specify my custom query for the alert”

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Rule-type.png)

5. Under “Define query filter,” set the conditions for the rule. You can use the following query to detect Nmap scan events.
6. This query will match all events with the action “nmap_scan.” Then click “Continue.”
7. Under the “About rule” section, give your rule a name and a description (Nmap Scan Detection).

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/define-custome-query.png)

8. In the “Actions” section, select the action you want to take when the rule is triggered. You can choose to send an email notification, create a Slack message, or trigger a custom webhook.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/add-action.png)

In this project I have set the action to send email to my email address

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Added-Email-action.png)

9. Finally, click the “Create and enable rule” button to create the alert.

![Screenshot](https://github.com/Thisishivam/SIEM-Project-Elastic/blob/main/Alert-created.png)

Once you’ve created the alert, it will monitor your logs for Nmap scan events. If an Nmap scan event is detected, the alert will be triggered and the selected action will be taken. You can view and manage your alerts on the “Alerts”
