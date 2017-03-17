# MAC 0338 - An�lise de Algoritmos
#### Carlinhos E. Ferreira:
* E-mail: cef@ime.usp.br
* Sala: 108C
* Tel: 3091-6079
* Monitoria, segunda-feira 12 hrs sala B-129.

#### Bibliografia
* CLRS - Introduction to Algorithms 2a edi��o
* Material extra: slides da Chris, p�ginas do PF ...

#### Programa
1. **Nota��o assint�tica** ($\mathcal{O}$)
1. **Nota��o assint�tica** ($\mathcal{O}$)
2. **An�lise:**
    * Pior caso
    * Probabilist�ca
    * Amortizada
3. **Paradigmas de projetos de alforitmos:**
    * Divis�o e conquista
    * PD
    * Guloso
4. **Introdu��o a teoria de complexidade computacional:**
    * Redu��o de problemas
    * classes de complexidade

#### Provas
* **P1** 7/4 - 10:00
* **P2** 26/5 - 10:00
* **P3** 5/7 - 14:00
*  MP = m�dia aritm�tica das Provas
* V�rias Listas

---

# Aula 8/3

Nesta aula mostraremos um exemplo de an�lise de algoritmos. Mostraremos porque as constantes podem ser ignoradas introduzindo a nota��o $\mathcal{O} ()$
    * CLRs 1.1, 1.2, 2.1, 2.2

### Probelma: Ordenar um vetor

Rearranjar os elementos de um vetor $A[1 \dots n]$ de forma que $A[1] \leq A[2] \leq \dots \leq A[n]$

**Exemplo:**

Dado:

42 | 12 | 10 | 7 | 15 | 21 | 20 | 32 | 8 | 14
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---|

Sa�da:

7 | 8 | 10 | 12 | 14 | 15 | 20 | 21 | 32 | 42
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---|

