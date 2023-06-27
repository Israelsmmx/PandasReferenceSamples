# Pandas Reference & common functions

@autor   Israel Sanchez
@Last update   06/24/2023
@tested on     docker containers running over windows

## Requisites (windows)

-  Docker Desktop
-  WSL 2 installed
-  Docker hub account
-  Internet access
-  Energy to start :) lol

## Preparatives with Docker Containers

### Jupyter with Python
    
        docker run -p 8888:8888 --name jupyter -v "${PWD}":/home/jovyan/work jupyter/datascience-notebook

### SQL Server

        docker run --name mssql2022 -v "${PWD}":/tmp -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=M1dNigt3ss" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest

SQL Server SA user pass:   M1dNigt3ss   -- Remember this

![image](https://github.com/Israelsmmx/PandasReferenceSamples/assets/84999244/6b307258-6be5-483d-8c6f-19d23d6ec7e0)

-------------------------------------------------------------------------------------

### Preparatives for SQL SErver after startup

* Download adventureworks BAK file from Microsoft repo

    <https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms

    ![image](https://github.com/Israelsmmx/PandasReferenceSamples/assets/84999244/0eb84ccf-0fee-47ad-a5e8-be7a86cd3e38)
    
* Check the container ID in

          docker ps  

    krump@KrumbaRumba:~/code/$ **docker ps**
    CONTAINER ID   IMAGE                                        COMMAND                  CREATED        STATUS                    PORTS                    NAMES
    9fba1dd731fd   jupyter/datascience-notebook                 "tini -g -- start-no…"   18 hours ago   Up 23 minutes (healthy)   0.0.0.0:8888->8888/tcp   jupyter
    **17c83a1327c9**   mcr.microsoft.com/mssql/server:2022-latest   "/opt/mssql/bin/perm…"   18 hours ago   Up 23 minutes             0.0.0.0:1433->1433/tcp   mssql2022

    

    
* Copy the AdventureWorks .bak file to the container

          docker cp AdventureWorksDW2022.bak 17c83a1327c9:/tmp
  
* Use SQL Server client list MSSQL Studio or Azure Data Explorer to restore AdventureWorksDW2022 from .BAK File located on /tmp/AdventureWorksDW2022.bak

### Preparatives for Jupyter Container to install SQL SERVER ODBC Libraries 


            sudo docker exec -it --user root jupyter bash

#### in jupyter container as root run the next code (looks to many lines I'm not expert but this install ODBC Client 18) & sync the dependencies locally
        
        apt update -y
        apt upgrade -y
        apt install gpg -y
        #Download the desired package(s)
        curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/msodbcsql18_18.2.2.1-1_amd64.apk
        curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/mssql-tools18_18.2.1.1-1_amd64.apk
        #(Optional) Verify signature, if 'gpg' is missing install it using 'apk add gnupg':
        curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/msodbcsql18_18.2.2.1-1_amd64.sig
        curl -O https://download.microsoft.com/download/1/f/f/1fffb537-26ab-4947-a46a-7a45c27f6f77/mssql-tools18_18.2.1.1-1_amd64.sig
        curl https://packages.microsoft.com/keys/microsoft.asc  | gpg --import -
        apt update -y
        gpg --verify msodbcsql18_18.2.2.1-1_amd64.sig msodbcsql18_18.2.2.1-1_amd64.apk
        gpg --verify mssql-tools18_18.2.1.1-1_amd64.sig mssql-tools18_18.2.1.1-1_amd64.apk
        apt update -y
        apt upgrade -y
        apt-get install apt-file -y
        apt-file update
        apt upgrade -y
        apt-file find msodbcsql18_18.2.2.1-1_amd64.apk
        apt-file find mssql-tools18_18.2.1.1-1_amd64.apk
        #Install the package(s)
        apt install -y unixodbc-dev -y
        apt install -y unixodbc -y
        apt update -y
        apt upgrade -y
        apt install gpg -y
        sudo apt install --reinstall software-properties-common -y
        apt-get install odbcinst -y
        sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
        echo "deb [arch=amd64] https://packages.microsoft.com/ubuntu/21.10/prod impish main" | sudo tee /etc/apt/sources.list.d/mssql-release.list
        sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/22.04/prod.list)"
        apt update -y 
        sudo ACCEPT_EULA=Y apt-get install -y msodbcsql18
        apt install msodbcsql18 -y

-------------------------------------------------------------------------------------


# Common LIbraries 


## Azure (future samples)
        pip install azure-core
        pip install azure-mgmt-compute
        pip install azure-mgmt-containerservice
        pip install azure-mgmt-containerinstance
        pip install azure-storage-blob
        pip install azure-keyvault
        pip install azure-mgmt-storage
        pip install azure.storage.blob
        pip install azure-storage-file-share
        pip install azure-storage-common
        pip install azure-mgmt-datalake-store
        pip install azure-mgmt-databricks
        pip install azure-data-tables
        pip install azure-mgmt-synapse
        pip install azure-mgmt-monitor



## Misc 
        pip install imageio
        pip install prophet


        pip install xgboost
        pip install matplotlib
        pip install seaborn
        pip install sklearn

## SQL / Hana / pandas
        pip install sqlalchemy
        pip install pyodbc 
        pip install pandas
        pip install pyarrow
        pip install sqlalchemy-hana
        pip install hdbcli

## Spark (future samples)
        pip install pyspark
        pip install ipython-sql
        pip install sparksql-magic
        pip install delta-spark


----------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------



# Initial libraries as extentions

- Cargar_datos_excel_pandas.ipynb

# Sample files for most common functions with Pandas


- 000 datos_faltantes.ipynb
- 000 filtrar_datos_dataframes.ipynb
- 000 series_de_pandas.ipynb
- 0001 dataframes_de_pandas.ipynb
- 001 tutorial-limpieza-de-datos.ipynb
- 002 tutorial-analisis-exploratorio-de-datos.ipynb
- 0_FunctionIncludes.ipynb
- 500 Ejercicio Distribution info SAP TABLES.ipynb
- 500 Ejercicio Optimize DataFrameSize.ipynb
- 500 Sample load from CSV and optimization of memory.ipynb

    Optimize the use of memory
  
- 501 Conectar con SQL Server.ipynb
- 502 Pandas DataFrame JOINS.ipynb

    joins dataframes using merge functionallity from pandas
  
- 503 frequently algoritms used on pandas.ipynb
- 600 SQL Server Adventure Works SAMPLES.ipynb
- 602 SQL SERVER ADVENTURE ALL Process.ipynb

    read tables from SQL Server and execute filters and merges
  
- 991_Extraer datos de SAPHana.ipynb



----------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------

