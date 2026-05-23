# Arrays, Slices e Maps em Go

Em Go, arrays, slices e maps são estruturas de dados usadas para armazenar coleções de valores. Cada uma tem características específicas de tamanho, capacidade e mutabilidade.

## Arrays

Um array em Go tem tamanho fixo definido na sua declaração.

```go
package main

import "fmt"

func main() {
    var numeros [3]int
    numeros[0] = 10
    numeros[1] = 20
    numeros[2] = 30

    fmt.Println(numeros)
}
```

### Características de arrays

- Tamanho fixo: o tamanho faz parte do tipo (`[3]int` é diferente de `[4]int`).
- Armazenam valores em posições numeradas.
- São úteis quando o tamanho conhecido e constante é importante.

### Declaração com valores

```go
valores := [3]string{"um", "dois", "três"}
```

## Slices

Slices são mais flexíveis que arrays. Eles representam uma fatia de um array e têm tamanho variável.

```go
package main

import "fmt"

func main() {
    frutas := []string{"maçã", "banana", "uva"}
    fmt.Println(frutas)
}
```

### Características de slices

- Tamanho dinâmico.
- Possuem `len` (comprimento) e `cap` (capacidade).
- Compartilham o mesmo array subjacente quando são derivados.

### Criar slice com `make`

```go
numeros := make([]int, 3, 5)
fmt.Println(len(numeros), cap(numeros))
```

### Adicionar elementos

```go
numeros := []int{1, 2, 3}
numeros = append(numeros, 4, 5)
fmt.Println(numeros)
```

### Fatiar slices

```go
valores := []int{10, 20, 30, 40}
sub := valores[1:3] // pega elementos nos índices 1 e 2
fmt.Println(sub)
```

### Cópia de slices

```go
origem := []int{1, 2, 3}
destino := make([]int, len(origem))
copy(destino, origem)
```

## Maps

Maps são coleções de pares chave-valor. As chaves devem ser de um tipo comparável.

```go
package main

import "fmt"

func main() {
    notas := map[string]int{
        "Ana": 8,
        "João": 9,
    }

    fmt.Println(notas)
}
```

### Características de maps

- Acesso por chave.
- Ordem não garantida.
- Boa opção para busca rápida de valores.

### Adicionar e remover elementos

```go
notas["Maria"] = 10
delete(notas, "João")
```

### Verificar existência

```go
valor, existe := notas["Ana"]
if existe {
    fmt.Println("Nota de Ana:", valor)
}
```

## Resumo

- `array`: tamanho fixo e tipo definido.
- `slice`: fatia dinâmica de um array, com crescimento possível.
- `map`: coleção chave-valor para pesquisa rápida.

Essas estruturas são predominantes em Go e são essenciais para manipular dados com eficiência.