1. Ordena��o por inser��o:

    A ideia do algoritmo �, em cada passo, considerar o vetor dividido em duas partes:

    Ordenado |  Bagun�a
    --- | ---

    Pegamos o primeiro elemento da parte bagun�adae procuramos o lugar dele na parte ordenada

    \[
        \underbrace{\text{20 | 25 | 35 | 40 | 44 | 55}}_{ordenado}\text{ | 38 | 99 | 10 | 65 | 50}
    \]

    **Ideia:**

    Comparamos com os valores anteriores a ele, deslocando o n�mero pra direita se for maior,  at� encontrar um n�mero menor ou igual ou at� verificar todos

    **C�digo:**

    Ordena por inser��o (A, n)
    ```
        0. j <- 2
        1. enquanto j <= n fa�a
        2.    x <- A[j]
        3.    i <- j - 1
        4.    enquanto i >= e A[i] > x
        5.        A[i+1] <- A[i]
        6.        i <- i - 1
        7.    A[i+1] <- x
        8.    j <- j + 1
    ```
    Invariante do primeiro while, at� a posi��o $j-1$ o vetor est� ordenado. Provar o segundo while com indu��o, um pouco mais complicado.

    **Quantas atribui��es o algoritmo faz?**

    A primeira observa��o � que depende da inst�ncia. Para algumas inst�ncias o n�mero de atribui��es � menor que para outras. O que queremos? O n�mero m�nimo, m�ximos ou m�dio de atribui��es? O melhor caso, pior caso ou caso m�dio?

    **An�lise:**

    Linha | Atribui��o
    --- | ---
    0 | $1$
    1 | $0$
    2 | $n-1$
    3 | $n-1$
    4 | $0$
    5 | $?\leq j-1 \leq n-1$
    6 | $?\leq j-1 \leq n-1$
    7 | $n-1$
    8 | $n-1$

    Olhando para o trecho 4-6 qual o n�mero m�ximo de atribui��es em fun��o de j?

    $$j-1$$

    Como sei que $j \leq n$, o total de atribui��es �:

    \[
        \leq 1 + n-1 + n-1 + (n-1).(n-1+n-1) + n-1 + n-1 \\
        = 1 + 4n - 4 + (n-1)(2n -2) = \\
        2n^2 - 1
    \]

    Essa an�lise est� muito pessimista, pois n�o � verdade que o loop interno executa n-1 vezes para todo j.

    $j$ | 5 - 6
    --- | ---
    2  | $\leq 1$
    3 | $\leq 2$
    4 | $\leq 3$
    $\vdots$ | $\vdots$

    O n�mero de atribui��es:
    \[
        \leq 1 + 4.(n-1) + 2 + 4 + \dots + 2(n-1) \\
        = 1 + 4(n-1) + 2 (1 + \dots + n-1) \\
        = 1 + 4(n-1) + 2 (\frac{n. (n-1)}{2}) \\
        = n^2 + 3n -3
    \]

    Assim obtemos uma estimativa mais precisa.
    Quando comparamos $n^2 + 3n - 3$ com $n^2$

    $n$ | $n^2 -3n - 3$| $n^2$
    --- | --- | ---|
    1 | 1 | 1
    2| 7 | 4
    3 | 15 | 9
    10 | 127 | 100
    100 | 10297 | 10000
    1000 | 1002997| 1000000

    Quando $n$ cresce $n^2$ domina os outros termos da fun��o.

    Se chamarmos de $t_i$ o tempo consumido na execu��o da linha i:
    \[
    \begin{array}{lr}
        0 \rightarrow t_0 \rightarrow 1 \ \text{vez} \\
        1 \rightarrow t_1 \rightarrow n \\
        2 \rightarrow t_2 \rightarrow n-1 \\
        3 \rightarrow t_3 \rightarrow n-1 \\
        4 \rightarrow t_4 \leq 2 + 3 + 4 + \dots + n \\
        5 \rightarrow t_5 \leq 1 + 2 + 3 + \dots + n-1 \\
        6 \rightarrow t_6 \leq 1 + 2 + 3 + \dots + n-1 \\
        7 \rightarrow t_7 \rightarrow n-1 \\
        8 \rightarrow t_8 \rightarrow n-1 \\
    \end{array}
    \]

    Ou seja, o consumo de tempo � limitado por
    \[
        \leq t_0 + n.t_1 + (n-1).(t_2+ t_3+t_7+t_8) +
        \frac{(n-1).(n+2)}{2}.t_4 + \frac{n.(n-1)}{2}.(t_5 + t_6) \\
        = \frac{t_4+t_5+t_6}{2}.n^2 + (t_1 + t_2+t_3+t_7+t_8 + \frac{t_4}{2}).n + t_0- t_1-t_2-t_3-t_7-t_8-t_4 \\
        = c_1.n^2 + c_2.n + c3
    \]  

    Onde $c_1, c_2, c_3$ s�o constantes que dependem da m�quina.
    Note que o que importa � que o tempo varia proporcionalmente a $n^2$ e � essa a ideia da **nota��o $\mathcal{O}()$.**

    _Intuitivamente_
    \[
        \mathcal{O}(f(n)) \approx \left.\begin{array}{lr}
        \text{conjunto de fun��es cujo crescimento} \\ \text{� limitado pelo
         crescimento de f(n)} \\
         \text{multiplicado por uma constante}
         \end{array}
         \right.
    \]

    Assim, $n^2, \frac{3}{2}.n^2, 1000.n^2, \frac{n^2}{1000}$ crescem na mesma velocidade.

    \[
        n^2 + 99.n \quad � \quad \mathcal{O}(n^2) \\
        33.n^2 \quad � \quad \mathcal{O}(n^2) \\
        75.n - 3 \quad � \quad \mathcal{O}(n^2) \\
    \]

    Mas, $\frac{n^3}{10000} + 7.n^2 + 15$ n�o � $\mathcal{O}(n^2)$.

    _Formalmente_

    Dizemos que uma fun��o $g(n)$ � $\mathcal{O}(f(n))$ se existem constantes positivas $c$ e $n_0$ tais que

    \[
        g(n) \leq c.f(n) \quad \quad n \geq n_0
    \]

    Escrevemos $g(n)$ � $\mathcal{O}(f(n))$ ou $g(n) = \mathcal{O}(f(n))$.

    Exemplos:

    * $10.n^2 e \mathcal{O}(n^2)$
    \[  
        10.n^2 \leq c.n^2 \quad \quad \forall n \geq n_0 \\
        \text{Tome } c = 10, n_0 = 1
    \]

    * $20.n^3 + 10.n.log_n + 5$ � $\mathcal{O}(n^3)$
    \[
        \leq 20.n^3 + 10.n^2 + n^2 \\
        \leq 20.n^3 + 11.n^2 \\
        \leq 31.n^3 \\
        \text{Tome } c = 31, n_0 = 5
    \]

    Podemos dizer:
    \[
        f(n) \in \mathcal{O}(g(n)) \\
        f(n) \text{ � } \mathcal{O}(g(n)) \\
        f(n) = \mathcal{O}(g(n))
    \]

---

# Aula 10/3

**Continua��o**
**Nota��o $\Omega$:**

