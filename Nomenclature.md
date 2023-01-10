<p align="right">
   <img src="http://img.shields.io/static/v1?label=STATUS&message=EM%20DESENVOLVIMENTO&color=RED&style=for-the-badge"/>
 <!--  <img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/>-->
</p>

# Nomenclature

	
<h2>IBM RPA - IBM Robotic Process Automation</h2> <br /><br />

## Set a Standard

To maintain an organized environment and facilitate maintenance, it is very important to have a naming pattern to be followed in the project. Having a standard streamlines understanding by other team members in case of maintenance or improvements. If you paid attention, to the examples of this framework it is possible to visualize some patterns, which we will discuss in more detail below.

## Script Name

The pattern in the script name that I use in this Git is Camel Case, I remove the prepositions and space or we can change the spaces to _ (underscore)

- Invoice Registration > InvoiceRegistration

As we can have more than one script for the same automated process, it is very important to define a prefix for the script name according to the project name. Normally I recommend using the name of the process itself, but in cases where the name is too long, we can create an acronym. Another alternative is to create a nickname for the project, this depends a lot on the company's culture.

Let's see some examples:

- Registration of products in SAP > SAP_RegistrationProducts_
- Registration of sales of Small and Medium Enterprises > SME_RegistrationSales_

In addition to the specific Project scripts, we have generic scripts that can be used in more than one project, for example, I have 5 projects that use SAP, so I can have a single Login script that the 5 projects consume, facilitating future maintenance. The ideal is to have a pattern for the name of these scripts in order to be different from the projects, let's see some scenarios:

- moduleSendEmail
- libLoginSAP


## Sub-Routine Name

In the Sub-Routine I also use Camel Case, I remove prepositions and spaces.

- Execute
- Inicializing

As can be seen in the sample script, we have some routines that start with `__` (2x underscore), this is my pattern, which I apply to know:

- That this routine is like a class in my script and I use them several times in my script;
  - `__CreatingDirectory` and `__RegisteringLog` normally these routines receive some value as an input parameter.
- Which is a base routine of my script structure
  - `__ErrorHandling` and `__CreatingDatabase`

## Variable Name

While for variable names the lower Camel Case is used and I also remove prepositions and spaces

- table row >> rowTable
- first name >> firstName

A very important point in the variables is to try to put a clear name of their use, avoiding abbreviations or using `${n1}` `${var1}` `${i}` `${row}`

- ${rowInvoiceTable}
- ${numberRegistering}
- ${adressCustomer}

Another possibility is to group variables, as in the example all log variables have the `log` prefix, another scenario is for example, you are reading a Json, and when mapping its data, you can use the JSON suffix, thus making it easier to find groups of variables.

- firstNameJson
- lastNameJson
- addresJson

I also have variables that start with `_`(underscore) that are used for:
- Store temporary values to apply some action, such as getting the raw text so I can remove only the necessary part later;
- Within a subroutine, such as converting the date and time format;
- Variables of the structure of my script
