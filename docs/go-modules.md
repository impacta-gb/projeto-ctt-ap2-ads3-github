# Gerenciamento de Pacotes - Go Modules

## Introdução

Go Modules é o sistema de gerenciamento de dependências oficial do Go, introduzido na versão 1.11. Ele permite que você defina e controle as versões das bibliotecas externas que seu projeto utiliza de forma simples e eficiente.

## O que é um módulo Go?

Um módulo Go é uma coleção de pacotes Go relacionados que são versionados juntos como uma unidade única. O módulo é definido por um arquivo `go.mod` que especifica:

- O caminho do módulo (seu identificador único)
- A versão mínima do Go necessária
- As dependências externas e suas versões

## Arquivos Principais

### go.mod

O arquivo `go.mod` é o núcleo do gerenciamento de dependências. Exemplo:

```go
module github.com/usuario/meu-projeto

go 1.21

require (
    github.com/gin-gonic/gin v1.9.1
    github.com/lib/pq v1.10.9
)

require (
    github.com/google/uuid v1.3.0 // indirect
)
```

### go.sum

O arquivo `go.sum` mantém os hashes criptográficos de todas as dependências para garantir a integridade e reprodutibilidade. Não deve ser editado manualmente.

```
github.com/gin-gonic/gin v1.9.1 h1:...
github.com/gin-gonic/gin v1.9.1/go.mod h1:...
github.com/lib/pq v1.10.9 h1:...
github.com/lib/pq v1.10.9/go.mod h1:...
```

## Comandos Essenciais

### Inicializar um novo módulo

```bash
go mod init github.com/usuario/meu-projeto
```

Cria um arquivo `go.mod` no diretório atual.

### Adicionar dependências

Ao importar um pacote e executar:

```bash
go mod tidy
```

O Go automaticamente:
- Baixa as dependências necessárias
- Remove dependências não utilizadas
- Atualiza `go.mod` e `go.sum`

### Instalar/Atualizar dependências

```bash
# Baixa as dependências listadas em go.mod
go mod download

# Atualiza todas as dependências para a versão patch mais recente
go get -u ./...

# Atualiza para a versão minor mais recente
go get -u=patch ./...

# Atualiza um pacote específico
go get github.com/gin-gonic/gin@latest
go get github.com/gin-gonic/gin@v1.9.1
```

### Listar dependências

```bash
# Lista todas as dependências diretas
go list -m all

# Lista dependências de um módulo específico
go list -m github.com/gin-gonic/gin
```

### Remover dependências não utilizadas

```bash
go mod tidy
```

### Verificar integridade das dependências

```bash
go mod verify
```

## Versionamento Semântico

Go utiliza versionamento semântico (SemVer):

- **MAJOR**: Mudanças incompatíveis com a API
- **MINOR**: Novas funcionalidades compatíveis
- **PATCH**: Correções de bugs

Exemplo: `v1.9.1`
- `1` = MAJOR
- `9` = MINOR
- `1` = PATCH

## Exemplo Prático

### 1. Criar um novo projeto

```bash
mkdir meu-projeto
cd meu-projeto
go mod init github.com/usuario/meu-projeto
```

### 2. Adicionar uma dependência

Crie um arquivo `main.go`:

```go
package main

import (
    "fmt"
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.GET("/", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "Olá, Mundo!",
        })
    })
    r.Run(":8080")
}
```

### 3. Baixar dependências

```bash
go mod tidy
```

Isso criará/atualizará:
- `go.mod` com as dependências necessárias
- `go.sum` com os hashes de verificação

### 4. Executar o projeto

```bash
go run main.go
```

## Boas Práticas

### 1. Sempre fazer commit de go.mod e go.sum

```bash
git add go.mod go.sum
git commit -m "Add dependencies"
```

### 2. Manter dependências atualizadas

Revise periodicamente se há atualizações disponíveis:

```bash
go list -u -m all
```

### 3. Usar versões específicas em produção

Em vez de usar `latest`, especifique versões:

```bash
go get github.com/gin-gonic/gin@v1.9.1
```

### 4. Executar tidy regularmente

Mantenha seu `go.mod` limpo:

```bash
go mod tidy
```

### 5. Verificar compatibilidade

Antes de atualizar dependências, execute testes:

```bash
go test ./...
```

## Trabalhando com Módulos Locais

Para usar um módulo local durante o desenvolvimento:

```bash
go mod edit -replace github.com/outro/modulo=/caminho/local/modulo
```

Para remover a substituição:

```bash
go mod edit -dropreplace github.com/outro/modulo
```

## Limpeza de Cache

Limpe o cache de módulos quando necessário:

```bash
# Limpar todo o cache
go clean -modcache

# Remover módulos não utilizados
go mod tidy
```

## Troubleshooting

### "missing go.sum entry"

Execute `go mod tidy` para regenerar o arquivo:

```bash
go mod tidy
```

### Conflito de versões

Use `go mod graph` para visualizar a árvore de dependências:

```bash
go mod graph
```

### Atualizar todos os módulos

```bash
go get -u ./...
go mod tidy
```

## Conclusão

Go Modules simplifica significativamente o gerenciamento de dependências em Go. Seguindo as boas práticas e entendendo os comandos principais, você pode manter seus projetos organizados e com dependências bem controladas.

Para mais informações, consulte a [documentação oficial do Go Modules](https://golang.org/ref/mod).
