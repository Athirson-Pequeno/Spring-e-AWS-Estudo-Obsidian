para começar a usar o swagger no seu projeto, adicione primeiro a depencia

```xml
<dependency>  
    <groupId>org.springdoc</groupId>  
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>  
    <version>${springdoc.version}</version>  
</dependency>
```

configurações básicas

criaremos uma classe para essas configurações, daremos o nome de OpenApiConfig e colocaremos a anotação @Configuration

criaremos o seguinte [[@Bean]]

```java
@Bean  
  public OpenAPI customOpenApi(){  
    return new OpenAPI();  
}
```

nosso bean configurado 

```java
@Bean  
public OpenAPI customOpenApi() {  
    return new OpenAPI()  
            .info(new Info()  
                    .title("RESTful API with Java 21 and Spring Boot 3")  
                    .version("v1")  
                    .description("Some description")  
                    .termsOfService("https://github.com/Athirson-Pequeno")  
                    .license(new License().name("Apache 2.0").url("https://github.com/Athirson-Pequeno"))  
            );  
}
```

para ver o JSON vá nesse link `http://localhost:8080/v3/api-docs`

para ver com uma interface `http://localhost:8080/swagger-ui/index.html`