Dizemos que uma fun��o $g(n)$ � $\Omega(f(n))$ se existem constantes positivas $c$ e $n_0$ tais que
\[
    c.f(n) \leq g(n) \quad \forall n \geq n_0
\]

Exemplo:
\[
    g(n) \geq \frac{n^2}{1000},  n \geq 0 \\
    g(n) = \Omega (n^2)
\]

**Melhor caso:**

Linha |Execu��o | Consumo de tempo
--- | --- | ---
0 | $1$ | $t_0$
1 | $n$ | $n.t_1$
2 | $n-1$ | $(n-1).t_2$
3 | $n-1$ | $(n-1)t_3$
4 | $\geq n-1$ | $\geq (n-1).t_4$
5 | $0$ |
6 | $0$ |
7 | $n-1$ | $(n-1).t_7$
8 | $n-1$ | $(n-1).t_8$

\[
    (t_1+t_2+t_3+t_4+t_7+t_8).n + t_0 -t_2-t_3-t_4-t_7-t_8 \\
    \geq c_1'.n +c_2' \Rightarrow \Omega(n)
\]

A fun��o ordena por inser��o, consome pelo menos $\Omega(n)$ unidades de tempo e no m�ximo $\mathcal{O}(n^2)$.

**Nota��o $\theta()$:**

Sejam $f(n)$ e $g(n)$ fun��es dos inteiros nos reais. Dizemos que $f(n) = \theta(g(n))$ se
\[
    f(n) \text{ � } \mathcal{O}(g(n)) e f(n) = \Omega(g(n))
\]
ou existem $c1, c2, n0$ tais que
\[
    c_1.g(n)\leq f(n) \leq c_2(g(n)) \quad \forall n\geq n_0
\]

Lembra que quando dizemos que um algoritmo $\mathcal{A}$ � $\theta(n)$ e outro $\mathcal{B}$ � $\theta(n.log \ n)$, estamos dizendo que $\mathcal{A}$ � assintoticamente ("n grandes") mais eficiente que $\mathcal{B}$. Lembre que h� constantes escondidas!

Exemplo:

$\mathcal{A}$ consome $100.n$ unidades de tempo $\rightarrow \theta(n)$

$\mathcal{B}$ consome $n.log_{10} \ n$ unidades de tempo $\rightarrow \theta(n.log \ n)$

O algoritmo $\mathcal{A}$ s� executa menos unidades de tempo que $\mathcal{B}$ para $n > 10^{100}$ (1 googol)

Desde o big Bang se passaram $4.10^17s$. O n�mero de �tomos do universo � estimado entre $4.10^{78}$ e $6.10^{79}$.

Em geral, um algoritmo $\theta(n)$ ou $\theta(n.log \ n)$ com constantes razo�veirs � eficiente.

* eficiente = polinomial
* $\theta(n^2)$ pode ser satisfat�rio.
* $\theta(2^n)$ dificilmente � aceit�vel.

\[
    \begin{array}{lr}
        \mathcal{O}(n) : \text{ linear} \\
        \mathcal{O}(n^2) : \text{ quadr�tico} \\
        \mathcal{O}(n^3) : \text{ c�bico} \\
        \mathcal{O}(2^n) : \text{ eponencial} \\
    \end{array}
\]

**An�lise do caso m�dio:**

SUpomos que a entrada � uma permuta��o de 1 a n escolhida com probabilidade uniforme.

Qual o n�mero esperado de execu��es das linhas 4-6?

Para cada $j$ o n�mero de t de vezes que a linha 4 executa � $1\leq t \leq j$

Para um $x$ na posi��o $j$ executamos o loop:

* 1 vez se $x > A[j-i]$ (maior de $A[1\dots j]$)
* 2 vezes se $x$ for o 2o maior

$\quad \ \ \vdots$

* j vezes se $x$ for o menor

Como a chance de cada situa��o � igula, tempos uma distribui��o uniforme de 1 at� j vezes que executaremos o loop. Portanto a probabilidade de executar t vezes  � $\frac{1}{j}$.

Assim o n�mero esperado de execu��es das linhas 4(5, 6) �:

\[
    = 1.\frac{1}{j} + 2$\frac{1}{j}$ + \dots +j.\frac{1}{j} \\
    = \frac{1}{j} (1 + 2 + \dots +j) \\
    = \frac{1}{j} (1 + 2 + \dots +j) \\
    = \frac{1}{j} (j.\frac{(1+j)}{2})
    = \frac{1}{j} (j.\frac{(1+j)}{2}) \\
    = \frac{(1+j)}{2}
\]

