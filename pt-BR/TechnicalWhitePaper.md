# EOS.IO White Paper Técnico

**26 de Junho de 2017**

**Resumo:** O software EOS.IO introduz uma nova arquitectura de blockchain projetada para permitir escalabilidade vertical e horizontal de aplicativos descentralizados. Isto é conseguido através da criação de um estrutura similar a um sistema operacional sobre o qual aplicações podem ser construídas. O software fornece conta, autenticação, banco de dados, comunicação assíncrona e o agendamento da execução de aplicações em centenas de núcleos de CPU ou clusters. A tecnologia resultante é uma arquitectura de blockchain que escala a milhões de transações por segundo, elimina taxas de usuário e permite a implantação rápida e simples de aplicações decentralizadas.

**ATENÇÃO: TOKENS CRUPTOGRÁFICOS REFERIDOS NESTE DOCUMENTO SE REFEREM A TOKENS CRIPTOGRÁFICOS DE UM BLOCKCHAIN LANÇADO QUE ADOTA O SOFTWARE EOS.IO. ELES NÃO SE REFEREM AOS TOKENS COMPATÍVEIS COM ERC-20 QUE ESTÃO SENDO DISTRIBUÍDOS NO BLOCKCHAIN ETHEREUM EM CONEXÃO COM A DISTRIBUIÇÃO DE TOKENS EOS.**

Copyright © 2017 block.one

Sem permissão, qualquer um pode usar, reproduzir ou distribuir qualquer material em este documento para usos não comerciais e educacionais (ex. sem taxa ou fins comerciais) desde que a fonte original e o aviso de direitos autorais sejam citados.

**AVISO LEGAL:** Este White Paper Técnico do EOS.IO é apenas para fins informativos. block.one não garante a precisão das conclusões neste white paper, e este white paper é fornecido "como está". block.one does not make and expressly disclaims all representations and warranties, express, implied, statutory or otherwise, whatsoever, including, but not limited to: (i) warranties of merchantability, fitness for a particular purpose, suitability, usage, title or noninfringement; (ii) that the contents of this white paper are free from error; and (iii) that such contents will not infringe third-party rights. block.one and its affiliates shall have no liability for damages of any kind arising out of the use, reference to, or reliance on this white paper or any of the content contained herein, even if advised of the possibility of such damages. In no event will block.one or its affiliates be liable to any person or entity for any damages, losses, liabilities, costs or expenses of any kind, whether direct or indirect, consequential, compensatory, incidental, actual, exemplary, punitive or special for the use of, reference to, or reliance on this white paper or any of the content contained herein, including, without limitation, any loss of business, revenues, profits, data, use, goodwill or other intangible losses.

