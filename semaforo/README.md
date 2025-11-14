

## 2 Metodologia

Foram executados dois programas em KJava:

**A) CorridaSemControle**
- 8 threads
- cada uma executando 250.000 incrementos
- total esperado: 2.000.000
- sem nenhuma sincronização


**B) CorridaComSemaforo**
- mesmo número de threads e operações
- mas usando `Semaphore(1, true)` para exclusão mútua
- garantindo que apenas uma thread executa `count++` por vez

_Os tempos foram medidos com `System.nanoTime()`._

## 3 Retornos Obtidos

### 3.1 Retorno da Versão sem Sincronização
``` ruby
Esperado: 2.000.000
Obtido:    707.412
Tempo:     0,038s
```

### Análise
O valor obtido é muito menor que o esperado devido à condição de corrida. A operação `count++` não é atômica: múltiplas threads leem e gravam o mesmo valor simultaneamente, causando perda de incrementos.

O tempo reduzido (0,038 s) ocorre porque não há bloqueios, filas ou alternâncias ordenadas de threads, tudo ocorre com máxima paralelização, porém sem correção.

### 3.2 Retorno da Versão Justo
``` ruby
Esperado: 2.000.000
Obtido:    2.000.000
Tempo:     18,702s
```

O valor é exato, pois o semáforo implementa exclusão mútua:

- quando uma thread chama acquire(), todas as outras esperam
- apenas um thread por vez executa count++
- a ordem de atendimento é FIFO (fair = true), evitando starvation

As operações `release()` e `acquire()` criam uma relação de happens-before, garantindo visibilidade e ordem das atualizações do contador entre threads.

O tempo elevado (18,702 s) reflete a forte serialização: embora existam várias threads, apenas uma atua na seção crítica de cada vez.