
[Maven blog erudio](https://www.erudio.com.br/blog/entendendo-o-apache-maven/)
[Pom do maven blog erudio](https://www.erudio.com.br/blog/entendendo-o-pom-do-maven/)


O maven é um gerenciador de dependencias, que facilita a busca e implementação de bibliotecas, ajuda a padronizar o projeto entre pessoas. O maven procura as depedencias declaradas e baixa todas elas em um diretorio local, o diretório "M2"

ex maven:

![[Pasted image 20240311181409.png]]

diretorio maven "M2"

![[Pasted image 20240311181529.png]]

nesse diretorio fica todas as dependecias ja usadas na minha máquina.

### pom.xml

O Pom é um dos arquivos mais importantes em um projeto Maven, e ele descreve uma série de configurações que o projeto terá e quais repositórios, dependências, plugins, dentre outras coisas que o seu projeto irá precisar.

![[Pasted image 20240311182308.png]]

a tag parent, serve para dizer que esse projeto faz parte de um projeto spring, a ideia é separar as aplicações para não atrapalhar as heranças, evitando duplicação de herança, ou seja, nosso projeto herda as propriedades necessarias do spring e passa para os filhos.

em seguida temos as propriedades do projeto, como nome, versão, java version

![[Pasted image 20240311182655.png]]

em seguida vem as dependencias internas do projeto, vejam que seguem a mesma estrutura, a diferença é que eles herdam a versão do stater, se quiser você pode mudar a versão manualmente, mas não é recomendado.

![[Pasted image 20240311182921.png]]

em seguida vem a tag de build

![[Pasted image 20240311183101.png]]

em alguns casos vem em seguida a tag de repository

![[Pasted image 20240311183221.png]]

e também em algum casos as dependencias devem ser excluidas, por conflitos, etc.

![[Pasted image 20240311183306.png]]
