Começamos criando uma classe com dois atributos, id que será do tipo long, e content que será uma string, nessa mesma classe criaremos o construtor e os getters e setters.

```java
package br.com.tizo;

public class Greeting {

private final long id;

private final String content;

public Greeting(long id, String content) {
	super();
	this.id = id;
	this.content = content;
}

public long getId() {
	return id;
}

public String getContent() {
	return content;
}

}
```


em seguidas criamos a class GreetingController da seguinte forma

```java
@RestController
public class GreetingController {
}
```

A anotação @RestController, é uma mistura do @Controller com @ResponseBody, o Spring criou essa anotação para facilitar a implementação de API RESTs, visto que essas funcionam retornando JSONs e XMLs.

```java
@RestController
public class GreetingController {

	private static final String template = "Hello, %s!";
	private static final AtomicLong counter = new AtomicLong();
	
@RequestMapping
public Greeting greeting(
		@RequestParam(value = "name", defaultValue = "World")
		String name) {
			return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
}
```

Criamos uma template para retorna a String na linha 3, e em seguidas criamos o metodo greeting que recebe um @RequestParam, com os atributos value, e defaultValue (que serve em casos de que o valor name não seja informado). Esse metodo retona um novo Greeting com o id 0 e a string formatada, combinando o template mais o name. O AtomicLong vai ser nosso id, cada vez que ele for chamado vai retornar um numero maior.

Feito isso, pode rodar o código em `localhost:8080/greeting` o retorno será

![[Pasted image 20240311195519.png]]

em `localhost:8080/greeting?name=Athirson` o retorno será

![[Pasted image 20240311195627.png]]


Perceba que quando não passamos o paramentro "name" a API retorna "Hello, World!", World é o nosso valor default, já quando passamos o parametro name podemos usar o nome enviado pela URL, e retornar o JSON