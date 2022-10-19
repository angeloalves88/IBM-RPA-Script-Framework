# IBM-RPA
IBM Robotic Process Automation

<h1>Modelo base de Script</h1> 
<h3>Contém tratamento de erro e base de dados para log</h3> 

<p align="center">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

> Status do Projeto: :heavy_check_mark: :warning: em desenvolvimento

### Tópicos 

:small_blue_diamond: [Descrição do projeto](#descrição-do-projeto)

:small_blue_diamond: [Funcionalidades](#funcionalidades)

:small_blue_diamond: [Deploy da Aplicação](#deploy-da-aplicação-dash)

:small_blue_diamond: [Pré-requisitos](#pré-requisitos)

:small_blue_diamond: [Como rodar a aplicação](#como-rodar-a-aplicação-arrow_forward)

... 

Insira os tópicos do README em links para facilitar a navegação do leitor

## Introdução 

<p align="justify">
   Quando uma empresa inicia a sua Jornada de Automação de Processos, normalmente ela não possui documentação de boas práticas para ser seguida como padrão dos projetos a serem implementados, muito menos um script base, com uma estrutura inicial criada para o inicio do desenvolvimento dos seus bots.
   Neste documento vou demonstrar e disponibilizar um modelo de script para ser utilizado como base dos seus bots, com tratamento de erro, um log de monitoramento local, e vou aproveitar para falar do meu padrão de nomenclatura de scripts, rotinas, e variaveis. 
</p>

## Estrutura de Log 

<p align="justify">
   No IBM RPA existe várias formas de ter um log de monitoramento da execução, dentre elas a mais estruturada é ter um banco de dados para ser utilizado nos projetos. Mas as vezes nos deparamos com empresas aonde não tem uma base de dados disponível para o uso do IBM RPA, por conta de suas políticas internas. Por conta deste cenário neste documento vou demonstrar como utilizar uma base de dados local utilizando o Sqlite. Caso tenha uma instância de banco de dados disponível, basta apenas trocar o comando de conexão com o banco de dados.
</p>

Será utilizado esta rotina toda a vez que formos registrar alguma mensagem no log, basicamente obtem a data/hora atual, conecta no DB e executa a query com os dados recebidos na chamada da sub rotina.

<h5>script</h5>

```
beginSub --name __RegisteringLog
	replaceText --texttoparse "${_logMessage}" --textpattern "\'" _logMessage=value
	getCurrentDateAndTime --localorutc "LocalTime" dateTimeNow=value
	sqliteConnect --connectionString "Data Source=${pathDataBase};Version=3;" conBd=connection success=success
	sqlExecute --connection ${conBd} --statement "INSERT INTO IBMRPA_LOG (\r\n   LOGDATE, PROJECT, SCRIPT,\r\n   LOGTYPE,REGISTERID,MESSAGE,\r\n   ERRORSUBROUTINE,ERRORLINE,\r\n   ERRORMESSAGE,PATHSCREENSHOT\r\n)\r\nVALUES (\r\n   \'${dateTimeNow}\',\'${_project}\',\'${_script}\',\r\n   \'${_logType}\',\'${_registerId}\',\'${_logMessage}\',\r\n   \'${_logErrorSubRoutine}\',\'${_logErrorLine}\',\r\n   \'${_logErrorMessage}\',\'${_logPathScreenshot}\'\r\n);" insertedRows=value
	sqlDisconnect --connection ${conBd}
endSub
```

![image](https://user-images.githubusercontent.com/46223364/196575429-69d51812-0465-481a-bd8e-75276de4e147.png)


Os dados que iremos inserir na tabela

![image](https://user-images.githubusercontent.com/46223364/196576959-43c0dcce-bd38-42d8-b2ca-0a747c541e55.png)

- dateTimeNow > Obtem o valor toda a vez que é executado a rotina
- _project and _script > Definido na rotina Initializing

![image](https://user-images.githubusercontent.com/46223364/196580625-f0e4354c-9357-4a09-b624-f9cd547a620f.png)

- _logType, _registerId and _logMessage > Informado no comando 'Run SubRoutine(goSub)'

![image](https://user-images.githubusercontent.com/46223364/196580558-cbf7acc1-add6-4d95-8a05-829b5c1e501f.png)

- _logErrorSubRoutine, _logErrorLine, _logErrorMessage, and _logPathScreenshot > Informado no comando 'Run SubRoutine(goSub)' da rotina __ErrorHandling

![image](https://user-images.githubusercontent.com/46223364/196580398-0cad3d16-3076-4104-95aa-d03e23c9856e.png)

=====

## Funcionalidades

:heavy_check_mark: Funcionalidade 1  

:heavy_check_mark: Funcionalidade 2  

:heavy_check_mark: Funcionalidade 3  

:heavy_check_mark: Funcionalidade 4  

## Layout ou Deploy da Aplicação :dash:

> Link do deploy da aplicação. Exemplo com netlify: https://certificates-for-everyone-womakerscode.netlify.app/

... 

Se ainda não houver deploy, insira capturas de tela da aplicação ou gifs

## Pré-requisitos

:warning: [Node](https://nodejs.org/en/download/)

...

Liste todas as dependencias e libs que o usuário deve ter instalado na máquina antes de rodar a aplicação 

## Como rodar a aplicação :arrow_forward:

No terminal, clone o projeto: 

```
git clone https://github.com/React-Bootcamp-WoMarkersCode/certificate-generator
```

... 

Coloque um passo a passo para rodar a sua aplicação. **Dica: clone o próprio projeto e verfique se o passo a passo funciona**

## Como rodar os testes

Coloque um passo a passo para executar os testes

```
$ npm test, rspec, etc 
```

## Casos de Uso

Explique com mais detalhes como a sua aplicação poderia ser utilizada. O uso de **gifs** aqui seria bem interessante. 

Exemplo: Caso a sua aplicação tenha alguma funcionalidade de login apresente neste tópico os dados necessários para acessá-la.

## JSON :floppy_disk:

### Usuários: 

|name|email|password|token|avatar|
| -------- |-------- |-------- |-------- |-------- |
|Lais Lima|laislima98@hotmail.com|lais123|true|https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcS9-U_HbQAipum9lWln3APcBIwng7T46hdBA42EJv8Hf6Z4fDT3&usqp=CAU|

... 

Se quiser, coloque uma amostra do banco de dados 

## Iniciando/Configurando banco de dados

Se for necessário configurar algo antes de iniciar o banco de dados insira os comandos a serem executados 

## Linguagens, dependencias e libs utilizadas :books:

- [React](https://pt-br.reactjs.org/docs/create-a-new-react-app.html)
- [React PDF](https://react-pdf.org/)

...

Liste as tecnologias utilizadas no projeto que **não** forem reconhecidas pelo Github 

## Resolvendo Problemas :exclamation:

Em [issues]() foram abertos alguns problemas gerados durante o desenvolvimento desse projeto e como foram resolvidos. 

## Tarefas em aberto

Se for o caso, liste tarefas/funcionalidades que ainda precisam ser implementadas na sua aplicação

:memo: Tarefa 1 

:memo: Tarefa 2 

:memo: Tarefa 3 

## Desenvolvedores/Contribuintes :octocat:

Liste o time responsável pelo desenvolvimento do projeto

| [<img src="https://avatars2.githubusercontent.com/u/46378210?s=400&u=071f7791bb03f8e102d835bdb9c2f0d3d24e8a34&v=4" width=115><br><sub>Diana Regina</sub>](https://github.com/Diana-ops) |  [<img src="https://avatars2.githubusercontent.com/u/46378210?s=400&u=071f7791bb03f8e102d835bdb9c2f0d3d24e8a34&v=4" width=115><br><sub>Diana Regina</sub>](https://github.com/Diana-ops) |  [<img src="https://avatars2.githubusercontent.com/u/46378210?s=400&u=071f7791bb03f8e102d835bdb9c2f0d3d24e8a34&v=4" width=115><br><sub>Diana Regina</sub>](https://github.com/Diana-ops) |
| :---: | :---: | :---: 

## Licença 

The [MIT License]() (MIT)

Copyright :copyright: Ano - Titulo do Projeto
