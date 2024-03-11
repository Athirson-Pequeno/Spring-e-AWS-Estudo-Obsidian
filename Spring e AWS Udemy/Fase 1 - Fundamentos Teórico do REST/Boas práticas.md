1 - Paginação, se for para buscar clientes pelo nome, é interessante buscar por exemplo de 5 em 5, de 10 em 10, isso vai retornar um JSON mais simples de trabalhar, processar e manipular, além de ser melhor para buscar no banco de dados.

2 - Definir recursos lógicos, deve separar os endpoints para cada modelo necessário

3 - Tolerância a falhas, sua API deve estar preparada para falhas ou seja contornar erros que podem acontecer e conseguir informar isso ao client.

4 - Cache, você deve manter em cache as informações muito requisitadas.

5 - Conectividade, sempre facilite a comunicação com sua API.

6 - Definir timeouts, é fundamental que você limite o tempo de comunicação do cliente, para evitar consumir recursos do servidor e impede que a aplicação caia.

7 - Documentação, sempre documente a sua API da melhor forma.

8 - Utilize SSL, aumenta a sua segurança

9 - Versionamento, para facilitar o uso da API e atualizações

10 - Teste e validação, para verificar se sua API está funcionando corretamente.

11 - Self-service, os clientes devem conseguir acessar só oq precisam da sua API.

12 - Monitore os endpoints para achar gargalos.