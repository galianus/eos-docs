# EOS.IO Documento Técnico

**26 de Junio del año 2017**

**Resumen:** El software de EOS.IO introduce una nueva arquitectura blockchain diseñada para facilitar la escalado vertical y horizontal de aplicaciones descentralizadas. Esto es logrado creando una construcción similar a un sistema operativo sobre la cual se pueden construir aplicaciones. El software proporciona cuentas, autenticación, bases de datos, comunicación asincrónica y la programación de aplicaciones en cientos de núcleos o clústeres de CPU. La tecnología resultante es una arquitectura blockchain que se adapta a millones de transacciones por segundo, elimina las tarifas de los usuarios y permite el despliegue rápido y sencillo de las aplicaciones descentralizadas.

**TENGA EN CUENTA: LOS TOKENS CRIPTOGRÁFICOS MENCIONADOS EN ESTE DOCUMENTO SE REFIEREN A LOS TOKENS CRIPTOGRÁFICOS EN UN BLOCKCHAIN LANZADO QUE ADOPTA EL SOFTWARE EOS.IO. NO SE REFIEREN A LOS TOKENS COMPATIBLES ERC-20 DISTRIBUIDOS EN EL BLOCKCHAIN DE CAMPO DE ETHEREUM EN RELACIÓN CON LA DISTRIBUCIÓN DE TOKEN EOS.**

Copyright © 2017 block.one

Sin permiso, cualquiera puede usar, reproducir o distribuir cualquier material en este documento técnico para uso no comercial y educativo (es decir, que no sea por una tarifa o con fines comerciales) siempre que se cite la fuente original y el aviso de copyright correspondiente.

**DESCARGO DE RESPONSABILIDAD:** Este documento técnico de EOS.IO es solo para fines informativos. block.one no garantiza la exactitud o las conclusiones alcanzadas en este documento, y este documento se proporciona "tal cual". block.one no realiza y renuncia expresamente a todas las representaciones y garantías, expresas, implícitas, legales o de otro tipo, incluidas, entre otras, las siguientes: (i) garantías de comerciabilidad, idoneidad para un propósito particular, conveniencia, uso, título o no infracción; (ii) que el contenido de este documento no contiene errores; y (iii) que dichos contenidos no infrinjan los derechos de terceros. block.one y sus afiliados no tendrán responsabilidad por daños y perjuicios de cualquier tipo que surjan del uso, referencia o confianza en este documento o en cualquier contenido aquí incluido, incluso si es informada la posibilidad de dichos daños. En ningún caso block.one o sus afiliados será responsable ante cualquier persona o entidad por los daños, pérdidas, responsabilidades, costos o gastos de cualquier tipo, ya sean directos o indirectos, consecuentes, compensatorios, incidentales, reales, ejemplares, punitivos o especiales, para el uso, la referencia o la confianza en este documento o en cualquier contenido aquí incluido, que incluye, entre otros, cualquier pérdida de negocios, ingresos, ganancias, datos, uso, buena voluntad u otras pérdidas intangibles.

