<p align="right">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

# Execution Architecture and Error Handling

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> <br /><br />

## Execution Architecture 	
	
Agora que já expliquei a base de dados e o monitoramento, vamos verificar como eu organizo meu script e como eu trato o cenário de falha. 
	
Cada projeto tem sua particularidade, este modelo é ideal para cenários que pode reprocessar o item que apresentou falha do inicio. Ou seja, não importa aonde quebrou, volta no inicio e tenta novamente.
	
	Esta estrutura é ideal para Projetos que utiliza a Feature do Orquestrador de Processos do IBM RPA. 
	Que ao apresentar falha na execução, retente, ao invés de para a execução da instância na primeira falha.
	

	
![image](https://user-images.githubusercontent.com/46223364/197346753-387ed76d-c8d5-4022-87ff-1d9828b32428.png)
        
`[Initialize]` - Rotina responsavel por carregar e atribuir todos os valores utlizados pelo script:   
- Obter Parametros;
- Definição de variáveis;
- Conexões com serviços externos (e-mail, provedores de fila, banco de dados);
- Criação de recursos necesários para execução do script;
     
![image](https://user-images.githubusercontent.com/46223364/197346890-7d6a5493-4dc7-4ab2-8754-323062acff8c.png)

        
`[Execute]` - Rotina responsável por conter toda a estrutura de exeucção do processo.
        
![image](https://user-images.githubusercontent.com/46223364/197347369-15b7c32a-4716-4039-84db-ed1770c02e03.png)
 
## Error Handling
	
`[__ErrorHandling]` - Rotina responsável para realizar o tratamento de erro   
- Capturar a imagem da tela e salvar   
- Capturar a mensagem de erro e escrever no log
- Limpa as variaveis de erro do log
- Reiniciar algum sistema, por exemplo fechar e abrir o aplicação/navegador, chamar a rotina que faz login, ....
- Validar se ocorreu menos de 3 tentativas de falha
        - Se sim, chama a rotina Execute novamente
        - senão, finaliza a execução com falha

![image](https://user-images.githubusercontent.com/46223364/197346987-9ff09e46-2067-4cae-aa96-38b2643fd85d.png)

	
	
	
	
	
	
