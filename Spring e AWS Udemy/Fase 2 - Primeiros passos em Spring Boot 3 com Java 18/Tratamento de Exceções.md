Para darmos inicio ao tratamento de exceções vamos inicialmente criar uma classe modelo para definir os atributos das exceções, veja abaixo

```java
package br.com.tizo.exceptions;

import java.io.Serializable;
import java.util.Date;

public class ExceptionResponse implements Serializable {

private static final long serialVersionUID = 1L;

private Date timestamp;

private String message;

private String details;

public ExceptionResponse(Date timestamp, String message, String details) {

super();

this.timestamp = timestamp;

this.message = message;

this.details = details;

}

public Date getTimestamp() {

return timestamp;

}

public String getMessage() {

return message;

}

public String getDetails() {

return details;

}

}
```

Essa classe vai ser usada para instanciar um objeto com data, mensagem e detalhes de cada exceção.

Como o erro no nosso exemplo é bem especifico (a API só pode receber numeros como parametro), vamos criar uma exceção personalizada, que será a seguinte classe.

```java
@ResponseStatus(HttpStatus.BAD_REQUEST)
public class UnsupportedMathOperationException extends RuntimeException{

private static final long serialVersionUID = 1L;

public UnsupportedMathOperationException(String ex) {

super(ex);

}

}
```

ela deve extender a RunTimeException e deve ter a serialVersionUID definida. nela também definimos um contrutor que receber uma string que é passado para superclasse.

por fim, criamos a classe que vai controlar as exceções, ela será responsavel por gerenciar os erros e enviar as mensagens corretas.

```java
@ControllerAdvice
@RestController
public class CustomizedResponseEntityExceptionHandler extends ResponseEntityExceptionHandler {

@ExceptionHandler(Exception.class)
public final ResponseEntity<ExceptionResponse> handleAllExceptions(
Exception ex, WebRequest request) {

ExceptionResponse exceptionResponse = new ExceptionResponse(
new Date(),
ex.getMessage(),
request.getDescription(false));

return new ResponseEntity<>(exceptionResponse, HttpStatus.INTERNAL_SERVER_ERROR);

}

@ExceptionHandler(UnsupportedMathOperationException.class)
public final ResponseEntity<ExceptionResponse> handleBadRequestExceptions(
Exception ex, WebRequest request) {

ExceptionResponse exceptionResponse = new ExceptionResponse(
new Date(),
ex.getMessage(),
request.getDescription(false));

return new ResponseEntity<>(exceptionResponse, HttpStatus.BAD_REQUEST);

}

}
```

A anotação `@ControllerAdvice` no Spring Boot é usada para centralizar o tratamento de exceções em todos os controllers da sua aplicação. Ela permite definir métodos globais que podem ser aplicados a todos ou a um conjunto específico de controllers para manipular exceções lançadas durante a execução das requisições HTTP.

Quando você adiciona a anotação `@ControllerAdvice` a uma classe, você pode definir métodos nesta classe que serão executados quando uma exceção específica ocorrer em qualquer controller mapeado na sua aplicação. Isso simplifica a lógica de tratamento de exceções, evitando a repetição do código de tratamento em vários controllers. 

nesse caso nós vamos lidar com Exception.class e UnsupportedMathOperationException.class(nossa exception customizada)

cada metodo retorna uma ExceptionResponse(nosso modelo de exceções) com a data, a mensagem e a descrição de cada erro, isso é enviado em formato JSON e ficará assim

![[Pasted image 20240313183038.png]]

para usar isso em um endpoint, basta lançar a exception usando o throw

```java
if(!isNumeric(numberOne) || !isNumeric(numberTwo)) {
	throw new UnsupportedMathOperationException("Please set a numeric value.");
}
```
