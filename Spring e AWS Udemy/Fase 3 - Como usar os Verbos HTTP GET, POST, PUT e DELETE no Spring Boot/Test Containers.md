começaremos colocando as seguintes dependencias

```xml
<dependency>  
    <groupId>org.testcontainers</groupId>  
    <artifactId>mysql</artifactId>  
    <version>${testcontainers.version}</version>  
    <scope>test</scope>  
</dependency>
<dependency>  
    <groupId>io.rest-assured</groupId>  
    <artifactId>rest-assured</artifactId>  
    <version>${rest-assured.version}</version>  
    <scope>test</scope>  
</dependency>
```

na nossa pasta de teste criaremos uma pagina resources e copiaremos no application.yml para ela
e adicionaremos a seguinde propriedade

```yml
server:  
  port: 8888
```

e removeremos as seguintes linhas

```yml
url: jdbc:mysql://localhost:3306/rest_with_spring_boot_erudio?useTimezone=true&serverTimezone=UTC  
username: root  
password: 36412
```

em seguida dentro do pacote test/config criaremos a classe TestConfigs

nela terá a seguinte propriedad

```java
public static final int SERVER_PORT = 8888;
```

em seguida criaremos a seguinte classe

```java
package br.com.tizo.integrationtests.testcontainers;  
  
public class AbstractIntegrationTest {  
}
```

ela recebe a seguinte anotação @ContextConfiguration(initializers = AbstractIntegrationTest.Initializer.class) a classe `AbstractIntegrationTest.Initializer.class` terá que ser criada por nós

a estrutura dessa classe é a seguinte

```java
package br.com.tizo.integrationtests.testcontainers;  
  
import org.springframework.context.ApplicationContextInitializer;  
import org.springframework.context.ConfigurableApplicationContext;  
import org.springframework.core.env.ConfigurableEnvironment;  
import org.springframework.core.env.MapPropertySource;  
import org.springframework.test.context.ContextConfiguration;  
import org.testcontainers.containers.MySQLContainer;  
import org.testcontainers.lifecycle.Startable;  
import org.testcontainers.lifecycle.Startables;  
  
import java.util.Map;  
import java.util.stream.Stream;  
  
  
@ContextConfiguration(initializers = AbstractIntegrationTest.Initializer.class)  
public class AbstractIntegrationTest {  
  
    public static class Initializer implements ApplicationContextInitializer<ConfigurableApplicationContext> {  
        static MySQLContainer<?> mySQLContainer = new MySQLContainer<>("mysql:8.4.0");  
  
         private static void startContainers(){  
             Startables.deepStart(Stream.of(mySQLContainer)).join();  
         }  
  
        @SuppressWarnings({"unchecked","rawtypes"})  
        @Override  
        public void initialize(ConfigurableApplicationContext applicationContext) {  
            startContainers();  
            ConfigurableEnvironment environment = applicationContext.getEnvironment();  
            MapPropertySource testcontainers = new MapPropertySource(  
                    "testcontainers",  
                    (Map) createConnectionConfiguration());  
            environment.getPropertySources().addFirst(testcontainers);  
        }  
  
        private Map<String,String> createConnectionConfiguration() {  
  
            return Map.of(  
                    "spring.datasource.url", mySQLContainer.getJdbcUrl(),  
                    "spring.datasource.username", mySQLContainer.getUsername(),  
                    "spring.datasource.password", mySQLContainer.getPassword()  
            );  
        }  
    }  
  
      
}
	```


Iniciando testes

criaremos a seguinte classe

```java
package br.com.tizo.integrationtests.testcontainers;  
  
import org.springframework.context.ApplicationContextInitializer;  
import org.springframework.context.ConfigurableApplicationContext;  
import org.springframework.core.env.ConfigurableEnvironment;  
import org.springframework.core.env.MapPropertySource;  
import org.springframework.test.context.ContextConfiguration;  
import org.testcontainers.containers.MySQLContainer;  
import org.testcontainers.lifecycle.Startable;  
import org.testcontainers.lifecycle.Startables;  
  
import java.util.Map;  
import java.util.stream.Stream;  
  
  
@ContextConfiguration(initializers = AbstractIntegrationTest.Initializer.class)  
public class AbstractIntegrationTest {  
  
    public static class Initializer implements ApplicationContextInitializer<ConfigurableApplicationContext> {  
        static MySQLContainer<?> mySQLContainer = new MySQLContainer<>("mysql:8.0.29");  
  
         private static void startContainers(){  
             Startables.deepStart(Stream.of(mySQLContainer)).join();  
         }  
  
        @SuppressWarnings({"unchecked","rawtypes"})  
        @Override  
        public void initialize(ConfigurableApplicationContext applicationContext) {  
            startContainers();  
            ConfigurableEnvironment environment = applicationContext.getEnvironment();  
            MapPropertySource testcontainers = new MapPropertySource(  
                    "testcontainers",  
                    (Map) createConnectionConfiguration());  
            environment.getPropertySources().addFirst(testcontainers);  
        }  
  
        private Map<String,String> createConnectionConfiguration() {  
  
            return Map.of(  
                    "spring.datasource.url", mySQLContainer.getJdbcUrl(),  
                    "spring.datasource.username", mySQLContainer.getUsername(),  
                    "spring.datasource.password", mySQLContainer.getPassword()  
            );  
        }  
    }  
  
      
}
```

e depois executaremos ela, lembre-se que o docker deve estar aberto no computador