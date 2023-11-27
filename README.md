[![NPM](https://img.shields.io/npm/l/react)](https://github.com/lucarauj/API-Upload-e-Download-Arquivos/blob/main/LICENSE)

# API de Upload e Download de Arquivos

<br>

## Dependências

- Spring Web

<br>

## Anotações

- [@Configuration](https://github.com/lucarauj/Anotacoes-Spring-Framework)
- [@ConfigurationProperties](https://github.com/lucarauj/Anotacoes-Spring-Framework)
- [@Controller](https://github.com/lucarauj/Anotacoes-Spring-Framework)
- [@GetMapping](https://github.com/lucarauj/Anotacoes-Spring-Framework)
- [@PathVariable](https://github.com/lucarauj/Anotacoes-Spring-Framework)
- [@PostMapping](https://github.com/lucarauj/Anotacoes-Spring-Framework)
- [@RequestParam](https://github.com/lucarauj/Anotacoes-Spring-Framework)
- [@RequestMapping](https://github.com/lucarauj/Anotacoes-Spring-Framework)

<br>

## Métodos e funções utilizados

<br>

## uploadFile

<br>

```
Paths.get(fileStorageProperties.getUploadDir()).toAbsolutePath().normalize()
```

- Paths.get(): permite criar um objeto Path a partir de uma ou mais partes de caminho especificadas como argumentos;
- fileStorageProperties.getUploadDir(): obtém o diretório de upload da instância de fileStorageProperties;
- toAbsolutePath(): método chamado no objeto Path retornado que converte o caminho relativo (se houver) em um caminho absoluto;
- normalize(): método chamado no objeto Path para normalizar o caminho removendo quaisquer partes redundantes ou ".." (pontos de volta) do caminho, tornando o caminho mais limpo e consistente.

<br>
<br>

```
MultipartFile
```

- classe usada para representar arquivos que são enviados por meio de uma solicitação HTTP, como um formulário de upload de arquivo em uma aplicação da web.

<br>
<br>

```
StringUtils.cleanPath(file.getOriginalFilename())
```

- usado para garantir que o nome do arquivo seja seguro e adequado para ser usado em operações de armazenamento de arquivos, como a construção de caminhos de arquivo, sem representar riscos de segurança ou problemas de compatibilidade com sistemas de arquivos;

<br>
<br>

```
Path targetLocation = fileStorageLocation.resolve(fileName);
file.transferTo(targetLocation);
```

- fileStorageLocation: local de armazenamento principal configurado para armazenar os arquivos, inicializado anteriormente no construtor da classe FileStorageController;
- resolve(fileName): combina o caminho fileStorageLocation com o nome do arquivo (fileName) para criar o caminho completo onde o arquivo será colocado.
- file.transferTo(targetLocation): o método transferTo() do objeto MultipartFile (file) move o conteúdo do arquivo para o local de armazenamento especificado em targetLocation.

<br>
<br>

```
String fileDownloadUri = ServletUriComponentsBuilder.fromCurrentContextPath()
.path("/api/files/download/")
.path(fileName)
.toUriString();
```

- ServletUriComponentsBuilder.fromCurrentContextPath(): cria uma instância de ServletUriComponentsBuilder que ajuda a construir URIs;
- fromCurrentContextPath(): inicializa o construtor com o contexto atual da solicitação, o que significa que a URI resultante incluirá o mesmo contexto (como a raiz do aplicativo) da solicitação atual;
- .path("/api/files/download/"): adiciona um segmento de caminho à URI. Nesse caso, está adicionando "/api/files/download/" à URI em construção, representando a rota onde os arquivos podem ser baixados;
- .path(fileName): o nome do arquivo (fileName) é adicionado à URI. Isso significa que a URI resultante incluirá o nome do arquivo que foi carregado e pode ser usado para identificar e recuperar o arquivo posteriormente;
- .toUriString(): converte a URI construída em uma representação de string. O resultado é armazenado na variável fileDownloadUri;

<br>
<br>

## downloadFile

<br>

```
@GetMapping("download/{fileName:.+}")
```

- ":.+": é uma expressão regular que garante que a variável {fileName} capture todo o nome do arquivo, incluindo a extensão;

<br>
<br>

```
public ResponseEntity<Resource> downloadFile(@PathVariable String fileName, HttpServletRequest request) throws IOException
```

- Aceita dois parâmetros: fileName, que é o nome do arquivo a ser baixado, e request, que é uma instância de HttpServletRequest usada para obter informações sobre a solicitação HTTP;

<br>
<br>

```
Path filePath = fileStorageLocation.resolve(fileName).normalize()
```

- cria um objeto Path chamado filePath que representa o caminho completo do arquivo a ser baixado. 
- resolve o caminho com base no diretório de armazenamento configurado anteriormente na classe e normaliza o caminho.

<br>
<br>

```
Resource resource = new UrlResource(filePath.toUri())
```

- cria um objeto Resource (geralmente uma UrlResource) com base no caminho do arquivo. Isso é usado para acessar e servir o arquivo.

<br>
<br>

```
String contentType = request.getServletContext().getMimeType(resource.getFile().getAbsolutePath())
```

- obtém o tipo de conteúdo (MIME type) do arquivo com base no seu caminho absoluto. 
- o tipo de conteúdo é usado para definir o cabeçalho Content-Type na resposta HTTP.

<br>
<br>

```
if (contentType == null) contentType = "application/octet-stream"
```

- define um valor padrão "application/octet-stream". 
- isso é comum para downloads de arquivos genéricos, quando o tipo de conteúdo não pode ser determinado com precisão.

<br>
<br>

```
ResponseEntity.ok()
.contentType(MediaType.parseMediaType(contentType))
.header(HttpHeaders.CONTENT_DISPOSITION, "attachment; fileName=\""
+ resource.getFilename() + "\"")
.body(resource)
```

- ResponseEntity.ok(): cria um objeto ResponseEntity com um status HTTP 200 (OK) [solicitação bem-sucedida]. A resposta conterá o arquivo para download.
- .contentType(MediaType.parseMediaType(contentType)): é definido o cabeçalho Content-Type da resposta HTTP. O tipo de conteúdo (MIME type) do arquivo é especificado usando o valor contentType, que foi obtido anteriormente. Isso informa ao cliente qual é o tipo de conteúdo do arquivo sendo enviado. 
- .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; fileName=\"" + resource.getFilename() + "\""): define o cabeçalho Content-Disposition. O valor "attachment" indica que o arquivo deve ser tratado como um anexo pelo navegador, o que normalmente resulta em uma caixa de diálogo de download sendo exibida para o usuário.
- resource.getFilename(): obtém o nome do arquivo do objeto Resource que representa o arquivo a ser baixado. Esse nome é usado no cabeçalho Content-Disposition para sugerir um nome de arquivo para o download.
- "attachment; fileName="" + resource.getFilename() + """: resulta em algo como "attachment; fileName="nome-do-arquivo.extensao"", onde "nome-do-arquivo.extensao" é o nome do arquivo real.
- .body(resource): o corpo da resposta HTTP é definido como o recurso (Resource) que representa o arquivo a ser baixado. Significa que o conteúdo do arquivo em si será retornado como parte do corpo da resposta HTTP.

<br>
<br>

```
(MalformedURLException e)
```

- captura exceções do tipo MalformedURLException que podem ocorrer ao criar o objeto UrlResource;

<br>
<br>

## listFiles

<br>

```
List<String> fileNames = Files.list(fileStorageLocation)
.map(Path::getFileName)
.map(Path::toString)
.collect(Collectors.toList());
```

- Files.list(fileStorageLocation): obtem um fluxo (Stream) de objetos Path que representam os arquivos e diretórios no diretório especificado (fileStorageLocation). 
- .map(Path::getFileName): método map usado para transformar cada objeto Path do fluxo em seu nome de arquivo. O Path::getFileName é uma referência a um método que extrai o nome do arquivo de um objeto Path.
- .map(Path::toString): aplica a transformação para cada nome de arquivo extraído no passo anterior, convertendo-os em strings.
- .collect(Collectors.toList()): A função collect é usada para coletar os resultados do fluxo de nomes de arquivo transformados em uma lista (List<String>).

<br>
<br>

## Utilizando o Postman

<br>

### Upload File

<p align="left"><img width="900px" src="https://github.com/lucarauj/API-Upload-e-Download-Arquivos/blob/main/images/uploadFile.png"/></p>

<br>

### Download File

<p align="left"><img width="800px" src="https://github.com/lucarauj/API-Upload-e-Download-Arquivos/blob/main/images/downloadFile(1).png"/></p>

<br>

### List Files

<p align="left"><img width="900px" src="https://github.com/lucarauj/API-Upload-e-Download-Arquivos/blob/main/images/listFiles.png"/></p>

<br>

## 👨‍🎓 Aluno

#### Lucas Araujo

<a href="https://www.linkedin.com/in/lucarauj"><img alt="lucarauj | LinkdeIN" width="40px" src="https://user-images.githubusercontent.com/43545812/144035037-0f415fc7-9f96-4517-a370-ccc6e78a714b.png" /></a>
