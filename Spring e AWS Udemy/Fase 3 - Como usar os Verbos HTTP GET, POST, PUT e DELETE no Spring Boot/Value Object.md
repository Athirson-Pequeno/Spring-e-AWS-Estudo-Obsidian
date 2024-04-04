Um "value object" é um conceito de programação orientada a objetos que se refere a um tipo de objeto cujo valor é determinado pelos dados que ele contém. Em outras palavras, um value object é imutável e sua identidade está diretamente ligada aos valores dos seus atributos.

Value objects são frequentemente usados em sistemas de software para representar conceitos que são identificados apenas por seus atributos, e não por uma identidade única. Por exemplo, um objeto que representa uma data (como "12 de abril de 2024") ou um objeto que representa um endereço (com atributos como rua, cidade, estado, etc.) podem ser value objects, pois sua identidade é definida pelos dados que eles contêm.

Algumas características comuns de value objects incluem:

1. **Imutabilidade:** Uma vez criado, um value object não pode ser modificado. Qualquer operação que aparentemente modifique o objeto na verdade cria um novo objeto com base nos valores existentes.

2. **Comparação baseada em valores:** Dois value objects são considerados iguais se tiverem os mesmos valores em seus atributos. A comparação não leva em conta a identidade do objeto.

3. **Sem identidade própria:** Ao contrário de outros tipos de objetos que podem ter uma identidade única (como um ID de banco de dados), value objects não têm uma identidade própria; sua identidade é derivada dos seus valores internos.

4. **Encapsulamento de comportamento:** Embora os value objects sejam imutáveis, podem ter métodos que realizam operações relacionadas aos seus dados internos. Esses métodos geralmente retornam um novo value object, mantendo a imutabilidade.

Em linguagens de programação como Java, C#, Python e muitas outras, é comum implementar value objects como classes simples que encapsulam seus dados e fornecem métodos para acessá-los de forma segura. Isso ajuda a garantir uma modelagem mais clara e robusta em muitos tipos de sistemas de software.


Exemplo de uma classe que utiliza a biblioteca ModelMapper prar converter um objeto em outro

```java
package br.com.tizo.mapper;  
  
import org.modelmapper.ModelMapper;  
  
import java.util.ArrayList;  
import java.util.List;  
  
public class Mapper {  
  
    private static ModelMapper mapper = new ModelMapper();  
  
    public static <O, D> D  parseObject(O origin, Class<D> destination) {  
  
        return mapper.map(origin, destination);  
    }  
    public static <O, D> List<D>  parseListObjects(List<O> origin, Class<D> destination) {  
        List<D> destinationObjects = new ArrayList<D>();  
  
        for (O o : origin){  
            destinationObjects.add(mapper.map(o, destination));  
        }  
  
        return destinationObjects;  
    }  
}
```