Com isso, o n�mero esperado de execu��es das linhas 4-6 em todas as itera��es $j = 2, 3,4 \dots, n$
\[
    \sum^n_{j=2} \frac{(1+j)}{2} = \frac{1}{2}. (3 + 4 \dots + n+1) \\
    = \frac{(n-1).(n+4)}{4}
\]

Ordena��o por inser��o tem um consumo de tempo no caso m�dio dado por
\[
    t_0 + (t_1+t_2+t_3+t_4+t_7+t_8).n - (t_2+t_3+t_4+t_7+t_8) + (t_4+t_5+t_6).\frac{(n-1).(n+4)}{4}
\]

Portanto no caso m�dio a ordena��o por inser��o � $\theta(n^2)$.

### Divis�o e conquista
    CLRS 2.3, 4.1, 4.2

� um paradgma de projeto de alforitmos que envolve os seguintes passos:

1. **Divide** o problema em subproblemas
2. **Conquista** resolve recursivamente os subproblemas
3. **Combina** junta as solu��es dos subproblemas para gerar a solu��o da inst�ncia original.

Vamos exemplificar o uso da t�cnica com o algoritmo **Mergesort**.

1. **Divide:** Divide a sequ�ncia de n elementos em duas susqu�ncias de aproximadamente $\frac{n}{2}$.
2. **Conquista:** ordena recursivamente as subsequ�ncias
3. **Combina:** intercala as subsequ�ncias ordenadas obtendo a sequ�ncia original ordenada.

##### _Mergesort (A, p, r)_

Rearranja os elementos de $A[p \dots r]$ para ordena��o

```
    1. se p < r
    2.    q <- floor((p+r) / 2)
    3.    Mergesort (A, p , q)
    4.    Mergesort (A, q+1, r)
    5.    Intercala (A, p , q, r)
```

A fun��o Intercala receve o vetor A e tr�s �ndices $p \leq q \leq r$ tais que $A[p\dots q]$ e $A[q+1 \dots r]$ est�o ordenados e rearraja os elementos de A de forma que $A[p \dots r]$ fique ordenado.

Exemplo:

Dado

12 | 25| 33 | 67 | 98 | llll| 20 | 41 | 43 | 95 | 102 | 110
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---| ---| ---|

Sa�da

12 | 20| 25 | 33 | 41 | 43 | 67| 95 | 98 | 102 | 110
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---| ---| ---|

Para intercalar pegamos um ponteiro pro come�o de cada subsequ�ncia e analisamos os n�mero de 2 em 2 e ordenamos, precisa se pensar no caso de quando uma das subsequ�ncias ter sido percorrida por copleta.

##### Intercala (A, p, q, r)
```
    0. B[p ... r] � um vetor auxiliar
    1. para i <- p at� q fa�a
    2.    B[i] <- A[i]
    3. para j <- q+1 at� r fa�a
    4.    B[j] <- A[r-j+q+1]
    5. i <- p
    6. j <- r
    7. para k <- p at� r fa�a
    8.    se B[i] <= B[j] e i <= q
    9.        A[k] <- B[i]
    10.       i <- i+1
    11.   sen�o
    12.       A[k] <- B[j]
    10.       j <- j-1
```

---

# Aula 15/03

An�lise do consumo de tempo do Intercala. Se $n = r - p + 1$

* Linhas 1-4 consumo $\theta(n)$
* Linhas 5-6 conusmo $\theta(1)$
* Linhas 7-13 consumo $\theta(n)$

Portanto o consumo de tempo do Intercala �: $\theta(n)$.

An�lise do consumo de tempo do Mergesort. Se chamamos de $T(n)$ o tempo consumido pelo algoritmo para uma inst�ncia de tamanho $n$.

* Linhas 1-2 consumo $\theta(1)$
* Linhas 3 conusmo $T(\lfloor{\frac{n}{2}}\rfloor)$
* Linhas 4 conusmo $T(\lceil{\frac{n}{2}}\rceil)$
* Linhas 5 consumo $\theta(n)$

\[
    T(n) = T(\lfloor{\frac{n}{2}}\rfloor) + T(\lceil{\frac{n}{2}}\rceil) + \theta (n)
\]

_Como calculat $T(n)$?_

Um m�todo para calcular esse tipo de recorr�ncia � o m�todo da expans�o:

1. Restrinja-se a uma pot�ncia de 2
2. Troque a nota��o $\theta(n)$ por $c_1.n$
3. Use uma constante $c_2$ para $T(1)$
1. Use expans�o para encontrar "um chute"
1. Verifique se sem chute est� certo.

