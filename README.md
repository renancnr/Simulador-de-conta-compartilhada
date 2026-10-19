# Simulador de Conta Compartilhada

## 1. Descrição do projeto

Esta atividade prática tem como objetivo demonstrar o problema de concorrência em Python por meio de uma simulação de saques em uma conta bancária compartilhada por várias threads.

O experimento é realizado em duas etapas: primeiro, o programa é executado sem mecanismos de sincronização; depois, é utilizada a biblioteca `threading.Lock()` para controlar o acesso ao saldo.

## 2. Objetivos

- Compreender o funcionamento de múltiplas threads.
- Identificar uma condição de corrida (*race condition*).
- Comparar os resultados obtidos sem proteção e com proteção.
- Entender a importância da exclusão mútua no acesso a recursos compartilhados.

## 3. Tecnologias utilizadas

- Python 3
- Biblioteca `threading`
- Biblioteca `time`

Não é necessário instalar bibliotecas externas.

## 4. Funcionamento do experimento

O saldo inicial da conta é de 1.000 unidades monetárias. São criadas dez threads, cada uma responsável por realizar um saque de 50 unidades.

O saldo esperado ao final da execução é calculado da seguinte maneira:

**Saldo final = 1.000 - (10 × 50) = 500**

### Etapa 1: execução sem proteção

No arquivo `corrida.py`, todas as threads compartilham a variável global `saldo`.

Cada thread lê o saldo atual, aguarda 0,01 segundo e grava o resultado da subtração. Como não existe sincronização, diferentes threads podem ler o mesmo valor antes que outra thread termine de atualizá-lo.

Esse comportamento caracteriza uma condição de corrida, pois o resultado pode depender da ordem de execução das threads.

### Etapa 2: execução com Lock

No arquivo `sincronizado.py`, foi utilizado `threading.Lock()`.

A instrução `with lock` protege a leitura do saldo, a espera simulada e a atualização do valor. Enquanto uma thread estiver na região protegida, as demais threads precisarão aguardar.

Dessa forma, os saques são realizados de maneira coordenada, preservando a consistência do saldo.

## 5. Como executar

É necessário ter o Python 3 instalado.

Execute os comandos abaixo no terminal, dentro da pasta do projeto.

Para executar a versão sem proteção:

```bash
python corrida.py
```

Para executar a versão com proteção:

```bash
python sincronizado.py
```

Caso o comando `python` não funcione no Windows, utilize `py` no lugar dele.

## 6. Resultados esperados

### Programa sem proteção

O saldo esperado é 500. Entretanto, como existe uma condição de corrida, o saldo obtido pode ser diferente do esperado.

O resultado varia conforme a execução, o sistema operacional e o comportamento do escalonamento das threads. A ocorrência de um saldo incorreto não é garantida em todas as execuções.

**Resultado observado:** preencher com o saldo obtido no terminal.

### Programa com proteção

O saldo esperado é 500.

Com o uso de `Lock`, cada saque lê o saldo atualizado e grava o novo valor de forma coordenada.

**Resultado observado:** preencher com o saldo obtido no terminal. O resultado correto esperado é 500.

## 7. Comparação

| Critério | Sem proteção | Com Lock |
|---|---|---|
| Recurso compartilhado | Sim | Sim |
| Sincronização | Não | Sim |
| Possibilidade de condição de corrida | Sim | Controlada na região protegida |
| Saldo esperado | 500 | 500 |
| Consistência do saldo | Não garantida | Garantida para os saques deste experimento |

## 8. Conclusão

A atividade demonstrou que o compartilhamento de variáveis entre threads exige mecanismos adequados de sincronização.

Na versão sem proteção, a leitura e a atualização do saldo podem ocorrer de forma conflitante, causando uma condição de corrida. Na versão protegida, o `threading.Lock()` garante a exclusão mútua durante a operação de saque.

Conclui-se que a sincronização é importante para preservar a integridade dos dados em programas concorrentes, especialmente em sistemas que manipulam recursos compartilhados, como contas bancárias e estoques.
