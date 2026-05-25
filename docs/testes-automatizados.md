# Testes Automatizados em Go

## Introdução

Go possui suporte integrado para testes automatizados através do pacote `testing`. Diferentemente de outras linguagens que precisam de frameworks externos, Go oferece uma abordagem simples e eficaz para escrever e executar testes diretamente no padrão da linguagem.

## Por que testar?

- **Confiabilidade**: Garante que seu código funciona como esperado
- **Refatoração segura**: Permite mudanças com confiança
- **Documentação viva**: Testes documentam como usar o código
- **Detecção de bugs**: Encontra problemas antes da produção
- **Qualidade**: Melhora a estrutura e design do código

## Estrutura Básica de um Teste

### Arquivo de teste

Testes em Go seguem a convenção: `nome_test.go`

```go
package main

import "testing"

func TestMinha Funcao(t *testing.T) {
    resultado := MinhaFuncao(5)
    esperado := 10
    
    if resultado != esperado {
        t.Errorf("Esperado %d, obteve %d", esperado, resultado)
    }
}
```

### Nomenclatura

- O arquivo deve terminar com `_test.go`
- A função de teste deve começar com `Test`
- A função recebe um pointer `*testing.T`

## Tipos de Testes

### 1. Testes Unitários

Testam uma unidade isolada do código:

```go
package math

import "testing"

func Add(a, b int) int {
    return a + b
}

func TestAdd(t *testing.T) {
    resultado := Add(2, 3)
    esperado := 5
    
    if resultado != esperado {
        t.Errorf("Add(2, 3) = %d, esperado %d", resultado, esperado)
    }
}
```

### 2. Testes de Tabela

Testam múltiplos casos com a mesma lógica:

```go
func TestAddTable(t *testing.T) {
    testes := []struct {
        nome     string
        a, b     int
        esperado int
    }{
        {"Positivos", 2, 3, 5},
        {"Negativos", -2, -3, -5},
        {"Misto", -2, 3, 1},
        {"Zero", 0, 0, 0},
    }
    
    for _, teste := range testes {
        t.Run(teste.nome, func(t *testing.T) {
            resultado := Add(teste.a, teste.b)
            if resultado != teste.esperado {
                t.Errorf("Add(%d, %d) = %d, esperado %d",
                    teste.a, teste.b, resultado, teste.esperado)
            }
        })
    }
}
```

### 3. Testes de Integração

Testam múltiplos componentes juntos:

```go
package database

import (
    "testing"
    "database/sql"
)

func TestSalvarUsuario(t *testing.T) {
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatalf("Erro ao conectar: %v", err)
    }
    defer db.Close()
    
    // Criar tabela
    _, err = db.Exec("CREATE TABLE usuarios (id INTEGER, nome TEXT)")
    if err != nil {
        t.Fatalf("Erro ao criar tabela: %v", err)
    }
    
    // Inserir dados
    _, err = db.Exec("INSERT INTO usuarios VALUES (?, ?)", 1, "João")
    if err != nil {
        t.Errorf("Erro ao inserir: %v", err)
    }
}
```

## Métodos do testing.T

| Método | Descrição |
|--------|-----------|
| `t.Error(args)` | Registra um erro e continua |
| `t.Errorf(format, args)` | Erro formatado e continua |
| `t.Fatal(args)` | Registra erro e para o teste |
| `t.Fatalf(format, args)` | Erro formatado e para o teste |
| `t.Fail()` | Marca como falhado e continua |
| `t.FailNow()` | Para a execução do teste |
| `t.Log(args)` | Registra uma mensagem |
| `t.Logf(format, args)` | Log formatado |
| `t.Skip(args)` | Pula o teste |
| `t.Skipf(format, args)` | Pula com mensagem formatada |
| `t.Name()` | Retorna o nome do teste |
| `t.Run(nome, func)` | Executa subteste |

## Executar Testes

### Executar todos os testes

```bash
go test ./...
```

### Executar testes de um pacote

```bash
go test ./pacote
```

### Executar teste específico

```bash
go test -run TestAdd
```

### Executar com verbose

```bash
go test -v ./...
```

### Executar com coverage

```bash
go test -cover ./...
```

### Gerar relatório de cobertura

```bash
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

### Executar com timeout

```bash
go test -timeout 30s ./...
```

## Exemplo Prático Completo

### Arquivo: `calculadora.go`

```go
package calculadora

import "fmt"

func Somar(a, b int) int {
    return a + b
}

func Subtrair(a, b int) int {
    return a - b
}

func Multiplicar(a, b int) int {
    return a * b
}

func Dividir(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("divisão por zero")
    }
    return a / b, nil
}
```

### Arquivo: `calculadora_test.go`

```go
package calculadora

import "testing"

