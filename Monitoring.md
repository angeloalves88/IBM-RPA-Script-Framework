<p align="right">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

# Monitoring

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> 

<p align="justify">
   	No IBM RPA existe várias formas de ter um log de monitoramento da execução, dentre elas a mais estruturada é ter um banco de dados para ser utilizado nos projetos. Mas as vezes nos deparamos com empresas aonde não tem uma base de dados disponível para o uso do IBM RPA, por conta de suas políticas internas. Por conta deste cenário neste documento vou demonstrar como utilizar uma base de dados local utilizando o Sqlite. Caso tenha uma instância de banco de dados disponível, basta apenas trocar o comando de conexão com o banco de dados. <br />
	Outra vantagem do uso do SQLite, é que o IBM RPA já tem um comando para criar o arquivo do database, e a estratura da tabela. Sendo necessário a instalação apenas de um leitor de SqLite para acompanhamento dos registros.
</p>

## Monitoramento

Abaixo podemos ver um screenshot da tabela de log dos registros que executaram com sucesso.
	
<h5><completed></h5>

![image](https://user-images.githubusercontent.com/46223364/197344997-38cf2d4b-d54c-49e3-9cbe-f22f91fbb342.png)

Nesta já podemos ver os casos de falhas, aonde as colunas de erro são preenchidas.
	
<h5><failed></h5>
	
![image](https://user-images.githubusercontent.com/46223364/197345234-a5b97305-078f-4aeb-b623-ba4d4fbbcb8c.png)

Por mais simples que seja esta estrutura de tabela do log, ela é muito util para o acompanhamento diario dos projetos, facilitando encontrar erro e idêntificar o processo que apresentou a falha, além de ter a mensagem de erro, linha e o screenshot da tela no momento do erro.	
