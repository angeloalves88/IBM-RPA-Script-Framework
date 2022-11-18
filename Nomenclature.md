<p align="right">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

# Nomenclature

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> <br /><br />

## Set a Standard

Para manter um ambiente organizado e facilitando a manutenção é muito importante ter um padrão de nomenclatura para ser seguido no projeto. Existindo um padrão agiliza o entendimento por outras membros do time em caso de manutenção ou melhorias. 
se prestou atenção, nos exemplos deste framework é possível visualizar alguns padrões, que vamos abordar abaixo com mais detalhes.

## Script Name

O padrão no script name que utilizo neste Git é Camel Case, removo as preposições e troco os espaços por _ (underline)

- Cadastramento de invoice > CadastramentoInvoice

Como podemos ter mais de um script para o mesmo processo automatizado é muito importante definir um prefixo para o nome do script de acordo com o nome do projeto. Normalmente oriento a utilizar o proprio nome do processo, mas em casos aonde o nome é muito extenso, podemos criar uma sigla. Uma outra alternativa é criar apelido para o projeto, isso vai muito da cultura da empresa.

Vamos ver alguns exemplos:

- Cadastramento de produtos no SAP > SAP_CadastroProduto_
- Cadastramentos de vendas de Pequenas e Medias Empresas > PME_

Além dos scripts especificos do Projeto, temos scripts genéricos que podem ser utilizados em mais de um projeto, por exemplo tenho 5 projetos que utilizam o SAP, então posso ter um único script de Login que os 5 projetos consome ele, facilitando a manutenção futura. O ideal é ter um padrão para o nome desses scripts visando ficar diferente dos projetos, vamos ver alguns cenários:

- moduleSendEmail
- libLoginSAP


## Sub-Routine Name

Na Sub-Rotina também utilizo o Camel Case, removo as preposições e os espaços.

- Execute
- Inicializing

Como pode ser observado no script de amostra, temos algumas rotinas que inicio com `__` (2x underline), isto é um padrão meu, que aplico para saber:

- Que esta rotina é tipo uma classe do meu script e eu utilizo elas várias vezes no meu script;
  - `__CreatingDirectory` and `__RegisteringLog` normalmente estas rotinas recebem algum valor como parametro de entrada.
- Que é uma rotina base da minha estrutura de script
  - `__ErrorHandling` and `__CreatingDatabase`

## Variable Name

Enquanto para os nomes de variaveis eu utilizo o low Camel Case e também removo as preposições e espaços

- linha da tabela >> rowTable
- first name >> firstName

Um ponto muito importante nas variaveis é tentar colocar um nome claro da sua função, evitar abreviações ou usar `${n1}` `${var1}` `${i}` `${row}`

- ${rowInvoiceTable}
- ${numberRegistering}
- ${adressCustomer}

Uma outra possibilidade é agrupar variaveis, como no exemplo todas as variaveis do log tem o prefixo `log`, um outro cenário é por exemplo, você está fazendo a leitura de um Json, e ao mapear os dados dele, pode utilizar o sufixo json, com isso facilita encontrar grupos de variaveis.

- firstNameJson
- lastNameJson
- addresJson

Tenho também as variáveis que iniciam com `_`(underline) que são utilizadas para:
- Armazenar valores temporoarios para aplicar alguma ação, como obter o texto bruto para eu remover apenas a parte necessaria posteriormente;
- Dentro de uma sub-rotina, como por exemplo de conversão do formato de data e hora;
- Variaveis da estrutura do meu script
