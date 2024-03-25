A anotação @Service no Spring Boot serve para marcar uma classe como um serviço gerenciado pelo Spring, permitindo a injeção de dependência e a configuração automática de componentes em um aplicativo Spring.

Exemplo de uma classe de serviço:

```java
@Service
public class PersonServices {}
```

#### Usando um classe de serviço

Para usar uma classe de serviço deve-se colocar a anotação @Autowired na declaração de variavel dessa classe.

Exemplo:

```java
@Autowired
private PersonServices service;
```

Esse código de seria o equivalente a

```java
private PersonServices service = new PersonServices();
```

a diferença é que no exemplo de cima o objeto service vai ser gerenciado pelo Spring.