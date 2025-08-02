# Introdução ao problema abordado

Nesta atividade investigamos como diferentes implementações em Python afetam o tempo de execução ao calcular a soma dos primeiros _N_ inteiros positivos. A partir de operações simples, exploramos quatro estratégias—laço `for`, built-in `sum(range…)`, fórmula analítica e vetorização NumPy—e comparamos seu desempenho em função de _N_. O objetivo é compreender o impacto de overhead de linguagem, otimizações internas e complexidade algorítmica em cenários reais.

# Descrição do funcionamento do código

O notebook está dividido em quatro blocos principais:

1. **Medida de tempo com laço `for` vs. fórmula analítica**

   - Define `soma_iterativa(N)` que acumula `1 + 2 + … + N` em um loop Python.
   - Define `soma_analitica(N)` que calcula diretamente `N*(N+1)//2`.
   - Para cada valor de _N_ (10⁶, 10⁷, 10⁸), registra-se `time.time()` antes e depois da chamada, imprimindo o tempo gasto.

2. **Comparação usando listas**

   - Gera `nums = list(range(1, N+1))`.
   - Compara o tempo de um laço `for` sobre `nums` e de `sum(nums)` para _N_ = 10⁶ e 10⁷, novamente usando `time.time()`.

3. **Visualização em escala log–log**

   - Reúne medições de todas as abordagens num dicionário de listas de tempos.
   - Monta gráfico `matplotlib` em escala `log–log` com uma curva para cada método, permitindo inspeção visual da escalabilidade.

4. **Exploração vetorizada com NumPy**
   - Define `soma_numpy(N)` que cria `np.arange(1, N+1, dtype=int64)` e aplica `np.sum()`.
   - Compara seus tempos com as demais abordagens para _N_ = 10⁶, 10⁷ e 10⁸, exibindo todos os resultados em uma tabela formatada.

# Discussão crítica dos resultados obtidos

- A **fórmula analítica** demonstrou complexidade constante (_O(1)_), com tempo irrelevante mesmo para _N_ muito grandes, pois executa apenas duas operações aritméticas.
- A abordagem **`sum(range…)`** apresentou complexidade linear (_O(N)_), mas com coeficiente de tempo baixo, beneficiando-se da implementação em C que evita bytecodes Python a cada iteração.
- O **laço `for`** puro também é _O(N)_, porém sensivelmente mais lento devido ao overhead de dispatch de bytecode em cada iteração (gerenciamento de iterador, incremento e atribuição).
- A versão que soma sobre uma **lista pré-criada** (`for + list`) manteve comportamento similar ao laço sobre `range`, mas com overhead adicional de acesso à lista.
- A **vetorização NumPy** (criação de array + `np.sum`) escalou linearmente, porém com desempenho competitivo: o loop interno ocorre em C/Cython, potencialmente usando SIMD ou threads, embora inclua o custo inicial de alocar e preencher o array.

A visualização em escala log–log deixou claro o gap entre as curvas: a fórmula plana, depois NumPy, `sum(range…)`, laço `for` e lista, nesta ordem de custo.

# Relação com os conteúdos da disciplina

Esta atividade consolida conhecimentos de:

- **Análise de algoritmos** (notação _O(1)_ vs. _O(N)_ e interpretação de gráficos log–log).
- **Implementação em Python** (diferenças entre loops em bytecode e rotinas C embutidas).
- **Estruturas de dados** (iteradores `range` vs. listas vs. arrays NumPy).
- **Computação científica** (uso de NumPy para otimização, compreensão de overhead de alocação e vetorização).

Esses temas alinham-se diretamente às aulas de complexidade, otimização de código e bibliotecas científicas.

# Conclusão e comentários finais

O exercício evidenciou que escolhas simples de implementação podem alterar drasticamente o desempenho, sem afetar a correção do resultado. O principal ganho veio da fórmula analítica (O(1)) e das rotinas otimizadas em C (`sum`, NumPy). Como desafio, foi importante estruturar medições confiáveis e interpretar curvas log–log. Para trabalhos futuros, sugere-se explorar:

- Comparações com o módulo `timeit`.
- Variações de tipos de dados (por ex. `float32` vs. `int64`).
- Outras estratégias de acumulação (e.g. comprehensions, `itertools.accumulate`).
- Análise de overhead de alocação de memória em NumPy para arrays ainda maiores.
