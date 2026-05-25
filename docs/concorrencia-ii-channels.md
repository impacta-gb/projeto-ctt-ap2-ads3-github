# Concorrência II: Channels

## O que são Channels?

Em Go, **channels** são estruturas que permitem a comunicação segura entre goroutines. Eles servem para enviar e receber valores entre goroutines, coordenando a execução e evitando concorrência insegura.

Channels são tipados e funcionam como filas: uma goroutine envia (`<-`) e outra recebe (`<-`) dados através do mesmo channel.

## Criando um Channel

```go
mensagens := make(chan string)
numeros := make(chan int)
```

### Channel Simples

```go
package main

import "fmt"

func main() {
    mensagens := make(chan string)

    go func() {
        mensagens <- "Olá do channel!"
    }()

    msg := <-mensagens
    fmt.Println(msg)
}
```

## Enviando e recebendo dados

- Enviar: `channel <- valor`
- Receber: `valor := <-channel`

### Exemplo de envio e recepção

```go
package main

import "fmt"

func main() {
    numeros := make(chan int)

    go func() {
        numeros <- 42
    }()

    valor := <-numeros
    fmt.Println(valor)
}
```

## Channels com Buffer

Channels podem ser criados com buffer para armazenar múltiplos valores sem bloqueio imediato.

```go
valores := make(chan int, 3)
```

### Exemplo de channel com buffer

```go
package main

import "fmt"

func main() {
    valores := make(chan int, 2)

    valores <- 10
    valores <- 20

    fmt.Println(<-valores)
    fmt.Println(<-valores)
}
```

## Fechando um Channel

Fechar um channel indica que não haverá mais valores enviados por ele.

```go
close(channel)
```

### Exemplo com close

```go
package main

import "fmt"

func main() {
    numeros := make(chan int, 3)
    numeros <- 1
    numeros <- 2
    numeros <- 3
    close(numeros)

    for valor := range numeros {
        fmt.Println(valor)
    }
}
```

## Seleção com select

O `select` permite trabalhar com múltiplos channels ao mesmo tempo.

```go
select {
case msg := <-canal1:
    fmt.Println("Canal 1 recebeu", msg)
case canal2 <- "dados":
    fmt.Println("Enviado para canal 2")
default:
    fmt.Println("Nenhuma comunicação disponível")
}
```

## Boas práticas

1. Use channels para sincronizar e comunicar goroutines.
2. Não compartilhe variáveis mutáveis diretamente entre goroutines; use channels.
3. Feche channels apenas do lado do remetente.
4. Prefira `range` em channels fechados para consumir todos os valores.

## Exemplo completo

```go
package main

import (
    "fmt"
    "sync"
)

func produtor(c chan<- int, wg *sync.WaitGroup) {
    defer wg.Done()
    for i := 1; i <= 5; i++ {
        c <- i
    }
    close(c)
}

func consumidor(c <-chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for valor := range c {
        fmt.Println("Recebido:", valor)
    }
}

func main() {
    var wg sync.WaitGroup
    canal := make(chan int)

    wg.Add(2)
    go produtor(canal, &wg)
    go consumidor(canal, &wg)

    wg.Wait()
}
```

## Resumo

Channels em Go são fundamentais para implementar concorrência segura e coordenada. Eles permitem passar mensagens entre goroutines sem precisar de bloqueios manuais, tornando o código mais claro e confiável.
