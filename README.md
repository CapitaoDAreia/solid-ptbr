# Notas pessoais: Princípios SOLID 

Essas anoações derivam das aulas ministradas por Robert C Martin no curso de MBA em Arquitetura Full Cycle da Full Cycle.

Must Read: 
- [Agile Software Development, Principles, Patterns, and Practices](https://www.amazon.com.br/Software-Development-Principles-Patterns-Practices/dp/0135974445) -Robert C Martin
- [Object-Oriented Software Construction](https://www.amazon.com/Object-Oriented-Software-Construction-Book-CD-ROM/dp/0136291554) - Bertrand Meyer
- [Family Values: A Behavioral Notion of Subtyping](https://apps.dtic.mil/sti/pdfs/ADA269102.pdf) - Barbara Liskov & Jeannette M. Wing
## História 
Os princípios SOLID nasceram de debates e propostas feitas entre Robert C Martin e alguns colegas enquanto trabalhavam juntos em alguns projetos, sendo um deles uma rede social chamada Usenet. Dentre esses colegas que contribuíram com o desenvolvimento do SOLID estão Jim Coclean, Barbara Liskov e Bertrand Meyer. 

---

## Por Que SOLID É Importante?
SOLID nos auxilia a escrever código de forma limpa, segura e sustentável. Considerando o contexto em que ocorre o desenvolvimento de software, tais objetivos não são meros caprichos ou formalidades.

Atualmente, o código que produzimos tem a finalidade de solucionar problemas empresariais críticos. Estamos lidando com softwares de meios de pagamento, sistemas embarcados para controlar comportamentos de automóveis, aeronaves e uma gama variada de aplicações. Assim, é evidente que o código que escrevemos tem um impacto direto na segurança física, psicológica e financeira das pessoas.

Portanto, é imprescindível que os desenvolvedores de software sejam profissionais, adotando padrões, disciplina e ética em sua prática.


---

## Código Sujo
Código sujo é caracterizado por uma quantidade significativa de más práticas. Ele pode atrasar a implementação de novas funcionalidades, gerar problemas inesperados de forma espontânea e é difícil de ser mantido.

Uma das principais causas desse cenário é a negligência dos envolvidos. Por diversos motivos, pode não haver uma administração eficiente do design do software, o que resulta na degradação gradual do código ao longo do tempo.

### Rigidez e Inflexibilidade
Um código sujo é frequentemente caracterizado por sua rigidez e inflexibilidade, o que se manifesta na resistência do código a mudanças. Isso pode ser atribuído a um alto nível de acoplamento ou outros fatores negativos. Quando tentamos modificar uma parte do código, é comum enfrentarmos problemas e descobrir que uma simples alteração em um módulo requer ajustes em outros 10, resultando em uma cadeia de mudanças.

Se existe uma semelhança entre essa definição e o seu código, significa que o seu código é inflexível e resistente a mudanças.


### Fragilidade
A fragilidade de um sistema se refere à sua propensão para quebrar de forma inesperada. Uma pequena modificação em uma parte do sistema pode resultar em falhas em outra parte distante, aparentemente não relacionada e essa falta de isolamento das mudanças é o que caracteriza a fragilidade. 

Este sintoma é particularmente preocupante para clientes e gerentes, pois é visível e tangível. Imagine a frustração de solicitar uma mudança mínima apenas para descobrir que isso causa falhas em áreas não relacionadas. 

Ruim, né?

Pois bem, sso indica uma perda de controle sobre o software, algo que clientes e gerentes percebem facilmente, o que pode levar a uma rigidez definitiva, onde os stakeholders decidem não realizar mais alterações por medo de mais problemas. 

E é aí que o seu software morreu por completo.

### Imobilidade
É um sintoma que se manifesta quando não é possível aplicar a reutilização em um módulo ou componente dado o seu altíssimo nível de acoplamento. É a incapacidade clara de reutilizar componentes/módulos.

---

## Prevenindo Código Sujo
Para evitar a degradação do software e o surgimento de código sujo, utilizamos alguns princípios que atuam como "firewalls", impedindo a proliferação de más práticas.

E como funciona isso? Bem, vejamos:

### Inversão de Dependência
- "Detail should depend upon high level policy"
- "High level policy should not depend upon detail"

No contexto da programação orientada a objetos, a Inversão de Dependência é um princípio chave. Ele propõe uma mudança na dinâmica de dependências entre os módulos do sistema. Em vez de os módulos de alto nível dependerem diretamente dos de baixo nível, as dependências são invertidas. Um módulo de alto nível declara suas dependências por meio de interfaces ou abstrações, e os módulos de baixo nível implementam essas interfaces. 

Quando aplicamos este princípio, detalhes dependem somente de políticas de alto nível, e políticas de alto nível não dependem de detalhes. Considere que políticas de alto nível são interfaces e abstrações e os detalhes são as implementações, são as concretizações dessas abstrações e interfaces. 

Dessa forma, a comunicação entre módulos ocorre no tempo de execução, reduzindo o acoplamento e aumentando a flexibilidade do sistema.

Também vale citar que:
- Dependência de código fonte: é quando declaramos em um módulo o uso de outro, importamos outro módulo, estabelecemos um acoplamento eferente;
- Dependência de fluxo de controle: é quando, em tempo de execução, o fluxo viaja de um módulo principal para outro.

Vejamos um exemplo onde os módulos de alto nível dependem diretamente dos módulos de baixo nível, viabilizando tanto a **dependência de código fonte** quando a **dependência de fluxo de controle**:
![image](https://hackmd.io/_uploads/HJd6nIj6a.png)

- Perceba que, como um efeito cascata, o módulo principal depende de módulos que dependem de módulos. E aqui, falamos dos dois tipos de dependência descritos.

No design orientado a objeto, algo diferente pode ser implementado mudando essa dinâmica. Podemos inverter as dependências, fazendo com que a dependência de código fonte vá na contramão do de fluxo de controle. Um módulo **HL1** deseja chamar um módulo **ML1**, e ao invés de fazê-lo, chama uma interface. Essa interface, por sua vez, é implementada pelo módulo **ML1**, deixando a comunicação entre módulos para o tempo de execução. 

Veja um exemplo dessa dinâmica abaixo:
![image](https://hackmd.io/_uploads/B1QWxPiap.png)

- Você entendeu? O componente **HL1** depende de uma abstração (interface) para chamar o método *F()*. Além disso, o módulo **ML1** implementa a interface que **HL1** consome. Deste modo, em tempo de execução, **HL1** executa o método implementado por **ML1**. Não é lindo?

#### Inversão de Dependência: Caso Prático
Imagine que você trabalha em um time de engenharia e chega uma demanda com a seguinte solicitação: "É necessário implementar um programa que lê de um teclado e imprime o que foi lido em uma impressora". 

Logo após o entendimento da demanda, você parte para o código e o resultado é este abaixo:

![image](https://hackmd.io/_uploads/HyAxLmapa.png)

- Perceba que existe uma relação de acoplamento aferente do módulo principal para os módulos **WritePrinter** e **ReadKeyboard**. Além da dependência de código fonte, existe também uma dependência de fluxo de controle.

Um tempo depois, chega uma nova demanda. Dessa vez, é necessário ler também a partir de um leitor de fita de papel e continuar entregando para a impressora. Você pensa na implementação e sua solução é a seguinte:

![image](https://hackmd.io/_uploads/rJ2yvmTpp.png)

- Dessa vez, você adiciona mais uma dependência ao seu módulo principal, aumentando a imobilidade do sistema em questão.

Como se não bastasse, já que um software útil nunca termina, surge uma nova demanda solicitando que seja possível escrever também no perfurador de papel, e sua implementação fica assim:
![image](https://hackmd.io/_uploads/BJILcmapa.png)

- É aqui que você percebe que o seu software está apodrecendo. Isso está acontecendo pela decisão inicial de acoplar completamente a entrada e saída de dados do módulo principal.

Agora, imagine que sua decisão inicial, ao escrever a primeira versão do seu projeto, seja essa:

![image](https://hackmd.io/_uploads/ryN0jmaa6.png)

- Perceba que, desta vez, o módulo principal depende de duas abstrações, uma é a Reader e a outra é a Writer. As duas abstrações possuem seus respectivos métodos e pouco importa para o seu módulo principal quem vai implementar essas abstrações. Neste caso, existe uma dependência de código fonte em relação a abstração, e uma dependência de fluxo de controle em relação a quem implementará essa abstração.

### Aplicando Princípios de Design de Classe
É amparado nessa técnica abordada anteriormente que os princípios SOLID funcionam, já que, apesar de o Princípio da Inversão de Dependência ser um princípio SOLID (o D, Dependency Inversion Principle), os demais princípios amparam-se nele como uma espécie de implementação de baixo nível, um ponto de partida, um fundamento. Isso acontece inclusive com os Padrões GoF. Enfim, vejamos:

#### Single Responsibility Principle
- "A class should have one and only one reason to change"
- "One actor that is the source of that change"

Diferente do que muitos podem concluir pelo nome deste princípio, ele não versa exatamente sobre a responsabilidade de um componente. O que na verdade está sendo dito é que um determinado componente deve possuir uma única razão para mudar, um único ator que deve ser a fonte dessa mudança.

Ou seja, o seu código deve estar separado por contextos. Por exemplo, em um sistema de CRM, não é interessante que uma função que auxilia cálculos de relatórios financeiros seja utilizada para auxiliar no cálculo de salários. 

E por que? 

Bem, a razão pela qual um software muda é o usuário. O software está constantemente se adaptando para atender a necessidades de usuários. No cenário exposto, imagine que o RH solicita uma mudança no cálculo de salários: você muda uma pequena função auxiliar. O resultado disso com certeza será refletido no cálculo de relatórios financeiros, o que expoe uma fragilidade na abordagem em questão. 

É por esse motivo que uma classe deve possuir somente uma razão para mudar, somente uma única fonte de mudança.

#### Open Closed Principle
- "Software artifact should be open for extension but closed for modification"

Este princípio nos orienta sobre o fator manutenibilidade de um artefato de software. A premissa básica é que qualquer artefato deve estar aberto para receber extensões que alterem ou incrementem o seu comportamento, mas fechados para modificação nas estruturas existentes.

O exemplo que usamos ao falar sobre **Inversão de Dependência** ilustra bem esse caso. Perceba que, ao depender de abstrações, o módulo principal não precisa ser alterado para que a leitura ou escrita seja estendida. E se for necessário mudar, a mudança é feita de forma localizada e isolada somente nos módulos de escrita ou leitura.

É interessante refletir sobre este princípio, pois a mudança é um fator que nos acompanhará enquanto estivermos desenvolvendo software. Compor estruturas que podem ser facilmente adaptadas, receptivas a incrementações, é uma alternativa sempre muito bem-vinda.

#### Liskov Substitution Principle
- "What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is uchanged when o1 is substituted for o2 then S is a subtype."

O Princípio de Substituição de Liskov foi formulado por Bárbara Liskov em 93, e diz que objetos podem ser substituídos por seus subtipos sem que para isso seja necessário modificar o cliente que os recebe.

Por exemplo, imagine que existe um determinado cliente que consome de um tipo **Forma**. Agora, imagine que existem dois subtipos que herdam desse tipo **Forma**: quadrado e retângulo. 

De acordo com o princípio em questão, deveria ser possível que **Forma** fossem substituível por **quadrado** e **retângulo** sem que seja necessário modificar o cliente.

No entanto, neste caso, não é possível. Repare que, a abordagem para o cálculo de área de um **quadrado** não é a mesma da abordagem para o cálculo de área de um **retângulo**. Além disso, as propriedades também diferem.
Desse modo, para que um dos subtipos substitua **Forma**, uma modificação no cliente seria necessária. 

E qual modificação seria essa? Bom, seria necessário para o cliente verificar o tipo antes de tomar qualquer decisão, evitando acessar dados não existentes ou qualquer outro comportamento inesperado.

Perceba que, além da violação do presente princípio, também violamos o Open Closed Principle, pois para fazer a substituição seria necessário fazer ajustes no cliente.

![image](https://hackmd.io/_uploads/SyO-YdGAp.png)

> OBS: Leve sempre em consideração a Regra da Representatividade, que diz: 
> - "o representante da coisas não compartilha as relações da coisa representada" 
> 
> Dois advogados que representam cada um uma pessoa em um processo de divórcio - onde essas pessoas representadas estão se divorciando - não estão se divorciando. Apesar da representação, os advogados não compartilham as mesmas relações do objeto representado.
>
> Isso é importante para que não pensemos em uma classe que representa um quadrado como um quadrado de fato. Sacou?

Outro exemplo que podemos citar, é um caso onde um **Client** consome um tipo **retangulo**, e cogitamos a mudança de **retangulo** para um possível subtipo, o **quadrado**.

![image](https://hackmd.io/_uploads/SkSvmHXAT.png)


Na mesma linha da problemática anterior, existem algumas considerações a se fazer:

1. O quadrado, diferente do retangulo, possui somente uma dimensão. Para que seja um quadrado todos os seus lados precisam ser exatamente iguais. O que não acontece com o retangulo. Ou seja, as propriedades diferem e isso impede uma substituição;
2. É possível, ao alterar altura do quadrado, determinar que a largura seja igualmente alterada, e vice-versa. No entanto, o cliente realmente espera isso? Considerando que o tipo anterior era um retangulo, o cliente espera que ao alterar altura, largura seja alterada? Com certeza não, o que pode ser outro ponto de inviabilidade para a substituição;
3. Agora, supondo que tudo o que já foi visto foi ignorado e a substituição de tipos foi feita mesmo assim. É necessário alterar o cliente para que haja uma adaptação de expectativas entre as antigas propriedades e as novas. E é aí que violamos o Opne Closed Principle.

Novamente, o Princípio da Substituição de Liskov se materializa quando, para um cliente, a substituição de um tipo consumido por seu subtipo pode ser realizada sem qualquer efeito colateral. O contrário disso é uma violação clara desse princípio.

>OBS: Algo importante para se observar é que, sempre que um subtipo desfaz de características de um tipo, o princípio em questão é quebrado. No geral, subclasses devem ter todas as características de suas classes mãe.

#### Interface Segregation Principle (ISP)
- "Don't depend on things you don't want"

Apesar do nome citar segregação de interfaces, a proposta do princípio não se limita somente a isso. Quando falamos do ISP, estamos falando de evitar dependências (tal qual implementações) desnecessárias entre dois ou mais componentes. 

Vamos começar discutindo um exemplo simples. Imagine que temos uma interface **Veiculo** que possui os métodos *ligarMotor* e *movimentar*. Agora imagine que implementemos essa interface em uma classe **Carro** e em uma classe **Bicicleta**. Teríamos algo parecido com o exemplo abaixo:

![image](https://hackmd.io/_uploads/SJPayPm06.png)

- Perceba que não faz o menor sentido uma bicicleta ligar um motor. Desse modo, o método *ligarMotor* na classe **Bicicleta** fica sem uma implementação. Então você deve pensar, por que raios este método está ali então?

Para ser mais preciso e prevenir a violação do Princípio da Segregação de Interfaces, podemos seguir pela abordagem abaixo:

![image](https://hackmd.io/_uploads/SJmxVD7CT.png)


- Perceba que dessa vez, temos três interfaces que segregam bem cada funcionalidade respeitando as especificidades da implementação proposta. Um veículo genérico se move, isso é verdade para todos os veículos. Além disso, as funcionalidades *ligarMotor* e *impulsionar* fazem sentido agora, estando cada uma delas implementadas de forma mais precisa.

Em um outro exemplo, temos um conjunto de módulos que depende de um único módulo. A este chamamos de "*Dependency Magnet*", um termo utilizado para se referir a um módulo que atrai acoplamento, que atrai dependências, que possui muitos acoplamentos aferentes, ou seja, muitos outros módulos dependem dele.

Veja no exemplo abaixo:

![image](https://hackmd.io/_uploads/ByX7HUmRT.png)

- Como já abordado anteriormente, diversas dependências de fluxo de controle e de código fonte podem gerar muitos problemas. Um nível de acoplamento muito alto também pode ser muito problemático.

Agora, e se aplicássemos o raciocínio do princípio em questão neste caso? Teríamos algo mais ou menos assim:

![image](https://hackmd.io/_uploads/SJrq_PXAp.png)

- Neste caso, resolvemos o problema do acoplamento utilizando diversas interfaces. Dessa vez, o módulo principal implementa diversas interfaces, cada uma com o seu objetivo bem definido. Os módulos de mais baixo nível, por sua vez, dependem da interface implementada pelo módulo principal.