Quando n � potencia de 2, $n = 2^k$ e $k = log n$
\[
    T(1) = c_2 \\
    T(n) = 2.T(\frac{n}{2})+ c_1.n, \quad n > 1 \\
    \begin{array}{lr}
    \quad \quad \quad \quad = 2.\left[2.T(\frac{n}{4}) + c_1.\frac{n}{2}\frac{n}{2}\right] + c_1.n \\
    \quad \quad \quad \quad = 4.T(\frac{n}{4}) + 2.c_1.n \\
    \quad \quad \quad \quad \vdots \\
    \quad \quad \quad \quad = 2^k.T(\frac{n}{2^k}) + k.c_1.n \\
    \quad \quad \quad \quad = c_2.n + c1.n.log \ n \\
    \quad \quad \quad \quad = \theta (n.log \ n)
    \end{array}
\]

Vamos conferir:

Para $n = 2^k \begin{cases} T(1) = c_2 \\ T(n) = 2.T(\frac{n}{2}) + c_1.n, & n > 1\end{cases}$

Vale $T(n) = c_2.n + c1.n.log \ n$.

**Prova por indu��o em k**

_Base_: $k = 0, n= 1, T(1) = c_2 = c_2.1 + c_1.1.0$

_Passo_: Tome $k \geq 1$

\[
    HI: T(2^{k-1}) = c_2.2^{k-1} + c_1.2^{k-1}.(k-1)
\]
Sabemos que

\[
    T(2^k) = 2.T(\frac{2^k}{2}) + c_1.2^k \\
    = 2.T(2^{k-1}) + c_1.2^k \\
    \overset{HI}{=} 2.\left(c_2.2^{k-1} + c_1.2^{k-1}.(k-1)\right) + c_1.2^k \\
    = 2.c_2.2^{k-1} + 2.c_1.2^{k-1}.(k-1) + c_1.2^k \\
    = c_2.2^k + c_1.(k-1).2^k + c_1.2^k \\
    = c_2.2^k + c1.k.2^k \\
    = c_2.n + c_1.n.log \ n_\blacksquare
\]

**Exerc�cios:**

1.
$T (n) =  \begin{cases}
        c_1 &  n = 1 \\
            T(\frac{n}{2}) + c_2  & n > 1\end{cases}$
\[
    T(1) = c_1 \\
    n = 2^k
\]

Expandindo
\[
    T(n) = T(\frac{n}{2}) + c_2 \\
    = \left( T(\frac{n}{4}) + c_2 \right) + c_2 \\
    = T(\frac{n}{4}) + 2.c_2 \\
    \vdots \\
    = T(\frac{n}{2^k}) + k.c_2 = T(1) + k.c_2 \\
    = c_1 + k.c_2 \Rightarrow \theta(1)
\]

Prova por indu��o

Base: $n= 1, T(1)=c_1= c_1 + 0.c_2$
Passo:
\[
    HI: T(2^{k-1}) = c_1 + (k-1).c_2 \\
    \\
    T(2^k) = T(\frac{2^k}{2}) + c_2 = T(2^{k-1}) + c_2 \\
    (c_1 + (k-1).c_2) + c_2 = c_1 + k.c_2
\]

2. $T(n) = T(n-1) + \theta (n)$

\[
    T(1) = c_1 \\
    \theta(n) = c_2.n
\]

Ent�o, expandindo

\[
    T(n) = T(n-1) + c_2.n \\
    = \left(T(n-2) + c_2.(n-1)\right) + c_2.n \\
    = T(n-2) + c_2.n + c_2.(n-1)\\
    = (T(n-3) + c_2.(n-2)) + + c_2.n + c_2.(n-1)
    \vdots \\
    = T(1) + 2.c_2 + 3.c_2 + \dots + (n-1).c_2 + n.c_2 \\
    = c_1 + c_2 (2 + 3 + \dots + n) = c_1 + c_2.\left(\frac{(n-1).(n+1)}{2}\right)
\]\

Prova por indu��o

Base: $n= 1, T(1) = c_1 = c_1 + c_2.(\frac{0.3}{2})$

Passo :
\[
    HI: T(n-1) = c_1 + c_2.\left(\frac{(n-2).(n+1)}{2}\right)
    \\
    T (n) = T(n-1) + c_2.n \\
    = c_1 + c2.\left(\frac{(n-1).(n-2)}{2} + n\right) \\
    = c_1 + c_2.\left(\frac{n^2 -n -2 + 2n}{2}\right)\\
    = c_1 + c_2.\left(\frac{n^2 + n -2}{2}\right)
    = c_1 + c_2.\left(\frac{(n-1)(n+2)}{2}\right)_ \blacksquare
\]

