Inicialmente adicionaremos a seguinte dependencia no spring
```xml
<dependency>  
    <groupId>org.springframework.hateoas</groupId>  
    <artifactId>spring-hateoas</artifactId>  
</dependency>
```

e depois configuraremos nossa classe de entidade, nesse caso a classe PersonVO, essa classe precisa agora estender a RepresentationModel ficando assim: 

```java
public class PersonVO extends RepresentationModel implements Serializable 
```

nota: a classe não pode ter o atributo id

na classe de serviço importaremos as seguintes bibliotecas

```java
import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.linkTo;  
import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.methodOn;
```

em seguida faremos o teste usando um exemplo de um findById implementado usando o JPA

```java
public PersonVO findById(Long id) throws Exception {  
    logger.info("FINDING ONE PERSON!");  
  
    var entity = ModelMapperUtil.parseObject(repository.findById(id).orElseThrow(() -> new ResourceNotFoundException("No records found for this id.")), PersonVO.class);  
  
    PersonVO vo = ModelMapperUtil.parseObject(entity, PersonVO.class) ;  
    vo.add(linkTo(methodOn(PersonController.class).findById(id)).withSelfRel());  
  
    return vo;  
}
```

o resultado será 

```json
{
    "address": "Queimadas - PB - BR",
    "first_name": "Athirson",
    "last_name": "Pequeno",
    "gender": "male",
    "key": 2,
    "_links": {
        "self": {
            "href": "http://localhost:8080/api/person/v1/2"
        }
    }
}
```

