Relatório — Jantar dos Filósofos

O problema do Jantar dos Filósofos é uma forma de representar situações em que vários processos precisam usar os mesmos recursos ao mesmo tempo. 
A ideia é imaginar cinco filósofos sentados em uma mesa redonda. Cada um passa um tempo pensando e, quando resolve comer, precisa pegar dois garfos: 
o da esquerda e o da direita. O detalhe é que esses garfos são compartilhados, então dois filósofos sempre disputam o mesmo garfo.

No protocolo mais simples, cada filósofo simplesmente tenta pegar um garfo primeiro e depois o outro. 
Isso parece funcionar, mas leva a um problema sério: o impasse. 
Esse impasse acontece quando todos pegam um garfo ao mesmo tempo e ficam esperando eternamente pelo outro. Como ninguém solta o garfo que já pegou, todo mundo fica travado e ninguém come. 
É exatamente isso que chamamos de deadlock.

Esse tipo de travamento acontece quando quatro condições aparecem ao mesmo tempo. 
Essas condições são conhecidas como Condições de Coffman, e servem justamente para explicar por que o sistema pode parar. Elas são:

  - Exclusão mútua: cada recurso só pode ser usado por uma pessoa por vez (um garfo não pode ser compartilhado).
  - Manter e esperar: o processo segura o que já tem enquanto espera por outro recurso (o filósofo segura um garfo enquanto espera o outro).
  - Não-preempção: o recurso não pode ser tomado à força, só quem está usando pode devolver (ninguém pode arrancar o garfo da mão do filósofo).
  - Espera circular: existe um ciclo em que cada processo espera por um recurso que outro processo está segurando (um depende do outro em círculo).

No protocolo ingênuo, essas quatro condições acontecem ao mesmo tempo, então o deadlock é possível.

Para resolver isso, utilizei uma estratégia chamada hierarquia de recursos. 
Ela é bem simples: cada garfo recebe um número, e todos os filósofos são obrigados a pegar sempre primeiro o garfo de menor número e só depois o de maior número. 
Essa regra quebra a última condição, a espera circular, porque impede que se forme um ciclo de dependências entre os filósofos. Se não existe ciclo, não existe travamento.

Essa solução garante que, mesmo que um filósofo tenha que esperar um pouco, em algum momento todos vão conseguir comer. O sistema deixa de travar e continua funcionando de forma justa para todos.

Pseudocódigo:

    const N = 5

    para cada filósofo p em 0..N-1:
        garfo_esquerda(p) = p
        garfo_direita(p)  = (p + 1) mod N

    processo Filosofo(p):
        enquanto verdadeiro:
            pensar()
            estado[p] <- "com fome"

        left  = min(garfo_esquerda(p), garfo_direita(p))
        right = max(garfo_esquerda(p), garfo_direita(p))

        adquirir(left)      // espera até o garfo 'left' estar livre
        adquirir(right)     // espera até o garfo 'right' estar livre

        estado[p] <- "comendo"
        comer()

        liberar(right)
        liberar(left)

        estado[p] <- "pensando"
