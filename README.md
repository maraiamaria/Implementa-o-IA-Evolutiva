# README — AcMsF (VRP)

## Equipe

* Nomes  — RAIMUNDO NONATO DE OLIVEIRA FALCAO NETO e MARIA EDUARDA RIBEIRO RAMOS

---

## Descrição do experimento

O objetivo foi **replicar e comparar** os resultados do trabalho de Ochelska-Mierzejewska et al. (2021) para operadores de crossover (ERX, PMX, OX, AEX, CX) e métodos de seleção (elitismo, rank, roleta, torneio) aplicados ao VRP. Foram feitas adaptações importantes na representação e em operadores para garantir soluções válidas e estabilidade experimental, e então comparados os desvios (%) médios entre a solução ótima conhecida e as soluções obtidas.

Principais adaptações implentadas na replicação:

* **Representação (Giant Tour + Split):** em vez de inserir retornos ao depósito (`zeros`) na sequência genética, usamos uma *lista única* com a ordem dos clientes (Giant Tour) e uma função automática de **divisão por capacidade (Split)** que gera retornos ao depósito quando necessário. Isso garantiu 100% de indivíduos válidos e reduziu rotas inválidas.
* **Mutação simplificada:** adotou-se **apenas Swap (troca simples)** em vez dos dois tipos de mutação do artigo original, por simplicidade de implementação — com impactos na diversidade e convergência.
* **Implementação dos crossovers do zero:** PMX, OX, AEX, ERX implementados manualmente em Python (sem uso de bibliotecas prontas).
* **Instâncias:** como nem todos os mapas usados no artigo são públicos, usamos instâncias padrão do CVRPLIB equivalentes.

---

## 1) Instruções de execução

Abra `AcMsF.ipynb` e execute as células na ordem. O notebook contém:

* parser das instâncias VRP,
* implementação dos operadores (PMX, OX, AEX, ERX, etc.),
* rotina Split (Giant Tour → rotas viáveis),
* loop experimental (variação de seleção × crossover × instância),
* geração de tabelas e gráficos de comparação.


### 2) Reproduzir tabelas do relatório

O notebook/script salva os arquivos em `.png` contendo as médias dos desvios por operador e por método de seleção.

---

## Resumo de resultados (valores principais)

A seguir, resumo dos principais números extraídos do relatório de comparação (nossa replicação vs artigo):

### Pequenas instâncias — média por método de seleção (desvio %)

| Método (Artigo) |  Resultado Artigo   |     Nosso resultado |
| --------------- | ------------------: | ------------------: | 
| elitismo        |         16,85%      |     **24,38%**      |
| rank            |         18,97%      |     **35,73%**      |
| roleta          |         16,87%      |     **15,34%**      |
| torneio         |         17,14%      |     **16,53%**      |

---

### Pequenas instâncias — média por crossover (desvio %)

| Crossover (Artigo)       | Resultado Artigo | Nosso resultado |
| ------------------------ | ---------------: | --------------: |
| alternating_edges (AEX)  |           14,69% |      **25,69%** |
| cycle (CX)               |           20,41% |      **26,09%** |
| edge_recombination (ERX) |           12,88% |      **15,46%** |
| order (OX)               |           20,26% |      **22,72%** |
| partially_mapped (PMX)   |           19,04% |      **24,78%** |

---

### Médias (instâncias médias / AcMsF) — seleção

| Método (Artigo) | Resultado Artigo | Nosso resultado |
| --------------- | ---------------: | --------------: |
| elitismo        |           63,87% |      **37,48%** |
| rank            |           58,05% |      **78,89%** |
| roleta          |           66,18% |      **31,63%** |
| torneio         |           58,05% |      **29,73%** |

---

### Médias (instâncias médias) — crossover


| Crossover (Artigo) | Resultado Artigo | Nosso resultado |
| ------------------ | ---------------: | --------------: |
| alternating_edges  |           74,92% |      **51,30%** |
| cycle              |           49,72% |      **50,61%** |
| edge_recombination |           82,42% |      **32,82%** |
| order              |           50,68% |      **43,70%** |
| partially_mapped   |           49,95% |      **43,73%** |

---

### Grandes instâncias — seleção

| Método (Artigo) | Resultado Artigo | Nosso resultado |
| --------------- | ---------------: | --------------: |
| elitismo        |          111,74% |      **58,19%** |
| rank            |          104,95% |     **142,55%** |
| roleta          |          109,40% |      **65,68%** |
| torneio         |          103,31% |      **54,95%** |

---

### Grandes instâncias — crossover

| Crossover (Artigo) | Resultado Artigo | Nosso resultado |
| ------------------ | ---------------: | --------------: |
| alternating_edges  |          142,79% |      **95,59%** |
| cycle              |           83,13% |      **85,42%** |
| edge_recombination |             157% |      **68,94%** |
| order              |           81,43% |      **74,13%** |
| partially_mapped   |           71,40% |      **77,65%** |

---

**Observações interpretativas:**

* O **ERX (Edge Recombination)** provou ser o operador mais eficiente na nossa replicação em instâncias médias e grandes — diferença atribuída à representação Giant Tour + Split que preserva arestas e garante viabilidade.
* O **Rank** exhibitou comportamento ruim em grandes instâncias na nossa versão, provavelmente por convergência prematura associada à mutação simplificada (apenas Swap).
* Métodos como **Roleta** e **Torneio** foram mais estáveis na nossa implementação do que no artigo, indicando ganho prático ao evitar indivíduos inválidos.
  Todos esses resultados e as tabelas completas estão no PDF de comparação. 

---

## Referências

* Ochelska-Mierzejewska, J.; Poniszewska-Marańda, A.; Marańda, W. Selected Genetic Algorithms for Vehicle Routing Problem Solving. Electronics 2021, 10, 3147.
* Relatório deste repositório: `Comparação AcMsF.pdf`. 

