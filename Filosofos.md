Relatório — Jantar dos Filósofos

O Jantar dos Filósofos é um modelo clássico para explicar os problemas que acontecem quando vários processos tentam usar recursos compartilhados ao mesmo tempo. 
No cenário, temos cinco filósofos sentados em volta de uma mesa. Cada um alterna entre pensar e comer, mas para comer precisa de dois garfos: 
o da esquerda e o da direita. Como esses garfos são compartilhados, surgem riscos de travamento e injustiças no sistema.

No protocolo mais simples, cada filósofo tenta pegar primeiro um garfo e depois o outro. O problema é que isso pode gerar um impasse (deadlock): 
todos pegam um garfo ao mesmo tempo e ficam esperando pelo segundo, que nunca fica livre. Ninguém solta o que pegou, então todos ficam travados para sempre.

Existe também outro problema possível: a fome (inanição). Ela acontece quando um filósofo nunca consegue comer porque sempre chega alguém antes dele. 
Mesmo que o sistema não trave completamente, um processo específico pode ficar eternamente esperando.

Para entender por que o impasse acontece, usamos as Condições de Coffman, que dizem que um deadlock só ocorre quando quatro condições se mantêm ao mesmo tempo: 
exclusão mútua, manter-e-esperar, não-preempção e espera circular. O protocolo ingênuo atende às quatro condições, por isso pode travar.

A solução adotada foi a estratégia da hierarquia de recursos. 
Nela, cada garfo recebe um número, e todos os filósofos são obrigados a pegar sempre primeiro o garfo de menor número e só depois o garfo de maior número. Essa regra elimina a condição de espera circular, 
porque impede que os filósofos fiquem esperando recursos em forma de ciclo. Sem ciclo, não há como o deadlock surgir.

Além disso, esse protocolo também contribui para evitar a inanição, porque o acesso aos garfos segue uma ordem previsível e justa. Ninguém fica bloqueado para sempre, 
pois a ordem de aquisição dos recursos impede que um filósofo seja sempre “furado” pelos outros.

Assim, a solução cumpre dois objetivos importantes: evita impasse e reduz o risco de fome, mantendo o sistema funcionando de forma correta e justa para todos.

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
