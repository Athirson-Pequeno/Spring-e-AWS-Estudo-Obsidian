Em Java, "migrations" geralmente se refere a uma prática relacionada ao controle de versão e gerenciamento de bancos de dados em aplicações web ou de software. As migrações de banco de dados são scripts ou classes que descrevem as mudanças na estrutura de um banco de dados ao longo do tempo, como adição de tabelas, alteração de campos ou remoção de índices.

Essas migrações são frequentemente utilizadas em conjunto com ferramentas de ORM (Object-Relational Mapping) como Hibernate, JPA (Java Persistence API) ou outras bibliotecas de persistência. Elas permitem que você mantenha o esquema do banco de dados sincronizado com a estrutura de dados da sua aplicação, facilitando a evolução e manutenção do sistema.

Por exemplo, suponha que você esteja desenvolvendo um aplicativo Java que utiliza um banco de dados MySQL e deseja adicionar uma nova tabela para armazenar informações de clientes. Você pode criar uma migração que contém o código SQL necessário para criar essa tabela. Quando você executa essa migração, o banco de dados é atualizado para refletir as mudanças definidas na migração.

Essas práticas são especialmente úteis em ambientes de desenvolvimento colaborativo, pois permitem que diferentes desenvolvedores trabalhem em alterações no banco de dados de forma organizada e controlada, mantendo um histórico das modificações feitas ao longo do tempo.

para configurar uma migration no Spring deixaremos o yml assim

![[Pasted image 20240418164252.png]]

ddl-auto none

e as dependencias assim

![[Pasted image 20240418164350.png]]

