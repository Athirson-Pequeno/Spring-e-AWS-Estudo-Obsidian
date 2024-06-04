
O conceito base para o upload é o multipart file, que é:

Um "multipart file" em HTTP se refere a um tipo de conteúdo enviado em uma requisição ou resposta HTTP que consiste em várias partes distintas, cada uma com seu próprio conjunto de dados. Esse tipo de conteúdo é geralmente usado quando é necessário transmitir mais do que apenas um único tipo de dado, como texto ou um arquivo, em uma única requisição ou resposta HTTP.

O formato "multipart" permite que diferentes partes de dados, como texto, imagens, vídeos ou outros tipos de arquivos, sejam enviadas simultaneamente em uma única requisição HTTP. Cada parte é separada por uma delimitação definida, geralmente especificada no cabeçalho da requisição ou resposta.

Isso é comumente usado em formulários da web que permitem o upload de arquivos, onde o conteúdo do formulário inclui não apenas texto, mas também um ou mais arquivos binários. O navegador envia esses dados para o servidor em um formato "multipart", permitindo que o servidor processe as diferentes partes separadamente.

No geral, o uso de "multipart" em HTTP é uma maneira eficiente de transmitir dados variados e complexos em uma única transação HTTP.


Para começar a configurar isso na API iremos no application.yml e vamos adicionar a seguinte configuração:

```yaml
spring:
	servlet:  
	  multipart:  
	    enabled: true  
	    file-size-threshold: 2KB 
	    max-file-size: 200MB  
	    max-request-size: 215MB
```

enabled: ativa o multipart
file-size-threshold: tamanho que acumulamos em memória antes de salvar em disco
max-file-size: tamanho máximo do arquivo
max-request-size: tamanho máximo da requisição

Atenção esse dados são para aprendizado cuidado para definir os valores corretos na sua API

Ainda no application.yml colocaremos a seguinte propriedade personalizada:

```yml
file:  
  upload-dir: /Udemy spring aws/Arquivos
```

que nesse caso é o local que salvaremos os arquivos.


## Config


No pacote de config, criaremos a classe abaixo que serve para buscar o diretório que configuramos no application.yml

```java
@Configuration  
@ConfigurationProperties(prefix = "file")  
public class FileStorageConfig {  
  
    private String uploadDir;  
  
    public String getUploadDir() {  
        return uploadDir;  
    }  
  
    public void setUploadDir(String uploadDir) {  
        this.uploadDir = uploadDir;  
    }  
  
}
```

Em seguida criaremos o nosso VO e Exceptions

Nossos modelos de Exceptions: 

1- Erro no nosso Storage 
```java
@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)  
public class FileStorageException extends RuntimeException{  
  
    @Serial  
    private static final long serialVersionUID = 1L;  
  
    public FileStorageException(String ex) {  
       super(ex);  
    }  
  
    public FileStorageException(String ex, Throwable cause) {  
       super(ex, cause);  
    }  
}
```

2- Erro no nosso arquivo
```java
@ResponseStatus(HttpStatus.NOT_FOUND)  
public class MyFileNotFoundException extends RuntimeException{  
  
    @Serial  
    private static final long serialVersionUID = 1L;  
  
    public MyFileNotFoundException(String ex) {  
       super(ex);  
    }  
  
    public MyFileNotFoundException(String ex, Throwable cause) {  
       super(ex, cause);  
    }  
  
}
```

Nosso VO de resposta

```java
public class UploadFileResponseVO implements Serializable {  
  
    @Serial  
    private static final long serialVersionUID = 1L;  
  
    private String fileName;  
    private String fileDownloadUri;  
    private String fileType;  
    private long size;  
  
    public UploadFileResponseVO() {  
    }  
  
    public UploadFileResponseVO(String fileName, String fileDownloadUri, String fileType, long size) {  
        this.fileName = fileName;  
        this.fileDownloadUri = fileDownloadUri;  
        this.fileType = fileType;  
        this.size = size;  
    }  
  
    public String getFileName() {  
        return fileName;  
    }  
  
    public void setFileName(String fileName) {  
        this.fileName = fileName;  
    }  
  
    public String getFileDownloadUri() {  
        return fileDownloadUri;  
    }  
  
    public void setFileDownloadUri(String fileDownloadUri) {  
        this.fileDownloadUri = fileDownloadUri;  
    }  
  
    public String getFileType() {  
        return fileType;  
    }  
  
    public void setFileType(String fileType) {  
        this.fileType = fileType;  
    }  
  
    public long getSize() {  
        return size;  
    }  
  
    public void setSize(long size) {  
        this.size = size;  
    }  
  
  
    @Override  
    public boolean equals(Object o) {  
        if (this == o) return true;  
        if (!(o instanceof UploadFileResponseVO that)) return false;  
        return size == that.size && Objects.equals(fileName, that.fileName) && Objects.equals(fileDownloadUri, that.fileDownloadUri) && Objects.equals(fileType, that.fileType);  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(fileName, fileDownloadUri, fileType, size);  
    }  
}
```



## Service

Vamos configurar o serviço que vai trabalhar com esses arquivos.

```java
@Service  
public class FileStorageService {  
  
    private final Path fileStorageLocation;  
  
    @Autowired  
    public FileStorageService(FileStorageConfig fileStorageConfig) {  
  
        Path path = Paths.get(fileStorageConfig.getUploadDir()).toAbsolutePath().normalize();  
  
        this.fileStorageLocation = path;  
  
        try {  
            Files.createDirectories(this.fileStorageLocation);  
        } catch (Exception e) {  
            throw new FileStorageException("Could not create the directory where the uploaded files will be stored!", e);  
        }  
    }  
  
    public String storeFile(MultipartFile file) {  
        String filename = StringUtils.cleanPath(file.getOriginalFilename());  
        try {  
            if (filename.contains(".."))  
                throw new FileStorageException("Sorry! Filename contains invalid path sequence: " + filename);  
  
            Path targetLocation = this.fileStorageLocation.resolve(filename);  
            Files.copy(file.getInputStream(), targetLocation, StandardCopyOption.REPLACE_EXISTING);  
  
            return "/" + filename;  
        } catch (Exception e) {  
            throw new FileStorageException("Could not store the file " + filename + ". Please  try again!", e);  
        }  
    }
```



## Controller

Nosso controller é bem simples, ele ficará assim:

```java
@Tag(name = "File Endpoint")  
@RestController  
@RequestMapping("/api/file/v1")  
public class FileController {  
  
    private Logger logger = Logger.getLogger(FileController.class.getName());  
  
    @Autowired  
    private FileStorageService fileStorageService;  
  
    @PostMapping("/uploadFile")  
    public UploadFileResponseVO uploadFile(@RequestParam("file") MultipartFile file) {  
  
        logger.info("Storing file.");  
  
        var filename = fileStorageService.storeFile(file);  
        String fileDownloadUri = ServletUriComponentsBuilder.fromCurrentContextPath().path("/api/file/v1/downloadFile").path(filename).toUriString();  
  
        return new UploadFileResponseVO(filename, fileDownloadUri, file.getContentType(), file.getSize());  
    }  
}
```

Esse controller só aceita um arquivo por vez, o método para aceitar vários é assim.

```java
  
@PostMapping("/uploadMultipleFiles")  
public List<UploadFileResponseVO> uploadMultipleFiles(@RequestParam("files") MultipartFile[] files) {  
  
    logger.info("Storing files.");  
  
   return Arrays.asList(files).stream().map(file -> uploadFile(file)).collect(Collectors.toList());  
}
```

Basicamente, recebemos uma lista de arquivos e fazemos um loop de repetição chamando o método uploadFile.