Se os parametros devem ser obrigatórios, devemos usar os path params, se não, usamos os querys params. Um resumo breve poder ser visto em [[REST#Parâmetros]]


Exemplo de um endpoint que recebe parametros por path params

```java
@RestController

public class MathController {

private static final AtomicLong counter = new AtomicLong();

@RequestMapping(value = "/sum/{numberOne}/{numberTwo}", method = RequestMethod.GET)
public Double sum(
	@PathVariable(value = "numberOne") String numberOne,
	@PathVariable(value = "numberTwo") String numberTwo) throws Exception {
	
	if(!isNumeric(numberOne) || !isNumeric(numberTwo)) {
		throw new Exception();
	}
return convertToDouble(numberOne) + convertToDouble(numberTwo);

}


private Double convertToDouble(String strNumber) {
	
	if (strNumber == null) return 0D;
		String number = strNumber.replaceAll(",", ".");
		if(isNumeric(number)) return Double.parseDouble(number);
	return 0D;

}

private boolean isNumeric(String strNumber) {
	
	if (strNumber == null) return false;
			String number = strNumber.replaceAll(",", ".");
		return number.matches("[-+]?[0-9]*\\.?[0-9]+");
	}
}
```

O código acima realiza a soma de dois números reais, a parte do código responsavel por receber esse valores é: 

```java
@RequestMapping(value = "/sum/{numberOne}/{numberTwo}", method = RequestMethod.GET)
public Double sum(
	@PathVariable(value = "numberOne") String numberOne,
	@PathVariable(value = "numberTwo") String numberTwo)
```

veja bem, esse endpoint conta com duas path variables, `numberOne`  e `numberTwo`, quando o usuario acessa por exemplo `http://localhost:8080/sum/2/4,23` os valores 2 e 4,23 serão atribuidos às variaveis numberOne e numberTwo, automaticamente, essas variaveis serão recebidas como String

em seguida na parte 

```java
private Double convertToDouble(String strNumber) {
	
	if (strNumber == null) return 0D;
		String number = strNumber.replaceAll(",", ".");
		if(isNumeric(number)) return Double.parseDouble(number);
	return 0D;

}
```

esse valor é recebido, verifica-se é nulo e então convertido em Double

esse codigo também chama o seguinte método

```java
private boolean isNumeric(String strNumber) {
	
	if (strNumber == null) return false;
			String number = strNumber.replaceAll(",", ".");
		return number.matches("[-+]?[0-9]*\\.?[0-9]+");
	}
}

```

que verifica se esse valor recebido é um numero real

em seguida

```java
if(!isNumeric(numberOne) || !isNumeric(numberTwo)) {
		throw new Exception();
	}
return convertToDouble(numberOne) + convertToDouble(numberTwo);
```

se os valores forem válidos, a API retorna a soma dos valores, senão uma exception é lançada.