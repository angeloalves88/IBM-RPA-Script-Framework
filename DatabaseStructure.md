
# Database and Script Structure

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> 

<p align="center">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

## Estrutura Data Base 

Como a ideia deste primeiro material é ter algo simples e de facil implementação para novos empresas que estão aderindo ao IBM RPA, estaremos utilizando apenas uma tabela de log. Está tabela pode ser utilizada por mais de um projeto/script por conta das colunas para aplicar filtros.
Abaixo esta a query de criação da tabela, 

```
CREATE TABLE IBMRPA_LOG (
    ID              INTEGER       PRIMARY KEY AUTOINCREMENT,		/* identificador do registro */
    LOGDATE         DATETIME      NOT NULL,				/* data e hora do registro*/
    PROJECT         INTEGER       NOT NULL,				/* nome do projeto*/
    SCRIPT          INTEGER       NOT NULL,				/* nome do script*/
    LOGTYPE         VARCHAR       NOT NULL,				/* tipo do log INFO/WARN/ERROR*/
    REGISTERID      VARCHAR,						/* identificador do registro que esta sendo executado*/
    MESSAGE         VARCHAR (255) NOT NULL,				/* mensagem para ser registrada*/
    ERRORSUBROUTINE VARCHAR,						/* Em caso de erro: A rotina que apresentou erro*/
    ERRORLINE       INTEGER,						/* Em caso de erro: A linha que apresentou erro*/
    ERRORMESSAGE    VARCHAR,						/* Em caso de erro: A mensagem do erro*/
    PATHSCREENSHOT  VARCHAR						/* Em caso de erro: O caminho que foi salvo o screenshot*/
);
```

## Estrutura do Script  

Temos a rotina "RegisteringLog" que é responsável por registrar todas mensagem no DB, basicamente obtem a data/hora atual, conecta no DB e executa a query com os dados recebidos na chamada da sub rotina e desconecta do DB.

<h5><script></h5>

```
beginSub --name __RegisteringLog
	replaceText --texttoparse "${_logMessage}" --textpattern "\'" _logMessage=value
	getCurrentDateAndTime --localorutc "LocalTime" dateTimeNow=value
	sqliteConnect --connectionString "Data Source=${pathDataBase};Version=3;" conBd=connection success=success
	sqlExecute --connection ${conBd} --statement "INSERT INTO IBMRPA_LOG (\r\n   LOGDATE, PROJECT, SCRIPT,\r\n   LOGTYPE,REGISTERID,MESSAGE,\r\n   ERRORSUBROUTINE,ERRORLINE,\r\n   ERRORMESSAGE,PATHSCREENSHOT\r\n)\r\nVALUES (\r\n   \'${dateTimeNow}\',\'${_project}\',\'${_script}\',\r\n   \'${_logType}\',\'${_registerId}\',\'${_logMessage}\',\r\n   \'${_logErrorSubRoutine}\',\'${_logErrorLine}\',\r\n   \'${_logErrorMessage}\',\'${_logPathScreenshot}\'\r\n);" insertedRows=value
	sqlDisconnect --connection ${conBd}
endSub
```
	
<h5><designer></h5>
	
![image](https://user-images.githubusercontent.com/46223364/196575429-69d51812-0465-481a-bd8e-75276de4e147.png)


Nesta imagem podemos visualizar a query e as variaveis do script que serão inseridos na tabela.

	
![image](https://user-images.githubusercontent.com/46223364/196576959-43c0dcce-bd38-42d8-b2ca-0a747c541e55.png)

	
- dateTimeNow > Obtem o valor toda a vez que é executado a rotina
- _project and _script > Definido na rotina Initializing

	![image](https://user-images.githubusercontent.com/46223364/196580625-f0e4354c-9357-4a09-b624-f9cd547a620f.png)

- _logType, _registerId and _logMessage > Informado no comando 'Run SubRoutine(goSub)'

	![image](https://user-images.githubusercontent.com/46223364/196580558-cbf7acc1-add6-4d95-8a05-829b5c1e501f.png)

- _logErrorSubRoutine, _logErrorLine, _logErrorMessage, and _logPathScreenshot > Informado no comando 'Run SubRoutine(goSub)' da rotina __ErrorHandling

	![image](https://user-images.githubusercontent.com/46223364/196580398-0cad3d16-3076-4104-95aa-d03e23c9856e.png)
	
	
	
	
```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```
