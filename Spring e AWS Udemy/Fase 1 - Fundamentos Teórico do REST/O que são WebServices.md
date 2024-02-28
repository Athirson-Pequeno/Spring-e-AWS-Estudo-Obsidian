WebService em uma definição bem direta, podemos dizer que é uma aplicação interoperável, que é hospedada e pode ser acessada através do protocolo HTTP. 

A definição do W3C  

```Para o W3C os webservices são aplicações cliente servidor que se comunicam pela World Wide Web's (WWW) através do protocolo HTTP (HyperText Transfer Protocol) possibilitando a interoperabilidade entre softwares e aplicações executando em uma grande variedade de plataformas e frameworks. Caracterizam-se por sua grande interoperabilidade e extensibilidade podendo ser combinados de forma baixamente acoplada para executarem operações complexas. Programas proveem simples serviços que podem interagir uns com os outros gerando soluções sofisticadas.```
## REST

O REST surgiu como uma forma de padronizar a comunicação na internet, antes existiam muitas tecnologias (RMI, SOAP, Corba, DCE, DCOM, etc.) e elas não se comunicavam bem entre si. Os padrões que se sobressaíram o SOAP e o REST 


### SOAP x REST


A principal diferença está no protocolo usado, o SOAP usa o HTTP para fazer chamadas RPC trafegando o XML, já o REST suporta diferentes formatos de arquivos nas suas chamadas HTTP.

Tabela comparativa

![[Pasted image 20240228192525.png]]

#### Como funciona o SOAP?  

Ele pega os dados do cliente e envolve em um uma capsula chamado de envelope SOAP, depois envia para o servidor.

Antes do SOAP

![[Pasted image 20240228192915.png]]

Depois do SOAP

![[Pasted image 20240228192824.png]]

#### Como funciona o REST?  

![[Pasted image 20240228193036.png]]