Portanto essa recorr�ncia descreve um algorimo $\theta(n^2)$

3. $T(n) = 3.T (\lfloor\frac{n}{2}\rfloor) + \theta (n)$

\[
    n = 2^k \\
    T(1) = c_1 \\
    \theta(n) = c_2.n
\]
Ent�o, expandindo
\[
    T(n) = 3.T(\frac{n}{2}) + c_2.n \\
    = 3.(3.T(\frac{n}{4}) + c_2.\frac{n}{2})) + c_2.n \\
    = 3^2.T(\frac{n}{4}) + 2.c_2.\frac{n}{2}) + c_2.n \\
    \vdots \\
    = 3^k.T(\frac{n}{2^k}) + \frac{3^{k-1}}{2^{k-1}}.c_2.n + \dots + \frac{3}{2}.c_2.n + c_2.n \\
    3^k.c_1 +  c_2n.\left(\frac{3^{k-1}}{2^{k-1}}\ + \frac{3^{k-2}}{2^{k-2}} +  \dots + \frac{3}{2} + 1\right) \\
    = 3^k.c_1  +  c_2.n.\left(\frac{\left(\frac{3^{k-1}}{2^{k-1}}\right) - 1}{\frac{3}{2} - 1}\right) \\
    = 3^{log \ n}.c_1 + 2.c_2.(3^{log \ n} - n) \\
\]
Como $3 = 2^{log \ 3}$
\[
    = c_1.\left(2^{log \ 3 }\right)^{log \ n} + 2.c_2.\left(\left(2^{log \ 3 }\right)^{log \ n} - n\right) \\
    = c_1.\left(2^{log \ n }\right)^{log \ 3} + 2.c_2.\left(\left(2^{log \ n }\right)^{log \ 3} - n\right) \\
    = c_1.n^{log \ 3} + 2.c_2(n^{log \ 3} - n)
\]
Portanto, $T(n) = \theta (n^{log \ 3})$

Prova por indu��o:

Base: $n =1, T(n) = c1.1 + 2.c_2 (1 - 1) = c_1$

Passo:

\[
    HI: T(2^{k-1}) = c_1.(2^{k-1})^{log \ 3} + 2.c_2. ((2^{k-1})^{log \ 3} - 2^{k-1}) \\
    T(2^k) = 3.T(2^{k-1}) + c_2.2^k \\
    = 3.c_1.(2^{k-1})^{log \ 3} + 3.2.c_2. ((2^{k-1})^{log \ 3} - 2^{k-1}) + c_2.2^k \\
    = 3.c_1.(2^{k-1})^{log \ 3} + 3.2.c_2. (2^{k-1})^{log \ 3} -  3.2.c_2.2^{k-1} + c_2.2^k \\
    = 3.c_1.(2^{k-1})^{log \ 3} + 3.2.c_2. (2^{k-1})^{log \ 3} -  3.c_2.2^{k} +  c_2.2^k  \\
    = 2^{log \ 3}.c_1.(2^{k-1})^{log \ 3} + 2^{log \ 3}.2.c_2.(2^{k-1})^{log \ 3} - 2.c_2.2^k \\
    = c_1.(2^{k})^{log \ 3} + 2.c_2.((2^{k})^{log \ 3} - 2^k)_ \blacksquare
\]

---

# Aula 17/03

#### Quicksort

M�todo de ordena��o vaseada em Divis�o e conquista.

**Divide:** particione (rearranja)o vetor $A[p \dots r]$ em duaspartes $A[p \dots q-1]$ e $A[q+1 \dots r]$ de tal forma que os elementos de
\[
    A[p \dots q-1] \leq A[q] < A[q+1 \dots r]
\]
Devolve o �ndice q.

**Conquista:** ordene $A[p \dots q-1]$ e $A[q+1 \dots r]$ recursivamente usando o Quicksort.

**Combina��o:** o vetor j� est� ordenado

Quicksort(A, p, r)

    1. se p < r
    2.    q <- Particione (A, p, r)
    3.    Quicksort (A, p, q-1)
    4.    Quicksort (A, q+1, r)

Como resolver o particione?

PROBLEMA: Rearranjar os elementos de um vetor $A[p \dots r] e devolver $p \leq q \leq r$ tal que
\[
    A[p \dots q-1] \leq A[q] < A[q+1 \dots r]
\]

Entra:

99 | 33| 55 | 77 | 11 | 22 | 88| 66 | 33 | 44 |
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---|

Sai:

11 | 33| 22| 33 | **44** | 99 | 55| 77 | 88 | 66 |
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---|

