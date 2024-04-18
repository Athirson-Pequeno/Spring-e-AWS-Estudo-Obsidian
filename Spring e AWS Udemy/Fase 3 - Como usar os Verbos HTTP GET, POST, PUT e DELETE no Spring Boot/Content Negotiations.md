Em Java Spring, a negociação de conteúdo (content negotiation) refere-se ao processo pelo qual um servidor e um cliente decidem sobre o formato de dados a serem trocados durante uma comunicação HTTP. Isso geralmente acontece em aplicativos web onde o servidor pode oferecer diferentes representações de um recurso (por exemplo, JSON, XML, HTML) e o cliente pode solicitar o formato desejado.

Existem várias maneiras de lidar com a negociação de conteúdo em uma aplicação Java Spring:

1. **Content-Type e Accept Headers:** O servidor pode verificar os cabeçalhos HTTP `Content-Type` e `Accept` para determinar o formato dos dados de entrada e saída, respectivamente. O Spring MVC fornece suporte para isso por meio de anotações como `@RequestBody` e `@ResponseBody`.

2. **ContentNegotiationManager:** O Spring Framework inclui o `ContentNegotiationManager` para configurar a negociação de conteúdo. Você pode definir diferentes estratégias de resolução de mídia (Media Types) com base em extensões de arquivo, cabeçalhos ou parâmetros de solicitação.

3. **MessageConverters:** O Spring utiliza `HttpMessageConverters` para converter objetos Java em formatos específicos de mídia e vice-versa. Por exemplo, um `MappingJackson2HttpMessageConverter` pode converter objetos Java em JSON e vice-versa.

Aqui está um exemplo simples de como você pode configurar a negociação de conteúdo em uma aplicação Java Spring:

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        configurer
            .defaultContentType(MediaType.APPLICATION_JSON)
            .mediaType("json", MediaType.APPLICATION_JSON)
            .mediaType("xml", MediaType.APPLICATION_XML);
    }

    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        converters.add(new MappingJackson2HttpMessageConverter());
        converters.add(new Jaxb2RootElementHttpMessageConverter());
    }
}
```

Neste exemplo, estamos configurando a negociação de conteúdo para JSON e XML. O Spring usará o `MappingJackson2HttpMessageConverter` para JSON e o `Jaxb2RootElementHttpMessageConverter` para XML.

Dentro dos controladores Spring, você pode usar anotações como `@RequestMapping` e `@ResponseBody` para indicar o formato de resposta desejado ou utilizar o `HttpServletResponse` diretamente para definir o cabeçalho `Content-Type`.


para poder serializar em JSON e em XML adicione essa dependencia

```xml
<dependency>  
    <groupId>com.fasterxml.jackson.dataformat</groupId>  
    <artifactId>jackson-dataformat-xml</artifactId>  
</dependency>
```


feito isso criaremos um pacote config e uma classe chamada WebConfig, essa classe vai implementar a WebMvcConfigurer em seguida colocaremos a anotação @Configuration ela serve para dizer ao Spring que essa classe contem configurações que ele dever ler antes de subir a aplicação. Continuando... agora nós vamos sobreescrever o seguinte metodo configureContentNegotiation e fazer a seguinte configuração: 

```java
@Override  
public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {  
    configurer
	    .favorParameter(true)  //true = aceita parametros
		.parameterName("mediaType")  //nome dos parametros
		.ignoreAcceptHeader(true)  //ignorar o accept header
		.useRegisteredExtensionsOnly(false)  //permite usar todas as extensoes
		.defaultContentType(MediaType.APPLICATION_JSON)  //media type pardrao
		.mediaType("json", MediaType.APPLICATION_JSON)  //media type JSON
		.mediaType("xml", MediaType.APPLICATION_XML);  //media type XML
}
```


no controller vamos mudar os produces e consumes das anotações de request para
```java
@PostMapping(  
        produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE},  
        consumes = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})  
public PersonVO create(@RequestBody PersonVO person) throws Exception {  
    return service.create(person);  
}
```

no postman a solicitação vai fica `http://localhost:8080/api/person/v1/2?mediaType=xml`

o método acima é para query params, abaixo vemos como fazer essa solicitação pelo header.

```java
configurer  
        .favorParameter(false)  
        .ignoreAcceptHeader(false)  
        .useRegisteredExtensionsOnly(false)  
        .defaultContentType(MediaType.APPLICATION_JSON)  
        .mediaType("json", MediaType.APPLICATION_JSON)  
        .mediaType("xml", MediaType.APPLICATION_XML);
```

solicitando no postman, a configuração do controler é a mesma

![[Pasted image 20240418174911.png]]


[[Serializar yml (yaml)]]