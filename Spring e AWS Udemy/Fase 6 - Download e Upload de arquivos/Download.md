
## Service

Para realizar o download nós iremos adicionar o seguinte método no nosso service.

```java
public Resource loadFileAsResource(String filename) {  
    try {  
        Path filePath = this.fileStorageLocation.resolve(filename).normalize();  
        Resource resource = new UrlResource(filePath.toUri());  
        if (resource.exists()) {  
            return resource;  
        } else {  
            throw new MyFileNotFoundException("File not found " + filename);  
        }  
    } catch (Exception e) {  
        throw new MyFileNotFoundException("File not found " + filename, e);  
    }  
}
```

## Controller

Já o nosso controller, fica assim.

```java
	@GetMapping("/downloadFile/{filename:.+}")  
public ResponseEntity<Resource> downloadFile(@PathVariable("filename") String filename, HttpServletRequest request) {  
  
    logger.info("Reading a file.");  
  
    Resource resource = fileStorageService.loadFileAsResource(filename);  
    String contentType = "";  
  
    try {  
        contentType = request.getServletContext().getMimeType(resource.getFile().getAbsolutePath());  
  
    }catch (Exception e){  
        logger.info("Could not determine file type.");  
    }  
  
    if (contentType.isBlank()) contentType = "application/octet-stream";  
  
    return ResponseEntity.ok()  
            .contentType(MediaType.parseMediaType(contentType))  
            .header(  
                    HttpHeaders.CONTENT_DISPOSITION,  
                    "attachment; filename=\"" + resource.getFilename() + "\"")  
            .body(resource);  
}
```

