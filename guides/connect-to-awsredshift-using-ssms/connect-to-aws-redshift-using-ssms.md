# Connect to AWS Redshift using SQL Server Management Studio (SSMS)

This guide walks through connecting and querying AWS Redshift data using SQL Server Management Studio.

## Pre-requisites

This guide assumes that you have:
- A supported Windows powered computer
- Installed SQL Server Management Studio (SSMS)


## Making the connection

#### 1. Download & Install the ODBC Driver
First you need to [download the 32 or 64-bit ODBC driver from Amazon's website](http://docs.aws.amazon.com/redshift/latest/mgmt/install-odbc-driver-windows.html)

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide01.png)

Next, install the downloaded ODBC Driver by following the instructions on the screen.

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide02.png)

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide03.png)

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide04.png)

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide05.png)


#### 2. Setup the ODBC Connection

Open the ODBC Data Sources for the driver version installed by clicking on the Start-menu icon and typing "ODBC".

Click on *ODBC Data Sources*

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide06.png)


Click on the System DSN tab. You will see a sample connection that got setup with installing the ODBC driver above. Ignore this for now and click *Add...*

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide07.png)


Ensure *Amazon Redshift* is selected and click *Finish*

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide08.png)


Fill out the ODBC Driver DSN Setup per provided instructions:
- Data Source Name: `CompanyDW` (or name of your choice - just keep it handy)
- Server: `provided`
- Port: `5439` (or provided)
- Database: `dw` (or provided)
- User: `provided`
- Password: `provided`
- SSL Authentication: `require`
- Additional Options: `Single Row Mode`

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide09.png)

Before clicking *OK* you can test connection by clicking *Test*. If all works you should see a message like the one below.

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide10.png)

Once saved, ensure your see your new connection in the System DSN list as per below:

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide11.png)


#### 3. Connect using SSMS by setting up Linked Server

Open SSMS and connect to a voluntary server that will be used for setting up a linked connection. If none exist we suggest just using your localhost:

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide12.png)

Let's prepare a tad for querying. Expand `Server Objets > Linked Servers > Providers` and right click on *MSDASQL*. Click *Properties*

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide13.png)

Ensure the following options are checked and click *OK*
- Nested queries
- Level zero only
- Allow inprocess
- Support 'Like' operator

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide14.png)


Now, right-click on *Linked Servers* and click *New Linked Server...*

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide15.png)


Fill out the information as seen below:
- Linked Server: `COMPANYDW` (or name of choice - just keep handy and simple)
- Server Type: Select `Other data source`
- Provider: Select `Microsoft OLE DB Provider for ODBC Drivers`
- Product name & Data Sources: `CompanyDW` (or whatever name specified earlier - this is important for Data Sources!)

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide16.png)


Click on *Security* on the left-hand bar and ensure *Be made using the login's current security context* is specified. Click *OK*.

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide17.png)


#### 4. Make your first query!

Click on the *New Query* button whith your new Linked Server selected. Queries are supported using OPENQUERY so you will need to defined your queries as follows:

`SELECT * FROM OPENQUERY(<LINKED-SERVER-NAME>, 'SELECT * FROM <database>.<schema>.<table name>')`

   ![](https://github.com/seibelsbi/sbi/blob/master/guides/connect-to-awsredshift-using-ssms/img/guide18.png)

