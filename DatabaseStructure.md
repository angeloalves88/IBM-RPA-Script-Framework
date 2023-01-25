<p align="right">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=UNDER%20DEVELOPMENT&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

# Database Structure

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> 

## Database structure 

As the idea of this material is to have something simple and easy to implement for new customers adhering to IBM RPA, I`ll only be using a log table. This table can be used by more than one project/script due to Project and Script columns.
Below is the table creation query.

```
CREATE TABLE IBMRPA_LOG (
    ID              INTEGER       PRIMARY KEY AUTOINCREMENT,		/* record identifier */
    LOGDATE         DATETIME      NOT NULL,				/* log date and time */
    PROJECT         INTEGER       NOT NULL,				/* project name */
    SCRIPT          INTEGER       NOT NULL,				/* script name*/
    LOGTYPE         VARCHAR       NOT NULL,				/* log type INFO/WARN/ERROR*/
    REGISTERID      VARCHAR,						/* identifier of the register being executed */
    MESSAGE         VARCHAR (255) NOT NULL,				/* message to be logged */
    ERRORSUBROUTINE VARCHAR,						/* In case of error: The routine that had an error */
    ERRORLINE       INTEGER,						/* In case of error: The line that had an error */
    ERRORMESSAGE    VARCHAR,						/* In case of error: The error message*/
    PATHSCREENSHOT  VARCHAR						/* In case of error: The path where the screenshot was saved*/
);
```

## Script structure

I have the "RegisteringLog" routine responsible for registering all messages in the DB, basically it gets the current date/time, connects to the DB and executes the query with the data received in the subroutine call, and disconnects from the DB.

<h5>Script</h5>

```
beginSub --name __RegisteringLog
	replaceText --texttoparse "${_logMessage}" --textpattern "\'" _logMessage=value
	getCurrentDateAndTime --localorutc "LocalTime" dateTimeNow=value
	sqliteConnect --connectionString "Data Source=${pathDataBase};Version=3;" conBd=connection success=success
	sqlExecute --connection ${conBd} --statement "INSERT INTO IBMRPA_LOG (\r\n   LOGDATE, PROJECT, SCRIPT,\r\n   LOGTYPE,REGISTERID,MESSAGE,\r\n   ERRORSUBROUTINE,ERRORLINE,\r\n   ERRORMESSAGE,PATHSCREENSHOT\r\n)\r\nVALUES (\r\n   \'${dateTimeNow}\',\'${_project}\',\'${_script}\',\r\n   \'${_logType}\',\'${_registerId}\',\'${_logMessage}\',\r\n   \'${_logErrorSubRoutine}\',\'${_logErrorLine}\',\r\n   \'${_logErrorMessage}\',\'${_logPathScreenshot}\'\r\n);" insertedRows=value
	sqlDisconnect --connection ${conBd}
endSub
```
	
<h5>Designer</h5>
	
![image](https://user-images.githubusercontent.com/46223364/196575429-69d51812-0465-481a-bd8e-75276de4e147.png)


In this image, we can visualize the query and the script variables that will be inserted in the table.

	
![image](https://user-images.githubusercontent.com/46223364/196576959-43c0dcce-bd38-42d8-b2ca-0a747c541e55.png)

# Variables
	
- `${dateTimeNow}` > Obtained by the command 'Get current date and time' every time the routine is executed
- `${_project}` and `${_script}` > Defined in the Initializing routine

	![image](https://user-images.githubusercontent.com/46223364/196580625-f0e4354c-9357-4a09-b624-f9cd547a620f.png)

- `${_logType}`, `${_registerId}`, and `${_logMessage}` > Informed in the command 'Run SubRoutine(goSub)'

	![image](https://user-images.githubusercontent.com/46223364/196580558-cbf7acc1-add6-4d95-8a05-829b5c1e501f.png)

- `${_logErrorSubRoutine}`, `${_logErrorLine}`, `${_logErrorMessage}`, and `${_logPathScreenshot}` > Informed in the command 'Run SubRoutine(goSub)' from routine __ErrorHandling

	![image](https://user-images.githubusercontent.com/46223364/196580398-0cad3d16-3076-4104-95aa-d03e23c9856e.png)
	
	
:small_blue_diamond: [Back to Readme](https://github.com/angeloalves88/IBM-RPA-Script-Template/blob/main/README.md)	
