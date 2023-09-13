# API de Upload e Download de Arquivos

<br>

## Dependências

- Spring Web

<br>

## Anotações

- @Configuration
- @COnfigurationProperties
- @Controller
- @PostMapping
- @RequestParam
- @RequestMapping

<br>

## Métodos e funções utilizados:

<br>

```Paths.get(fileStorageProperties.getUploadDir()).toAbsolutePath().normalize()```

- Paths.get(): permite criar um objeto Path a partir de uma ou mais partes de caminho especificadas como argumentos;
- fileStorageProperties.getUploadDir(): obtém o diretório de upload da instância de fileStorageProperties;
- toAbsolutePath(): método chamado no objeto Path retornado que converte o caminho relativo (se houver) em um caminho absoluto;
- normalize(): método chamado no objeto Path para normalizar o caminho removendo quaisquer partes redundantes ou ".." (pontos de volta) do caminho, tornando o caminho mais limpo e consistente.

<br>

```MultipartFile```

- classe usada para representar arquivos que são enviados por meio de uma solicitação HTTP, como um formulário de upload de arquivo em uma aplicação da web.

<br>

```StringUtils.cleanPath(file.getOriginalFilename())```

- usado para garantir que o nome do arquivo seja seguro e adequado para ser usado em operações de armazenamento de arquivos, como a construção de caminhos de arquivo, sem representar riscos de segurança ou problemas de compatibilidade com sistemas de arquivos;

<br>

```
Path targetLocation = fileStorageLocation.resolve(fileName);
file.transferTo(targetLocation);
```

- fileStorageLocation: local de armazenamento principal configurado para armazenar os arquivos, inicializado anteriormente no construtor da classe FileStorageController;
- resolve(fileName): combina o caminho fileStorageLocation com o nome do arquivo (fileName) para criar o caminho completo onde o arquivo será colocado.
- file.transferTo(targetLocation): o método transferTo() do objeto MultipartFile (file) move o conteúdo do arquivo para o local de armazenamento especificado em targetLocation.

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