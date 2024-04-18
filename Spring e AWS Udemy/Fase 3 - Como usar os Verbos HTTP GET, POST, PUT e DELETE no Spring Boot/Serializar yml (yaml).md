Começando, adicione primeiro a seguinte dependencia
```xml
<dependency>  
    <groupId>com.fasterxml.jackson.dataformat</groupId>  
    <artifactId>jackson-dataformat-yaml</artifactId>  
</dependency>
```

Em seguida criaremos a classe YamlJackson2HttpMessageConverter no pacote `*.serialization.converter` essa classe estende a classe AbstractJackson2HttpMessageConverter  

essa classe terá o seguinte construtor 

```java
protected YamlJackson2HttpMessageConverter() {  
    super(new YAMLMapper().setSerializationInclusion(  
            JsonInclude.Include.NON_NULL),  
            MediaType.parseMediaType("application/x-yaml")  
    );  
}
```

na classe WebConfig adicionaremos o seguinte atributo `private static final MediaType MEDIA_TYPE_APPLICATION_YAML = MediaType.valueOf("application/x-yaml");` em seguida deixaremos nosso configurer assim

```java
 configurer  
            .favorParameter(false)  
            .ignoreAcceptHeader(false)  
            .useRegisteredExtensionsOnly(false)  
            .defaultContentType(MediaType.APPLICATION_JSON)  
            .mediaType("json", MediaType.APPLICATION_JSON)  
            .mediaType("xml", MediaType.APPLICATION_XML)  
            .mediaType("x-yaml", MEDIA_TYPE_APPLICATION_YAML)  
    ;  
}
```

em seguida no controler adicionaremos `"application/x-yaml"` nos produces e consumes das requisições 