# Tratamento de Erros em Go

## Introdução

Em Go, erros são valores. A linguagem não usa exceções como outras linguagens; em vez disso, a função retorna um valor `error` que deve ser verificado.

## O tipo `error`

O tipo embutido `error` é uma interface:

```go
package main

import "errors"

func main() {
    var err error = errors.New("algo deu errado")
    if err != nil {
        println(err.Error())
    }
}
```

## Verificando erros

O padrão mais comum em Go é retornar o resultado e o erro como valores separados:

```go
package main

import (
    "fmt"
    "strconv"
)

func parseNumber(text string) (int, error) {
    num, err := strconv.Atoi(text)
    if err != nil {
        return 0, err
    }
    return num, nil
}

func main() {
    value, err := parseNumber("123")
    if err != nil {
        fmt.Println("Erro ao converter número:", err)
        return
    }
    fmt.Println("Valor convertido:", value)
}
```

## Tratamento idiomático

Sempre verifique o erro imediatamente após a chamada da função:

```go
result, err := doSomething()
if err != nil {
    return err
}
```

Isso evita que o programa continue com um estado inválido.

## Criando erros personalizados

Use `errors.New` ou `fmt.Errorf` para criar mensagens de erro:

```go
package main

import (
    "errors"
    "fmt"
)

func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("divisão por zero")
    }
    return a / b, nil
}

func main() {
    result, err := divide(10, 0)
    if err != nil {
        fmt.Println("Erro:", err)
        return
    }
    fmt.Println(result)
}
```

Para adicionar contexto, use `fmt.Errorf`:

```go
if err != nil {
    return fmt.Errorf("falha ao abrir arquivo %s: %w", fileName, err)
}
```

## Comparando erros

Use `errors.Is` e `errors.As` para comparar ou extrair erros:

```go
package main

import (
    "errors"
    "fmt"
)

var ErrNotFound = errors.New("não encontrado")

func findItem(id int) error {
    return ErrNotFound
}

func main() {
    err := findItem(10)
    if errors.Is(err, ErrNotFound) {
        fmt.Println("Item não encontrado")
    }
}
```

## Resumo

- Em Go, erros são valores do tipo `error`.
- Verifique `err != nil` logo após a chamada de função.
- Use `fmt.Errorf` com `%w` para empacotar erros.
- Compare erros com `errors.Is` e `errors.As`.

Agora você tem um guia básico sobre o tratamento de erros em Go.