Estruturas de Controle em Go (If, For, Switch)

As estruturas de controle em Go permitem definir o fluxo de execução do programa, tomando decisões e repetindo ações conforme determinadas condições.

1. If

A estrutura if é usada para executar um bloco de código somente quando uma condição for verdadeira.

Sintaxe básica
if condição {
    // código executado se a condição for verdadeira
}
Exemplo
package main

import "fmt"

func main() {
    idade := 20

    if idade >= 18 {
        fmt.Println("Maior de idade")
    }
}
If com Else

O else define o que acontece quando a condição for falsa.

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
If com Else If

Usado para múltiplas condições.

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
If com inicialização

Go permite criar variáveis diretamente dentro do if.

package main

import "fmt"

func main() {
    if idade := 18; idade >= 18 {
        fmt.Println("Maior de idade")
    }
}

A variável criada só existe dentro do bloco if.

2. For

Em Go, o for é a única estrutura de repetição da linguagem.

Ele substitui estruturas como:

while
do while
foreach
For tradicional
package main

import "fmt"

func main() {
    for i := 1; i <= 5; i++ {
        fmt.Println(i)
    }
}
For como While
package main

import "fmt"

func main() {
    contador := 0

    for contador < 5 {
        fmt.Println(contador)
        contador++
    }
}
Loop infinito
package main

import "fmt"

func main() {
    for {
        fmt.Println("Executando...")
    }
}

Normalmente usado em:

servidores
workers
processamento contínuo
For com Range

Muito usado para percorrer arrays, slices, strings e maps.

package main

import "fmt"

func main() {
    nomes := []string{"Ana", "Carlos", "Maria"}

    for indice, nome := range nomes {
        fmt.Println(indice, nome)
    }
}
Ignorando valores no Range

Quando não precisar do índice:

for _, nome := range nomes {
    fmt.Println(nome)
}

O _ ignora valores não utilizados.

3. Switch

O switch é utilizado para múltiplas decisões, deixando o código mais organizado que vários if else.

Switch básico
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
Switch com múltiplos casos
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
Switch sem condição

Funciona como vários if else.

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
Fallthrough

Em Go, o switch não executa automaticamente os próximos casos.

Para continuar a execução, usa-se fallthrough.

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

Saída:

Um
Dois
Resumo
Estrutura	Função
if	Executa código baseado em condição
else / else if	Trata condições alternativas
for	Estrutura de repetição
range	Percorre coleções
switch	Seleção entre múltiplos casos
fallthrough	Continua para o próximo case
Boas práticas em Go
Prefira switch quando houver muitas condições.
Evite loops infinitos sem controle.
Use range para iterar coleções.
Mantenha condições simples e legíveis.
Evite muitos níveis de if aninhados.
Conclusão

As estruturas de controle são fundamentais em Go e permitem criar programas dinâmicos, organizados e eficientes. O domínio de if, for e switch é essencial para desenvolver aplicações robustas utilizando a linguagem Go.