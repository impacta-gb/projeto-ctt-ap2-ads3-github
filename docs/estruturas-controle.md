# Estruturas de Controle em Go (If, For, Switch)

As estruturas de controle em Go permitem decidir o fluxo do programa, repetindo ações e selecionando caminhos diferentes com base em condições.

## 1. If

A estrutura `if` executa um bloco de código apenas quando a condição é verdadeira.

Sintaxe básica:

```go
if condicao {
    // código executado quando condicao for verdadeira
}
```

Exemplo:

```go
package main

import "fmt"

func main() {
    idade := 20

    if idade >= 18 {
        fmt.Println("Maior de idade")
    }
}
```

### If com else

Use `else` para definir o que acontece quando a condição é falsa.

```go
package main

import "fmt"

func main() {
    nota := 6

    if nota >= 7 {
        fmt.Println("Aprovado")
    } else {
        fmt.Println("Reprovado")
    }
}
```

### If com else if

Use `else if` para várias condições.

```go
package main

import "fmt"

func main() {
    nota := 8

    if nota >= 9 {
        fmt.Println("Excelente")
    } else if nota >= 7 {
        fmt.Println("Aprovado")
    } else {
        fmt.Println("Reprovado")
    }
}
```

### If com inicialização

Go permite criar variáveis dentro do `if`, que só existem naquele bloco.

```go
package main

import "fmt"

func main() {
    if idade := 18; idade >= 18 {
        fmt.Println("Maior de idade")
    }
}
```

## 2. For

Em Go, `for` é a única estrutura de repetição.

### For tradicional

```go
package main

import "fmt"

func main() {
    for i := 1; i <= 5; i++ {
        fmt.Println(i)
    }
}
```

### For como while

```go
package main

import "fmt"

func main() {
    contador := 0

    for contador < 5 {
        fmt.Println(contador)
        contador++
    }
}
```

### Loop infinito

```go
package main

import "fmt"

func main() {
    for {
        fmt.Println("Executando...")
    }
}
```

`for {}` é comum em servidores ou em loops que aguardam eventos.

### For com range

O `range` percorre arrays, slices, strings e maps.

```go
package main

import "fmt"

func main() {
    nomes := []string{"Ana", "Carlos", "Maria"}

    for indice, nome := range nomes {
        fmt.Println(indice, nome)
    }
}
```

Se não precisar do índice:

```go
for _, nome := range nomes {
    fmt.Println(nome)
}
```

O `_` descarta o valor não usado.

## 3. Switch

O `switch` seleciona um entre vários casos de forma clara e organizada.

### Switch básico

```go
package main

import "fmt"

func main() {
    dia := 3

    switch dia {
    case 1:
        fmt.Println("Domingo")
    case 2:
        fmt.Println("Segunda")
    case 3:
        fmt.Println("Terça")
    default:
        fmt.Println("Dia inválido")
    }
}
```

### Switch com múltiplos casos

```go
package main

import "fmt"

func main() {
    letra := "a"

    switch letra {
    case "a", "e", "i", "o", "u":
        fmt.Println("Vogal")
    default:
        fmt.Println("Consoante")
    }
}
```

### Switch sem expressão

Neste caso, cada `case` é uma condição booleana.

```go
package main

import "fmt"

func main() {
    idade := 20

    switch {
    case idade < 12:
        fmt.Println("Criança")
    case idade < 18:
        fmt.Println("Adolescente")
    default:
        fmt.Println("Adulto")
    }
}
```

### Fallthrough

Por padrão, Go não passa automaticamente para o próximo `case`.
Use `fallthrough` se quiser que o fluxo continue.

```go
package main

import "fmt"

func main() {
    numero := 1

    switch numero {
    case 1:
        fmt.Println("Um")
        fallthrough
    case 2:
        fmt.Println("Dois")
    }
}
```

Saída:

```
Um
Dois
```

## Resumo

- `if`: executa código quando a condição é verdadeira
- `else` / `else if`: define outros caminhos
- `for`: única estrutura de repetição em Go
- `range`: itera coleções
- `switch`: seleciona entre vários casos
- `fallthrough`: continua para o próximo `case`

## Boas práticas

- Prefira `switch` quando houver várias condições semelhantes.
- Evite `for` infinito sem condição de saída clara.
- Use `range` para iterar coleções.
- Mantenha condições simples e legíveis.
- Evite muitos `if` aninhados.

## Conclusão

As estruturas de controle em Go são essenciais para escrever programas legíveis e eficientes. Dominar `if`, `for` e `switch` ajuda a construir lógica clara e organizada na linguagem Go.
