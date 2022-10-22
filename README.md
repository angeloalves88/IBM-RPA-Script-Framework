
# Template para desenvolvimento

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> 

<p align="center">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>


### Tópicos 

:small_blue_diamond: [Descrição do projeto](#descrição-do-projeto)

:small_blue_diamond: [Funcionalidades](#funcionalidades)

:small_blue_diamond: [Deploy da Aplicação](#deploy-da-aplicação-dash)

:small_blue_diamond: [Pré-requisitos](#pré-requisitos)

:small_blue_diamond: [Como rodar a aplicação](#como-rodar-a-aplicação-arrow_forward)

... 


## Introdução 

<p align="justify">
   Quando uma empresa inicia a sua Jornada de Automação de Processos, normalmente ela não possui documentação de boas práticas para ser seguida como padrão dos projetos a serem implementados, muito menos um script base, com uma estrutura inicial criada para o inicio do desenvolvimento dos seus bots.<br />  
	Neste documento vou demonstrar e disponibilizar um modelo de script para ser utilizado como base dos seus bots, com tratamento de erro, um log de monitoramento local, e vou aproveitar para falar do meu padrão de nomenclatura de scripts, rotinas, e variaveis. 
</p>

## Monitoramento

<p align="justify">
   	No IBM RPA existe várias formas de ter um log de monitoramento da execução, dentre elas a mais estruturada é ter um banco de dados para ser utilizado nos projetos. Mas as vezes nos deparamos com empresas aonde não tem uma base de dados disponível para o uso do IBM RPA, por conta de suas políticas internas. Por conta deste cenário neste documento vou demonstrar como utilizar uma base de dados local utilizando o Sqlite. Caso tenha uma instância de banco de dados disponível, basta apenas trocar o comando de conexão com o banco de dados. <br />
	Outra vantagem do uso do SQLite, é que o IBM RPA já tem um comando para criar o arquivo do database, e a estratura da tabela. Sendo necessário a instalação apenas de um leitor de SqLite para acompanhamento dos registros.
</p>

<h5><designer></h5>
	
![image](https://user-images.githubusercontent.com/46223364/197344583-f2d167f6-fe7d-49aa-a956-7924ab76ab36.png)


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

## Monitoramento

Abaixo podemos ver um screenshot da tabela de log dos registros que executaram com sucesso.
	
<h5><completed></h5>

![image](https://user-images.githubusercontent.com/46223364/197344997-38cf2d4b-d54c-49e3-9cbe-f22f91fbb342.png)

Nesta já podemos ver os casos de falhas, aonde as colunas de erro são preenchidas.
	
<h5><failed></h5>
	
![image](https://user-images.githubusercontent.com/46223364/197345234-a5b97305-078f-4aeb-b623-ba4d4fbbcb8c.png)

Por mais simples que seja esta estrutura de tabela do log, ela é muito util para o acompanhamento diario dos projetos, facilitando encontrar erro e idêntificar o processo que apresentou a falha, além de ter a mensagem de erro, linha e o screenshot da tela no momento do erro.	
	
## Estrutura do Tratamento de erro 	
	
Agora que temos a base de dados e o rastreamento, vamos verificar como eu trato o cenário de falha. 
	
Cada projeto tem sua particularidade, este modelo é ideal para cenários que pode reprocessar o item que apresentou falha do inicio. Ou seja, não importa aonde quebrou, volta no inicio e tenta novamente.
	
	Esta estrutura é ideal para Projetos que utiliza a Feature do Orquestrador de Processos do IBM RPA. 
	Que ao apresentar falha na execução, retente, ao invés de para a execução da instância na primeira falha.
	

	
![image](https://user-images.githubusercontent.com/46223364/197346753-387ed76d-c8d5-4022-87ff-1d9828b32428.png)
        
[Initialize] - Rotina responsavel por carregar e atribuir todos os valores utlizados pelo script:   
- Obter Parametros;
- Definição de variáveis;
- Conexões com serviços externos (e-mail, provedores de fila, banco de dados);
- Criação de recursos necesários para execução do script;
     
![image](https://user-images.githubusercontent.com/46223364/197346890-7d6a5493-4dc7-4ab2-8754-323062acff8c.png)

        
[Execute] - Rotina responsável por conter toda a estrutura de exeucção do processo.
        
![image](https://user-images.githubusercontent.com/46223364/197347369-15b7c32a-4716-4039-84db-ed1770c02e03.png)
  
	
[__ErrorHandling] - Rotina responsável para realizar o tratamento de erro   
- Capturar a imagem da tela e salvar   
- Capturar a mensagem de erro e escrever no log
- Limpa as variaveis de erro do log
- Reiniciar algum sistema, por exemplo fechar e abrir o aplicação/navegador, chamar a rotina que faz login, ....
- Validar se ocorreu menos de 3 tentativas de falha
        - Se sim, chama a rotina Execute novamente
        - senão, finaliza a execução com falha

![image](https://user-images.githubusercontent.com/46223364/197346987-9ff09e46-2067-4cae-aa96-38b2643fd85d.png)

	
	
	
	
	
	
	
	
	
	
:warning: 	
	
:heavy_check_mark:	
	
:memo: Tarefa 3 	
	
- [React PDF](https://react-pdf.org/)	
	
	
=====


## Licença 

The [MIT License]() (MIT)

Copyright :copyright: Ano - Titulo do Projeto
