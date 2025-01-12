# Overview
Apache Airflow is a workflow orchestration tool. In this repository we will be go through with the steps to set up airflow in a AWS EC2 machine. We will install airflow using PyPI airflow package.

# Prerequisites
- AWS free tier account
- Knowledge about setting up and spin up AWS EC2, PostgreSQL
- Basic knowledge of SQL

# Installation Process
## Spin up AWS EC2
- Log in to the AWS console and create a new EC2 instance based on Windows Server 2025 Base, choose all the configuration under free tier.
- Spin up the EC2 instance and connect EC2 in local machine using RDP connection.
## Install WSL
- In EC2 open command prompt in administrator mode.
- Execute this command to install Windows Subsystem for Linux (WSL), we will install Ubuntu as a Linux distribution.
  
  ```bash wsl --install -d Ubuntu
