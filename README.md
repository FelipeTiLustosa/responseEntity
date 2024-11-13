## ResponseEntity
O `ResponseEntity` é uma classe do Spring Framework que facilita a criação de respostas HTTP personalizadas em aplicações Java Spring. Ele é frequentemente usado nos métodos de controladores REST para retornar respostas mais flexíveis e completas, incluindo status HTTP, cabeçalhos e o corpo da resposta.

### O que é `ResponseEntity`

A classe `ResponseEntity<T>` é uma resposta que pode conter:
1. **Status HTTP**: o código de status HTTP que informa o resultado da requisição, como `200 OK`, `404 NOT FOUND`, `500 INTERNAL SERVER ERROR`, entre outros.
2. **Cabeçalhos HTTP**: metadados que acompanham a resposta, como informações de cache, tipo de conteúdo, etc.
3. **Corpo da resposta**: o conteúdo que queremos enviar de volta, seja um objeto, uma lista de objetos ou até mesmo uma mensagem de erro.

### Exemplo Básico de `ResponseEntity`

Aqui está um exemplo de um endpoint que usa o `ResponseEntity` para retornar uma resposta HTTP 200 (OK) com um objeto JSON como corpo:

```java
@RestController
@RequestMapping("/api")
public class MeuController {

    @GetMapping("/saudacao")
    public ResponseEntity<String> saudacao() {
        return ResponseEntity.ok("Olá, seja bem-vindo!");
    }
}
```

Neste caso, o método `saudacao` retorna uma mensagem de saudação. O método `ResponseEntity.ok()` cria uma resposta com o código de status 200 e define "Olá, seja bem-vindo!" como o corpo da resposta.

### Principais Métodos de Fábrica de `ResponseEntity`

O `ResponseEntity` oferece métodos estáticos que facilitam a criação de respostas com diferentes status:

1. **`ResponseEntity.ok()`**: Retorna um status HTTP 200 (OK).
   ```java
   return ResponseEntity.ok(objeto);
   ```

2. **`ResponseEntity.status(HttpStatus.STATUS).body(objeto)`**: Define um status específico com um corpo.
   ```java
   return ResponseEntity.status(HttpStatus.CREATED).body(objeto);  // Para status 201
   ```

3. **`ResponseEntity.notFound()`**: Retorna um status HTTP 404 (Not Found).
   ```java
   return ResponseEntity.notFound().build();
   ```

4. **`ResponseEntity.badRequest()`**: Retorna um status HTTP 400 (Bad Request).
   ```java
   return ResponseEntity.badRequest().body("Erro: Requisição inválida");
   ```

### Personalizando Cabeçalhos

Você pode adicionar cabeçalhos à resposta com o `ResponseEntity`, usando o objeto `HttpHeaders`:

```java
@GetMapping("/com-cabecalho")
public ResponseEntity<String> saudacaoComCabecalho() {
    HttpHeaders headers = new HttpHeaders();
    headers.add("Custom-Header", "ValorPersonalizado");
    
    return ResponseEntity.ok()
                         .headers(headers)
                         .body("Olá, com cabeçalho personalizado!");
}
```

### Exemplo Completo

Um exemplo mais completo, com código de status 201 (Created) e retorno de um objeto de recurso criado:

```java
@PostMapping("/criar")
public ResponseEntity<Usuario> criarUsuario(@RequestBody Usuario usuario) {
    Usuario usuarioCriado = usuarioService.salvar(usuario);
    URI location = ServletUriComponentsBuilder.fromCurrentRequest()
                   .path("/{id}")
                   .buildAndExpand(usuarioCriado.getId())
                   .toUri();
    return ResponseEntity.created(location).body(usuarioCriado);
}
```

Aqui, o método `created(location)` cria uma resposta com status 201 e adiciona o cabeçalho `Location`, indicando onde o novo recurso pode ser encontrado.

### Vantagens do `ResponseEntity`

- **Controle completo** da resposta HTTP.
- Permite retornar **diferentes status** e **mensagens de erro** conforme necessário.
- **Facilita o tratamento de exceções** em endpoints REST, retornando mensagens claras para o cliente.