**C�digo:** Particione (A, p, r)

    1. x <- A[r]
    2. i <- p - 1
    3. para j <- p at� r fa�a
    4.    se A[j] <= x
    5.        i <- i + 1
    6.        A[i] <-> A[j]
    7. devolve i

#### An�lise de desempenho

####Particione

* Linhas 1-2 consumo $\theta(1)$
* Linhas 3-4 consumo $\theta(n)$
* Linhas 5-6 consumo $\mathcal{O}(n)$
* Linhas 7 consumo $\theta(1)$

Portanto consome tempo $\theta(n)$

####Quicksort

Se chamarmos de T(n) o tempo consumido pelo Quicksort para uma inst�ncia de n elementos, teremos:

$T(n) = \begin{cases}
            1, & n = 1\\
            T(k) + T(n - k - 1) + \theta(n), & n \geq 2
        \end{cases}$
onde k =  q - p

#####Pior caso

Seja $T_{MAX}(n)$ o tempo de pior caso do quicksort para uma inst�ncia de tamanho n.

Algu�m pode chutar que o pior caso ocorre quando o particione devolve sempre a �ltima posi��o $(q=r)$.

$T_{MAX}(n) = \begin{cases}
                1, & n = 1\\
                T_{MAX}(n-1) + \theta(n), & n\geq2
                \end{cases}$

_Exerc�cio:_ $T_{MAX}(n) = \theta (n^2)$

Sendo mais cuidadoso

$T_{MAX}(n) = \begin{cases}
                1, & n = 1\\
                \underset{0 \leq k \leq n-1}{max}\left\{T_{MAX}(k) + T_{MAX}(n-k-1)\right\} + \theta(n), & n\geq2
                \end{cases}$

n | 0 |1 | 2| 3|
--- | ---| ---| ---| --|
| $T_{MAX}(n) $ |1 | 1| {1 + 1, 1 + 1} = 4 | {1 + 4, 1 + 1, 1 + 4} = 8

Vamos mostrar que
\[
    T_{MAX}(n) \leq n^2 + 1
\]  

Prova: Indu��o em n

Base: n = 0, n= 1

Passo:
\[
    HI: \text{ Para qualquer} k < n \Rightarrow T_{MAX}(n) \leq n^2 + 1
\]
Tome $n > 1$
\[
    T_{MAX}(n) = \underset{0 \leq k \leq n-1}{max}\left\{T_{MAX}(k) + T_{MAX}(n-k-1)\right\} + n \\
    \overset{HI}{\leq} \underset{0 \leq k \leq n-1}{max} \left\{ k^2 + 1 + (n-k-1)^2 + 1\right\} + n \\
    \leq \underset{0 \leq k \leq n-1}{max}\left\{k^2 + 1 + n^2 - 2nk - 2n + k^2 - 2k + 1 + 1\right\} + n \\
    \leq \underset{0 \leq k \leq n-1}{max}\left\{2k^2 (-2n +2)k + n^2 - 2n + 3\right\} + n
\]

M�ximo da fun��o de segundo grau
\[
    f(k) = 2k^2 (-2n +2)k + n^2 - 2n + 3, \quad 0\leq k\leq n-1 \\
    f'(k) = 4k + (-2n+2) \\
    f''(k) = 4 > 0, \text{ portanto m�ximo est� nos extremos}
    \\
    f(0) = n^3 - 2n + 3 \\
    f(n-1) = 2(n-1)^2 + (-2 + 2)(n-1) + n^2 - 2n +3 - 2(n-1)
\]
Pontos de m�ximo 0 e n-1. Ent�o
\[
    T_{MAX}(n) \leq n^2n + 3 + n \leq n^2 - n + 3 \\
    \text{Como } -n + 3 < 1\\
    \leq n^2 + 1
\]

Portanto o consumo de tempo no pior caso � $\mathcal{O} (n^2)$
_Exerc�cio:_ $T_{MAX}(n) \geq \frac{n^2}{2}, \ n \geq 1$

Portanto o consumo de tempo no pior caso � $\theta (n^2)$

#####Melhor caso

Seja $T_{MIN}(n)$ o consumo de tempo de mehor caso  Quicksort para uma inst�ncia de tamanho n.

