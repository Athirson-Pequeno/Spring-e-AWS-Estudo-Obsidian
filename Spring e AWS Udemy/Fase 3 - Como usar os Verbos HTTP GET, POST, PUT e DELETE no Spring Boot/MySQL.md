para conectar no banco de dados mysql vc tem que configurar o application.yml

```yaml
spring:  
  application:  
    name: rest-with-spring-boot-and-java-erudio  
  datasource:  
    dbcp2:  
      driver-class-name: com.mysql.cj.jdbc.Driver  
      url: jdbc:mysql://localhost:3306/rest_with_spring_boot_erudio?useTimezone=true&serverTimezone=UTC  
      username: root  
      password: 36412  
  jpa:  
    hibernate:  
      ddl-auto: update  
    properties:  
      hibernate:  
        dialect: org.hibernate.dialect.MySQL8Dialect  
      show-sql: false
      
```

o trecho abaixo serve para configurar a conexão com o banco de dados, ele configura o local onde o banco está, o driver certo, o usuário e a senha.


```
datasource:  
    dbcp2:  
      driver-class-name: com.mysql.cj.jdbc.Driver  
      url: jdbc:mysql://localhost:3306/rest_with_spring_boot_erudio?useTimezone=true&serverTimezone=UTC  
      username: root  
      password: 36412  
      
```


NESSE TRECHO OUVE UM ERRO
```
Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.
```
FOI RESOLVIDO DEIXANDO O CÓDIGO ASSIM


```yaml
spring:  
  application:  
    name: rest-with-spring-boot-and-java-erudio  
  datasource:  
      driver-class-name: com.mysql.cj.jdbc.Driver  
      url: jdbc:mysql://localhost:3306/rest_with_spring_boot_erudio?useTimezone=true&serverTimezone=UTC  
      username: root  
      password: 36412  
  jpa:  
    hibernate:  
      ddl-auto: update  
    properties:  
      hibernate:  
        dialect: org.hibernate.dialect.MySQL8Dialect  
      show-sql: false
      
```

EXPLICAÇÃO

![[Pasted image 20240327190859.png]]


o trecho abaixo configura o JPA, uma dependecia do spring, o atributo que deve se atentar é o dll-auto, no exemplo ele está como update, mas é recomendado deixar em none, veja oq cada atributo faz.

- validate: valida o esquema, sem fazer alterações no banco de dados.
- create-only: a criação do banco de dados será gerada.
- drop: a exclusão do banco de dados será gerada.
- update: atualiza o esquema.
- create: cria o esquema, destruindo dados anteriores.
- create-drop: descarta o esquema quando a SessionFactory é fechada explicitamente, geralmente quando a aplicação é encerrada.
- none: não faz nada com o esquema, sem fazer alterações no banco de dados.


```yml
jpa:  
    hibernate:  
      ddl-auto: update  
    properties:  
      hibernate:  
        dialect: org.hibernate.dialect.MySQL8Dialect  
      show-sql: false
      
```

