# Introdução à Linguagem Go

## O que é Go?

Go, também conhecida como Golang, é uma linguagem de programação criada pelo Google em 2009.

A linguagem foi desenvolvida com foco em:
- simplicidade;
- desempenho;
- concorrência;
- produtividade.

Go é muito utilizada no desenvolvimento de:
- APIs REST;
- microsserviços;
- sistemas distribuídos;
- aplicações em nuvem;
- ferramentas de linha de comando.

---

# Principais Características

- Sintaxe simples e moderna
- Compilação rápida
- Excelente desempenho
- Concorrência nativa com goroutines
- Gerenciamento automático de memória
- Forte utilização em backend e cloud computing

---

# Instalação do Go

## Instalação no Linux (Ubuntu)

Atualize os pacotes:

```bash
sudo apt update
```

Instale o Go:

```bash
sudo apt install golang-go
```

Verifique a instalação:

```bash
go version
```

---

## Instalação no Windows

1. Acesse o site oficial:
   https://go.dev/dl/

2. Baixe o instalador `.msi`

3. Execute a instalação normalmente.

4. Após instalar, abra o terminal e execute:

```bash
go version
```

---

## Instalação no macOS

Usando Homebrew:

```bash
brew install go
```

Verifique a instalação:

```bash
go version
```

---

# Primeiro Programa em Go

Crie um arquivo chamado:

```bash
main.go
```

Conteúdo:

```go
package main

import "fmt"

func main() {
    fmt.Println("Olá, Go!")
}
```

Execute o programa:

```bash
go run main.go
```

Saída esperada:

```bash
Olá, Go!
```

---

# Conclusão

Go é uma linguagem moderna, eficiente e muito utilizada no mercado atual, principalmente em aplicações backend e ambientes de computação em nuvem.
