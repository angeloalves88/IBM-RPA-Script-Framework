<p align="right">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

# Execution Architecture and Error Handling

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> <br /><br />

## Execution Architecture 	
	
Now that I've explained the database and monitoring, let's check out how I organize my script and how I handle the failure scenario.
	
Each project has its particularity, this model is ideal for scenarios that can reprocess the item that failed at the beginning. That is, no matter where it broke, go back to the beginning and try again.
	
	This framework is ideal for Projects that use the IBM RPA Process Orchestrator Feature. 
	That when presenting failure in the execution, retry, instead of for the execution of the instance in the first failure.
	

	
![image](https://user-images.githubusercontent.com/46223364/197346753-387ed76d-c8d5-4022-87ff-1d9828b32428.png)
        
`[Initialize]` - Routine responsible for loading and assigning all the values used by the script during execution:   
- Get Parameters;
- Get Credentials;
- Definitions of variables;
- Connections to external services (email, queue providers, database);
- Validation of resources needed to run the script:
- 	Network paths;
- 	Files;
- 	between others.
     
![image](https://user-images.githubusercontent.com/46223364/197346890-7d6a5493-4dc7-4ab2-8754-323062acff8c.png)

        
`[Execute]` - Routine responsible for containing the entire process execution structure.
        
![image](https://user-images.githubusercontent.com/46223364/197347369-15b7c32a-4716-4039-84db-ed1770c02e03.png)
 
## Error Handling
	
`[__ErrorHandling]` - Routine responsible for performing error handling   
- Capture screenshot and save
- Capture the error message and write it to the log
- Clear the log error variables
- Restart a system, for example, close and open the application/browser, call the routine that logs in,...
- Validate if less than 3 failure attempts occurred
        - If yes, call the Execute routine again
        - otherwise, terminate the failed execution

![image](https://user-images.githubusercontent.com/46223364/197346987-9ff09e46-2067-4cae-aa96-38b2643fd85d.png)



:small_blue_diamond: [Back to Readme](https://github.com/angeloalves88/IBM-RPA-Script-Template/blob/main/README.md)	
	
	
	
	
