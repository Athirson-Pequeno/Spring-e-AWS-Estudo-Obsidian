 
## Visão Geral

Definindo por "Representational State Transfer (REST) é um estilo de arquitetura de software para sistemas distribuídos de hipermídia, como a World Wide Web" - IBM

6 Restrições do REST

﻿
- 1 Cliente-servidor
	Clientes e servidores separados.

- 2 Stateless server
	O servidor não deve guardar o estado do cliente. Cada request de um cliente contém todas as informações necessárias para atendê-la.

- 3 Cacheable
	O cliente deve ser informado sobre as propriedades de cache de um recurso para que possa decidir quando deve ou não utilizar cache.

- 4 Interface uniforme
	Existe uma interface uniforme entre cliente e servidor. 
	+ Identificação de recursos (URI).
	+  Manipulação de recursos a partir de suas representações.
	+  Mensagens auto descritivas.
	+ Hypermedia as the engine of application state - [[HATEOAS]]

- 5 Sistema em camadas
	Deve suportar conceitos como balanceamento de carga, proxies e firewalls.

- 6 Código sob Demanda (RARO DE ACONTECER)
	Na maioria das vezes, um servidor retorna representação estática de recursos no formato XML ou JSON. Porém, quando necessário, os servidores podem fornecer código executável ao cliente. 


## Servidores RESTful

### Vantagens dos Web Services RESTful

- REST é um padrão arquitetural basicamente leve por natureza. Então quando você tiver limitações de banda prefira WebServices REST;

- Desenvolvimento fácil e rápido;

- Aplicativos Mobile tem ganhado cada vez mais espaço e precisam interagir rapidamente com os servidores e o padrão REST é mais rápido no processamento de dados das requests e responses.


### Request e Response

Nos WebServices quem faz as requests são as aplicações client

Representação Request

![[Pasted image 20240229170415.png]]

Representação Response

![[Pasted image 20240229170539.png]]

[Link sobre protocolo HTTP](https://personal.ntu.edu.sg/ehchua/programming/webprogramming/HTTP_Basics.html)

## Parâmetros

**Path Params** - são obrigatórios, se não for definido nenhum valor ocorrerá erros

_o trecho destacado representa os Path Params dessa URL_
﻿
´https://your_host/api/books/v1/find-with-paged-search/==asc==/==10==/==1==´

Essa URL simula uma API que retorna uma lista de livros

asc - parâmetro para ordenar a lista
10 - numero de itens na pagina
1 - página para ser renderizada


**Query Params** - não são obrigatórios

_o trecho destacado representa os Query Params dessa URL_
﻿
´https://your_host/api/books/v1/find-by-title==?firstName== =Clean==&lastName=== =Coder´

? - inicia as querys
firstName=Clean - passa o parâmetro fistName com o valor Clean
& - adiciona mais um parâmetro
lastName=Code - passa o parâmetro lastName com o valor Code

a cada novo parâmetro se adiciona um & entre eles, como é opcional não se precisa passar todos os parâmetros disponíveis na URL

**Headers e Body Params** - 

Heardes Params são passados no Header da request(requisição), esses parâmetros não são passados pelo navegador, devem ser passados através de um client (Postman, Isomnia), apesar de existir os parâmetros padrões(deaults) no protocolo HTTP nós podemos criar nossos próprios parâmetros

Body Params são usados para passar dados mais complexos, como JSONs, XMLs, vai depender do que a API aceita, são muito usados para fazer POST, PUT e PATH, geralmente usados para salvar e atualizar dados no banco de dados

## Status Code

- 1xx Informacionais
- 2xx Sucesso
- 3xx Redirecionamento
- 4xx Erro de Client
- 5xx Erro de Server

[Documentação Oracle sobre status code](https://docs.oracle.com/en/cloud/iaas/messaging-cloud/csmes/rest-api-http-status-codes-and-error-messages-reference.html#GUID-AAB1EE32-BE4A-4ACC-BEAC-ABA85EB41919)
[HTTP Status Codes](https://www.restapitutorial.com/httpstatuscodes.html)
[Dzone](https://dzone.com/refcardz/rest-foundations-restful?chapter=5)
[HTTP Status Codes em Serviços REST](http://www.semeru.com.br/blog/http-status-codes-em-servicos-rest/)

### 2xx

![[Pasted image 20240229182059.png]]

### 4xx e 5xx

![[Pasted image 20240229182240.png]]
![[Pasted image 20240229182450.png]]


## Verbos HTTP

Verbo, método, operação, termos para a mesma coisa

Para ter uma noção de como funciona é legal fazer uma comparação com CRUD(Create, Read, Update, Delete), ficando da seguinte maneira

![[Pasted image 20240229184415.png]]

### GET

![[Pasted image 20240229184514.png]]
![[Pasted image 20240229184610.png]]

==**Único verbo que NÃO suporta parâmetros via body**==


### POST

![[Pasted image 20240229184758.png]]
![[Pasted image 20240229184824.png]]

### PUT

![[Pasted image 20240229184929.png]]
![[Pasted image 20240229184824.png]]

### DELETE

![[Pasted image 20240229185126.png]]
![[Pasted image 20240229184824.png]]


## Verbos "Secundários"

### PATCH

![[Pasted image 20240229185609.png]]

### HEAD

![[Pasted image 20240229185652.png]]

### TRACE

![[Pasted image 20240229185722.png]]

### OPTIONS

![[Pasted image 20240229185815.png]]

### CONNECT

![[Pasted image 20240229185957.png]]

