
É importante criar um versionamento de uma API para evitar quebra e implementar mais facilmente atualizações, umas das formas de versionamento é o versionamento por URL, temos 3 maneiras de fazer isso, subdomínio, path param ou query string.

## Versionamento pela URL
### Subdomínio 

![[Pasted image 20240307103321.png]]

Pelo subdomínio, especificamos a versão logo no início da URL, no caso acima v1, destacado em verde.

### Path Param

![[Pasted image 20240307103440.png]]

Pelo Path especificamos a versão depois da base URL, é a mais utilizada por facilitar a navegação entre versões, é a melhor de se utilizar em quesito boas práticas.

### Query string

![[Pasted image 20240307103637.png]]

Antigamente era a mais usada, mas dificulta em navegar em outras versões, além de que em uma URL com muitos parâmetros dificulta a leitura.


## Versionamento pelo Hearder

Nesse caso os dados são enviados pelo cabeçalho da requisição, o ponto positivo pelo header é que sua URL fica mais limpa, o problema dela é que dificulta  a hora de fazer um versionamento e usar coisas mais simples, se deve implementar mais coisas nesse cenário.
### Header com Accept

Usando a key accept

![[Pasted image 20240307103818.png]]

Essa teoricamente a maneira de correta de fazer.
### Custom Header Param

Criando um custom header para requisição.

![[Pasted image 20240307103955.png]]
