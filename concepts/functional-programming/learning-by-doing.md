# Programação Funcional

Hoje relembrei um pouco de programação funcional enquanto estava lendo um exemplo retratado no livro Clean Architecture.

O Exemplo de uma função que imprime o quadrado dos 25 primeiros números inteiros:
```clojure
(println (take 26 (map (fn [x] (* x x)) (range))))
```

## Analisando passo a passo

Vamos analisar esse código Clojure passo a passo:

```clojure
(println (take 26 (map (fn [x] (* x x)) (range))))
```

### 1. **`(range)`**

O Clojure tem uma função chamada `range` que gera uma sequência infinita de números inteiros começando em `0`. Então, ao chamar `(range)`, você obtém uma sequência como:

```clojure
(0 1 2 3 4 5 6 7 8 9 10 11 12 13 ...)
```

> 💡 Essa sequência vai até o infinito, mas você pode limitá-la com outras funções.

### 2. **`(fn [x] (* x x))`**

Aqui, você tem uma função anônima (ou "lambda") que recebe um argumento `x` e retorna o quadrado de `x`:

```clojure
(fn [x] (* x x))
```

Em outras palavras, para cada valor de `x` que essa função recebe, ela retorna `x` multiplicado por `x`. Por exemplo:
- Se `x = 2`, ela retorna `2 * 2 = 4`.
- Se `x = 3`, ela retorna `3 * 3 = 9`.

### 3. **`map`**

A função `map` aplica a função fornecida a cada elemento de uma sequência. No seu caso, a função `map` aplicará a função anônima `(fn [x] (* x x))` a cada elemento da sequência gerada por `(range)`.

Então, o `map` produzirá uma sequência onde cada número é elevado ao quadrado:
```clojure
(0 1 4 9 16 25 36 49 64 81 100 121 144 169 196 225 256 289 324 361 400 441 484 529 576 625 ...)
```

### 4. **`take 26`**

A função `take` recebe dois argumentos: o primeiro é o número de elementos que você quer extrair de uma sequência, e o segundo é a sequência em si. Aqui, `take 26` significa que você vai pegar os primeiros 26 elementos da sequência gerada por `(map (fn [x] (* x x)) (range))`.

Então, você obterá os primeiros 26 números quadrados:

```clojure
(0 1 4 9 16 25 36 49 64 81 100 121 144 169 196 225 256 289 324 361 400 441 484 529 576 625)
```

### 5. **`println`**

A função `println` simplesmente imprime no terminal o valor fornecido. Aqui, ela imprime a lista de números quadrados que foram obtidos pelo `take 26`.

### Resumo Final:

O código faz o seguinte:
1. Gera uma sequência infinita de números inteiros começando em `0` com `(range)`.
2. Aplica a função que eleva cada número ao quadrado usando `map`.
3. Pega os primeiros 26 números dessa sequência com `take 26`.
4. Imprime esses 26 números quadrados no terminal com `println`.

A saída será:

```
(0 1 4 9 16 25 36 49 64 81 100 121 144 169 196 225 256 289 324 361 400 441 484 529 576 625)
```

## Sintaxe básica de closure neste exemplo

Aqui estão as explicações breves das principais sintaxes de Clojure utilizadas no código:

1. **Funções**:
   - Em Clojure, as funções são definidas ou chamadas utilizando parênteses. O primeiro elemento dentro dos parênteses é o nome da função, e os demais são os argumentos.
   - Exemplo: 
     ```clojure
     (println "Hello") ; Chama a função println com o argumento "Hello"
     ```

2. **Função Anônima (`fn`)**:
   - As funções anônimas são definidas com `fn`, seguidas por uma lista de parâmetros entre colchetes `[]` e o corpo da função.
   - Exemplo:
     ```clojure
     (fn [x] (* x x)) ; Define uma função anônima que recebe x e retorna x * x
     ```

3. **`map`**:
   - A função `map` aplica uma função a cada elemento de uma sequência e retorna uma nova sequência com os resultados.
   - Exemplo:
     ```clojure
     (map inc [1 2 3]) ; Aplica a função inc (incremento) a cada elemento, retornando (2 3 4)
     ```

4. **`take`**:
   - A função `take` pega os primeiros `n` elementos de uma sequência.
   - Exemplo:
     ```clojure
     (take 3 (range)) ; Retorna os primeiros 3 números da sequência infinita gerada por range: (0 1 2)
     ```

5. **`range`**:
   - Gera uma sequência de números inteiros, começando por padrão em `0`. Sem argumentos, gera uma sequência infinita.
   - Exemplo:
     ```clojure
     (range) ; Gera a sequência infinita: (0 1 2 3 ...)
     ```

6. **`println`**:
   - Imprime o valor ou sequência fornecida no terminal.
   - Exemplo:
     ```clojure
     (println "Hello, World!") ; Imprime "Hello, World!" no terminal
     ```

7. **Sequência (`list`)**:
   - As listas em Clojure são criadas com parênteses e podem conter valores ou chamadas de função.
   - Exemplo:
     ```clojure
     (1 2 3) ; Uma lista contendo os números 1, 2 e 3
     ```

Esses elementos combinados formam a base de como o Clojure manipula dados e funções.