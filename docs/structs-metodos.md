Structs e Métodos em Go

Em Go, as structs são utilizadas para agrupar dados relacionados em uma única estrutura. Já os métodos permitem adicionar comportamentos a essas estruturas, deixando o código mais organizado e próximo da programação orientada a objetos.

1. Structs

Uma struct é um tipo personalizado que agrupa diferentes campos.

Sintaxe básica
type NomeStruct struct {
    campo tipo
}
Exemplo simples
package main

import "fmt"

type Pessoa struct {
    Nome  string
    Idade int
}

func main() {
    pessoa := Pessoa{
        Nome:  "João",
        Idade: 25,
    }

    fmt.Println(pessoa)
}

Saída:

{João 25}
Acessando campos

Os campos são acessados usando .

package main

import "fmt"

type Produto struct {
    Nome  string
    Preco float64
}

func main() {
    produto := Produto{
        Nome:  "Notebook",
        Preco: 3500,
    }

    fmt.Println(produto.Nome)
    fmt.Println(produto.Preco)
}
Alterando valores
package main

import "fmt"

type Usuario struct {
    Nome string
}

func main() {
    usuario := Usuario{
        Nome: "Carlos",
    }

    usuario.Nome = "Maria"

    fmt.Println(usuario)
}
Struct aninhada

Structs podem conter outras structs.

package main

import "fmt"

type Endereco struct {
    Cidade string
    Estado string
}

type Pessoa struct {
    Nome      string
    Endereco  Endereco
}

func main() {
    pessoa := Pessoa{
        Nome: "Ana",
        Endereco: Endereco{
            Cidade: "São Paulo",
            Estado: "SP",
        },
    }

    fmt.Println(pessoa)
}
Struct anônima

Uma struct pode ser criada sem definir um tipo separado.

package main

import "fmt"

func main() {
    carro := struct {
        Marca string
        Ano   int
    }{
        Marca: "Toyota",
        Ano:   2024,
    }

    fmt.Println(carro)
}
2. Métodos

Métodos são funções associadas a uma struct.

Eles permitem adicionar comportamentos aos tipos personalizados.

Sintaxe de método
func (variavel Tipo) NomeMetodo() {
    // código
}

A parte (variavel Tipo) é chamada de receiver.

Exemplo básico
package main

import "fmt"

type Pessoa struct {
    Nome string
}

func (p Pessoa) Apresentar() {
    fmt.Println("Olá, meu nome é", p.Nome)
}

func main() {
    pessoa := Pessoa{
        Nome: "João",
    }

    pessoa.Apresentar()
}

Saída:

Olá, meu nome é João
Métodos com retorno
package main

import "fmt"

type Retangulo struct {
    Largura float64
    Altura  float64
}

func (r Retangulo) Area() float64 {
    return r.Largura * r.Altura
}

func main() {
    ret := Retangulo{
        Largura: 10,
        Altura:  5,
    }

    fmt.Println(ret.Area())
}
Receiver por valor

Quando o receiver é passado por valor, a struct é copiada.

func (p Pessoa) Metodo() {
    // cópia da struct
}

Alterações dentro do método não afetam o valor original.

Receiver por ponteiro

Usado quando precisamos modificar a struct original ou evitar cópias desnecessárias.

package main

import "fmt"

type Contador struct {
    Valor int
}

func (c *Contador) Incrementar() {
    c.Valor++
}

func main() {
    contador := Contador{}

    contador.Incrementar()
    contador.Incrementar()

    fmt.Println(contador.Valor)
}

Saída:

2
Funções vs Métodos
Função	Método
Independente	Associado a uma struct
Recebe parâmetros comuns	Possui receiver
Uso geral	Representa comportamento do tipo
Exemplo prático completo
package main

import "fmt"

type Conta struct {
    Titular string
    Saldo   float64
}

func (c *Conta) Depositar(valor float64) {
    c.Saldo += valor
}

func (c *Conta) Sacar(valor float64) {
    c.Saldo -= valor
}

func (c Conta) ExibirSaldo() {
    fmt.Println("Saldo:", c.Saldo)
}

func main() {
    conta := Conta{
        Titular: "Maria",
        Saldo:   1000,
    }

    conta.Depositar(500)
    conta.Sacar(200)

    conta.ExibirSaldo()
}
Boas práticas
Use nomes claros para structs.
Prefira métodos para comportamentos relacionados ao tipo.
Utilize receiver por ponteiro quando precisar alterar dados.
Mantenha structs simples e organizadas.
Evite structs gigantes com muitas responsabilidades.
Quando usar Structs?

Structs são ideais para representar:

usuários
produtos
pedidos
contas bancárias
veículos
entidades do sistema em geral
Conclusão

Structs e métodos são fundamentais em Go e permitem modelar dados e comportamentos de forma organizada e eficiente. Embora Go não seja totalmente orientado a objetos, esses recursos oferecem uma abordagem simples, performática e muito poderosa para desenvolvimento de aplicações.