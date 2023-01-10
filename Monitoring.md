<p align="right">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

# Monitoring

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> 

<p align="justify">
   	In IBM RPA there are several ways to have an execution monitoring log, among them the most structured is to have a database to be used in projects. But sometimes we come across companies that do not have a database available for the use of IBM RPA, due to their internal policies. Because of this scenario in this document, I will demonstrate how to use a local database using [Sqlite](https://www.sqlite.org). If you have a database instance available, just change the database connection command. <br />
	Another advantage of using SQLite is that IBM RPA already has a command to create the database file with the table structure. It is only necessary to install an SQLite reader to monitor the records.
</p>

## Monitoring

Below we can see a screenshot of the log table of the records that were successfully executed. We can see in the `REGISTERID` column the number of the record that is being processed, so we can easily separate the log blocks of each processed item.
	
<h5><completed></h5>

![image](https://user-images.githubusercontent.com/46223364/197344997-38cf2d4b-d54c-49e3-9cbe-f22f91fbb342.png)

In this, we can already see the cases of failures, where the error columns are filled. Facilitating the location of the error by the monitoring team.
	
<h5><failed></h5>
	
![image](https://user-images.githubusercontent.com/46223364/197345234-a5b97305-078f-4aeb-b623-ba4d4fbbcb8c.png)

As simple as this log table structure is, it is very useful for the daily monitoring of projects, making it easier to find errors and identify the process that presented the failure, in addition to having the error message, line, and screen screenshot on the error time.	

	
:small_blue_diamond: [Back to Readme](https://github.com/angeloalves88/IBM-RPA-Script-Template/blob/main/README.md)
	
	
