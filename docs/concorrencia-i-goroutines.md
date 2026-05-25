# Concorrência I: Goroutines

## O que é uma Goroutine?

Uma **goroutine** é uma função ou método que executa concorrentemente com outras goroutines dentro do mesmo espaço de endereço em Go. É a forma que a linguagem Go implementa paralelismo leve e eficiente.

As goroutines são muito mais leves que threads do sistema operacional e milhares delas podem ser executadas simultânea e eficientemente.

## Características Principais

- **Leve**: Uma goroutine consome poucos quilobytes de memória
- **Simples**: Criada com uma única palavra-chave `go`
- **Eficiente**: Go gerencia automaticamente o escalonamento entre threads do SO
- **Não bloqueante**: Uma goroutine bloqueada não bloqueia as outras

## Criando uma Goroutine

Para executar uma função como uma goroutine, use a palavra-chave `go` antes da chamada:

```go
go funcao()
```

### Exemplo Básico

```go
package main

import (
    "fmt"
    "time"
)

func dizOla(nome string) {
    for i := 1; i <= 3; i++ {
        fmt.Printf("Olá, %s! (%d)\n", nome, i)
        time.Sleep(100 * time.Millisecond)
    }
}

func main() {
    go dizOla("Alice")
    go dizOla("Bob")
    
    time.Sleep(1 * time.Second)
    fmt.Println("Fim do programa")
}
```

## Sincronização com WaitGroup

Para esperar que todas as goroutines terminem, use `sync.WaitGroup`:

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup
    
    // Adiciona 2 goroutines à espera
    wg.Add(2)
    
    go func() {
        defer wg.Done()
        fmt.Println("Goroutine 1 executada")
    }()
    
    go func() {
        defer wg.Done()
        fmt.Println("Goroutine 2 executada")
    }()
    
    // Aguarda todas as goroutines terminarem
    wg.Wait()
    fmt.Println("Todas as goroutines terminaram")
}
```

## Comunicação entre Goroutines com Channels

Channels são a forma recomendada para goroutines se comunicarem:

```go
package main

import (
    "fmt"
)

func main() {
    // Cria um channel de strings
    mensagens := make(chan string)
    
    go func() {
        mensagens <- "Olá do goroutine!"
    }()
    
    // Recebe a mensagem
    msg := <-mensagens
    fmt.Println(msg)
}
```

## Boas Práticas

1. **Sempre aguarde as goroutines**: Use `WaitGroup` ou channels
2. **Evite race conditions**: Proteja dados compartilhados com mutex ou channels
3. **Feche channels**: Sinalize fim de comunicação fechando o channel
4. **Não crie goroutines infinitas**: Sem forma de pará-las, causam vazamento de recursos

## Exemplo: Processamento Paralelo

```go
package main

import (
    "fmt"
    "sync"
)

func processarNumero(numero int, wg *sync.WaitGroup, resultado chan int) {
    defer wg.Done()
    resultado <- numero * numero
}

func main() {
    var wg sync.WaitGroup
    resultados := make(chan int, 5)
    
    numeros := []int{1, 2, 3, 4, 5}
    
    for _, num := range numeros {
        wg.Add(1)
        go processarNumero(num, &wg, resultados)
    }
    
    // Fecha channel quando todas as goroutines terminarem
    go func() {
        wg.Wait()
        close(resultados)
    }()
    
    // Lê os resultados
    for res := range resultados {
        fmt.Printf("Resultado: %d\n", res)
    }
}
```

## Resumo

Goroutines são a forma Go de fazer concorrência de forma simples e eficiente. Com a palavra-chave `go`, `WaitGroup` para sincronização e `channels` para comunicação, você pode construir programas altamente concorrentes e responsivos.
