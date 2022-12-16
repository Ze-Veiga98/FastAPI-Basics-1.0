# Enunciado do Projeto 1 - Fprog 2022/23 LEFT

## Data de entrega: 23 de dezembro de 2022, às 16h59

## LOG alterações

- 9dez22 - Publicação do enunciado.

## 1. Introdução

O desenvolvimento de projetos complexos, sejam informáticos ou não,
requer a sua subdivisão em tarefas.
Estas tarefas são atribuídas a utilizadores que realizam as atividades
necessárias para completar com êxito cada tarefa.
O método [kanban](https://en.wikipedia.org/wiki/Kanban) foi desenvolvido
pela Toyota, no fim dos anos 40 do século passado, com o objetivo de
facilitar a produção *just-in-time*.
Mais recentemente a Microsoft ([Anderson 2013](https://www.amazon.com/Kanban-David-J-Anderson-ebook/dp/B0057H2M70))
adaptou o conceito ao desenvolvimento de software, mas o *kanban* é
utilizado em diversas outras áreas.
O objectivo deste projeto é o desenvolvimento
de um sistema de gestão de tarefas tipo *kanban*, com a contabilização
dos tempos gastos por cada tarefa e cada utilizador.
O objetivo do primeiro projeto é escrever um programa em **Python3**,
correspondendo às funções descritas na secção seguinte.

![kanban1](kanban1.jpg)

## 2. Especificação do problema
A execução de um projeto é divida em **tarefas**, que podem ser executadas em
paralelo.
As tarefas são executadas por utilizadores do sistema, que são responsáveis
por elas durante o tempo da sua execução.
Ao longo da sua execução a tarefa vai sendo associada a diversas
**atividades**, uma de cada vez.
Cada atividade executa uma operação específica correspondente a uma coluna
na representação *kanban*.
O tempo total de execução de uma tarefa deve ser contabilizado para poder
ser comparado com o tempo inicialmente previsto.
Assim, pode-se determinar atrasos na execução do projeto na sua totalidade
e, caso se justifique, acrescentar mais utilizadores.
Da mesma forma, cada **utilizador** deve contabilizar o tempo total gasto nas
diversas tarefas do projeto a que vai executando.

Inicialmente já existe uma atividade com a descrição `"TO_DO"` onde
serão inicialmente criadas todas as tarefas.
Na sua criação as tarefas têm todas um tempo gasto nulo (`0`),
não têm colaborador associado e são colocadas na atividade `"TO_DO"`.
As tarefas podem depois ser movidas para outras atividades, entretanto
criadas, desde que tenham um colaborador associado.
Sempre que uma tarefa regressa à atividade `"TO_DO"`, deixa de ter
um colaborador associado e o tempo gasto regressa a zero (`0`).

A representação das entidades fica ao critério de cada um, mas deve ser
documentada no código entregue (*doc-string* ou comentário `#`).

Apenas os construtores e as funções para as quais a verificação da
correção dos argumentos é necessária devem verificar a validade dos
argumentos.

Todas as entidades devem respeitar as barreiras de abstração dos outros
tipos de dados: atividade, tarefa e utilizador.
A funções auxiliares não devem ser chamadas diretamente e servem apenas
para garantir as barreiras de abstração.

A solução apresentada deve ser minimamente eficiente.

![kanban2](kanban2.jpg)

### 2.1 Representação da atividade
A atividade é usada para representar os tipos de operações a que
as tarefas podem ser sujeitas.
Além da atividade inicial `"TO_DO"` podem ser criadas inúmeras outras
atividades, dependendo do tipo de projeto a desenvolver.

Cada atividade é descrita por uma cadeia de carateres (`str`) constituída
apenas por letras maiúsculas e `_` (*underscore*),
com 4 a 12 carateres de comprimento (*inclusivé*).

#### 2.1.1 `cria_atividade: str -> atividade`
Esta função recebe uma cadeia de carateres correspondente à descrição
da atividade e devolve a atividade correspondente.
A função verifica a validade dos seus argumentos, gerando uma exceção
`ValueError` com a mensagem `cria_atividade: argumentos inválidos`
caso o seu argumento não seja válido.
```
>>> cria_atividade('to_do')
Traceback (most recent call last): <...>
ValueError: cria_atividade: argumentos inválidos
>>> act = cria_atividade('DONE')
```
#### 2.1.2 `eh_atividade: any -> bool`
Esta função recebe um argumento de qualquer tipo e devolve `True` se o seu
argumento corresponde a uma atividade e `False` caso contrário, sem nunca
gerar erros.
```
>>> eh_atividade(cria_atividade('DONE'))
True
>>> eh_atividade('DONE')
False
```
#### 2.1.3 `atividade_descricao: atividade -> str`
Esta função devolve a descrição do argumento atividade.
```
>>> act = cria_atividade('DONE')
>>> atividade_descricao(act)
'DONE'
```
#### 2.1.4 `atividade_tarefas: atividade -> tuple[tarefa]`
Esta função devolve um tuplo com todas as tarefas que se encontram a
executar na atividade, ordenado alfabeticamente pela descrição
da tarefa.
```
>>> act = cria_atividade('DONE')
>>> atividade_tarefas(act)
()
```
#### 2.1.5 Funções auxiliares
Deve realizar as funções `atividade_to_do()`,
`atividade_insere(act: atividade, tsk: tarefa)` e
`atividade_remove(act: atividade, tsk: tarefa)` para garantir a
barreira de abstração da atividade.

### 2.2 Representação do utilizador
O utilizador representa alguém que colabora na execução das tarefas.
Um utilizador pode colaborar simultaneamente em diversas tarefas,
qualquer que seja a atividade a que estas tarefas estejam associadas.

O utilizador é descrito por um identificador e uma descrição.
O identificador de um utilizador é uma cadeia de carateres não vazia,
com um comprimento máximo de 12 carateres (*inclusivé*), não podendo
conter o caráter **':'** (*dois pontos*).
A descrição do utilizador é uma cadeia de carateres constituída apenas
por carateres alfabéticos (maiúsculas ou minúsculas) e o caráter **'  '**
(*espaço branco*), com um comprimento não inferior a 12 carateres.

#### 2.2.1 `cria_utilizador: str x str -> utilizador`
Esta função recebe duas cadeias de carateres correspondentes ao
identificador do utilizador e à sua descrição, devolvendo
um utilizador com essas características.
A função verifica a validade dos seus argumentos, gerando uma exceção
`ValueError` com a mensagem `cria_utilizador: argumentos inválidos`
caso os seus argumentos não sejam válidos.
```
>>> user = cria_utilizador('ist99999', 'João da Silva')
```
#### 2.2.2 `eh_utilizador: any -> bool`
Esta função recebe um argumento de qualquer tipo e devolve `True` se o seu
argumento corresponde a um utilizador e `False` caso contrário, sem nunca
gerar erros.
```
>>> user = cria_utilizador('ist99999', 'João da Silva')
>>> eh_utilizador(user)
True
>>> eh_utilizador('ist99999')
False
```
#### 2.2.3 `utilizador_str: utilizador -> str`
Esta função devolve uma cadeia de carateres com o identificador do
utilizador seguido do tempo gasto, do número de tarefas a que está
associado no momento e da descrição do utilizador,
separados pelo caráter **':'**.
```
>>> user = cria_utilizador('ist99999', 'João da Silva')
>>> utilizador_str(user)
'ist99999:0.0:0:João da Silva'
```
#### 2.2.4 `utilizador_tarefas: utilizador -> str`
Esta função devolve uma cadeia de carateres com a descrição
de todas as tarefas a que o utilizador está a associado,
uma descrição por linha (separados por **'\n'**).
```
>>> user = cria_utilizador('ist99999', 'João da Silva')
>>> utilizador_tarefas(user)
''
```

#### 2.2.5 Funções auxiliares
Deve realizar as funções `utilizador_tempo(user: utilizador, dur: int)`,
`utilizador_insere(user: utilizador, tsk: tarefa)` e
`utilizador_remove(user: utilizador, tsk: tarefa)` para garantir a
barreira de abstração do utilizador.

### 2.3 Representação da tarefa
A tarefa corresponde a uma entidade que executa um conjunto de atividades,
sob a responsabilidade de um colaborador, até completar as operações
necessárias.

Cada tarefa é representada por uma descrição e uma duração,
estando associada uma atividade e podendo ter associado
um utilizador (ou colaborador).
A descrição da tarefa é única, ou seja, não podem haver outras tarefas
com a mesma descrição.
A descrição da tarefa é uma cadeia de carateres que não pode ser vazia ou
conter apenas carateres brancos (ver `isspace()`).
Todas as durações são valores numéricos (valores inteiros ou em vírgula
flutuante) não negativos.

#### 2.3.1 `cria_tarefa: str x float -> tarefa`
Esta função recebe uma descrição e uma duração, devolvendo a tarefa
criada ou a exceção `ValueError` com a mensagem
`cria_tarefa: argumentos inválidos`
caso os seus argumentos não sejam válidos.
```
>>> tsk = cria_tarefa('projeto de FP', 12.0)
```
#### 2.3.2 `eh_tarefa: any -> bool`
Esta função recebe um argumento de qualquer tipo e devolve `True` se o seu
argumento corresponde a uma tarefa e `False` caso contrário, sem nunca
gerar erros.
```
>>> eh_tarefa(cria_tarefa('projeto de FP', 12.0))
True
>>> eh_tarefa(1.0)
False
```
#### 2.3.3 `tarefa_atividade: tarefa -> atividade`
Esta função recebe uma tarefa e devolve a atividade a que está
associada.
```
>>> tsk = cria_tarefa('projeto de FP', 12.0)
>>> atividade_descricao(tarefa_atividade(tsk))
'TO_DO'
```
#### 2.3.4 `tarefa_representacao: tarefa -> tuple`
Esta função devolve um tuplo com cinco elementos:
a descrição da tarefa, a descrição da atividade associada,
o identificador do colaborador associado à tarefa e
as durações prevista e já utilizada (até ao momento).
Se a tarefa não tiver um colaborador associado,
o seu identificador deve ser substituído pela cadeia de carateres
vazia.
```
>>> tsk = cria_tarefa('projeto de FP', 12.0)
>>> tarefa_representacao(tsk)
('projeto de FP', 'TO_DO', '', 12.0, 0.0)
```
#### 2.3.5 `tarefa_colaborador: tarefa x utilizador x float -> tarefa`
Esta função altera o colaborador associado a uma tarefa.
A função recebe uma tarefa, um colaborador e uma duração,
devolvendo o seu primeiro argumento depois de alterado.
A duração representa o tempo gasto pelo colaborador anterior na
realização da sua parte da operação a que a tarefa é sujeita na
atividade atual.
A duração pode ser omitida, sendo considerada zero (`0`).
Se uma tarefa estiver associada à atividade `"TO_DO"` o valor da
duração deve ser ignorado, pois a tarefa ainda não iniciou a sua
execução, mesmo que já tenha um colaborador associado.
Em caso de erro deve ser gerada uma exceção
`ValueError` com a mensagem `tarefa_colaborador: operação inválida`.
```
>>> tsk = cria_tarefa('projeto de FP', 12.0)
>>> user = cria_utilizador('ist99999', 'João da Silva')
>>> tarefa_representacao(tarefa_colaborador(tsk, user))
('projeto de FP', 'TO_DO', 'ist99999', 12.0, 0.0)
```
#### 2.3.6 `tarefa_move: tarefa x atividade x float -> tarefa`
Esta função altera a atividade associada a uma tarefa.
A função recebe uma tarefa, uma atividade e uma duração,
devolvendo o seu primeiro argumento depois de alterado.
A tarefa deixa de estar associada à atividade atual e passa a
estar associada à atividade indicada no argumento.
A duração representa o tempo gasto pelo colaborador na realização
da sua parte da operação a que a tarefa é sujeita na atividade atual.
A duração pode ser omitida, sendo considerada zero (`0`).
Se uma tarefa estiver atualmente associada à atividade `"TO_DO"`,
esta só pode ser movida se a tarefa tiver um colaborador associado.
Em caso de erro deve ser gerada uma exceção
`ValueError` com a mensagem `tarefa_move: operação inválida`.
```
>>> tsk = cria_tarefa('projeto de FP', 12.0)
>>> act = cria_atividade('IN_PROGRESS')
>>> tarefa_move(tsk, act)
Traceback (most recent call last): <...>
ValueError: tarefa_move: operação inválida
>>> user = cria_utilizador('ist99999', 'João da Silva')
>>> tsk = tarefa_colaborador(tsk, user)
>>> tarefa_representacao(tarefa_move(tsk, act, 5.6))
('projeto de FP', 'IN_PROGRESS', 'ist99999', 12.0, 5.6)
```
#### 2.3.7 `tarefa_atraso: tarefa -> float`
Esta função devolve a diferença entre a duração inicial prevista
e o tempo já gasto da tarefa.
```
>>> tsk = cria_tarefa('projeto de FP', 12.0)
>>> doing = cria_atividade('IN_PROGRESS')
>>> done = cria_atividade('DONE')
>>> user = cria_utilizador('ist99999', 'João da Silva')
>>> tsk = tarefa_colaborador(tsk, user)
>>> tsk = tarefa_move(tsk, doing)
>>> tsk = tarefa_move(tsk, done, 5.6)
>>> tarefa_atraso(tsk)
6.4
```
#### 2.3.8 `tarefa_descricao: tarefa x str -> str`
Esta função altera a descrição associada a uma tarefa.
A função recebe uma tarefa e uma nova descrição,
devolvendo a descrição anterior.
```
>>> tsk = cria_tarefa('projeto de FP', 12.0)
>>> tarefa_descricao(tsk, 'projeto de FC')
'projeto de FP'
```

#### 2.3.9 Funções auxiliares
Deve realizar a função `tarefas()` para garantir a
barreira de abstração da tarefa.

## 3. Teste do projeto

Para executar os testes o ficheiro a testar deve ser designado por **kanban.py**.

Para testar o seu programa poderá executar os [testes públicos](test_public.py)
disponibilizados no repositório git *fp22info* da disciplina.

Além dos testes públicos existe um conjunto de testes privados e
testes às barreiras de abstração.
Esta avaliação, embora automática, só é realizada após
a data limite de entrega do projeto.

A título experimental, uma vez que a funcionalidade só foi disponibilizada em
outubro a ainda não está devidamente testada, poderá executar os testes,
públicos e privados (quando disponíveis), em *continuous integration*
(os testes correm automaticamente em cada `git push`).
Para tal deverá submeter um ficheiro nomeado `.gitlab-ci.yml` no diretório
principal do projeto com o conteúdo:
```
public:
    stage: test
    script:
        - python test_public.py
private:
    stage: test
    variables:
        PRJ: $CI_PROJECT_NAME
    script:
        - python $UPLOAD
```
Para os testes públicos executarem como indicado, deve igualmente submeter
o ficheiros `test_public.py` e observar os resultados no *browser* do
seu projeto em **jobs** no separador **CI/CD**.
Quando os testes privados estiverem disponíveis, os seus resultados podem
ser observados num página **wiki** do seu projeto com a data atual.
Os testes privados só executam em intervalos superiores a 15 minutos.

## 4. Entrega do Projeto

- A entrega do projeto é efetuada até à data limite exclusivamente através
do repositório atribuído a cada aluno (`fp22ist1xxxxxx`):
`https://gitlab.rnl.tecnico.ulisboa.pt/fp22left/fp22ist1xxxxxx`

- o projeto é avaliado com base na última entrega anterior à data limite,
sendo todos os *commits* após essa data ignorados.
Não se esqueça que só após um *git push* é que o código é submetido ao
servidor.

- Não é permitida a utilização de qualquer módulo ou função não disponível
*built-in* em **Python3** (não usar *import*).

- A realização de pelo menos um *commit* pressupõe o compromisso de honra
que apenas o aluno com acesso ao repositório realizou o projeto.
Será utilizada uma ferramenta automática de deteção de cópias, a ocorrência
de cópias será comunicada às entidades competentes e a nota do projeto será
0 (zero). Cópias, mesmo que parciais, são consideradas cópias.

## 5. Avaliação do Projeto

Na avaliação do projeto serão consideradas as seguintes componentes:

1. A primeira componente avalia o desempenho da funcionalidade do programa
realizado. Esta componente é avaliada automaticamente entre 0 e 12 valores.
Existem limites de tempo e memória durante a avaliação automática.

2. A segunda componente avalia a abstração do código, nomeadamente o
respeito pelas barreiras de abstração indicadas, num total de 4 valores.
Esta avaliação, embora automática, só é realizada após a data limite da entrega.

3. A terceira componente avalia a qualidade do código entregue, nomeadamente
os seguintes aspectos: comentários, indentação, estruturação, modularidade,
abstração, entre outros. Esta componente poderá variar entre -4 valores e
+4 valores relativamente à classificação calculada acima e será
atribuída posteriormente.

* Note-se que o facto de um projeto passar com sucesso o conjunto de testes
disponibilizado não implica que esse projeto esteja totalmente correcto,
pois os testes não são exaustivos. É da responsabilidade do aluno garantir
que o código produzido está correto no contexto do enunciado.

* Em caso algum será disponibilizado qualquer tipo de informação sobre os
casos de teste utilizados pelo sistema de avaliação automática. A totalidade
dos ficheiros de teste usados na avaliação do projeto serão disponibilizados
na página da disciplina após a data limite de entrega.

## 6. Recomendações para o desenvolvimento do projeto

As seguintes recomendações e aspetos correspondem a sugestões para evitar
maus hábitos de trabalho (e, consequentemente, más notas no projeto):

- Leia todo o enunciado, procurando perceber o objetivo das várias funções
pedidas. Em caso de dúvida de interpretação, utilize o horário de dúvidas
para esclarecer as suas questões.

- No processo de desenvolvimento do projeto, comece por implementar as
várias funções pela ordem apresentada no enunciado, seguindo as
metodologias estudadas na disciplina. Ao desenvolver cada uma das
funções pedidas, comece por perceber se pode usar alguma das anteriores.

- Para verificar a funcionalidade das suas funções, utilize os exemplos
fornecidos como casos de teste. Tenha o cuidado de reproduzir fielmente
as mensagens de erro e restantes *outputs*, conforme ilustrado nos vários
exemplos.

- Não pense que o projeto pode ser realizado nos últimos dias, devendo
considerar a *Lei de Murphy*:
todos os problemas são mais difíceis do que parecem; tudo demora mais
tempo do que nós pensamos; e se alguma coisa puder correr mal, ela vai
correr mal na pior altura possível.

- Não duplique código. Se duas funções são muito semelhantes é natural
que estas possam ser fundidas numa única, eventualmente com mais argumentos.

- Não se esqueça que as funções excessivamente grandes e complexas são
penalizadas no que respeita ao estilo de programação (`lizard`).

- Respeite as regras do estilo de programação python (`PEP-8`) no que
respeita `apresentação, comentários, convenções de nomes, *etc*. (`pylint`).

- A atitude “vou pôr agora o programa a correr de qualquer maneira e
depois preocupo-me com o estilo” é totalmente errada.

- Quando o programa gerar um erro, preocupe-se em descobrir qual a
causa do erro. As “marteladas” no código têm o efeito de distorcer cada
vez mais o código.

Bom trabalho.
