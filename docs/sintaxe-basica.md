# Sintaxe Básica e Variáveis em GO

## Estrutura Básica de um Programa

Todo programa em Go começa com um pacote.

```go
package main

import "fmt"

func main() {
    fmt.Println("Olá, Go!")
}
```

---

# Explicação do Código

- `package main`: define o pacote principal do programa.
- `import "fmt"`: importa o pacote responsável por entrada e saída.
- `func main()`: função principal executada pelo programa.
- `fmt.Println()`: imprime mensagens no terminal.

---

# Variáveis

Variáveis armazenam valores na memória.

## Declaração Tradicional

```go
var nome string = "João"
var idade int = 20
```

---

## Declaração Curta

Go permite declarar variáveis de forma simplificada:

```go
cidade := "São Paulo"
```

---

# Tipos de Dados Básicos

## Inteiros

```go
var numero int = 10
```

## Números Decimais

```go
var preco float64 = 19.90
```

## Texto

```go
var mensagem string = "Olá"
```

## Booleanos

```go
var ativo bool = true
```

---

# Constantes

Constantes possuem valores fixos.

```go
const PI = 3.14
```

---

# Exemplo Completo

```go
package main

import "fmt"

func main() {
    nome := "Maria"
    idade := 25

    fmt.Println(nome)
    fmt.Println(idade)
}
```

---

# Conclusão

A sintaxe de Go é simples e organizada, facilitando o aprendizado e o desenvolvimento de aplicações eficientes.