\[
    T_{MIN}(n) = \underset{0 \leq k \leq n-1}{min} \left\{ T_{MIN}(k) + T_{MIN}(n - k -1)\right\}  + n, \ n \geq 2
\]
 Vamos mostrar que $T_{MIN}(n) \geq (n+1)log (n+1), n \geq 1

 Base: n = 0, n=1 OK

 Passo
 \[
 HI: \text{Para } k < n  \text{ vale }  T_{MIN}(k) \geq (k+1) . log(k+1) \\
 T_{MIN}(n) = \underset{0 \leq k \leq n-1}{min} \left\{ T_{MIN}(k) + T_{MIN}(n - k -1)\right\}  + n
  \\
 \geq \underset{0 \leq k \leq n-1}{min} \left\{ (k+1).log(k+1) + (n - k -1).log(n-k-1)\right\}  + n \\
 \]

 M�nimo da fun��o:
 _Exercicio_

 Com isso, o consumo de tempo de melhor caso do Quicksort � $\Omega (n.log \ n)$

##### Conclus�o
 Com isso, o consumo de tempo do Quicksort � $\mathcal{O}(n^2)$ e $\Omega (n.log \ n)$

##### Caso m�dio

Apesar do consumo de tempo de pior caso do Quicksort ser $\theta(n^2)$, sua performance pr�tica � compar�vel a algoritmos cujo consumo de tempo � $\theta(n.log \ n)$.
\[
    \text{Por que?}
\]

Vamos supor que o particione sempre garante, por exemplo, $\frac{1}{3}$ da seq�ncia de um lado e $\frac{2}{3}$ do outro.

Considere a recorr�ncia

$T(n) = \begin{cases}
        1, & n = 1\\
        T(\left\lceil \frac{n}{3} \right\rceil) + T(\left\lfloor \frac{2n}{3} \right\rfloor)+ n, & n \geq 2
        \end{cases}$

�rvore de recorr�ncia
\[
    n \\
    \swarrow  \ \searrow \\
    \frac{n}{3} \quad  \quad \frac{2n}{3} \\
    \swarrow  \ \searrow \quad \swarrow  \ \searrow \\
    \frac{n}{9} \quad  \quad \frac{2n}{9} \frac{2n}{9} \quad  \quad \frac{4n}{9} \\
    \vdots \quad \quad \quad \vdots \quad \quad \vdots \quad \quad \quad \vdots
\]

O n�mero de n�ceis desta �rvore � o maior k tal que $\frac{2^k.n}{3^k} = 1$ ou seja, $\leq k = log_\frac{3}{2} n$

Voltando a recorr�ncia

n | 0 | 1 | 2 | 3 | 4 |
--- | ---| ---| --| --| -|
$T(n)$ | $1$ | $1 $| $4$ | $1+ 4 + 3 = 8$| $4 +8+4 = 16$

$T(n) \leq 20.n.log \ n, n \geq 2$

Prova:

Base: n = 2 OK

Passo: Toma $n> 2$

\[
    HI : T(k) \leq 20.k.log \ k
    \\
    T(n) = T(\left\lceil \frac{n}{3}\right\rceil) + T(\left\lfloor \frac{2n}{3}\right\rfloor) + n
    \\
    \leq 20.\left\lceil \frac{n}{3}\right\rceil.log\left\lceil \frac{n}{3}\right\rceil + 20.\left\lfloor \frac{2n}{3}\right\rfloor.log \left\lfloor \frac{2n}{3}\right\rfloor + n
    \\
    \leq 20.\left(\frac{n+2}{3}\right)\left\lceil\log \frac{n}{3}\right\rceil + 20.\frac{2n}{3}.log \frac{2n}{3} + n
    \\
    \leq 20.\left(\frac{n+2}{3}\right).\left(\log \frac{n}{3} + 1\right) + 20.\frac{2n}{3}.log \frac{2n}{3} + n
    \\
    \leq 20.\left(\frac{n+2}{3}\right).\log \frac{2n}{3}+ 20.\frac{2n}{3}.log \frac{2n}{3} + n
    \\
    = 20.n.log\frac{2n}{3} + \frac{40}{3}.log \frac{2n}{3} + n  
    \\
    < 20.n.log\frac{2n}{3} +14.log \frac{2n}{3} + n
    \\
    20.n.log \ n + 20.n.log\frac{2}{3} + 14.log \ n +14.log \frac{2}{3} + n
    \\
    log\frac{2}{3} = - 0.584\dots
    \\
    < 20.n.log \ n - 11,6n + n + 14.log \ n - 8,12
    \\
    < 20.n.log \ n \underbrace{- 10n + 14.log \ n - 8}_{< 0 }
    \\
    < 20.n.log \ n
\]

##### An�lise
* Linhas 1 consumo $\theta(1)$
* Linhas 2 consumo $\theta(n)$
* Linhas 1-2 consumo $\theta()$