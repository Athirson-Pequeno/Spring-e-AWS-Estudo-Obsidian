Paginação com pageble e query params 

Iremos modificar nosso findAll assim

```java
public ResponseEntity<Page<PersonVO>> findAll(@RequestParam(value = "page", defaultValue = "0") Integer page,  
                                              @RequestParam(value = "limit", defaultValue = "15") Integer limit,  
                                              ) {  
    Pageable pageable = PageRequest.of(page,limit);  
    return ResponseEntity.ok(service.findAll(pageable));  
}
```

adicionamos os parametros page e limit, nosso findAll agora recebe um Pageable

resumo copilot sobre o Pageable

Claro! O **Pageable** é uma interface do Spring Framework que permite a paginação de resultados em consultas de banco de dados. Ele é frequentemente usado em conjunto com o Spring Data JPA para facilitar a paginação e a ordenação dos dados.

Aqui estão os principais pontos sobre o uso do **Pageable** no Spring Boot:

1. **Spring Data JPA e Repositórios:**
    
    - Para usar o **Pageable**, primeiro precisamos criar um repositório que estenda a interface `PagingAndSortingRepository`. Por exemplo:
        
        ```java
        public interface ProductRepository extends PagingAndSortingRepository<Product, Integer> {
            List<Product> findAllByPrice(double price, Pageable pageable);
        }
        ```
        
    - Ao estender essa interface, obtemos os métodos `findAll(Pageable pageable)` e `findAll(Sort sort)` para paginação e ordenação.
2. **Criação de um objeto PageRequest:**
    
    - Para paginar os resultados, criamos um objeto `PageRequest` passando o número da página desejada e o tamanho da página:
        
        ```java
        Pageable firstPageWithTwoElements = PageRequest.of(0, 2);
        Pageable secondPageWithFiveElements = PageRequest.of(1, 5);
        ```
        
    - O índice da página começa em zero.
    - No Spring MVC, também podemos obter a instância do `Pageable` no controlador usando o Spring Data Web Support.
3. **Invocando o método do repositório:**
    
    - Uma vez que temos o objeto `PageRequest`, podemos passá-lo como argumento para o método do repositório:
        
        ```java
        Page<Product> allProducts = productRepository.findAll(firstPageWithTwoElements);
        List<Product> allTenDollarProducts = productRepository.findAllByPrice(10, secondPageWithFiveElements);
        ```
        
    - O método `findAll(Pageable pageable)` retorna por padrão um objeto `Page<T>`. No entanto, podemos escolher retornar um `Page<T>`, um `Slice<T>` ou uma `List<T>` em nossos métodos personalizados que retornam dados paginados.
4. **Informações adicionais:**
    
    - [Um objeto `Page<T>` não apenas contém a lista de produtos, mas também sabe sobre o número total de páginas disponíveis](https://www.baeldung.com/spring-data-jpa-pagination-sorting)[1](https://www.baeldung.com/spring-data-jpa-pagination-sorting).
    - [O **Pageable** é implementado usando LIMIT e OFFSET (ou uma abordagem semelhante) no banco de dados SQL subjacente](https://stackoverflow.com/questions/69373467/how-does-spring-boot-work-with-pageable-in-controllers-method-argument)[2](https://stackoverflow.com/questions/69373467/how-does-spring-boot-work-with-pageable-in-controllers-method-argument).

Espero que isso esclareça o uso do **Pageable** no Spring Boot! Se você tiver mais alguma dúvida, fique à vontade para perguntar. 😊

nesse exemplo nossa solicitações fica assim

{{baseUrl}}/api/person/v1?page=5&limit=20

pagina 5 
com 20 elementos cada

definindo ordem, o primeiro parameto do Sort.by é a ordem de retornar os dados e o segundo é por qual atributo vai ser ordenado

```java
var sortDirection = "desc".equalsIgnoreCase(direction) ? Sort.Direction.DESC : Sort.Direction.ASC;  
  
Pageable pageable = PageRequest.of(page,limit,Sort.by(sortDirection, "firstName"));  
return ResponseEntity.ok(service.findAll(pageable));
```

adicionando HATEOAS no nosso findAll paged

```java
@Autowired  
PagedResourcesAssembler<PersonVO> assembler;  
  
public PagedModel<EntityModel<PersonVO>> findAll(Pageable pageable) {  
  
    var personPage = repository.findAll(pageable);  
    var personVOsPage = personPage.map(p-> ModelMapperUtil.parseObject(p, PersonVO.class));  
    personVOsPage.map(personVO -> personVO.add(linkTo(methodOn(PersonController.class).findById(personVO.getKey())).withSelfRel()));  
  
    Link link = linkTo(methodOn(PersonController.class).findAll(pageable.getPageNumber(),pageable.getPageSize(), "asc")).withSelfRel();  
    return assembler.toModel(personVOsPage, link);  
}
```

é preciso da injeção do PagedResourcesAssembler e criar um link para adicionar no retorno, também é necessário retornar um `PagedModel<EntityModel<?>>` a interrogação deve ser substituída pela classe que de retorno do findAll

query personalizada para buscar por nome 

```java
@Query("SELECT p FROM Person p WHERE p.firstName LIKE LOWER(CONCAT ('%',:firstName,'%'))")  
Page<Person> findPersonsByName(@Param("firstName") String firstName, Pageable pageable);
```

o trecho LIKE faz retornar os itens que contenham parte do trecho não necessariamente o nome igual