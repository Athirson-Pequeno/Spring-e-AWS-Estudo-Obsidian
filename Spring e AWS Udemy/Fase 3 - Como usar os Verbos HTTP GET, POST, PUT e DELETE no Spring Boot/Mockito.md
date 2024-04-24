Mockito é uma ferramenta de testes, para implementar no seu projeto coloque a seguinte dependencia no pom.xml

```xml
<dependency>  
    <groupId>org.mockito</groupId>  
    <artifactId>mockito-core</artifactId>  
    <scope>test</scope>  
</dependency>
```

se estiver no intelij vá na classe que vc quer testar e implemente usando a sugestão da lampada
![[Pasted image 20240424164038.png]]

![[Pasted image 20240424164120.png]]

selecione os metodos e ok, iremos usar o JUnit5 para testar

Colocaremos as seguintes anotações na classe de teste

```java
@TestInstance(TestInstance.Lifecycle.PER_CLASS)  
@ExtendWith(MockitoExtension.class)  
class PersonServicesTest 
```

criaremos em seguida um Objeto mockado e um serviço mockado 


```java
MockPerson input;  
@InjectMocks  
private PersonServices service;
```

na função setUp do teste deixaremos da seguinte forma

```java
@BeforeEach  
void setUpMocks() {  
    input = new MockPerson();  
    MockitoAnnotations.openMocks(this);  
}
```

exemplo de teste

```java
@Test  
void findById() {  
    Person person = input.mockEntity();  
    person.setId(1L);  
    when(repository.findById(1L)).thenReturn(Optional.of(person));  
    fail("Not yet implemented");  
}
```

explicando

primeiramente criamos um objeto person a partir de um mock que vem de uma função da clasee MockPerson

em seguida setamos um id para esse objeto

em seguida usaremo o when, antes de usar precisamos adicionar o seguinte import 
`import static org.mockito.Mockito.when;`

o when vai servir para dizer ao projeto que quando usarmos o repository.findById ele vai usar um mock não o repositorio real e vai retornar esse optional.of(person)

esse repository foi implementado da seguinte forma

```java
@Mock  
PersonRepository repository;
```

nosso primeiro teste ficará assim

```java
@Test  
void findById() {  
    Person person = input.mockEntity(1);  
    person.setId(1L);  
    when(repository.findById(1L)).thenReturn(Optional.of(person));  
  
    var result = service.findById(1L);  
    assertNotNull(result);  
    assertNotNull(result.getKey());  
    assertNotNull(result.getLinks());  
    assertTrue(result.toString().contains("</api/person/v1/1>;rel=\"self\""));  
    assertEquals("Addres Test1", result.getAddress());  
    assertEquals("First Name Test1", result.getFirstName());  
    assertEquals("Last Name Test1", result.getLastName());  
    assertEquals("Female", result.getGender());  
  
}
```

