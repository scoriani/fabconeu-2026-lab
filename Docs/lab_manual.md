
# Table of contents

1.[Part 0 - Sign in to Azure and setup Azure resources](#part-0---sign-in-to-azure-and-setup-azure-resources)
2. [Part 1 - Connect to your Azure HorizonDB database using VS Code Extension for PostgreSQL](#part-1---connect-to-your-azure-horizondb-database-using-vs-code-extension-for-postgresql)
    1. [Open VS Code and set up database connection to Azure PostgreSQL](#open-vs-code-and-set-up-database-connection-to-azure-postgresql)
    2. [Create Connection](#create-connection)
    3. [Explore VS Code Extension for PostgreSQL Dashboard](#explore-vs-code-extension-for-postgresql-dashboard)    
3. [Part 2 and 3 - Data Setup and Agentic App Development](#part-2-and-3---data-setup-and-agentic-app-development)

===

# Part 0 - Sign in to Azure and setup Azure resources
In this section, we'll open Edge browser in the lab environment and sign in to the Azure portal to review the Azure resources we will use in this lab.

1. Double-select on the **Microsoft Azure Portal** icon on the desktop.


2. Once logged in to the Azure portal landing page, select **View all resources**.


3. Observe the two Azure resources, we'll be using these during the course of this lab:
    - Azure OpenAI instance
    - Azure HorizonDB database instance

===

In this section, we are going to allow-list the Postgres extensions we are going to use in this lab by creating a **Parameter Group** for HorizonDB.

Parameter groups are a new concept in HorizonDB.  Parameter groups act as containers for cluster configuration values that can be applied to one or more database clusters. Instead of managing configuration settings for each cluster individually, you can define them in a parameter group and connect that group with multiple clusters to ensure consistency across your environment.  In our case, we only have one cluster in our lab, however, we need to still create a parameter group to enable the Postgres extensions we are going to use in our lab.

Parameter groups are first-class resources in Azure and are surfaced within the specific resource group and subscription defined in their resource identifier.

1. Click on the HorizonDB instance in the portal


2. Expand **"Settings"** in the left navigation, and click **"Parameters"**


3. Click the **"create"** link at the top of the page


4. For **"Parameter group name"** enter **"lab"**, then click **Next**


5. In the top filter bar type **"exten"** to bring **"azure.extensions"** to the top parameter to edit it

    1. Click the drop down box next to **"azure.extensions"** and select the following extensions we are going to use in this lab:
    
	> age, pg_diskann, pg_textsearch, vector, azure_ai

	2. Ensure your selections looks like the screen shot below.


6. Again in the top filter bar, now type **"shared_pre"** to bring **"shared_preload_libraries"** to the top parameter to edit it

	1. Click the drop down box next to **"shared_preload_libraries"**, and select the extensions **"age"** and **"pg_textsearch"**
    
	2. Then click **"Create"**


===

# Part 1 - Connect to your Azure HorizonDB database using VS Code Extension for PostgreSQL

## Open VS Code and set up database connection to Azure PostgreSQL

1. Go to your desktop and double select the **VS Code** icon to open VS Code on your lab VM.


2. Once inside VS Code, you should be already in the **"C:\\Lab"** folder.  If not, select **File** > **Open Folder...** > Choose **"C:\\Lab"** to open this folder into your workspace


3. Now, in the **"LAB"** folder, look for a **".env"** file and double click to open it	

4. With the **".env"** file open, let's look at some of the variables defined.  This file contains all the credentials needed to connect to the Azure OpenAI and Azure HorizonDB instances that were deployed during the creation of this lab.  Most of these credentials we will not need to copy/paste as we will programmatically load them into our code notebook in a later step in this lab.

	But for the next couple steps, we will use the following values of these variables to copy/paste to make our connection to the HorizonDB database from within VS Code:

	- AZURE_PG_HOST
    - AZURE_PG_USER
    - AZURE_PG_PASSWORD

5. In the next few steps, we are going to use the VS Code Extension for PostgreSQL to add a connection to our HorizonDB database. Leave the **".env"** file open, we will use it in the next few steps. On the left navigation, select the **elephant** icon.


===

## Create Connection

1. Once the extension loads, in the **POSTGRESQL** panel select the **Add Connection** button.


2. Fill out the connection form with the following values"

	- For **SERVER NAME**, copy/paste the **AZURE_PG_HOST** value from the `.env` file
        - Example: **horizondb-lab-australiaeast-czhpjspykdk4q.e557d0d51d1e.australiaeast.horizondb.azure.com**
    - For **AUTHENTICATION TYPE**, choose **Password** *(Note: Entra ID is coming soon for HorizonDB)*
    - For **USER NAME**, type **labUser**
    - For **PASSWORD**, copy/paste the **AZURE_PG_PASSWORD** value from the `.env` file
        - Example: **Zcohzrudys5q3e!**
    - For **DATABASE**, leave blank
    - For **CONNECTION NAME**, type **lab**


3. Next, click **"Test Connection"**, and you should see a green check box appear.
	
    > **Note:** during the lab creation process we automatically allow-listed this VM's IP address to allow connections into your instance of HorizonDB. In the future, you will need to ensure you take this step to open access to connect to your HorizonDB database either directly with a query editor tool, or programmatically.


4. Lastly, click **"Save & Connect"** to save the connection and open the connection to the HorizonDB database


**Congratulations, you just signed in to your Azure HorizonDB database using the VS Code Extension for PostgreSQL!**

===

## Explore VS Code Extension for PostgreSQL Dashboard

1. Now that we have our connection created, let's explore the VS Code Extension for PostgreSQL and our HorizonDB database.  First, right-click on your **"lab"** connection we just created, and select the "Dashboard" option from the context menu:


2. When the Dashboard loads, you will see it provides a robust set of performance details such as **wait events, disk i/o, transactions, storage, and more**.

3. To continue exploring the VS Code Extension for PostgrSQL, now expand the **"Databases"** node under the **"lab"** connection.  Look for the **"postgres"** database, right-click it and select **"New Query"** from the context menu.

4. Now run the following query by copying and pasting the following SQL block into the query editor window, then click the green play arrow on the top right to execute the SQL statement.  The purpose of this SQL query is just to illustrate the process of running queries and seeing results using the VS Code Extension for PostgreSQL.

	During the course of this lab most SQL queries will be ran programmatically via Python code and the pscyopg Python package.  However, there are a few queries you will need to run using the query editor in the VS Code Extension, so stay tuned for those!

	```SQL

    SELECT
    	current_database() AS database_name,
        current_user AS connected_user,
        now() AS server_time,
        version() AS postgres_version;
    ```

===

# Part 2 and 3 - Data Setup and Agentic App Development

For the remainder of the lab we are going to work from two Jupyter Python Notebooks within VS Code.  All further lab instructions will be in-line within each notebook. The first notebook (Notebook 1) is a data setup notebook and the second notebook (Notebook 2) is the agentic application development notebook.

These notebooks are both located in the **"C:\\Lab"** folder structure under the folder **"Code"**:

- **1-data-setup.ipynb** (Notebook 1)
- **2-app-development.ipynb** (Notebook 2)

Additionally, there is a third, optional notebook, which is a diagnostics notebook for ensuring server settings and configurations:

- **3-diagnostics.ipynb** (Notebook 3)

## Open Notebook 1 - Data Setup

1. Within VS Code, on the left navigation bar, select the **Explorer** icon to return to the **Explorer** view.


2. Expand the **Code** folder and look for a file name **1-data-setup.ipynb** (Notebook 1), then double-click the file.

3. This will open the first notebook. Read each section of the notebook and follow the in-line instructions.

4. Once you complete Notebook 1, return to the **Code** folder and open the second notebook with the file name **2-app-development.ipynb** (Notebook 2).  Again, follow the in-line instructions and that will complete the lab.

	>[!alert] At this point, continue the lab following the instructions in the Notebook 1 in VS Code.