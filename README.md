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
- Download a Linux distribution (Preferably Ubuntu 24.04) from this (official linux download site)[https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions].
- Restart the EC2 machine and boot again.
## Set up Airflow Directory
- Open command prompt as administrator mode and create a new root directory, go into the directory.

```bash
mkdir 
