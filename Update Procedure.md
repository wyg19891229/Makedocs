BCID 3.1.0 Update Procedure
===========================

>*Upgrading from BCID 3.0.2 to 3.1.0*

Deploy Admin Console & API Packages
-----------------------------------

>   *IMPORTANT*: Make sure you run a BACKUP of your Database using SQL Server Management Studio before proceeding! Some changes to the DB structure cannot be undone. Relevant data will also be lost but regenerated when mobile devices reconnect.

1.  Unpack API package to a new destination folder (e.g., **“/bioconnect_n3.1.0”**)
```
cd bioconnect_n/..
mkdir bioconnect_n3.1.0
cd bioconnect_n3.1.0
tar xf \<release package tar\>
```
>   *Note: Package unzips to ‘release’ in current directory so make sure not to overwrite the current release folder.*

2.  Confirm that **release/config/database.yml** and **release/config/puma.rb** in the new directory match the previous release or copy accordingly.

-   *Puma bind URI needs to be appropriate for localhost connection from NGINX bind*

-   *Database config should inherit all settings from environment variables*
```
cd app_path/../..
cp bioconnect_n/release/config/database.yml \\
bioconnect_n3.1.0/release/config/database.yml
cp bioconnect_n/release/config/puma.rb \\
bioconnect_n3.1.0/release/config/puma.rb
```
3.  Add environment variables for AWS:SNS to [.bashrc] and source it to the current shell

	*3.1.  Open the **.bashrc** in vi editor*
	```vi \~/.bashrc```

	*3.2.  Add these lines to the export section*
	```export BC_AWS_CRED_ID={to be provided}```
	```export BC_AWS_CRED_KEY={to be provided}```

	 *3.3. Save/exit the vi editor*
	```:wq```

	*3.4.  Source the **.bashrc** to the current shell*
	```source \~/.bashrc```

4.  Navigate to the new release directory
```
cd bioconnect_n3.1.0/release
```

5. Run bundler to install dependencies
```
bundle install
```

6.  Make sure proxy settings and added credentials are valid
```
bundle exec rails bc:utility:test_aws
```
7.  Verify migration status and confirm migrations that are currently down
```
bundle exec rails db:migrate:status
```
**We would expect the following migrations to be down**
```
down    20171120174030  Delete system admin
down    20171121185738  Sns application authenticator association
down    20171121191437  Remove notification service id from authenticator
down    20180115181453  Add match score to user verification
down    20180123154240  Add resolved at time stamp to user verification
```
8.  Identify Puma process ID and kill the process
```
ps -efH \| grep puma
kill {pid}
```
>   *IMPORTANT*: At this point you must have completed your database backup or you will lose the opportunity to do so after this step.

9.  Update the database schema
```
bundle exec rails db:migrate
```
>   *Note: This reports changes that it makes to the database*

10.  Start the Puma process
```
bundle exec puma -d -C config/puma.rb
```
11.  Backup HTTP folder
```
mkdir \~/admin_console3.0.1
cp -r /var/www/html/\* \~/admin_console3.0.1
cd /var/www/html/
tar xf admin_console
```
12.  Configure the {endpoint_url} by editing **\<bundle.js\>**
``` 
sudo vi /var/www/html/bundle.js
```
13.  Type "/endpoint_url" to find the appropriate location in the file and edit the string between the quotation marks

>*Note: NGINX should NOT require a restart after making the change.*

14.  Perform a step-up scenario action with a push notification to test that the system is working as intended.


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0Nzg0NjI0XX0=
-->