- [Antecedentes](#background)
- [Requisitos para aplicaciones Blockchain](#requirements-for-blockchain-applications) 
  - [Soporte para millones de usuarios](#support-millions-of-users)
  - [Uso libre](#free-usage)
  - [Actualizaciones Sencillas y Recuperación de Errores](#easy-upgrades-and-bug-recovery)
  - [Baja latencia](#low-latency)
  - [Rendimiento secuencial](#sequential-performance)
  - [Rendimiento paralelo](#parallel-performance)
- [Algoritmo de Consenso (DPOS)](#consensus-algorithm-dpos) 
  - [Confirmación de Transacción](#transaction-confirmation)
  - [Transacción como Prueba de Participación (TaPoS)](#transaction-as-proof-of-stake-tapos)
- [Cuentas](#accounts) 
  - [Mensajes & Controladores](#messages--handlers)
  - [Gestión de permisos basada en roles](#role-based-permission-management) 
    - [Niveles de permisos nominados](#named-permission-levels)
    - [Grupos de manejo de mensajes nominados](#named-message-handler-groups)
    - [Asignación de permisos](#permission-mapping)
    - [Evaluación de permisos](#evaluating-permissions) 
      - [Grupos de permisos predeterminados](#default-permission-groups)
      - [Evaluación paralela de permisos](#parallel-evaluation-of-permissions)
  - [Mensajes con retraso obligatorio](#messages-with-mandatory-delay)
  - [Recuperación de claves robadas](#recovery-from-stolen-keys)
- [Ejecución paralela determinista de aplicaciones](#deterministic-parallel-execution-of-applications) 
  - [Minimizando la latencia de comunicación](#minimizing-communication-latency)
  - [Controladores de mensajes de sólo lectura](#read-only-message-handlers)
  - [Transacciones atómicas con múltiples cuentas](#atomic-transactions-with-multiple-accounts)
  - [Evaluación parcial del estado del Blockchain](#partial-evaluation-of-blockchain-state)
  - [Mejor esfuerzo subjetivo de programación](#subjective-best-effort-scheduling)
- [Modelo de Token y uso de recursos](#token-model-and-resource-usage) 
  - [Mediciones objetivas y subjetivas](#objective-and-subjective-measurements)
  - [El receptor paga](#receiver-pays)
  - [Delegando capacidad](#delegating-capacity)
  - [Separación de los costos de transacción del valor del token](#separating-transaction-costs-from-token-value)
  - [Costos de almacenamiento del estado](#state-storage-costs)
  - [Recompensas de bloque](#block-rewards)
  - [Aplicaciones de beneficios comunitarios](#community-benefit-applications)
- [Gobernanza](#governance) 
  - [Congelación de cuentas](#freezing-accounts)
  - [Cambio de código de cuenta](#changing-account-code)
  - [Constitución](#constitution)
  - [Actualizando el Protocolo & Constitución](#upgrading-the-protocol--constitution) 
    - [Cambios de emergencia](#emergency-changes)
- [Scripts & Máquinas Virtuales](#scripts--virtual-machines) 
  - [Mensajes definidos por esquema](#schema-defined-messages)
  - [Base de datos definida por esquema](#schema-defined-database)
  - [Separación de la autenticación de la aplicación](#separating-authentication-from-application)
  - [Arquitectura independiente de la Máquina Virtual](#virtual-machine-independent-architecture) 
    - [Asamblea Web (WASM)](#web-assembly-wasm)
    - [Máquina Virtual de Ethereum (EVM)](#ethereum-virtual-machine-evm)
- [Comunicación Inter Blockchain](#inter-blockchain-communication) 
  - [Pruebas Merkle para validación de cliente ligero (LCV)](#merkle-proofs-for-light-client-validation-lcv)
  - [Latencia de la comunicación Interchain](#latency-of-interchain-communication)
  - [Prueba de integridad](#proof-of-completeness)
- [Conclusión](#conclusion)

# Antecedentes

La tecnología Blockchain se introdujo en el año 2008 con el lanzamiento de la moneda de bitcoin, y desde entonces los empresarios y desarrolladores han intentado generalizar la tecnología para admitir una gama más amplia de aplicaciones en una sola plataforma de blockchain.

Mientras que varias plataformas blockchain han tenido dificultades para soportar aplicaciones funcionales descentralizadas, blockchains específicos de aplicaciones como el Exchange Descentralizado de BitShares (2014) y la Plataforma de la Red Social Steem (2016) se han convertido en blockchains muy utilizados con decenas de miles de usuarios activos diariamente. Lo han logrado aumentando el rendimiento a miles de transacciones por segundo, lo que reduce la latencia a 1,5 segundos, eliminando las tarifas y brindando a los usuarios una experiencia similar a la que ofrecen actualmente los servicios centralizados.

Las plataformas existentes de blockchain están cargadas de grandes tarifas y sus capacidades computacionales limitadas impiden la adopción generalizada de la blockchain.

# Requisitos para Aplicaciones Blockchain

Para lograr un uso generalizado, las aplicaciones en el blockchain requieren una plataforma lo suficientemente flexible como para cumplir con los siguientes requisitos:

## Soporte para Millones de Usuarios

La interrupción de negocios como Ebay, Uber, AirBnB y Facebook requiere una tecnología blockchain capaz de manejar a decenas de millones de usuarios activos diariamente. En ciertos casos, las aplicaciones pueden no funcionar a menos que se alcance una masa crítica de usuarios y, por lo tanto, una plataforma que pueda manejar una cantidad masiva de usuarios es primordial.

## Uso libre

Los desarrolladores de aplicaciones necesitan la flexibilidad para ofrecer a los usuarios servicios gratuitos; los usuarios no deberían tener que pagar para usar la plataforma o beneficiarse de sus servicios. Una plataforma blockchain que sea de uso gratuita para los usuarios probablemente obtenga una adopción más generalizada. Los desarrolladores y las empresas pueden crear estrategias efectivas de monetización.

## Actualizaciones Sencillas y Recuperación de Errores

Las empresas que crean aplicaciones basadas en blockchain necesitan la flexibilidad para mejorar sus aplicaciones con nuevas funciones.

Todo el software no trivial está sujeto a errores, incluso con la verificación formal más rigurosa. La plataforma debe ser lo suficientemente robusta como para corregir errores cuando estos ocurran de forma inevitable.

## Baja latencia

Una buena experiencia de usuario exige reacciones confiables con un retraso de no más de unos pocos segundos. Demoras más largas frustran a los usuarios y hacen que las aplicaciones creadas en una blockchain sean menos competitivas con las alternativas existentes que no son blockchain.

## Rendimiento secuencial

Hay algunas aplicaciones que simplemente no se pueden implementar con algoritmos paralelos debido a pasos secuencialmente dependientes. Las aplicaciones como los exchanges necesitan un rendimiento secuencial suficiente para manejar grandes volúmenes y, por lo tanto, se requiere una plataforma con un rendimiento secuencial rápido.

## Rendimiento Paralelo

Las aplicaciones a gran escala necesitan dividir la carga de trabajo entre múltiples CPU y computadoras.

# Algoritmo de Consenso (DPOS)

El software EOS.IO utiliza el único algoritmo de consenso descentralizado capaz de cumplir los requisitos de rendimiento de las aplicaciones en el blockchain, [Prueba de Participación Delegada (DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper). Bajo este algoritmo, aquellos que tienen tokens en una cadena de bloques adoptando el software EOS.IO podrán seleccionar productores de bloque a través de un sistema de votación de aprobación continua y cualquiera puede elegir participar en la producción de bloques y se les dará la oportunidad de producir bloques proporcionalmente al total de votos que han recibido en relación con todos los demás productores. Para blockchains privados, la administración podría usar los tokens para agregar y eliminar personal de IT.

El software EOS.IO permite producir bloques exactamente cada 3 segundos y exactamente un productor está autorizado a producir un bloque en cualquier momento dado. Si el bloque no se produce a la hora programada, entonces el bloque para ese intervalo de tiempo se omite. Cuando se omiten uno o más bloques, hay un espacio de 6 o más segundos en el blockchain.

El uso de los bloques de software EOS.IO se produce en rondas de 21. Al comienzo de cada ronda se eligen 21 productores de bloques únicos. Los 20 mejores por aprobación total se eligen automáticamente en cada ronda y el último productor se elige proporcionalmente a su número de votos en relación con otros productores. Los productores seleccionados se mezclan usando un número pseudoaleatorio derivado del tiempo del bloque. Esta reorganización se realiza para garantizar que todos los productores mantengan una conectividad equilibrada con todos los demás productores.

Si un productor pierde un bloque y no ha producido ningún bloque en las últimas 24 horas, se los retira de consideración hasta que notifiquen al blockchain su intención de comenzar a producir bloques nuevamente. Esto garantiza que la red funcione sin problemas minimizando la cantidad de bloques perdidos al no programar aquellos que no han demostrado ser confiables.

En condiciones normales, una blockchain DPOS no experimenta bifurcaciones porque los productores de bloques cooperan para producir bloques en lugar de competir. En caso de que haya una bifurcación, el consenso cambiará automáticamente a la cadena más larga. Esta métrica funciona porque la velocidad a la que se agregan los bloques a una bifurcación de la blockchain se correlaciona directamente con el porcentaje de productores de bloques que comparten el mismo consenso. En otras palabras, una bifurcación blockchain con más productores crecerá en longitud más rápido que una con menos productores. Además, ningún productor de bloques debería producir bloques en dos bifurcaciones al mismo tiempo. Si un productor de bloques es atrapado haciendo esto, entonces ese productor de bloques probablemente será expulsado. La evidencia criptográfica de esta doble producción también puede usarse para eliminar automáticamente a los abusadores.

## Confirmación de Transacción

Los blockchains DPOS típicos tienen una participación del productor del 100% en bloques. Una transacción puede considerarse confirmada con 99.9% de certeza después de un promedio de 1.5 segundos desde el momento de la transmisión.

Hay algunos casos extraordinarios en los que una falla de software, congestión de Internet, o un productor de bloque malicioso creará dos o más bifurcaciones. Para tener la certeza absoluta de que una transacción es irreversible, un nodo puede optar por esperar la confirmación de 15 de los 21 productores de bloques. Según una configuración típica del software EOS.IO, esto tomará un promedio de 45 segundos en circunstancias normales. Por defecto, todos los nodos considerarán que un bloque confirmado por 15 de los 21 productores es irreversible y no cambiarán a una bifurcación que excluya dicho bloque, independientemente de su longitud.

Es posible que un nodo advierta a los usuarios que existe una alta probabilidad de que estén en una bifurcación minoritaria dentro de los 9 segundos posteriores al inicio de una bifurcación. Después de 2 bloques perdidos consecutivamente hay un 95% de probabilidad de que un nodo esté en una bifurcación minoritaria. Con 3 bloques perdidos consecutivamente hay una certeza del 99% de estar en una bifurcación minoritaria. Es posible generar un modelo predictivo robusto que utilizará información sobre qué nodos omitidos, tasas de participación recientes y otros factores para advertir rápidamente a los operadores de que algo anda mal.

La respuesta a tal advertencia depende por completo de la naturaleza de las transacciones comerciales, pero la respuesta más simple es esperar 15/21 confirmaciones hasta que se detenga la advertencia.

## Transacción como Prueba de Participación (TaPoS)

El software EOS.IO requiere que cada transacción incluya el hash de un encabezado de bloque reciente. Este hash tiene dos propósitos:

1. previene la repetición de una transacción en bifurcaciones que no incluyen el bloque referenciado; y
2. señala a la red que un usuario en particular y su participación están en una bifurcación específica.

Con el tiempo, todos los usuarios terminan confirmando directamente el blockchain, lo que dificulta forjar cadenas falsas, ya que la falsificación no podría migrar transacciones de la cadena legítima.

# Cuentas

El software EOS.IO permite que todas las cuentas estén referenciadas por un nombre único legible por humanos de 2 a 32 caracteres de longitud. El nombre es elegido por el creador de la cuenta. Todas las cuentas se deben financiar con el saldo mínimo de la cuenta en el momento en que se crean para cubrir el costo del almacenamiento de los datos de la cuenta. Los nombres de cuenta también admiten espacios de nombres, de modo que el propietario de la cuenta @domain es el único que puede crear la cuenta @user.domain.

En un contexto descentralizado, los desarrolladores de aplicaciones pagarán el costo nominal de la creación de la cuenta para registrar un nuevo usuario. Las empresas tradicionales ya gastan sumas importantes de dinero por los clientes que adquieren en forma de publicidad, servicios gratuitos, etc. El costo de financiación de una nueva cuenta de blockchain debería ser insignificante en comparación. Afortunadamente, no es necesario crear cuentas para usuarios que ya se hayan registrado en otra aplicación.

## Mensajes & Controladores

Cada cuenta puede enviar mensajes estructurados a otras cuentas y puede definir scripts para manejar los mensajes cuando se reciben. El software EOS.IO proporciona a cada cuenta su propia base de datos privada a la que solo pueden acceder sus propios controladores de mensajes. Los scripts de manejo de mensajes también pueden enviar mensajes a otras cuentas. La combinación de mensajes y controladores automáticos de mensajes es la forma en que EOS.IO define los contratos inteligentes.

## Gestión de Permisos Basada en Roles

La gestión de permisos implica determinar si un mensaje está o no debidamente autorizado. La forma más simple de administración de permisos es verificar que una transacción tenga las firmas requeridas, pero esto implica que las firmas requeridas ya sean conocidas. En general, la autoridad está ligada a individuos o grupos de individuos y, a menudo, está compartimentada. El software EOS.IO proporciona un sistema de gestión de permisos declarativos que proporciona cuentas de granularidad y control de alto nivel sobre quién puede hacer qué y cuándo.

Es fundamental que la autenticación y la gestión de permisos estén estandarizadas y separadas de la lógica comercial de la aplicación. Esto permite el desarrollo de herramientas para administrar permisos de una manera general y también brinda oportunidades significativas para la optimización del rendimiento.

Cada cuenta puede ser controlada mediante cualquier combinación ponderada de otras cuentas y claves privadas. Esto crea una estructura de autoridad jerárquica que refleja cómo se organizan los permisos en la realidad, y hace que el control multiusuario de los fondos sea más fácil que nunca. El control de múltiples usuarios es el principal factor que contribuye a la seguridad y, cuando es usado adecuadamente, puede eliminar en gran medida el riesgo de robo debido a la piratería.

El software EOS.IO permite a las cuentas definir qué combinación de claves y/o cuentas puede enviar un tipo de mensaje particular a otra cuenta. Por ejemplo, es posible tener una clave para la cuenta de redes sociales de un usuario y otra para acceder al exchange. Incluso es posible dar permiso a otras cuentas para actuar en nombre de la cuenta de un usuario sin asignarles claves.

### Nominación de Niveles de Permisos

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Con el software EOS.IO, las cuentas pueden definir niveles de permisos con nombre, cada uno de los cuales puede derivarse de permisos nombrados de mayor nivel. Cada nivel de permiso nombrado define una autoridad; una autoridad es un control de múltiples firmas de umbral que consiste en claves y/o niveles de permisos nombrados de otras cuentas. Por ejemplo, se puede configurar el nivel de permiso "Amigo" de una cuenta para que la cuenta sea controlada por igual por cualquiera de los amigos de la cuenta.

Otro ejemplo es el blockchain de Steem, que tiene nombres para los tres niveles de permiso de codificado duro: propietario, activo y publicación. El permiso de publicación solo puede realizar acciones sociales como votar y publicar, mientras que el permiso activo puede hacer todo excepto cambiar al propietario. El permiso del propietario está destinado al almacenamiento en frío y puede hacer todo. El software EOS.IO generaliza este concepto al permitir que cada titular de la cuenta defina su propia jerarquía, así como la agrupación de acciones.

### Grupos de Controladores de Mensajes Nominados

El software de EOS.IO permite que cada cuenta organice sus propios controladores de mensajes en grupos nominados y anidados. Estos grupos de controladores de mensajes nominados pueden ser referenciados por otras cuentas cuando configuran sus niveles de permisos.

El grupo de controladores de mensajes de nivel más alto es el nombre de la cuenta y el nivel más bajo es el tipo de mensaje individual que recibe la cuenta. Estos grupos pueden ser referenciados así:   **@accountname.groupa.subgroupb.MessageType**.

Bajo este modelo, es posible que un contrato de intercambio agrupe la creación de orden y la cancelación por separado del depósito y el retiro. Esta agrupación según el contrato de intercambio es una conveniencia para los usuarios del intercambio.

### Asignación de Permisos

El software de EOS.IO permite que cada cuenta defina una asignación entre un Grupo de Controladores de Mensajes Nominados de cualquier cuenta y su propio Nivel de Permiso Nominado. Por ejemplo, un titular de cuenta puede asignar la aplicación de medios sociales del titular de la cuenta al grupo de permisos "Amigo" del titular de la cuenta. Con esta asignación, cualquier amigo podría publicar como titular de la cuenta en las redes sociales del titular de la cuenta. Aunque publiquen como titular de la cuenta, aún usarían sus propias claves para firmar el mensaje. Esto significa que siempre será posible identificar qué amigos usaron la cuenta y de qué manera.

### Evaluación de Permisos

Al enviar un mensaje de tipo "**Acción**", de **@alice** a **@bob**, el software EOS.IO primero verificará si **@alice** ha definido una asignación de permisos para **@bob.groupa.subgroup.Action**. Si no se encuentra nada, se marcará una asignación para **@bob.groupa.subgroup**, luego **@bob.groupa**, y por último **@bob**. Si no es encontrada otra coincidencia, entonces la asignación supuesta será para el grupo de permisos nombrado **@alice.active**.

Una vez que se identifica una asignación, la autoridad de firma se valida utilizando el proceso de múltiples firmas de umbral y la autoridad asociada con el permiso nominado. Si eso falla, atraviesa el permiso principal y finalmente el permiso del propietario, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Grupos de Permisos Predeterminados

La tecnología EOS.IO también permite que todas las cuentas tengan un grupo "propietario" que puede hacer todo, y un grupo "activo" que puede hacer todo excepto cambiar el grupo propietario. Todos los demás grupos de permisos se derivan de "activo".

#### Evaluación Paralela de Permisos

El proceso de evaluación de permisos es de "solo lectura" y los cambios en los permisos realizados por las transacciones no surten efecto hasta el final de un bloque. Esto significa que todas las claves y la evaluación de permisos para todas las transacciones se pueden ejecutar en paralelo. Además, esto significa que es posible una validación rápida del permiso sin iniciar la costosa lógica de la aplicación que tendría que revertirse. Por último, significa que los permisos de transacción pueden evaluarse a medida que se reciben las transacciones pendientes y no es necesario volver a evaluarlas a medida que se aplican.

Considerando todo, la verificación de permisos representa un porcentaje significativo del cálculo requerido para validar las transacciones. Hacer esto un proceso de solo lectura y trivialmente paralelizable permite un aumento dramático en el rendimiento.

Al reproducir el blockchain para regenerar el estado determinístico del registro de mensajes, no hay necesidad de evaluar los permisos nuevamente. El hecho de que una transacción esté incluida en un buen bloque conocido es suficiente para omitir este paso. Esto reduce drásticamente la carga computacional asociada con la reproducción de una blockchain cada vez mayor.

## Mensajes con Retraso Obligatorio

El tiempo es un componente crítico en lo referente a la seguridad. En la mayoría de los casos, no es posible saber si una clave privada ha sido robada hasta que se haya utilizado. La seguridad basada en el tiempo es aún más crítica cuando las personas tienen aplicaciones que requieren que se mantengan claves en las computadoras conectadas a Internet para el uso diario. El software EOS.IO permite a los desarrolladores de aplicaciones indicar que ciertos mensajes deben esperar un período de tiempo mínimo después de ser incluidos en un bloque antes de poder aplicarse. Durante este tiempo pueden ser cancelados.

Los usuarios pueden recibir un aviso por correo electrónico o mensaje de texto cuando se transmite uno de estos mensajes. Si no lo autorizaron, pueden usar el proceso de recuperación de la cuenta para recuperar su cuenta y retraer el mensaje.

La demora requerida depende de qué tan sensible sea una operación. Pagar por un café no puede tener demoras y ser irreversible en segundos, mientras que comprar una casa puede requerir un período de limpieza de 72 horas. La transferencia de una cuenta completa a un nuevo control puede demorar hasta 30 días. Los retrasos exactos elegidos corresponden a desarrolladores de aplicaciones y usuarios.

## Recuperación de Llaves Robadas

El software EOS.IO proporciona a los usuarios una forma de restaurar el control de su cuenta cuando le son robadas las claves. El propietario de una cuenta puede usar cualquier clave de propietario que estuvo activa en los últimos 30 días junto con la aprobación de su asociado de recuperación de cuenta designado para restablecer la clave de propietario en su cuenta. El socio de recuperación de cuenta no puede restablecer el control de la cuenta sin la ayuda del propietario.

El hacker no tiene nada que ganar al intentar pasar por el proceso de recuperación porque ya "controlan" la cuenta. Además, si pasaran por el proceso, el socio de recuperación probablemente exigiría identificación y autenticación de múltiples factores (teléfono y correo electrónico). Esto probablemente comprometa al hacker o no gane nada en el proceso.

Este proceso también es muy diferente de un arreglo simple de múltiples firmas. Con una transacción con varias firmas, hay otra empresa que participa en cada transacción que se ejecuta, pero con el proceso de recuperación, el agente es solo parte en el proceso de recuperación y no tiene poder sobre las transacciones diarias. Esto reduce drásticamente los costos y las responsabilidades legales para todos los involucrados.

# Ejecución Paralela Determinista de Aplicaciones

El consenso de la blockchain depende del comportamiento determinista (reproducible). Esto significa que toda ejecución paralela debe estar libre del uso de exclusiones mutuas u otras formas primitivas de bloqueo. Sin bloqueos debe haber alguna forma de garantizar que todas las cuentas solo puedan leer y escribir su propia base de datos privada. También significa que cada cuenta procesa los mensajes secuencialmente y que el paralelismo estará en el nivel de la cuenta.

En un blockchain basado en software EOS.IO, el trabajo del productor de bloque consiste en organizar la entrega de mensajes en hilos independientes para que puedan evaluarse en paralelo. El estado de cada cuenta depende solo de los mensajes que se le envían. El cronograma es el resultado de un productor de bloques y se ejecutará de manera determinista, pero el proceso para generar el cronograma no necesita ser determinista. Esto significa que los productores de bloques pueden utilizar algoritmos paralelos para programar transacciones.

Parte de la ejecución paralela significa que cuando una secuencia de comandos genera un nuevo mensaje, no se entrega inmediatamente, sino que se programa para entregarse en el siguiente ciclo. La razón por la que no se puede entregar inmediatamente es porque el receptor puede estar modificando activamente su propio estado en otro hilo.

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