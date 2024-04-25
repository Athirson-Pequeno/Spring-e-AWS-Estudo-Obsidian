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

customizando swagger

limitar acesso swagger

```yml
springdoc:  
  pathsToMatch: /api/**/v1/**  
  swagger-ui:  
    use-root-path: true
```
em pathsToMarch colocaremos os caminhos que o swagger pode ver

no controller nós podemos adicionar tag na classe, desse jeito

```java
@Tag(name = "People", description = "Endpoints fot Managing People")
```

no nosso controller as funções podem receber configurações mais complexas, veja a seguir

```java
@Operation(  
        summary = "Finds all People",  
        description = "Finds all People",  
        tags = {"People"},  
        responses = {  
                @ApiResponse(  
                        description = "Success",  
                        responseCode = "200",  
                        content = {  
                                @Content(  
                                        mediaType = "application/json",  
                                        array = @ArraySchema(schema = @Schema(implementation = PersonVO.class)  
                                        )  
                                )  
                        }  
                ),  
                @ApiResponse(description = "Bad Request", responseCode = "400", content = @Content),  
                @ApiResponse(description = "Unauthorized", responseCode = "401", content = @Content),  
                @ApiResponse(description = "Not Found", responseCode = "404", content = @Content),  
                @ApiResponse(description = "Internal Error", responseCode = "500", content = @Content)  
    }  
)
```

abaixo um resumo de cada item
  

1. **@Operation**
    
    - `summary`: Um resumo conciso do que a operação faz, neste caso, "Finds all People" indica que a operação retorna todos os registros de pessoas.
    - `description`: Uma descrição mais detalhada da operação, que também indica que ela encontra todas as pessoas.
    - `tags`: Uma lista de tags associadas a essa operação, aqui, é a tag "People" para classificar essa operação relacionada a pessoas.
2. **@ApiResponse**
    
    - Cada `@ApiResponse` representa uma possível resposta da operação.
    - `description`: Descreve o tipo de resposta.
    - `responseCode`: O código HTTP correspondente à resposta, como 200 para sucesso, 400 para erro de solicitação, 401 para não autorizado, 404 para não encontrado e 500 para erro interno do servidor.
    - `content`: O conteúdo da resposta, que pode ser especificado com um `@Content`.
3. **@Content**
    
    - `mediaType`: O tipo de mídia da resposta, aqui, "application/json" indica que a resposta é em formato JSON.
    - `array`: Indica que a resposta é uma matriz (array) de objetos.
    - `schema`: Define o esquema do objeto de resposta, neste caso, é especificada uma implementação de `PersonVO.class` como o esquema para a resposta JSON.


