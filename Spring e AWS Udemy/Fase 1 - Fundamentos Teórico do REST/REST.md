 
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



## Níveis De Maturidade REST

Links:

https://www.crummy.com/writing/speaking/2008-QCon/act3.html
https://www.boaglio.com/index.php/2016/11/03/modelo-de-maturidade-de-richardson-os-passos-para-a-gloria-do-rest/
https://dzone.com/refcardz/rest-foundations-restful?chapter=3
https://martinfowler.com/articles/richardsonMaturityModel.html
 [[HATEOAS - FASE 1]]


RESTFul é o maior nível do REST, o REST está incluso no RESTFul
#### Nível 0 - Pântano de XML (The Swamp of POX)

Imagine que você tem uma oficina e tem uma API Rest, responsável por gravar clientes, gravar peças, gravar pedidos, gravar chamados, gravar as ordens de serviços. E está tudo amontoado em um único endpoint que lhe permite gerir todos os aspectos da sua empresa.

#### Nível 1 - Recursos 

Nesse ponto, as informações já estão organizadas por recursos e voltando ao exemplo da oficina, agora já existe um endpoint para gravar clientes, um endpoint para gravar fornecedor, um endpoint para abrir ordens de serviço, outro endpoint para gerir equipamentos e por aí vai.

As coisas já estão um pouco mais organizadas, mas ainda não existe uma preocupação com usar os verbos adequados a cada situação.

#### Nível 2 - Verbos HTTP

No nível dois, os verbos http, como o get, o post, o push e o delete passam a ser usados, além de separar a informação por recursos como você já deve ter notado e incremental. Não se chega ao nível dois sem passar pelo nível um e não se chega ao nível três sem passar pelo nível dois. Logo, o nível um tem todas as melhorias em cima do que tem no zero. Já o nível dois tem uma melhoria em cima de tudo que tem.

#### Nível 3 - Hypermedia Controls 

Imagine por exemplo que eu tenho acesso a uma listagem de pessoas e cada pessoa da lista tem um link que me permite acessar as informações da pessoa específica. E no recurso pessoa eu tenho os links para as operações que eu posso realizar. No recurso de pessoas, por exemplo, ele me retorna o link que eu posso usar para deletar, para fazer um update, para cadastro novo, para listar etc.


O último nível é o Nível 3

==*ATENÇÃO*==

![[Pasted image 20240305174650.png]]


## Além do RESTFul

1 - Limitar Requests, isso ajuda no faturamento das API

https://www.keycdn.com/support/rate-limiting
https://nordicapis.com/everything-you-need-to-know-about-api-rate-limiting/
https://cloud.google.com/solutions/rate-limiting-strategies-techniques
https://blog.cloudflare.com/counting-things-a-lot-of-different-things/
https://www.cloudflare.com/learning/bots/what-is-rate-limiting/
https://www.ibm.com/support/knowledgecenter/SSPREK_9.0.7/com.ibm.isam.doc/config/concept/c_ratelim.html
https://develop.zendesk.com/hc/en-us/articles/360001074328-Best-practices-for-avoiding-rate-limiting

C#

https://github.com/stefanprodan/AspNetCoreRateLimit
https://github.com/search?l=C%23&q=rate+limit+api&type=Repositories

Java

https://github.com/search?l=Java&q=rate+limit+api&type=Repositories

2 - Criar um SDK, ajuda aos desenvolvedores usarem sua API mais facilmente, sempre deve atualizar o SDK ao fazer uma alteração na API
