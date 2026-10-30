Simulador do Algoritmo do Banqueiro (Dijkstra)

Disciplina: Sistemas Operacionais
Referência: Capítulo 6 – Sistemas Operacionais Modernos, Tanenbaum & Bos.
Desenvolvedora: Ana Carolina Quintela Alves
Linguagem: Python 3

1. Sobre o projeto

Este projeto implementa um simulador do Algoritmo do Banqueiro (Banker's Algorithm), desenvolvido por Edsger W. Dijkstra, cujo objetivo é gerenciar a alocação de recursos entre diferentes processos de um sistema operacional.

O algoritmo tem como principal finalidade evitar situações de deadlock (impasse), garantindo que os recursos sejam distribuídos somente quando a alocação resultante mantiver o sistema em um estado seguro.

O simulador trabalha com múltiplos processos e diferentes tipos de recursos, permitindo:

Calcular a quantidade de recursos disponíveis.
Identificar a necessidade restante de cada processo.
Verificar se o estado atual do sistema é seguro.
Determinar uma sequência segura de execução dos processos.
Avaliar solicitações de novos recursos.
Conceder ou negar solicitações conforme a disponibilidade e a segurança do sistema.
Realizar rollback quando uma tentativa de alocação resulta em um estado inseguro.
2. Conceitos utilizados

O algoritmo utiliza quatro estruturas principais:

Estrutura	Significado
E (Existing)	Quantidade total de recursos existentes no sistema.
C (Current)	Matriz que representa os recursos atualmente alocados a cada processo.
Max	Matriz que representa a quantidade máxima de recursos que cada processo poderá solicitar.
Need	Matriz que representa a necessidade restante de cada processo.
A (Available)	Vetor que representa os recursos disponíveis no momento.
2.1. Cálculo da necessidade (Need)

A necessidade restante de cada processo é calculada pela diferença entre a quantidade máxima declarada e a quantidade já alocada.

Fórmula:

Need = Max - Allocation

2.2. Cálculo dos recursos disponíveis (Available)

Os recursos disponíveis são calculados subtraindo do total existente a soma dos recursos alocados a todos os processos.

Fórmula:

Available = E - Soma(Allocation)

2.3. Verificação de estado seguro

Para verificar se o sistema está seguro, o algoritmo utiliza dois vetores auxiliares:

Work: representa os recursos temporariamente disponíveis durante a simulação.
Finish: indica se determinado processo já foi considerado concluído.

O algoritmo procura um processo cuja necessidade seja menor ou igual aos recursos disponíveis:

Need[i] <= Work

Quando encontra um processo que pode ser atendido, considera sua execução concluída e devolve ao vetor Work os recursos que estavam alocados a ele.

Esse procedimento é repetido até que todos os processos sejam concluídos ou até que não seja possível encontrar outro processo que possa ser atendido.

Se todos os processos puderem concluir sua execução, o sistema está em estado seguro. Caso contrário, o sistema encontra-se em estado inseguro.

3. Configuração inicial do simulador

O exemplo utiliza cinco processos (P0, P1, P2, P3 e P4) e três tipos de recursos (R0, R1 e R2).

3.1. Recursos totais
python
total_resources = [10, 5, 7]


Isso significa que o sistema possui:

10 unidades do recurso R0;
5 unidades do recurso R1;
7 unidades do recurso R2.
3.2. Matriz de alocação atual
Processo	R0	R1	R2
P0	0	1	0
P1	2	0	0
P2	3	0	2
P3	2	1	1
P4	0	0	2
3.3. Matriz de necessidade máxima
Processo	R0	R1	R2
P0	7	5	3
P1	3	2	2
P2	9	0	2
P3	2	2	2
P4	4	3	3

A partir dessas informações, o simulador calcula automaticamente as necessidades restantes e os recursos disponíveis.

4. Teste 1 – Verificação do estado inicial
Objetivo

O primeiro teste tem como objetivo verificar se o sistema está inicialmente em um estado seguro, considerando os recursos existentes, as alocações atuais e as necessidades máximas declaradas pelos processos.

O método utilizado é:

python
banker.display_state()
is_safe, sequence = banker.is_safe_state(verbose=True)

4.1. Estado inicial do sistema

Após os cálculos, o simulador apresenta os seguintes valores:

Recursos totais:

E = [10, 5, 7]

Recursos disponíveis:

A = [3, 3, 2]

Matriz de necessidade (Need):

Processo	Need (R0, R1, R2)
P0	[7, 4, 3]
P1	[1, 2, 2]
P2	[6, 0, 0]
P3	[0, 1, 1]
P4	[4, 3, 1]
4.2. Análise da execução

O algoritmo inicia com o vetor Work igual ao vetor Available:

Work = [3, 3, 2]

Em seguida, verifica quais processos podem ser executados com os recursos disponíveis.

Passo 1 – Execução de P1

O processo P1 necessita de [1, 2, 2].

Como sua necessidade é menor ou igual ao vetor Work [3, 3, 2], ele pode ser executado.

Após sua conclusão, libera os recursos que estavam alocados a ele: [2, 0, 0].

Novo vetor Work:

[5, 3, 2]

Passo 2 – Execução de P3

O processo P3 necessita de [0, 1, 1].

Como sua necessidade pode ser atendida pelo vetor Work [5, 3, 2], sua execução é permitida.

Após liberar seus recursos alocados [2, 1, 1], o vetor Work passa a ser:

[7, 4, 3]

Passo 3 – Execução de P0

O processo P0 necessita de [7, 4, 3].

Como os recursos disponíveis são exatamente [7, 4, 3], o processo pode ser executado.

Após sua conclusão, libera [0, 1, 0].

Novo vetor Work:

[7, 5, 3]

Passo 4 – Execução de P2

O processo P2 necessita de [6, 0, 0].

Como essa quantidade pode ser atendida pelos recursos disponíveis, P2 executa e libera [3, 0, 2].

Novo vetor Work:

[10, 5, 5]

Passo 5 – Execução de P4

O processo P4 necessita de [4, 3, 1].

Como os recursos disponíveis são [10, 5, 5], sua execução também pode ser concluída.

Após liberar [0, 0, 2], o vetor Work passa a ser:

[10, 5, 7]

4.3. Resultado do teste 1

O simulador apresenta a seguinte mensagem:

text
SUCESSO: O sistema está em um ESTADO SEGURO!

Sequência Segura de Execução:
P1 -> P3 -> P0 -> P2 -> P4


Interpretação:

O primeiro teste demonstrou que o sistema está em estado seguro, pois foi possível encontrar uma sequência na qual todos os cinco processos conseguem executar e finalizar sem que ocorra falta de recursos.

A sequência segura encontrada foi:

P1 → P3 → P0 → P2 → P4

Ao final da simulação, todos os recursos são devolvidos, fazendo com que o vetor Work retorne à quantidade total de recursos existentes no sistema.

Portanto, o estado inicial atende às condições de segurança do Algoritmo do Banqueiro.

5. Teste 2 – Requisição válida e segura de P1
Objetivo

O segundo teste tem como objetivo simular uma solicitação de novos recursos pelo processo P1 e verificar se essa solicitação pode ser concedida sem comprometer a segurança do sistema.

A solicitação realizada é:

python
banker.request_resources(
    process_index=1,
    request_vector=[1, 0, 2]
)


Isso significa que P1 está solicitando:

1 unidade do recurso R0;
0 unidades do recurso R1;
2 unidades do recurso R2.
5.1. Verificação da solicitação

O algoritmo realiza três verificações antes de efetivar a alocação.

Primeira verificação – Solicitação menor ou igual à necessidade (Request <= Need)

Antes da solicitação, P1 possui a seguinte necessidade:

Need[P1] = [1, 2, 2]

A solicitação é:

Request = [1, 0, 2]

Comparando os vetores:

[1, 0, 2] <= [1, 2, 2]

A condição é verdadeira.

Portanto, P1 não está solicitando mais recursos do que sua necessidade máxima declarada.

Segunda verificação – Solicitação menor ou igual aos recursos disponíveis (Request <= Available)

O vetor de recursos disponíveis antes da solicitação é:

Available = [3, 3, 2]

A solicitação é:

Request = [1, 0, 2]

Comparando os vetores:

[1, 0, 2] <= [3, 3, 2]

A condição também é verdadeira.

Isso significa que o sistema possui recursos suficientes para atender à solicitação.

Terceira verificação – Teste de segurança

Como as duas primeiras condições foram satisfeitas, o algoritmo realiza uma alocação provisória.

Os vetores são atualizados da seguinte forma:

text
Available = [3, 3, 2] - [1, 0, 2]
Available = [2, 3, 0]

Allocation[P1] = [2, 0, 0] + [1, 0, 2]
Allocation[P1] = [3, 0, 2]

Need[P1] = [1, 2, 2] - [1, 0, 2]
Need[P1] = [0, 2, 0]


Após a atualização, o algoritmo executa novamente a verificação de estado seguro.

5.2. Análise da execução após a solicitação

O novo vetor Work inicial passa a ser:

Work = [2, 3, 0]

O algoritmo verifica novamente os processos.

Passo 1 – P1

Necessidade: [0, 2, 0]

Disponibilidade: [2, 3, 0]

P1 pode executar e libera sua nova alocação [3, 0, 2].

Novo Work:

[5, 3, 2]

Passo 2 – P3

Necessidade: [0, 1, 1]

P3 pode executar e libera [2, 1, 1].

Novo Work:

[7, 4, 3]

Passo 3 – P0

Necessidade: [7, 4, 3]

P0 pode executar e libera [0, 1, 0].

Novo Work:

[7, 5, 3]

Passo 4 – P2

Necessidade: [6, 0, 0]

P2 pode executar e libera [3, 0, 2].

Novo Work:

[10, 5, 5]

Passo 5 – P4

Necessidade: [4, 3, 1]

P4 pode executar e libera [0, 0, 2].

Novo Work:

[10, 5, 7]

5.3. Resultado do teste 2

O simulador apresenta a seguinte mensagem:

text
SUCESSO: O sistema está em um ESTADO SEGURO!

Sequência Segura de Execução:
P1 -> P3 -> P0 -> P2 -> P4

REQUISIÇÃO CONCEDIDA A P1!

O estado resultante permanece SEGURO.


Interpretação:

O segundo teste demonstrou que uma solicitação de recursos pode ser concedida quando existe disponibilidade suficiente e a nova configuração do sistema continua permitindo a execução de todos os processos.

Mesmo após a concessão dos recursos a P1, foi possível encontrar uma sequência segura de execução.

Portanto, o algoritmo autoriza a solicitação e mantém a nova alocação.

5.4. Estado do sistema após a concessão
Processo	Alocado (C)	Necessidade (Need)
P0	[0, 1, 0]	[7, 4, 3]
P1	[3, 0, 2]	[0, 2, 0]
P2	[3, 0, 2]	[6, 0, 0]
P3	[2, 1, 1]	[0, 1, 1]
P4	[0, 0, 2]	[4, 3, 1]

Novo vetor de recursos disponíveis:

Available = [2, 3, 0]

É importante observar que a alocação de P1 foi efetivamente atualizada e permanece registrada no sistema após a conclusão do teste.

6. Teste 3 – Requisição de recursos por P4
Objetivo

O terceiro teste tem como finalidade demonstrar o comportamento do algoritmo quando um processo solicita uma quantidade de recursos superior à disponibilidade atual do sistema.

A solicitação realizada é:

python
banker.request_resources(
    process_index=4,
    request_vector=[3, 3, 0]
)


Isso significa que P4 está solicitando:

3 unidades do recurso R0;
3 unidades do recurso R1;
0 unidades do recurso R2.
6.1. Verificação da solicitação

Após o segundo teste, os recursos disponíveis são:

Available = [2, 3, 0]

A necessidade restante de P4 é:

Need[P4] = [4, 3, 1]

A solicitação é:

Request = [3, 3, 0]

Primeira verificação – Request <= Need

Comparando:

[3, 3, 0] <= [4, 3, 1]

A condição é verdadeira.

Portanto, P4 está solicitando uma quantidade dentro de sua necessidade máxima declarada.

Segunda verificação – Request <= Available

Comparando:

[3, 3, 0] <= [2, 3, 0]

A condição é falsa para o recurso R0, pois P4 solicita 3 unidades, mas o sistema possui apenas 2 unidades disponíveis.

A comparação que provoca a negação é:

3 > 2

Dessa forma, a solicitação não pode ser atendida naquele momento.

6.2. Resultado do teste 3

O simulador apresenta a seguinte mensagem:

text
PROCESSO BLOQUEADO: Recursos insuficientes disponíveis no momento.

Solicitado: 3 > Disponível 2 para o recurso R0

P4 deve aguardar a liberação de recursos por outros processos.


Interpretação:

O terceiro teste demonstra que o algoritmo impede uma alocação quando a quantidade solicitada ultrapassa os recursos disponíveis.

Nesse caso, a solicitação de P4 é válida em relação à sua necessidade máxima, mas não pode ser atendida devido à falta de unidades disponíveis do recurso R0.

O algoritmo interrompe a solicitação antes de realizar qualquer alteração nas matrizes de alocação e necessidade.

Consequentemente:

Nenhum recurso adicional é concedido a P4.
A matriz Allocation permanece inalterada.
A matriz Need permanece inalterada.
O vetor Available permanece inalterado.
P4 deverá aguardar a liberação de recursos por outros processos.

É importante destacar que, neste teste, a solicitação é negada por insuficiência de recursos disponíveis, e não por uma comprovação de estado inseguro.

A verificação de segurança não chega a ser executada, pois a solicitação não ultrapassa a segunda etapa do método request_resources().

7. Comparação dos resultados dos testes

A tabela abaixo apresenta um resumo dos três cenários executados pelo simulador.

Teste	Processo	Solicitação	Resultado
Teste 1	Todos	Verificação inicial	Estado seguro
Teste 2	P1	[1, 0, 2]	Solicitação concedida
Teste 3	P4	[3, 3, 0]	Solicitação negada
Resumo das condições avaliadas

Teste 1: O sistema possui uma sequência segura para executar todos os processos.

Teste 2: A solicitação de P1 atende aos limites de necessidade e disponibilidade. Após a alocação provisória, o sistema continua seguro, permitindo a concessão.

Teste 3: A solicitação de P4 respeita sua necessidade máxima, mas ultrapassa a quantidade disponível do recurso R0. Por isso, o algoritmo bloqueia a solicitação.

8. Mecanismo de Rollback

O código também implementa um mecanismo de rollback para situações em que uma solicitação passa pelas verificações de necessidade e disponibilidade, mas provoca um estado inseguro após a alocação provisória.

O procedimento está implementado no método request_resources():

python
if is_safe:
    print("REQUISIÇÃO CONCEDIDA!")
    return True
else:
    for j in range(self.num_resources):
        self.available[j] += request_vector[j]
        self.allocation[process_index][j] -= request_vector[j]
        self.need[process_index][j] += request_vector[j]

    print("Estado original restaurado.")
    return False


Esse mecanismo restaura os valores originais das estruturas caso a tentativa de alocação resulte em um estado inseguro.

Observação: O rollback está implementado no simulador, mas não é acionado em nenhum dos três testes apresentados. No terceiro teste, a solicitação é interrompida anteriormente, devido à insuficiência de recursos disponíveis.

9. Conclusão

Com a execução dos três testes, foi possível demonstrar o funcionamento do Algoritmo do Banqueiro na administração de recursos de um sistema operacional.

O primeiro teste comprovou que o estado inicial do sistema é seguro, identificando uma sequência de execução na qual todos os processos conseguem finalizar suas atividades.

O segundo teste demonstrou que o algoritmo permite a concessão de recursos adicionais quando a solicitação respeita os limites declarados e a alocação resultante mantém o sistema em estado seguro.

Já o terceiro teste evidenciou que o algoritmo impede solicitações que não podem ser atendidas devido à insuficiência de recursos disponíveis, preservando a configuração atual do sistema.

Dessa forma, o simulador demonstra na prática como o Algoritmo do Banqueiro contribui para o gerenciamento eficiente de recursos, evitando alocações que possam comprometer a execução dos processos e prevenindo situações de deadlock.

O desenvolvimento permitiu compreender a importância da análise prévia de cada solicitação, da identificação de sequências seguras e da utilização de mecanismos de reversão de alocação.
