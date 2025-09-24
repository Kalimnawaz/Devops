TASK:

The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. 
The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

PostgreSQL database server is already installed on the Nautilus database server.
a. Create a database user kodekloud_aim and set its password to Rc5C9EyvbU.
b. Create a database kodekloud_db6 and grant full permissions to user kodekloud_aim on this database.

Note: Please do not try to restart PostgreSQL server service.

Step 1: Switch to PostgreSQL user

On most Linux systems, the default PostgreSQL superuser is postgres. So first:

sudo -i -u postgres


Now you are the postgres user.

Step 2: Access PostgreSQL prompt
psql


This will open the PostgreSQL interactive prompt: postgres=#

Step 3: Create database user

Run the following command:

CREATE USER kodekloud_aim WITH PASSWORD 'Rc5C9EyvbU';


✅ This will create the user with the specified password.

Step 4: Create database
CREATE DATABASE kodekloud_db6;

Step 5: Grant full privileges to user on the database
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db6 TO kodekloud_aim;

Step 6: Verify

To check user creation:

\du


To check database creation:

\l


You should see kodekloud_aim as a role and kodekloud_db6 as a database with full access granted.

Step 7: Exit PostgreSQL
\q

Step 8: Exit postgres user shell
exit


Here’s a single-line command that does everything for Q17 without entering the interactive psql shell:

sudo -i -u postgres psql -c "CREATE USER kodekloud_aim WITH PASSWORD 'Rc5C9EyvbU';" \
-c "CREATE DATABASE kodekloud_db6;" \
-c "GRANT ALL PRIVILEGES ON DATABASE kodekloud_db6 TO kodekloud_aim;"

After running this, you can verify with:

sudo -i -u postgres psql -c "\du"
sudo -i -u postgres psql -c "\l"
