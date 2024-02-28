 

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