- [Background](#background)
- [Requisitos para Aplicações Blockchain](#requirements-for-blockchain-applications) 
  - [Suporte a Milhões de Usuários](#support-millions-of-users)
  - [Uso Gratuito](#free-usage)
  - [Simplicidade nas Atualizações e Recuperação de Falhas](#easy-upgrades-and-bug-recovery)
  - [Baixa Latência](#low-latency)
  - [Desempenho Sequencial](#sequential-performance)
  - [Desempenho Paralelo](#parallel-performance)
- [Algoritmo de Consenso (DPOS)](#consensus-algorithm-dpos) 
  - [Confirmação da Transação](#transaction-confirmation)
  - [Transação como Prova de Participação (TaPoS)](#transaction-as-proof-of-stake-tapos)
- [Contas](#accounts) 
  - [Mensagens & Handlers](#messages--handlers)
  - [Gerenciamento de Permissões Baseadas em Papeis](#role-based-permission-management) 
    - [Níveis de Permissão Nomeados](#named-permission-levels)
    - [Grupos de Handlers de Mensagens Nomeados](#named-message-handler-groups)
    - [Mapeamento de Permissões](#permission-mapping)
    - [Avaliação das Permissões](#evaluating-permissions) 
      - [Grupos de Permissão Padrão](#default-permission-groups)
      - [Avaliação Paralela de Permissões](#parallel-evaluation-of-permissions)
  - [Mensagens com Atraso Obrigatório](#messages-with-mandatory-delay)
  - [Recuperação de Chaves Roubadas](#recovery-from-stolen-keys)
- [Execução Paralela Determinística de Aplicações](#deterministic-parallel-execution-of-applications) 
  - [Minimizando a Latência de Comunicação](#minimizing-communication-latency)
  - [Handlers de Mensagens Somente de Leitura](#read-only-message-handlers)
  - [Transações Atômicas com Múltiplas Contas](#atomic-transactions-with-multiple-accounts)
  - [Avaliação Parcial do Estado do Blockchain](#partial-evaluation-of-blockchain-state)
  - [Agendamento Subjetivo baseado em Melhor Esforço](#subjective-best-effort-scheduling)
- [Modelo de Token e Uso de Recursos](#token-model-and-resource-usage) 
  - [Medições Objetivas e Subjetivas](#objective-and-subjective-measurements)
  - [Receptor Paga](#receiver-pays)
  - [Delegando Capacidade](#delegating-capacity)
  - [Separando os Custos de Transação do Valor do Token](#separating-transaction-costs-from-token-value)
  - [Custos de Armazenamento do Estado](#state-storage-costs)
  - [Recompensas de Bloco](#block-rewards)
  - [Aplicações de Benefício da Comunidade](#community-benefit-applications)
- [Governança](#governance) 
  - [Congelamento de Contas](#freezing-accounts)
  - [Alterando o Código da Conta](#changing-account-code)
  - [Constituição](#constitution)
  - [Atualizando o Protocolo & Constituição](#upgrading-the-protocol--constitution) 
    - [Mudanças de Emergência](#emergency-changes)
- [Scripts & Máquinas Virtuais](#scripts--virtual-machines) 
  - [Mensagens Definidas por Esquema](#schema-defined-messages)
  - [Banco de Dados Definido por Esquema](#schema-defined-database)
  - [Separando a Autenticação da Aplicação](#separating-authentication-from-application)
  - [Arquitetura Independente de Máquina Virtual](#virtual-machine-independent-architecture) 
    - [Web Assembly (WASM)](#web-assembly-wasm)
    - [Máquina Virtual de Ethereum (EVM)](#ethereum-virtual-machine-evm)
- [Comunicação Inter Blockchain](#inter-blockchain-communication) 
  - [Provas de Merkle para Validação Leve de Cliente (LCV)](#merkle-proofs-for-light-client-validation-lcv)
  - [Latência de Comunicação Interchain](#latency-of-interchain-communication)
  - [Proof of Completeness](#proof-of-completeness)
- [Conclusão](#conclusion)

# Contexto

A tecnologia Blockchain foi introduzida em 2008 com o lançamento da moeda bitcoin e desde então empreendedores e desenvolvedores tem intentado generalizar a tecnologia para oferecer suporte a uma mais ampla gama de aplicações num único blockchain.

Enquanto varias plataformas de blockchain têm tido dificuldades para suportar aplicações descentralizadas em funcionamento, blockchains especializados por aplicação como o exchange descentralizado BitShares (2014) e a plataforma de mídia social Steem (2016) tornaram-se blockchains intensamente utilizados com dezenas de milhares de usuários ativos diariamente. Eles conseguiram isto aumentando o desempenho para milhares de transações por segundo, reduzindo a latência para 1,5 segundos, eliminando taxas, e proporcionando uma experiência de usuário semelhante aos actualmente prestados pelos serviços centralizados existentes.

Plataformas de blockchain existentes, estão sobrecarregadas por grandes taxas e capacidade computacional limitada que impede a adoção generalizada do blockchain.

# Requisitos para Aplicações Blockchain

Para ser adotadas amplamente, aplicações que rodam no blockchain exigem uma plataforma que é suficientemente flexível para atender aos seguintes requisitos:

## Suporte a Milhões de Usuários

Disromper empresas como Ebay, Uber, AirBnB e Facebook, requer uma tecnologia blockchain capaz de lidar com dezenas de milhões de usuários ativos diariamente. Em certos casos, os aplicativos podem não funcionar a menos que uma massa crítica de usuários é atingida e, portanto, uma plataforma que pode lidar com uma quantidade massiva de usuários é fundamental.

## Uso Gratuito

Os desenvolvedores de aplicativos precisam da flexibilidade para oferecer aos usuários serviços gratuitos; usuários não devem ter que pagar para usar a plataforma ou se beneficiar de seus serviços. Uma plataforma de blockchain que é gratuita para ser usada pelos usuários provavelmente ganhará mais ampla adoção. Desenvolvedores e empresas podem criar estratégias de monetização eficaz.

## Atualizações fáceis e Recuperação de Falhas

Empresas construindo aplicações baseadas em blockchain precisam de flexibilidade para melhorar suas aplicações com novas funcionalidades.

Todos os softwares mais complexos são sujeitos a errores, mesmo com a verificação formal mais rigorosa. A plataforma deve ser robusta o suficiente para permitir a correção de erros quando eles ocorrerem inevitavelmente.

## Baixa Latência

Uma boa experiência do usuário exige feedback confiável com atraso de não mais que alguns segundos. Atrasos maiores frustram os usuários e fazem que as aplicações construídas sobre um blockchain sejam menos competitivas com alternativas não baseadas no blockchain.

## Desempenho Sequencial

Existem alguns aplicativos que não podem ser implementados com algoritmos paralelos devido a passos sequencialmente dependentes. Aplicações tais como exchanges precisam de bastante desempenho sequencial para lidar com grandes volumes e, portanto, uma plataforma com desempenho rápido sequencial é necessária.

## Desempenho Paralelo

Aplicações de grande escala precisam dividir a carga de trabalho entre vários processadores e computadores.

# Algoritmo de Consenso (DPOS)

O software EOS.IO utiliza o único algoritmo de consenso descentralizado capaz de atender aos requisitos de desempenho de aplicativos sobre o blockchain, [Prova de Participação Delegada (DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper). Neste algoritmo, quem detêm tokens de um blockchain adotando o software EOS pode selecionar produtores de bloco através de um sistema de votação de aprovação continua e qualquer um pode optar por participar na produção de blocos e será dada a oportunidade de produzir blocos proporcionais aos votos totais recebidos em relação a todos os outros produtores. Para blockchains privados os gestores poderiam usar os tokens para adicionar e remover o pessoal de TI.

O software EOS.IO permite que blocos sejam produzidos exatamente a cada 3 segundos e exatamente um único produtor está autorizado a produzir um bloco em qualquer ponto no tempo. Se o bloco não é produzido no horário agendado, então o bloco para aquele slot de tempo é ignorado. Quando um ou mais blocos são ignorados, há uma lacuna de 6 ou mais segundos no blockchain.

Usando o software EOS.IO blocos são produzidos em rodadas de 21. No início de cada rodada 21 produtores de blocos únicos são escolhidos. Os top 20 pela aprovação total são automaticamente escolhidos a cada rodada e o último produtor é escolhido proporcional ao seu número de votos em relação a outros produtores. Os produtores selecionados são embaralhados usando um número pseudo-aleatório derivado da hora do bloco. Esse embaralhamento é feito para garantir que todos os produtores mantenham conectividade equilibrada para todos os outros produtores.

Se um produtor perde um bloco e não produziu qualquer bloco nas últimas 24 horas, eles são removidos da consideração até eles notificarem a blockchain da sua intenção de começar a produzir blocos novamente. Isso garante que a rede opera sem problemas, minimizando o número de blocos que falhou por não agendar aqueles que se provou que não são confiáveis.

Em condições normais um blockchain DPOS não experimenta qualquer fork porque os produtores do bloco cooperarem para produzir blocos, ao invés de competir. No caso há uma bifurcação, consenso mudará automaticamente para a cadeia mais longa. Esta métrica funciona porque a taxa em que blocos são adicionados a um fork de cadeia blockchain é diretamente correlacionada com a porcentagem de produtores do bloco que compartilham o mesmo consenso. Em outras palavras, um fork de blockchain com mais produtores nele irá crescer em comprimento mais rápido do que um com poucos produtores. Além disso, nenhum produtor de bloco deve produzir blocos em dois forks ao mesmo tempo. Se um produtor de bloco é pego fazendo isto então o produtor de bloco será provavelmente eliminado. Evidencias criptográficas dessa produção dupla podem também ser usadas para remover automaticamente os abusadores.

## Confirmação da Transação

Os blockchains DPOS típicos tem 100% de participação dos produtores de blocos. Uma transação pode ser considerada confirmada com 99,9% de certeza, após uma média de 1,5 segundos de tempo de transmissão.

Existem alguns casos extraordinários onde um bug de software, congestionamento da Internet ou um produtor de bloco malicioso irá criar dois ou mais forks. Para ter absoluta certeza que uma transação é irreversível, um nó pode optar por esperar a confirmação de 15 dos 21 produtores de blocos. Baseado em uma configuração típica do software EOS.IO, isto levará a uma média de 45 segundos em circunstâncias normais. Por padrão, todos os nós irão considerar um bloco confirmado por 15 dos 21 produtores irreversível e não vão mudar para um fork que exclui aquele bloco independentemente do comprimento.

É possível para um nó avisar os usuários que há uma alta probabilidade de que eles estão em um fork minoritário dentro de 9 segundos do início de um fork. Depois de 2 blocos consecutivos perdidos há uma probabilidade de 95%, que um nó é um fork minoritário. Com 3 blocos perdidos consecutivos há um 99% de certeza de ser um fork minoritário. É possível gerar um modelo preditivo robusto que irá utilizar a informação sobre quais nós perdeu, recentes taxas de participação e de outros fatores que rapidamente avisar os operadores que algo está errado.

A resposta a tal aviso depende inteiramente da natureza das transações de negócio, mas a resposta mais simples é esperar por 15/21 confirmações até as paradas de advertência.

## Transação como Prova de Participação (TaPoS)

O software EOS.IO requer que cada transação inclua o hash de um cabeçalho de bloco recente. Este hash serve dois propósitos:

1. impede que uma repetição de uma transação num fork que não incluei o bloco referenciado; e
2. sinaliza a rede que um usuário específico e sua participação estão num fork específico.

Ao longo do tempo, todos os usuários acabam diretamente confirmando o blockchain, o que torna difícil forjar falsas correntes já que o falsificador não será capaz de migrar as operações da corrente legítima.

# Contas

O software EOS.IO permite que todas as contas sejam referenciadas por um nome humanamente legível exclusivo de 2 a 32 caracteres de comprimento. O nome é escolhido pelo criador da conta. Todas as contas devem ser financiadas com o saldo mínimo no momento que elas são criadas para cobrir o custo de armazenamento dos dados da conta. Os nomes de conta também oferecem suporte a namespaces tal que o proprietário da conta @domain é o único que pode criar a conta @user.domain.

Em um contexto descentralizado, os desenvolvedores de aplicativos vão pagar o custo nominal de criação da conta para inscrever um novo usuário. As empresas tradicionais já gastam quantias significativas de dinheiro por cliente que eles adquirem na forma de serviços de publicidade, enciclopédia, etc. O custo do financiamento de uma nova conta no blockchain deve ser insignificante em comparação. Felizmente, não há nenhuma necessidade de criar contas de usuários que já se inscreveram por outro aplicativo.

## Mensagens & Handlers

Cada conta pode enviar mensagens estruturadas para outras contas e pode definir scripts para manipular mensagens quando elas são recebidas. O software EOS.IO dá para cada conta seu próprio banco de dados particular, que só pode ser acessado por seus próprios handlers de mensagens. Scripts de manipulação de mensagens também podem enviar mensagens para outras contas. É a combinação de mensagens e manipuladores de mensagens automatizadas como EOS.IO define contratos inteligentes.

## Gerenciamento de Permissões Baseadas em Papeis

Gestão de permissão envolve determinar se ou não uma mensagem esta devidamente autorizada. A forma mais simples de gerenciamento de permissão é verificar que uma transação tem as assinaturas necessárias, mas isto implica que as assinaturas requeridas já são conhecidas. Geralmente autoridade é vinculada a indivíduos ou grupos de indivíduos e muitas vezes é compartimentada. O software EOS.IO fornece um sistema de gestão de permissão declarativa que da para as contas um alto nível controle com boa granularidade sobre quem pode fazer o quê e quando.

É crítico que o gerenciamento de autenticação e autorização seja padronizado e separado da lógica de negócios do aplicativo. Isso permite que ferramentas a serem desenvolvidas para gerenciar permissões de forma geral e também oferecem oportunidades significativas para otimização de desempenho.

Cada conta pode ser controlada por qualquer combinação ponderada de outras contas e chaves privadas. Isto cria uma estrutura de autoridade hierárquica que reflete como as permissões são organizadas na realidade e facilita mais do que nunca o controle por vários usuários sobre os fundos. Controle multi-usuário é o fator único com maior contribuição para a segurança, e, quando usado corretamente, pode eliminar consideravelmente o risco de roubo devido a hacking.

O software EOS.IO permite que as contas definam qual a combinação de chaves e/ou contas podem enviar um tipo específico de mensagem para outra conta. Por exemplo, é possível ter uma chave para uma conta de usuário mídia social e outro para acesso ao exchange. É até possível dar outras contas permissão para agir em nome de uma conta de usuário sem atribuir-lhes as chaves.

### Níveis de Permissão Nomeados

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Usando o software EOS.IO, contas podem definir níveis de permissões nomeadas cada um dos quais pode ser derivado de permissões nomeada de um nível mais alto. Cada nível de permissões nomeadas define uma autoridade; uma autoridade é uma verificação de múltiplas assinatura consistindo de chaves e/ou níveis de permissão nomeados de outras contas. Por exemplo, o nível de permissão de "Amigo" de uma conta pode ser definido para a conta para ser controlado igualitariamente por qualquer um dos amigos da conta.

Outro exemplo é o blockchain Steem que tem marretados três níveis de permissão nomeados: proprietário, ativo e postar. A permissão de postar só pode realizar ações sociais, tais como votação e postar, enquanto a permissão ativa pode fazer tudo, exceto a mudança do proprietário. A permissão de proprietário é para armazenamento frio e é capaz de fazer tudo. O software EOS.IO generaliza este conceito, permitindo que cada titular de conta defina sua própria hierarquia, bem como o agrupamento de ações.

### Grupos de Handlers de Mensagens Nomeados

O software EOS.IO permite que cada conta organize os seus próprios manipuladores de mensagens em grupos nomeados e aninhados. Estes grupos de manipuladores de mensagem nomeados podem ser referenciados por outras contas quando eles configuram seus níveis de permissão.

O grupo de manipulador de mensagem de nível mais alto é o nome da conta e o nível mais baixo é o tipo de mensagem individual, sendo recebido pela conta. Esses grupos podem ser referenciados da seguinte forma: **@accountname.groupa.subgroupb.MessageType**.

Sob este modelo, é possível para um contrato de câmbio agrupar a criação da ordem e o seu cancelamento separadamente de depositar e retirar. Este agrupamento pelo contrato de câmbio é uma conveniência para os usuários do exchange.

### Mapeamento de Permissões

O software EOS.IO permite que cada conta defina um mapeamento entre um Grupo Nomeado de Manipuladores de Mensagens de qualquer conta e seu Nível de Permissão Nomeado. Por exemplo, um titular de conta poderia mapear a aplicação de mídia social do titular da conta para o grupo de permissão do titular da conta "Amigo". Com esse mapeamento, qualquer amigo poderia postar como titular da conta na mídia social do titular da conta. Mesmo que eles postariam como titular da conta, ainda usariam suas próprias chaves para assinar a mensagem. Isto significa que sempre é possível identificar quais amigos usaram a conta e de que maneira.

### Avaliando Permissões

Quando se esta entregando uma mensagem do tipo "**Action**", de **@alice** para **@bob** o software EOS.IO primeiro verificará se **@alice** definiu um mapeamento de permissão para **@bob.groupa.subgroup.Action**. Se nada for encontrado então será verificado se existem mapeamentos para **@bob.groupa.subgroup**, depois **@bob.groupa** e, por último, **@bob**. Se não for encontrada nenhuma correspondência, então será assumido o mapeamento do grupo de permissão nomeada **@alice.active**.

Depois de um mapeamento ser identificado em seguida é validada a autoridade da assinatura usando o processo de múltiplas assinaturas e autoridade associada com a permissão nomeada. Se isso falhar, então ele percorre a permissão do pai e, finalmente, a permissão do proprietário, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Grupos de Permissão Padrão

A tecnologia EOS.IO também permite que todas as contas tenham um "owner" que pode fazer tudo e um grupo "active" que pode fazer tudo, exceto alterar o grupo proprietário. Todos os outros grupos de permissão são derivados de "active".

#### Avaliação Paralela de Permissões

O processo de avaliação de permissão é "somente leitura" e as alterações de permissões feitas por transações não terão efeito até ao final de um bloco. Isto significa que todas as chaves e avaliações de permissão para todas as transações podem ser executadas em paralelo. Além disso, isto significa que uma rápida validação da permissão é possível sem iniciar a custosa lógica do aplicativo que teria de ser revertida. Por fim, significa que as permissões de transação podem ser avaliadas enquanto as transações pendentes são recebidas e não precisam ser re-avaliadas quando elas são aplicadas.

Quando consideramos todas as coisas, a verificação de permissão representa uma percentagem significativa do poder computacional necessário para validar as transações. Fazer disto um processo solo leitura e trivialmente paralelizável permite um aumento dramático no desempenho.

Quando se esta re-processando o blockchain para regenerar o estado determinístico do log de mensagens não há nenhuma necessidade de avaliar as permissões novamente. O fato de que uma transação estar incluída em um bloco conhecidamente bom é suficiente para ignorar esta etapa. Isto reduz drasticamente a carga computacional associada a re-processamento de um blockchain sempre em crescimento.

## Mensagens com Atraso Obrigatório

Tempo é um componente crítico de segurança. Na maioria dos casos, não é possível saber se uma chave privada foi roubada até que ela tenha sido usada. Segurança baseada em tempo é ainda mais crítica quando as pessoas têm aplicações que requerem que as chaves sejam mantidas em computadores conectados à internet para o seu uso diário. O software EOS.IO permite que os desenvolvedores de aplicativos indiquem que certas mensagens devam esperar um período mínimo de tempo depois de ser incluídas em um bloco antes que elas possam ser aplicadas. Durante este tempo pode ser canceladas.

Os usuários podem então receber avisos através de e-mail ou mensagem de texto quando uma dessas mensagens é transmitida. Se eles não autorizaram isso, então eles podem usar o processo de recuperação de conta para recuperar sua conta e cancelar a mensagem.

The required delay depends upon how sensitive an operation is. Paying for a coffee can have no delay and be irreversible in seconds, while buying a house may require a 72 hour clearing period. Transferring an entire account to new control may take up to 30 days. The exact delays chosen are up to application developers and users.

## Recovery from Stolen Keys

The EOS.IO software provides users a way to restore control of their account when their keys are stolen. An account owner can use any owner key that was active in the last 30 days along with approval from their designated account recovery partner to reset the owner key on their account. The account recovery partner cannot reset control of the account without the help of the owner.

There is nothing for the hacker to gain by attempting to go through the recovery process because they already "control" the account. Furthermore, if they did go through the process, the recovery partner would likely demand identification and multi-factor authentication (phone and email). This would likely compromise the hacker or gain the hacker nothing in the process.

This process is also very different from a simple multi-signature arrangement. With a multi-signature transaction, there is another company that is party to every transaction that is executed, but with the recovery process the agent is only a party to the recovery process and has no power over the day-to-day transactions. This dramatically reduces costs and legal liabilities for everyone involved.

# Deterministic Parallel Execution of Applications

Blockchain consensus depends upon deterministic (reproducible) behavior. This means all parallel execution must be free from the use of mutexes or other locking primitives. Without locks there must be some way to guarantee that all accounts can only read and write their own private database. It also means that each account processes messages sequentially and that parallelism will be at the account level.

In an EOS.IO software-based blockchain, it is the job of the block producer to organize message delivery into independent threads so that they can be evaluated in parallel. The state of each account depends only upon the messages delivered to it. The schedule is the output of a block producer and will be deterministically executed, but the process for generating the schedule need not be deterministic. This means that block producers can utilize parallel algorithms to schedule transactions.

Part of parallel execution means that when a script generates a new message it does not get delivered immediately, instead it is scheduled to be delivered in the next cycle. The reason it cannot be delivered immediately is because the receiver may be actively modifying its own state in another thread.

## Minimizing Communication Latency

Latency is the time it takes for one account to send a message to another account and then receive a response. The goal is to enable two accounts to exchange messages back and forth within a single block without having to wait 3 seconds between each message. To enable this, the EOS.IO software divides each block into cycles. Each cycle is divided into threads and each thread contains a list of transactions. Each transaction contains a set of messages to be delivered. This structure can be visualized as a tree where alternating layers are processed sequentially and in parallel.

        Block
    
          Cycles (sequential)
    
            Threads (parallel)
    
              Transactions (sequential)
    
                Messages (sequential)
    
                  Receiver and Notified Accounts (parallel)
    

Transactions generated in one cycle can be delivered in any subsequent cycle or block. Block producers will keep adding cycles to a block until the maximum wall clock time has passed or there are no new generated transactions to deliver.

It is possible to use static analysis of a block to verify that within a given cycle no two threads contain transactions that modify the same account. So long as that invariant is maintained a block can be processed by running all threads in parallel.

## Read-Only Message Handlers

Some accounts may be able to process a message on a pass/fail basis without modifying their internal state. If this is the case then these handlers can be executed in parallel so long as only read-only message handlers for a particular account are included in one or more threads within a particular cycle.

## Atomic Transactions with Multiple Accounts

Sometimes it is desirable to ensure that messages are delivered to and accepted by multiple accounts atomically. In this case both messages are placed in one transaction and both accounts will be assigned the same thread and the messages applied sequentially. This situation is not ideal for performance and when it comes to "billing" users for usage, they will get billed by the number of unique accounts referenced by a transaction.

For performance and cost reasons it is best to minimize atomic operations involving two or more heavily utilized accounts.

## Partial Evaluation of Blockchain State

Scaling blockchain technology necessitates that components are modular. Everyone should not have to run everything, especially if they only need to use a small subset of the applications.

An exchange application developer runs full nodes for the purpose of displaying the exchange state to its users. This exchange application has no need for the state associated with social media applications. EOS.IO software allows any full node to pick any subset of applications to run. Messages delivered to other applications are safely ignored because an application's state is derived entirely from the messages that are delivered to it.

This has some significant implications on communication with other accounts. Most significantly it cannot be assumed that the state of the other account is accessible on the same machine. It also means that while it is tempting to enable "locks" that allow one account to synchronously call another account, this design pattern breaks down if the other account is not resident in memory.

All state communication among accounts must be passed via messages included in the blockchain.

## Subjective Best Effort Scheduling

The EOS.IO software cannot obligate block producers to deliver any message to any other account. Each block producer makes their own subjective measurement of the computational complexity and time required to process a transaction. This applies whether a transaction is generated by a user or automatically by a script.

On a launched blockchain adopting the EOS.IO software, at a network level all transactions are billed a fixed computational bandwidth cost regardless of whether it took .01ms or a full 10 ms to execute it. However, each individual block producer using the software may calculate resource usage using their own algorithm and measurements. When a block producer concludes that a transaction or account has consumed a disproportionate amount of the computational capacity they simply reject the transaction when producing their own block; however, they will still process the transaction if other block producers consider it valid.

In general so long as even 1 block producer considers a transaction as valid and under the resource usage limits then all other block producers will also accept it, but it may take up to 1 minute for the transaction to find that producer.

In some cases a producer may create a block that includes transactions that are an order of magnitude outside of acceptable ranges. In this case the next block producer may opt to reject the block and the tie will be broken by the third producer. This is no different than what would happen if a large block caused network propagation delays. The community would notice a pattern of abuse and eventually remove votes from the rogue producer.

This subjective evaluation of computational cost frees the blockchain from having to precisely and deterministically measure how long something takes to run. With this design there is no need to precisely count instructions which dramatically increases opportunities for optimization without breaking consensus.

# Token Model and Resource Usage

**PLEASE NOTE: CRYPTOGRAPHIC TOKENS REFERRED TO IN THIS WHITE PAPER REFER TO CRYPTOGRAPHIC TOKENS ON A LAUNCHED BLOCKCHAIN THAT ADOPTS THE EOS.IO SOFTWARE. THEY DO NOT REFER TO THE ERC-20 COMPATIBLE TOKENS BEING DISTRIBUTED ON THE ETHEREUM BLOCKCHAIN IN CONNECTION WITH THE EOS TOKEN DISTRIBUTION.**

All blockchains are resource constrained and require a system to prevent abuse. With a blockchain that uses EOS.IO software, there are three broad classes of resources that are consumed by applications:

1. Bandwidth and Log Storage (Disk);
2. Computation and Computational Backlog (CPU); and
3. State Storage (RAM).

Bandwidth and computation have two components, instantaneous usage and long-term usage. A blockchain maintains a log of all messages and this log is ultimately stored and downloaded by all full nodes. With the log of messages it is possible to reconstruct the state of all applications.

The computational debt is calculations that must be performed to regenerate state from the message log. If the computational debt grows too large then it becomes necessary to take snapshots of the blockchain's state and discard the blockchain's history. If computational debt grows too quickly then it may take 6 months to replay 1 year worth of transactions. It is critical, therefore, that the computational debt be carefully managed.

Blockchain state storage is information that is accessible from application logic. It includes information such as order books and account balances. If the state is never read by the application then it should not be stored. For example, blog post content and comments are not read by application logic so they should not be stored in the blockchain's state. Meanwhile the existence of a post/comment, the number of votes, and other properties do get stored as part of the blockchain's state.

Block producers publish their available capacity for bandwidth, computation, and state. The EOS.IO software allows each account to consume a percentage of the available capacity proportional to the amount of tokens held in a 3-day staking contract. For example, if a blockchain based on the EOS.IO software is launched and if an account holds 1% of the total tokens distributable pursuant to that blockchain, then that account has the potential to utilize 1% of the state storage capacity.

Adopting the EOS.IO software on a launched blockchain means bandwidth and computational capacity are allocated on a fractional reserve basis because they are transient (unused capacity cannot be saved for future use). The algorithm used by EOS.IO software is similar to the algorithm used by Steem to rate-limit bandwidth usage.

## Objective and Subjective Measurements

As discussed earlier, instrumenting computational usage has a significant impact on performance and optimization; therefore, all resource usage constraints are ultimately subjective and enforcement is done by block producers according to their individual algorithms and estimates.

That said, there are certain things that are trivial to measure objectively. The number of messages delivered and the size of the data stored in the internal database are cheap to measure objectively. The EOS.IO software enables block producers to apply the same algorithm over these objective measures but may choose to apply stricter subjective algorithms over subjective measurements.

## Receiver Pays

Traditionally, it is the business that pays for office space, computational power, and other costs required to run the business. The customer buys specific products from the business and the revenue from those product sales is used to cover the business costs of operation. Similarly, no website obligates its visitors to make micropayments for visiting its website to cover hosting costs. Therefore, decentralized applications should not force its customers to pay the blockchain directly for the use of the blockchain.

A launched blockchain that uses the EOS.IO software does not require its users to pay the blockchain directly for its use and therefore does not constrain or prevent a business from determining its own monetization strategy for its products.

## Delegating Capacity

A holder of tokens on a blockchain launched adopting the EOS.IO software who may not have an immediate need to consume all or part of the available bandwidth, can give or rent such unconsumed bandwidth to others; the block producers running EOS.IO software on such blockchain will recognize this delegation of capacity and allocate bandwidth accordingly.

## Separating Transaction costs from Token Value

One of the major benefits of the EOS.IO software is that the amount of bandwidth available to an application is entirely independent of any token price. If an application owner holds a relevant number of tokens on a blockchain adopting EOS.IO software, then the application can run indefinitely within a fixed state and bandwidth usage. In such case, developers and users are unaffected from any price volatility in the token market and therefore not reliant on a price feed. In other words, a blockchain that adopts the EOS.IO software enables block producers to naturally increase bandwidth, computation, and storage available per token independent of the token's value.

A blockchain using EOS.IO software also awards block producers tokens every time they produce a block. The value of the tokens will impact the amount of bandwidth, storage, and computation a producer can afford to purchase; this model naturally leverages rising token values to increase network performance.

## State Storage Costs

While bandwidth and computation can be delegated, storage of application state will require an application developer to hold tokens until that state is deleted. If state is never deleted then the tokens are effectively removed from circulation.

Every user account requires a certain amount of storage; therefore, every account must maintain a minimum balance. As storage capacity of the network increases this minimum required balance will fall.

## Block Rewards

A blockchain that adopts the EOS.IO software will award new tokens to a block producer every time a block is produced. In these circumstances, the number of tokens created is determined by the median of the desired pay published by all block producers. The EOS.IO software may be configured to enforce a cap on producer awards such that the total annual increase in token supply does not exceed 5%.

## Community Benefit Applications

In addition to electing block producers, pursuant to a blockchain based on the EOS.IO software, users can elect 3 community benefit applications also known as smart contracts. These 3 applications will receive tokens of up to a configured percent of the token supply per annum minus the tokens that have been paid to block producers. These smart contracts will receive tokens proportional to the votes each application has received from token holders. The elected applications or smart contracts can be replaced by newly elected applications or smart contracts by token holders.

# Governance

Governance is the process by which people reach consensus on subjective matters that cannot be captured entirely by software algorithms. An EOS.IO software-based blockchain implements a governance process that efficiently directs the existing influence of block producers. Absent a defined governance process, prior blockchains relied ad hoc, informal, and often controversial governance processes that result in unpredictable outcomes.

A blockchain based on the EOS.IO software recognizes that power originates with the token holders who delegate that power to the block producers. The block producers are given limited and checked authority to freeze accounts, update defective applications, and propose hard forking changes to the underlying protocol.

Embedded into the EOS.IO software is the election of block producers. Before any change can be made to the blockchain these block producers must approve it. If the block producers refuse to make changes desired by the token holders then they can be voted out. If the block producers make changes without permission of the token holders then all other non-producing full-node validators (exchanges, etc) will reject the change.

## Freezing Accounts

Sometimes a smart contact behaves in an aberrant or unpredictable manner and fails to perform as intended; other times an application or account may discover an exploit that enables it to consume an unreasonable amount of resources. When such issues inevitably occur, the block producers have the power to rectify such situations.

The block producers on all blockchains have the power to select which transactions are included in blocks which gives them the ability to freeze accounts. A blockchain using EOS.IO software formalizes this authority by subjecting the process of freezing an account to a 17/21 vote of active producers. If the producers abuse the power they can be voted out and an account will be unfrozen.

## Changing Account Code

When all else fails and an "unstoppable application" acts in an unpredictable manner, a blockchain using EOS.IO software allows the block producers to replace the account's code without hard forking the entire blockchain. Similar to the process of freezing an account, this replacement of the code requires a 17/21 vote of elected block producers.

## Constitution

The EOS.IO software enables blockchains to establish a peer-to-peer terms of service agreement or a binding contract among those users who sign it, referred to as a "constitution". The content of this constitution defines obligations among the users which cannot be entirely enforced by code and facilitates dispute resolution by establishing jurisdiction and choice of law along with other mutually accepted rules. Every transaction broadcast on the network must incorporate the hash of the constitution as part of the signature and thereby explicitly binds the signer to the contract.

The constitution also defines the human-readable intent of the source code protocol. This intent is used to identify the difference between a bug and a feature when errors occur and guides the community on what fixes are proper or improper.

## Upgrading the Protocol & Constitution

The EOS.IO software defines a process by which the protocol as defined by the canonical source code and its constitution, can be updated using the following process:

1. Block producers propose a change to the constitution and obtains 17/21 approval.
2. Block producers maintain 17/21 approval for 30 consecutive days.
3. All users are required to sign transactions using the hash of the new constitution.
4. Block producers adopt changes to the source code to reflect the change in the constitution and propose it to the blockchain using the hash of a git commit.
5. Block producers maintain 17/21 approval for 30 consecutive days.
6. Changes to the code take effect 7 days later, giving all full nodes 1 week to upgrade after ratification of the source code.
7. All nodes that do not upgrade to the new code shut down automatically.

By default configuration of the EOS.IO software, the process of updating the blockchain to add new features takes 2 to 3 months, while updates to fix non-critical bugs that do not require changes to the constitution can take 1 to 2 months.

### Emergency Changes

The block producers may accelerate the process if a software change is required to fix a harmful bug or security exploit that is actively harming users. Generally speaking it could be against the constitution for accelerated updates to introduce new features or fix harmless bugs.

# Scripts & Virtual Machines

The EOS.IO software will be first and foremost a platform for coordinating the delivery of authenticated messages to accounts. The details of scripting language and virtual machine are implementation specific details that are mostly independent from the design of the EOS.IO technology. Any language or virtual machine that is deterministic and properly sandboxed with sufficient performance can be integrated with the EOS.IO software API.

## Schema Defined Messages

All messages sent between accounts are defined by a schema which is part of the blockchain consensus state. This schema allows seamless conversion between binary and JSON representation of the messages.

## Schema Defined Database

Database state is also defined using a similar schema. This ensures that all data stored by all applications is in a format that can be interpreted as human readable JSON but stored and manipulated with the efficiency of binary.

## Separating Authentication from Application

To maximize parallelization opportunities and minimize the computational debt associated with regenerating application state from the transaction log, EOS.IO software separates validation logic into three sections:

1. Validating that a message is internally consistent;
2. Validating that all preconditions are valid; and
3. Modifying the application state.

Validating the internal consistency of a message is read-only and requires no access to blockchain state. This means that it can be performed with maximum parallelism. Validating preconditions, such as required balance, is read-only and therefore can also benefit from parallelism. Only modification of application state requires write access and must be processed sequentially for each application.

Authentication is the read-only process of verifying that a message can be applied. Application is actually doing the work. In real time both calculations are required to be performed, however once a transaction is included in the blockchain it is no longer necessary to perform the authentication operations.

## Virtual Machine Independent Architecture

It is the intention of the EOS.IO software-based blockchain that multiple virtual machines can be supported and new virtual machines added over time as necessary. For this reason, this paper will not discuss the details of any particular language or virtual machine. That said, there are two virtual machines that are currently being evaluated for use with an EOS.IO software-based blockchain.

### Web Assembly (WASM)

Web Assembly is an emerging web standard for building high performance web applications. With a few small modifications Web Assembly can be made deterministic and sandboxed. The benefit of Web Assembly is the widespread support from industry and that it enables contracts to be developed in familiar languages such as C or C++.

Ethereum developers have already begun modifying Web Assembly to provide suitable sandboxing and determinism in with their [Ethereum flavored Web Assembly (WASM)](https://github.com/ewasm/design). This approach can be easily adapted and integrated with EOS.IO software.

### Ethereum Virtual Machine (EVM)

This virtual machine has been used for most existing smart contracts and could be adapted to work within an EOS.IO blockchain. It is conceivable that EVM contracts could be run within their own sandbox inside an EOS.IO software-based blockchain and that with some adaptation EVM contracts could communicate with other EOS.IO software blockchain applications.

# Inter Blockchain Communication

EOS.IO software is designed to facilitate inter-blockchain communication. This is achieved by making it easy to generate proof of message existence and proof of message sequence. These proofs combined with an application architecture designed around message passing enables the details of inter-blockchain communication and proof validation to be hidden from application developers.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

## Merkle Proofs for Light Client Validation (LCV)

Integrating with other blockchains is much easier if clients do not need to process all transactions. After all, an exchange only cares about transfers in and out of the exchange and nothing more. It would also be ideal if the exchange chain could utilize lightweight merkle proofs of deposit rather than having to trust its own block producers entirely. At the very least a chain's block producers would like to maintain the smallest possible overhead when synchronizing with another blockchain.

The goal of LCV is to enable the generation of relatively light-weight proof of existence that can be validated by anyone tracking a relatively light-weight data set. In this case the objective is to prove that a particular transaction was included in a particular block and that the block is included in the verified history of a particular blockchain.

Bitcoin supports validation of transactions assuming all nodes have access to the full history of block headers which amounts to 4MB of block headers per year. At 10 transactions per second, a valid proof requires about 512 bytes. This works well for a blockchain with a 10 minute block interval, but is no longer "light" for blockchains with a 3 second block interval.

The EOS.IO software enables lightweight proofs for anyone who has any irreversible block header after the point in which the transaction was included. Using the hash-linked structure shown below it is possible to prove the existence of any transaction with a proof less than 1024 bytes in size. If it is assumed that validating nodes are keeping up with all block headers in the past day (2 MB of data), then proving these transactions will only require proofs 200 bytes long.

There is little incremental overhead associated with producing blocks with the proper hash-linking to enable these proofs which means there is no reason not to generate blocks this way.

When it comes time to validate proofs on other chains there are a wide variety of time/ space/ bandwidth optimizations that can be made. Tracking all block headers (420 MB/year) will keep proof sizes small. Tracking only recent headers can offer a trade off between minimal long-term storage and proof size. Alternatively, a blockchain can use a lazy evaluation approach where it remembers intermediate hashes of past proofs. New proofs only have to include links to the known sparse tree. The exact approach used will necessarily depend upon the percentage of foreign blocks that include transactions referenced by merkle proof.

After a certain density of interconnectedness it becomes more efficient to simply have one chain contain the entire block history of another chain and eliminate the need for proofs all together. For performance reasons, it is ideal to minimize the frequency of inter-chain proofs.

## Latency of Interchain Communication

When communicating with another outside blockchain, block producers must wait until there is 100% certainty that a transaction has been irreversibly confirmed by the other blockchain before accepting it as a valid input. Using an EOS.IO software-based blockchain and DPOS with 3 second blocks and 21 producers, this takes approximately 45 seconds. If a chain's block producers do not wait for irreversibility it would be like an exchange accepting a deposit that was later reversed and could impact the validity of the blockchain's consensus.

## Proof of Completeness

When using merkle proofs from outside blockchains, there is a significant difference between knowing that all transactions processed are valid and knowing that no transactions have been skipped or omitted. While it is impossible to prove that all of the most recent transactions are known, it is possible to prove that there have been no gaps in the transaction history. The EOS.IO software facilitates this by assigning a sequence number to every message delivered to every account. A user can use these sequence numbers to prove that all messages intended for a particular account have been processed and that they were processed in order.

# Conclusion

The EOS.IO software is designed from experience with proven concepts and best practices, and represents fundamental advancements in blockchain technology. The software is part of a holistic blueprint for a globally scalable blockchain society in which decentralised applications can be easily deployed and governed.