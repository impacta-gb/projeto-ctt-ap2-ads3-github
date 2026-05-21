# Structs e Métodos em Go

Em Go, `structs` são tipos compostos que agrupam campos nomeados, e métodos permitem associar comportamento a esses tipos.

## Structs

Uma `struct` define um conjunto de campos que podem ter tipos diferentes.

```go
package main

import "fmt"

type Pessoa struct {
    Nome string
    Idade int
}

func main() {
    p := Pessoa{
        Nome: "Ana",
        Idade: 25,
    }

    fmt.Println(p)
}
```

### Características de structs

- Agrupam valores relacionados em um único tipo.
- Campos têm nome e tipo.
- São úteis para representar entidades do domínio.
- Podem ser anônimas ou nomeadas.

### Instanciando structs

```go
p1 := Pessoa{"João", 30}

p2 := Pessoa{
    Nome: "Maria",
    Idade: 22,
}

p3 := Pessoa{}
```

### Acessando campos

```go
fmt.Println(p2.Nome)
p2.Idade = 23
```

## Métodos

Métodos são funções que têm um receptor, permitindo que sejam chamados como parte de um tipo.

```go
type Retangulo struct {
    Largura, Altura float64
}

func (r Retangulo) Area() float64 {
    return r.Largura * r.Altura
}

func main() {
    ret := Retangulo{Largura: 3, Altura: 4}
    fmt.Println("Área:", ret.Area())
}
```

### Receptores por valor e por ponteiro

- Receptor por valor (`r Retangulo`) faz uma cópia do valor.
- Receptor por ponteiro (`r *Retangulo`) permite modificar o valor original.

```go
func (r *Retangulo) Dobrar() {
    r.Largura *= 2
    r.Altura *= 2
}
```

### Exemplo com receptor por ponteiro

```go
func main() {
    ret := Retangulo{Largura: 3, Altura: 4}
    ret.Dobrar()
    fmt.Println(ret.Area())
}
```

## Métodos em `structs` compostas

Structs podem conter outras structs, e métodos ainda podem ser definidos para o tipo externo.

```go
type Endereco struct {
    Rua string
    Cidade string
}

type Cliente struct {
    Nome string
    Endereco
}

func (c Cliente) Info() string {
    return c.Nome + " mora em " + c.Cidade
}
```

## Resumo

- `struct` é um tipo composto com campos nomeados.
- Métodos associam funções a tipos usando receptores.
- Use receptor por valor quando não precisar modificar o original.
- Use receptor por ponteiro quando quiser alterar o valor ou evitar cópias grandes.
