No exemplo de deadlock proposto, temos duas threads (T1 e T2) e dois locks (LOCK_A e LOCK_B). A Thread 1 tenta primeiro adquirir o LOCK_A e depois o LOCK_B. Já a Thread 2 faz o caminho inverso: primeiro LOCK_B e depois LOCK_A. Esse padrão de uso dos locks é suficiente para gerar um impasse se as duas tentarem executar ao mesmo tempo.

Quando isso acontece, a T1 pode pegar o LOCK_A e, logo em seguida, a T2 pega o LOCK_B. Depois disso, cada uma tenta adquirir o segundo lock. A T1 fica esperando o LOCK_B, que está com a T2, e a T2 fica esperando o LOCK_A, que está com a T1. Nenhuma libera o que já segurou e ambas ficam paradas para sempre. Esse é exatamente o cenário de deadlock: duas threads vivas, mas sem progresso.

Esse comportamento se encaixa nas condições de Coffman, que são as quatro condições necessárias para que um deadlock ocorra:

  - Exclusão mútua: cada lock só pode ser adquirido por uma thread por vez (LOCK_A e LOCK_B são exclusivos).
  - Manter-e-esperar (hold and wait): cada thread mantém um lock enquanto espera o outro (T1 segura A e espera B; T2 segura B e espera A).
  - Não preempção: os locks não podem ser tirados à força das threads; só são liberados quando a própria thread sai do bloco synchronized.
  - Espera circular: T1 espera por um recurso que está com T2, e T2 espera por um recurso que está com T1, formando um ciclo de espera.

No código original, essas quatro condições aparecem juntas, então o deadlock é possível e geralmente aparece ao rodar o programa.

Para corrigir o problema, podemos usar a mesma ideia da hierarquia de recursos usada no Jantar dos Filósofos. Em vez de cada thread pegar os locks em ordens diferentes, definimos uma ordem global fixa: por exemplo, primeiro sempre LOCK_A, depois sempre LOCK_B. Assim, todas as threads seguem a mesma ordem de aquisição dos recursos.

Com isso, a condição de espera circular deixa de existir, porque nenhuma thread mais tenta pegar B antes de A. Se todas sempre tentam adquirir A antes de B, não é possível formar um ciclo em que uma thread tenha B e espere A enquanto a outra tem A e espera B. Ao quebrar essa condição, o deadlock deixa de ser possível.


Essa solução é um exemplo prático de tratamento de deadlock por prevenção, quebrando uma das condições necessárias (espera circular) através de uma hierarquia de recursos, da mesma forma que foi feito na solução do problema dos filósofos.