func TestSomar(t *testing.T) {
    testes := []struct {
        a, b     int
        esperado int
    }{
        {2, 3, 5},
        {-2, 2, 0},
        {0, 0, 0},
    }
    
    for _, teste := range testes {
        resultado := Somar(teste.a, teste.b)
        if resultado != teste.esperado {
            t.Errorf("Somar(%d, %d) = %d, esperado %d",
                teste.a, teste.b, resultado, teste.esperado)
        }
    }
}

func TestDividir(t *testing.T) {
    resultado, err := Dividir(10, 2)
    if err != nil {
        t.Errorf("Dividir(10, 2) retornou erro: %v", err)
    }
    if resultado != 5 {
        t.Errorf("Dividir(10, 2) = %d, esperado 5", resultado)
    }
}

func TestDividirPorZero(t *testing.T) {
    _, err := Dividir(10, 0)
    if err == nil {
        t.Error("Dividir(10, 0) deveria retornar erro")
    }
}
```

## Setup e Teardown

### TestMain

Executar código antes e depois de todos os testes:

```go
func TestMain(m *testing.M) {
    // Setup
    fmt.Println("Iniciando testes...")
    
    // Executar testes
    exitCode := m.Run()
    
    // Teardown
    fmt.Println("Testes finalizados")
    
    os.Exit(exitCode)
}
```

### Setup e Teardown por teste

```go
func TestComSetup(t *testing.T) {
    // Setup
    recurso := criarRecurso()
    defer func() {
        // Teardown
        recurso.Limpar()
    }()
    
    // Teste
    resultado := recurso.Processar()
    if resultado != esperado {
        t.Error("Falhou")
    }
}
```

## Subtestes

Agrupar testes relacionados:

```go
func TestUsuario(t *testing.T) {
    t.Run("Criar usuário válido", func(t *testing.T) {
        usuario := NovoUsuario("João", "email@test.com")
        if usuario.Nome != "João" {
            t.Error("Nome inválido")
        }
    })
    
    t.Run("Criar usuário inválido", func(t *testing.T) {
        usuario := NovoUsuario("", "invalido")
        if usuario != nil {
            t.Error("Deveria retornar nil")
        }
    })
}
```

Executar subtestes específicos:

```bash
go test -run TestUsuario/Criar
```

## Testes Paralelos

Executar testes em paralelo:

```go
func TestParalelo(t *testing.T) {
    t.Parallel()
    
    // Teste
    resultado := MinhaFuncao()
    if resultado != esperado {
        t.Error("Falhou")
    }
}
```

Executar com múltiplas goroutines:

```bash
go test -parallel 4 ./...
```

## Boas Práticas

### 1. Escrever testes descritivos

```go
// ❌ Ruim
func TestFunc(t *testing.T) { }

// ✅ Bom
func TestSomarDoisNumerosPositivos(t *testing.T) { }
```

### 2. Usar tabelas para casos múltiplos

```go
// ✅ Bom - Fácil adicionar casos
testes := []struct {
    entrada  int
    esperado int
}{
    {1, 2},
    {2, 4},
}
```

### 3. Testar casos extremos

```go
testes := []struct {
    entrada  int
    esperado int
}{
    {0, 0},           // Caso zero
    {-1, -2},         // Negativo
    {math.MaxInt, ?}, // Limite
}
```

### 4. Usar subtestes para organização

```go
t.Run("Casos válidos", func(t *testing.T) { })
t.Run("Casos inválidos", func(t *testing.T) { })
```

### 5. Manter testes simples e focados

Cada teste deve validar uma coisa específica.

## Coverage (Cobertura)

Ver cobertura de testes:

```bash
go test -cover ./...
```

Gerar relatório HTML:

```bash
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out -o coverage.html
```

Definir cobertura mínima:

```bash
go test -cover ./... -coverprofile=coverage.out
go tool cover -func=coverage.out | grep total | awk '{print $3}'
```

## Exemplo com Mocks

```go
package servico

import "testing"

type MockDB struct {
    dados map[string]string
}

func (m *MockDB) Get(chave string) string {
    return m.dados[chave]
}

func TestServico(t *testing.T) {
    mock := &MockDB{
        dados: map[string]string{
            "usuario:1": "João",
        },
    }
    
    // Usar mock no lugar do banco real
    servico := NovoServico(mock)
    resultado := servico.ObterUsuario("usuario:1")
    
    if resultado != "João" {
        t.Error("Falhou")
    }
}
```

## Conclusão

Os testes automatizados em Go são simples, eficientes e fazem parte da linguagem. Usar `go test` regularmente garante que seu código seja confiável e mantenível. Comece com testes unitários simples e evolua para estratégias mais complexas conforme necessário.

Para mais informações, consulte a [documentação oficial de testing do Go](https://golang.org/pkg/testing/).
