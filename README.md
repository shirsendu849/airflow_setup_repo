# Overview
Apache Airflow is an open-source workflow orchestration tool. In this repository we will be go through with the steps to set up airflow in a AWS EC2 machine. We will install airflow using PyPI airflow package.

# Prerequisites
- AWS free tier account
- Knowledge about setting up and spin up AWS EC2
- Basic knowledge of SQL
- PgAdmin (GUI tool for Postgtres) setup
- Shell commands

# Installation Process
## Spin up AWS EC2
- Log into the AWS console and create a new EC2 instance based on Windows Server 2019, choose all the configuration under free tier to avoid unnecessary cost.
- Spin up the EC2 instance and connect EC2 in local machine using RDP connection.
## Install WSL
Apache airflow natively supports Linux/ Debian environment, so we need to install WSL on top of Windows to get the required environment.
- In EC2 open Power Shell in administrator mode.
- Execute this command to enable Windows Subsystem for Linux (WSL) feature.
  
  ```bash
  Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
- Download a Linux distribution (preferably Ubuntu 24.04) from this [official site](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions).
- Go to the downloads folder in windows file system and open power shell at this directory. Paste this command to rename the downloaded .zip file as Ubuntu and then unzip it.

  ```bash
  Rename-Item .\ Ubuntu_2404.0.5.0_x64.appx .\Ubuntu.zip
  
  Expand-Archive .\Ubuntu.zip .\Ubuntu
- Navigate to the **.\Ubuntu_2404.0.5.0_x64.appx** file and execute this command to install Ubuntu from the .appx file.

   ```bash
  Add-AppxPackage .\Ubuntu_2404.0.5.0_x64.appx
- Add WSL path to the windows environment variable.
  
  ```bash
  $userenv = [System.Environment]::GetEnvironmentVariable("Path", "User")[System.Environment]::SetEnvironmentVariable("PATH", $userenv + ";C:\Users\Administrator\Ubuntu", "User")
- Find an application for **Ubuntu 24.04 LTS** that provides a shell interface for accessing the operating system. When you open the shell for the first time, set up a username and password as part of the 
 initial configuration.

## Virtual Environment setup
- **Python 3.12** is pre-installed in **Ubuntu 24.04 LTS**, open ubuntu shell and paste this command to install PIP, check the version.

  ```bash
  sudo apt-get install python3-pip
  
  pip --version
- Run the following command to install a package that creates an isolated Python virtual environment for the Airflow application, ensuring there are no package conflicts.

  ```bash
  sudo apt-get install python3-venv
- Create a python virtual environment and activate it.

  ```bash
  python3 -m venv airflow_env
  
  source airflow_env/bin/activate

## Airflow Root Directory Setup
- Create a new directory named **airflow** and get into the folder.

  ```bash
  mkdir airflow
  
  cd airflow
- Open the **.bashrc** file and add the following line to update the airflow root directory in the bash profile.

  ```bash
  nano ~/.bashrc
  
  AIRFLOW_HOME=~/airflow
- Save and exit the file and check if the environment variable works or not. Write following command that should automatically navigate to the airflow root directory from any location.

  ```bash
  cd $AIRFLOW_HOME

## Airflow Package Installation
- To install Apache Airflow with the async, postgres, google, and snowflake extras execute this command
  
  ```bash
  AIRFLOW_VERSION=2.10.4
  PYTHON_VERSION="$(python -c 'import sys; print(f"{sys.version_info.major}.{sys.version_info.minor}")')"
  CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
  pip install "apache-airflow[async,postgres,google,snowflake]==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
- Check there is any broken package or not. Now gets all the dependent packages names into the txt file.
  
  ```bash
  pip check
  
  pip freeze > requirements.txt

## Metadata DB Setup
- Install postgres from the [official site](https://www.postgresql.org/download/windows/) and open **PgAdmin**.
- Open new sql editor and fire this queries one by one to create new metadata database for airflow, attach new role and set permission for accessing metadata DB.
  
  ```bash
  CREATE DATABASE airflow_db WITH ENCODING=’ UTF8’;;

  CREATE USER airflow_user WITH PASSWORD 'airflow_pass';

  GRANT ALL PRIVILEGES ON DATABASE airflow_db TO airflow_user;

  GRANT ALL ON SCHEMA public TO airflow_user;
  
## Airflow Configuration
- Now open maximize the ubuntu shell and trigger this command to get airflow config file and metadata database instance
  
  ```bash
  airflow db migrate
- Install postgres package to establish sql alchemy connection.

  ```bash
  pip install psycopg2-binary
  
- Open the airflow.cfg file using the nano editor and locate the sql_alchemy_conn variable in the [database] section. Airflow uses SQLite as the default metadata database. To switch to a new database, comment 
  out the existing SQLite connection. Then, update the sql_alchemy_conn variable with the new connection string in the following format, replacing it with the database, username, and password you set up earlier.

  ```bash
  nano airflow.cfg

  #sql_alchemy_conn = sqlite:////home/mjunctionetl/airflow/airflow.db
                                                                                                                                               
  sql_alchemy_conn = postgresql+psycopg2://airflow_user:airflow_pass@127.0.0.1:5432/airflow_db
- Set **load_examples** variable from **True** to **False** so that it avoids unnecessary examples load in the **Airflow UI**.
- Set **executor = LocalExecutor** instead of **SequentialExecutor** for better parallel task execution. 
- Now save the airflow.cfg file and back to the shell interface.
- Again run following command to migrate metadata database from Sqlite to Postgres
  
  ```bash
  airflow db migrate
  
## Airflow User Creation & Login
- Execute the following command to create a new user with Admin privilege.

  ```bash
  airflow users create –-username airflow
  --password airflow --firstname airflow
  --lastname user --role Admin
  --email admin@email.com
- Now run the airflow scheduler

  ```bash
  airflow scheduler
- Open another ubuntu shell, activate virtual environment, navigate root directory and run the airflow webserver

  ```bash
  cd $AIRFLOW_HOME

  source airflow_env/bin/activate

  airflow webserver
- Now you can paste this URL in browser and can access the Airflow UI http://localhost:8080



  
   

