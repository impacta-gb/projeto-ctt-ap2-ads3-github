# Arrays, Slices e Maps em Go

## Arrays

Arrays em Go são estruturas de dados de tamanho fixo que armazenam elementos do mesmo tipo.

### Declarando Arrays

```go
// Array de 5 inteiros
var numeros [5]int

// Array inicializado
var dias = [7]string{"Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado"}

// Array com inferência de tipo
meses := [12]string{
    "Janeiro", "Fevereiro", "Março",
    "Abril", "Maio", "Junho",
    "Julho", "Agosto", "Setembro",
    "Outubro", "Novembro", "Dezembro",
}
```

### Acessando Elementos

```go
numeros := [5]int{10, 20, 30, 40, 50}

fmt.Println(numeros[0])  // Primeiro elemento: 10
fmt.Println(numeros[4])  // Último elemento: 50
fmt.Println(len(numeros)) // Comprimento: 5
```

### Modificando Elementos

```go
numeros := [3]int{1, 2, 3}
numeros[1] = 99
fmt.Println(numeros) // [1 99 3]
```

## Slices

Slices são mais flexíveis que arrays. Eles são referências para arrays subjacentes e podem crescer dinamicamente.

### Criando Slices

```go
// Slice vazio
var frutas []string

// Slice com make
numeros := make([]int, 5)        // Slice de 5 elementos, inicializados com zero
capacidade := make([]int, 3, 10) // Slice de 3 elementos, capacidade de 10

// Slice literal
cores := []string{"vermelho", "verde", "azul"}
```

### Operações com Slices

```go
// Adicionando elementos
frutas := []string{"maçã", "banana"}
frutas = append(frutas, "laranja")
frutas = append(frutas, "uva", "pera")

// Removendo elementos
numeros := []int{1, 2, 3, 4, 5}
numeros = append(numeros[:2], numeros[3:]...) // Remove o elemento do índice 2

// Copiando slices
origem := []int{1, 2, 3}
destino := make([]int, len(origem))
copy(destino, origem)
```

### Slicing (Fatiamento)

```go
numeros := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

// [início:fim] - do índice início até fim-1
fmt.Println(numeros[2:5])  // [2 3 4]

// [:fim] - do início até fim-1
fmt.Println(numeros[:3])   // [0 1 2]

// [início:] - do índice início até o final
fmt.Println(numeros[7:])   // [7 8 9]

// [:] - slice completo
fmt.Println(numeros[:])    // [0 1 2 3 4 5 6 7 8 9]
```

## Maps

Maps são estruturas de dados que armazenam pares chave-valor, onde cada chave é única.

### Criando Maps

```go
// Map vazio
var idades map[string]int

// Map com make
idades = make(map[string]int)

// Map literal
idades := map[string]int{
    "João": 25,
    "Maria": 30,
    "Pedro": 35,
}

// Map com tipos diferentes
dados := map[string]interface{}{
    "nome": "João",
    "idade": 25,
    "ativo": true,
}
```

### Operações com Maps

```go
// Adicionando/Modificando valores
idades["Ana"] = 28
idades["João"] = 26 // Modifica o valor existente

// Acessando valores
idade := idades["João"]
fmt.Println(idade) // 26

// Verificando se uma chave existe
idade, existe := idades["Carlos"]
if existe {
    fmt.Println("Idade de Carlos:", idade)
} else {
    fmt.Println("Carlos não encontrado")
}

// Deletando uma entrada
delete(idades, "Pedro")

// Iterando sobre um map
for nome, idade := range idades {
    fmt.Printf("%s tem %d anos\n", nome, idade)
}
```

## Diferenças Importantes

### Arrays vs Slices

```go
// Array - tamanho fixo
var array [5]int

// Slice - tamanho dinâmico
var slice []int

// Slice de array
array := [5]int{1, 2, 3, 4, 5}
slice := array[1:4] // [2, 3, 4]
```

### Arrays vs Maps

- **Arrays/Slices**: Acesso por índice numérico
- **Maps**: Acesso por chave (pode ser string, int, etc.)

```go
// Array/Slice
nomes := []string{"João", "Maria", "Pedro"}
fmt.Println(nomes[0]) // João

// Map
idades := map[string]int{"João": 25, "Maria": 30}
fmt.Println(idades["João"]) // 25
```

## Exemplos Práticos

### Exemplo 1: Sistema de Notas

```go
package main

import "fmt"

func main() {
    // Array de notas
    notas := [4]float64{7.5, 8.0, 6.5, 9.0}

    // Calcula média
    var soma float64
    for _, nota := range notas {
        soma += nota
    }
    media := soma / float64(len(notas))

    fmt.Printf("Média das notas: %.2f\n", media)
}
```

### Exemplo 2: Lista de Compras

```go
package main

import "fmt"

func main() {
    // Slice de compras
    compras := []string{"arroz", "feijão", "carne"}

    // Adiciona mais itens
    compras = append(compras, "leite", "pão")

    // Remove um item
    indice := 2 // remover "carne"
    compras = append(compras[:indice], compras[indice+1:]...)

    fmt.Println("Lista de compras:")
    for i, item := range compras {
        fmt.Printf("%d. %s\n", i+1, item)
    }
}
```

### Exemplo 3: Agenda Telefônica

```go
package main

import "fmt"

func main() {
    // Map de contatos
    contatos := map[string]string{
        "João":  "99999-1111",
        "Maria": "99999-2222",
        "Pedro": "99999-3333",
    }

    // Adiciona novo contato
    contatos["Ana"] = "99999-4444"

    // Busca contato
    nome := "Maria"
    if telefone, existe := contatos[nome]; existe {
        fmt.Printf("Telefone de %s: %s\n", nome, telefone)
    } else {
        fmt.Println("Contato não encontrado")
    }

    // Lista todos os contatos
    fmt.Println("\nAgenda completa:")
    for nome, telefone := range contatos {
        fmt.Printf("%s: %s\n", nome, telefone)
    }
}
```

## Boas Práticas

1. **Use slices em vez de arrays** quando precisar de flexibilidade
2. **Inicialize maps com make()** para evitar nil pointer
3. **Verifique se chaves existem** antes de acessar valores em maps
4. **Use len()** para obter o tamanho de arrays, slices e maps
5. **Use cap()** para verificar a capacidade de slices
6. **Considere usar structs** para dados mais complexos em vez de maps aninhados

## Exercícios

1. Crie um programa que:
   - Leia 5 números do usuário
   - Armazene em um slice
   - Calcule e exiba a média, maior e menor valor

2. Implemente uma agenda telefônica que:
   - Permita adicionar contatos
   - Permita buscar por nome
   - Permita listar todos os contatos
   - Permita remover contatos

3. Crie um programa que:
   - Conte a frequência de cada palavra em um texto
   - Use um map para armazenar palavra -> frequência
   - Exiba as palavras ordenadas por frequência