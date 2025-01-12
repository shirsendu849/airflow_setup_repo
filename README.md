# Overview
Apache Airflow is a workflow orchestration tool. In this repository we will be go through with the steps to set up airflow in a AWS EC2 machine. We will install airflow using PyPI airflow package.

# Prerequisites
- AWS free tier account
- Knowledge about setting up and spin up AWS EC2
- Basic knowledge of SQL
- PgAdmin (GUI tool for Postgtres) set up

# Installation Process
## Spin up AWS EC2
- Log in to the AWS console and create a new EC2 instance based on Windows Server 2019, choose all the configuration under free tier to avoid unnecessary cost.
- Spin up the EC2 instance and connect EC2 in local machine using RDP connection.
## Install WSL
Apache airflow natively supports Linux/ Debian environment, so we need to install WSL on top of windows to get the required environment.
- In EC2 open Power Shell in administrator mode.
- Execute this command to enable install Windows Subsystem for Linux (WSL) feature.
  
  ```bash
  Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
- Download a Linux distribution (preferably Ubuntu 24.04) from this [official site](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions).
- Go to the downloads folder in file system and open power shell in this directory. Paste this command to rename the package zipped as Ubuntu and then unzip it.

  ```bash
  Rename-Item .\ Ubuntu_2404.0.5.0_x64.appx .\Ubuntu.zip
  Expand-Archive .\Ubuntu.zip .\Ubuntu
- Navigate to the **.\Ubuntu_2404.0.5.0_x64.appx** file and install the Ubuntu from the .appx file using this command.

   ```bash
  Add-AppxPackage .\Ubuntu_2404.0.5.0_x64.appx
- Add WSL path to the windows environment variable.
  
  ```bash
  $userenv = [System.Environment]::GetEnvironmentVariable("Path", "User")[System.Environment]::SetEnvironmentVariable("PATH", $userenv + ";C:\Users\Administrator\Ubuntu", "User")
- Look for **Ubuntu 24.04 LTS** apllication that basically provide shell for accessing Ubuntu. Set up username and password when you are first time open the shell.
   

