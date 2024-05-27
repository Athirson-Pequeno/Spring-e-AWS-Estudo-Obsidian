desabilitar globalmente é perigoso, para configurar corretamente faça o seguinte

Pelo controller

usando o @CrossOrigin, senão definir nada vai permitir acesso de tudo, mas se por exemplo fizer assim `@CrossOrigin(origins = "http://localhost:8080")` só vai poder ser acessado pelo endereço `http://localhost:8080` se quiser mais que um endereço basta usar chaves, assim ``
`@CrossOrigin(origins = {"http://localhost:8080", "https://erudio.com.br"})`

De forma global

no nosso application.yml colocaremos o seguinte codigo com os endereços que queremos liberar

```java
cors:  
  originPatters: http://localhost:3000,http://localhost:8080,https://erudio.com.br
```

atenção não pode ter espaço entre os dominios.

no nosso WebConfig colocaremos a seguinte propriedade

```java
@Value("${cors.originPatterns:default}")  
private String corsOriginPatterns = "";
```

o default depois do ponto vc pode colocar qualquer url para caso de erro e ele nao encontre o valor no yml

em seguida colocaremos o seguinte metodo

```java
@Override  
public void addCorsMappings(CorsRegistry registry) {  
   var allowedOrigins = corsOriginPatterns.split(",");  
   registry.addMapping("/**")  
           //.allowedMethods("GET", "POST", "PUT")  
           .allowedMethods("*")  
           .allowedOrigins(allowedOrigins)  
           .allowCredentials(true);  
}
```

`addMapping("/**")  = os caminhos para configurar`
`allowedMethods("*") = metodos para configurar`
`allowedOrigins(allowedOrigins) = origens aceitas`
`allowCredentials(true) = se aceita autenticação`


criar teste do cors

Crie a classe web config assim

```java
package br.com.tizo.config;  
  
public class TestConfigs {  
  
    public static final int SERVER_PORT = 8888;  
    public static final String HEADER_PARAM_AUTHORIZATION = "Authorization";  
    public static final String HEADER_PARAM_ORIGIN = "Origin";  
  
  
    public static final String CONTENT_TYPE_YAML = "application/x-yaml";  
    public static final String CONTENT_TYPE_XML = "application/xml";  
    public static final String CONTENT_TYPE_JSON = "application/json";  
  
}
```

e depois essa de teste

```java
package br.com.tizo.integrationtests.controller.withjson;  
  
import br.com.tizo.config.TestConfigs;  
import br.com.tizo.integrationtests.testcontainers.AbstractIntegrationTest;  
import br.com.tizo.integrationtests.vo.PersonVO;  
import io.restassured.builder.RequestSpecBuilder;  
import io.restassured.filter.log.LogDetail;  
import io.restassured.filter.log.RequestLoggingFilter;  
import io.restassured.filter.log.ResponseLoggingFilter;  
import io.restassured.specification.RequestSpecification;  
import org.junit.jupiter.api.BeforeAll;  
import org.junit.jupiter.api.MethodOrderer.OrderAnnotation;  
import org.junit.jupiter.api.Order;  
import org.junit.jupiter.api.Test;  
import org.junit.jupiter.api.TestMethodOrder;  
import org.springframework.boot.test.context.SpringBootTest;  
import org.testcontainers.shaded.com.fasterxml.jackson.databind.DeserializationFeature;  
import org.testcontainers.shaded.com.fasterxml.jackson.databind.ObjectMapper;  
  
import java.io.IOException;  
  
import static io.restassured.RestAssured.given;  
import static org.junit.jupiter.api.Assertions.*;  
  
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)  
@TestMethodOrder(OrderAnnotation.class)  
public class PersonControllerJsonTest extends AbstractIntegrationTest {  
  
    private static RequestSpecification specification;  
    private static ObjectMapper objectMapper;  
    private static PersonVO person;  
  
    @BeforeAll  
    public static void setup(){  
       objectMapper = new ObjectMapper();  
       objectMapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);  
  
       person = new PersonVO();  
  
    }  
  
    @Test  
    @Order(1)  
    public void testCreate() throws IOException {  
       mockPerson();  
       specification = new RequestSpecBuilder()  
             .addHeader(TestConfigs.HEADER_PARAM_ORIGIN, "https://erudio.com.br")  
             .setBasePath("/api/person/v1")  
             .setPort(TestConfigs.SERVER_PORT)  
             .addFilter(new RequestLoggingFilter(LogDetail.ALL))  
             .addFilter(new ResponseLoggingFilter(LogDetail.ALL))  
             .build();  
  
  
       var content  =  
          given().spec(specification)  
                .contentType(TestConfigs.CONTENT_TYPE_JSON)  
                .body(person)  
                .when()  
                   .post()  
                .then()  
                   .statusCode(200)  
                .extract()  
                   .body()  
                      .asString();  
  
       PersonVO createdPerson = objectMapper.readValue(content, PersonVO.class);  
       person = createdPerson;  
  
       assertTrue(createdPerson.getId()> 0);  
  
       assertNotNull(createdPerson.getId());  
       assertNotNull(createdPerson.getFirstName());  
       assertNotNull(createdPerson.getLastName());  
       assertNotNull(createdPerson.getGender());  
       assertNotNull(createdPerson.getAddress());  
  
       assertEquals("Lambu", createdPerson.getFirstName());  
       assertEquals("Caolho", createdPerson.getLastName());  
       assertEquals("Sousa, Paraiba, Brasil", createdPerson.getAddress());  
       assertEquals("Macho", createdPerson.getGender());  
  
  
    }  
  
    private void mockPerson() {  
  
       person.setFirstName("Lambu");  
       person.setLastName("Caolho");  
       person.setAddress("Sousa, Paraiba, Brasil");  
       person.setGender("Macho");  
  
    }  
  
}
```

