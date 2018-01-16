# EOS.IO Documento Técnico

**26 de Junio de 2017**

**Resumen:** El software EOS.IO introduce una nueva arquitectura Blockchain diseñada para facilitar la escalabilidad vertical y horizontal de aplicaciones descentralizadas. Esto es logrado creando una construcción similar a un sistema operativo sobre la cual se pueden construir aplicaciones. El software proporciona cuentas, autenticación, bases de datos, comunicación asíncrona y la ejecución programada de aplicaciones en cientos de núcleos CPU o clústeres. La tecnología resultante es una arquitectura blockchain que se adapta a millones de transacciones por segundo, elimina las tarifas de los usuarios y permite el despliegue rápido y sencillo de aplicaciones descentralizadas.

**ATENCIÓN: TENGA EN CUENTA QUE LOS TOKENS CRIPTOGRÁFICOS MENCIONADOS EN ESTE DOCUMENTO SE REFIEREN A LOS TOKENS CRIPTOGRÁFICOS PROPIOS DE UNA BLOCKCHAIN QUE HA SIDO ADOPTADA POR EL SOFTWARE EOS.IO. NO SE REFIEREN A LOS TOKENS COMPATIBLES ERC-20 DISTRIBUIDOS EN LA BLOCKCHAIN DE ETHEREUM EN CONEXIÓN CON LA DISTRIBUCIÓN DE TOKEN EOS.**

Copyright © 2017 block.one

Sin permiso, cualquier persona puede usar, reproducir o distribuir cualquier material en este documento técnico para uso no comercial y educativo (es decir, que no sea por una tarifa o con fines comerciales) siempre que se cite la fuente original y el aviso de copyright correspondiente.

**DESCARGO DE RESPONSABILIDAD:** Este documento técnico de EOS.IO es solo para fines informativos. block.one no garantiza la exactitud o las conclusiones alcanzadas en este documento, y este documento se proporciona "tal cual". block.one no realiza y renuncia expresamente a todas las representaciones y garantías, expresas, implícitas, legales o de otro tipo, incluidas, entre otras, las siguientes: (i) garantías de comerciabilidad, idoneidad para un propósito particular, conveniencia, uso, título o no infracción; (ii) que el contenido de este documento no contiene errores; y (iii) que dichos contenidos no infrinjan los derechos de terceros. block.one y sus afiliados no tendrán responsabilidad por daños y perjuicios de cualquier tipo que surjan del uso, referencia o confianza en este documento o en cualquier contenido aquí incluido, incluso si es informada la posibilidad de dichos daños. En ningún caso block.one o sus afiliados serán responsables ante cualquier persona o entidad por los daños, pérdidas, responsabilidades, costos o gastos de cualquier tipo, ya sean directos o indirectos, consecuentes, compensatorios, incidentales, reales, ejemplares, punitivos o especiales. por el uso, la referencia o la confianza en este documento o cualquier contenido aquí incluido, que incluye, entre otros, cualquier pérdida de negocios, ingresos, ganancias, datos, uso, buena voluntad u otras pérdidas intangibles.

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
  - [Mensajes & Manejadores](#messages--handlers)
  - [Gestión de Permisos Basada en Roles](#role-based-permission-management) 
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

## Minimizando Latencia de Comunicación

La latencia es el tiempo que le toma a una cuenta para enviar un mensaje a otra cuenta y luego recibir una respuesta. El objetivo es habilitar dos cuentas para poder intercambiar mensajes dentro y fuera de un bloque sin tener que esperar 3 segundos entre cada mensaje. Para habilitar esto, el software EOS.IO dividirá cada bloque en ciclos. Cada ciclo se divide en hilos y cada hilo contiene a su vez una lista de transacciones. Cada transacción contiene un conjunto de mensajes para que sean entregados. Esta estructura se puede visualizar como un árbol donde las capas alternas serán procesadas secuencialmente y en paralelo.

        Bloque
    
          Ciclos (secuenciales)
    
            Hilos (paralelo)
    
              Transacciones (secuenciales)
    
                Mensajes (secuenciales)
    
                  Receptor y Cuentas Notificadas (paralelo)
    

Las transacciones que son generadas en un ciclo se pueden entregar en cualquier ciclo o bloque subsiguiente. Los productores de bloques seguirán agregando ciclos a un bloque hasta que haya transcurrido el tiempo máximo del reloj o no existan nuevas transacciones generadas para entregar.

Es posible utilizar el análisis estático de un bloque para verificar si dentro de un ciclo dado no hay dos subprocesos que contengan transacciones que modifiquen la misma cuenta. Siempre que se mantenga este invariante, se puede procesar un bloque ejecutando todos los hilos en paralelo.

## Controladores de Mensajes para Sólo Lectura

Algunas cuentas pueden procesar un mensaje con aprobación o sin aprobación sin ver modificado su estado interno. De ser este es el caso, estos controladores se pueden ejecutar en paralelo, siempre que solo los controladores de mensajes de solo lectura para una cuenta en particular estén incluidos en uno o más subprocesos dentro de un ciclo en particular.

## Transacciones Atómicas con Múltiples Cuentas

En algunas oportunidades es deseable asegurar que los mensajes sean entregados y aceptados por múltiples cuentas atómicamente. En estos casos, ambos mensajes se colocan en una transacción y a ambas se les asigna el mismo subproceso y los mensajes se aplicarán secuencialmente. Esta situación no es ideal para el rendimiento y, cuando se trata de "facturar" a los usuarios por el uso, se facturará por el número de cuentas únicas a las que se haga referencia en una transacción.

Por razones de rendimiento y costo, es mejor minimizar las operaciones atómicas que involucren dos o más cuentas muy utilizadas.

## Evaluación Parcial del Estado de la Blockchain

La escalada de la tecnología blockchain requiere que los componentes sean modulares. Todos no deberían tener que ejecutar todo, especialmente si solo necesitan usar un pequeño subconjunto de las aplicaciones.

Un desarrollador de aplicaciones de exchange ejecuta nodos completos con el fin de mostrar el estado del exchange a sus usuarios. Esta aplicación del exchange no necesita el estado asociado con las aplicaciones de redes sociales. El software EOS.IO permite que cualquier nodo completo seleccione cualquier subconjunto de aplicaciones para que sea ejecutado. Los mensajes entregados a otras aplicaciones se ignoran de forma segura porque el estado de una aplicación se deriva completamente de los mensajes que se le envían.

Esto tiene algunas implicaciones significativas en la comunicación con otras cuentas. Lo más significativo es que no se puede suponer que el estado de la otra cuenta esté accesible en la misma máquina. Esto también significa que si bien es tentador habilitar "bloqueos" que permitan a una cuenta llamar sincrónicamente a otra cuenta, este patrón de diseño se rompe si la otra cuenta no reside en la memoria.

Toda la comunicación del estado entre las cuentas se debe pasar a través de mensajes incluidos en el blockchain.

## Mejor Esfuerzo Subjetivo de Programación

El software EOS.IO no puede obligar a los productores de bloques a enviar ningún mensaje a ninguna otra cuenta. Cada productor de bloques hace su propia medición subjetiva de la complejidad computacional y el tiempo requerido para procesar una transacción en particular. Esto aplica ya sea que una transacción sea generada por un usuario o automáticamente por una secuencia de comandos.

En un blockchain lanzado que adopta el software EOS.IO, a nivel de red se facturan todas las transacciones en un costo de ancho de banda computacional fijo independientemente de si se necesitaron .01ms o 10 ms completos para ejecutarlo. Sin embargo, cada productor individual de bloque que usa el software puede calcular el uso de recursos usando su propio algoritmo y medidas. Cuando un productor de bloques concluye que una transacción o cuenta ha consumido una cantidad desproporcionada de capacidad computacional, simplemente rechaza la transacción cuando producen su propio bloque; sin embargo, aún procesarán la transacción si otros productores de bloques lo consideran válido.

En general, siempre que 1 productor de bloques considere que una transacción es válida y está por debajo de los límites de uso de recursos, todos los demás productores de bloques también la aceptarán, pero la transacción puede demorar hasta 1 minuto en encontrar a ese productor.

En algunos casos, un productor puede crear un bloque que incluya transacciones que son de un orden de magnitud fuera de los rangos aceptables. En este caso, el próximo productor de bloques puede optar por rechazar el bloque y el tercer productor romperá el empate. Esto no es diferente a lo que sucedería si un bloque grande causara retrasos en la propagación de la red. La comunidad notaría un patrón de abuso y eventualmente eliminaría los votos asignados al productor deshonesto.

Esta evaluación subjetiva del costo computacional libera la blockchain de tener que medir de forma precisa y determinista cuánto tarda algo en ejecutarse. Con este diseño, no es necesario contar las instrucciones con precisión, lo que aumenta drásticamente las oportunidades de optimización sin romper el consenso.

# Modelo de Tokens (fichas) y Uso de Recursos

**TOME EN CUENTA: LOS TOKENS CRIPTOGRÁFICOS MENCIONADOS EN ESTE DOCUMENTO SE REFIEREN A LOS TOKENS CRIPTOGRÁFICoS EN UNA BLOCKCHAIN LANZADA QUE ADOPTA EL SOFTWARE EOS.IO. NO SE REFIEREN A LOS TOKENS COMPATIBLES ERC-20 DISTRIBUIDOS EN EL BLOCKCHIAN DE ETHEREUM EN RELACIÓN CON LA DISTRIBUCIÓN DE LOS TOKEN EOS.**

Todas las blockchains tienen recursos limitados y requieren un sistema para evitar el abuso. Con un blockchain que usa el software EOS.IO, hay tres clases amplias de recursos consumidos por las aplicaciones:

1. Ancho de banda y almacenamiento de registro (Disco);
2. Computación y Acumulación Computacional (CPU); y
3. Estado de Almacenamiento (RAM).

El ancho de banda y la computación tienen dos componentes, el uso instantáneo y el uso a largo plazo. Un blockchain mantiene un registro de todos los mensajes y este registro finalmente se almacena y descarga por todos los nodos completos. Con el registro de mensajes es posible reconstruir el estado de todas las aplicaciones.

La deuda computacional son cálculos que se deben realizar para regenerar el estado del registro de mensajes. Si la deuda computacional crece demasiado, será necesario tomar instantáneas del estado de la blockchain y descartar la historia de la blockchain. Si la deuda computacional crece demasiado rápido, puede llevar 6 meses repetir las transacciones por un año. Esto es crítico, por lo tanto, la deuda computacional debe ser cuidadosamente administrada.

El almacenamiento del estado del Blockchain es una información accesible desde la lógica de la aplicación. Incluye información como libros de pedidos y saldos de cuenta. Si el estado nunca es leído por la aplicación, entonces no debe ser almacenado. Por ejemplo, el contenido y los comentarios de las publicaciones del blog no son leídos por la lógica de la aplicación, por lo que no deberían almacenarse en el estado de la blockchain. Mientras tanto, la existencia de una publicación o comentario, el número de votos y otras propiedades se almacenan como parte del estado de la blockchain.

Los productores de bloques publican su capacidad disponible para ancho de banda, computación y estado. El software EOS.IO permite que cada cuenta consuma un porcentaje de la capacidad disponible proporcional a la cantidad de tokens (fichas) en un contrato de participación de 3 días. Por ejemplo, si se lanza un blockchain basada en el software EOS.IO y si una cuenta tiene el 1% del tokens total distribuible según esa blockchain, entonces esa cuenta tiene el potencial de utilizar el 1% del estado de la capacidad de almacenamiento.

La adopción del software EOS.IO en un blockchain lanzado significa que el ancho de banda y la capacidad computacional se asignan en forma de reserva fraccionaria porque son transitorios (la capacidad no utilizada no se puede guardar para uso futuro). El algoritmo utilizado por el software EOS.IO es similar al algoritmo utilizado por Steem para limitar el uso del ancho de banda.

## Mediciones Objetivas y Subjetivas

Como se discutió anteriormente, instrumentar el uso computacional tiene un impacto significativo en el rendimiento y la optimización; por lo tanto, todas las restricciones de uso de recursos serán, en última instancia, subjetivas y la aplicación la realizan los productores de bloques de acuerdo con sus algoritmos y estimaciones individuales.

Dicho esto, hay ciertas cosas que son triviales para medir objetivamente. La cantidad de mensajes entregados y el tamaño de los datos almacenados en la base de datos interna son baratos de medir objetivamente. El software EOS.IO permite a los productores de bloques aplicar el mismo algoritmo sobre las medidas objetivas, pero puede optar por aplicar algoritmos subjetivos más estrictos sobre las medidas subjetivas.

## El Receptor Paga

Tradicionalmente, es el negocio el que paga por el espacio de oficina, el poder computacional y otros costos necesarios para administrar el negocio. El cliente compra productos específicos del negocio y los ingresos de esas ventas de productos se utilizan para cubrir los costos comerciales de operación. Del mismo modo, ningún sitio web obliga a sus visitantes a hacer micropagos por visitar su sitio web para cubrir los costos de alojamiento. Por lo tanto, las aplicaciones descentralizadas no deberían obligar a sus clientes a pagar la blockchain directamente por el uso de la blockchain.

Un blockchain lanzado que utilice el software EOS.IO no requiere que sus usuarios paguen el blockchain directamente para su uso y, por lo tanto, no restringe ni impide que un negocio determine su propia estrategia de monetización para sus productos.

## Delegando Capacidad

Un titular de tokens en un blockchain lanzado que adopte el software EOS.IO que puede no tener una necesidad inmediata de consumir todo o parte del ancho de banda disponible, puede dar o alquilar dicho ancho de banda no consumido a otros; los productores de bloques que ejecutan el software EOS.IO en dicha blockchain reconocerán esta delegación de capacidad y asignarán el ancho de banda en consecuencia.

## Separación de los Costos de Transacción del Valor del Token

Uno de los principales beneficios del software EOS.IO es que la cantidad de ancho de banda disponible para una aplicación es totalmente independiente de cualquier precio simbólico. Si el propietario de una aplicación posee un número relevante de tokens en una blockchain que adopta el software EOS.IO, entonces la aplicación puede ejecutarse indefinidamente dentro de un estado fijo y uso de ancho de banda. En tal caso, los desarrolladores y usuarios no se ven afectados por la volatilidad de los precios en el mercado de tokens y, por lo tanto, no dependen de un precio de alimentación. En otras palabras, un blockchain que adopta el software EOS.IO permite a los productores de bloques aumentar de forma natural el ancho de banda, el cálculo y el almacenamiento disponibles por token independientemente del valor del token.

Una blockchain que usa el software EOS.IO también otorga tokens a los productores de bloques cada vez que producen un bloque. El valor de los tokens tendrá un impacto en la cantidad de ancho de banda, almacenamiento y computación que un productor puede permitirse comprar; este modelo aprovechará de forma natural los valores de token ascendentes para aumentar el rendimiento de la red.

## Costos de Almacenamiento del Estado

Si bien se puede delegar el ancho de banda y el cálculo, el almacenamiento del estado de la aplicación requerirá que el desarrollador de la aplicación tenga tokens hasta que se elimine ese estado. Si el estado nunca se elimina, los tokens se eliminan efectivamente de la circulación.

Cada cuenta de usuario requiere una cierta cantidad de almacenamiento; por lo tanto, cada cuenta debe mantener un saldo mínimo. A medida que la capacidad de almacenamiento de la red aumenta, este mínimo requerido se reducirá.

## Recompensas de Bloque

Una blockchain que adopte el software EOS.IO otorgará nuevos tokens a un productor de bloques cada vez que se produzca un bloque. En estas circunstancias, el número de tokens creados está determinado por la media del salario deseado publicado por todos los productores de bloques. El software EOS.IO puede configurarse para imponer un tope a los premios al productor de manera que el aumento anual total en el suministro del token no exceda el 5%.

## Aplicaciones de Beneficios Comunitarios

Además de elegir a los productores de bloques, de acuerdo con un blockchain basado en el software EOS.IO, los usuarios pueden elegir 3 aplicaciones de beneficio comunitario también conocidas como contratos inteligentes. Estas 3 aplicaciones recibirán tokens de un porcentaje configurado anualmente del suministro de tokens menos los tokens que se han pagado para bloquear a los productores. Estos contratos inteligentes recibirán tokens proporcionales a los votos que cada aplicación ha recibido de los titulares de tokens. Las aplicaciones elegidas o los contratos inteligentes pueden ser reemplazados por aplicaciones recién elegidas o contratos inteligentes por titulares de tokens.

# Gobernanza

La gobernanza es el proceso mediante el cual las personas llegan a un consenso sobre asuntos subjetivos que no pueden ser captados completamente por algoritmos de software. Un blockchain basado en software EOS.IO implementa un proceso de gobierno que dirige eficientemente la influencia existente de los productores de bloques. En ausencia de un proceso de gobernanza definido, las blockchain anteriores dependían de procesos de gobierno ad hoc, informales y, a menudo, muy controvertidos, que generaban resultados impredecibles.

Un blockchain basada en el software EOS.IO reconoce que la potencia se origina con los titulares de tokens que delegan esa potencia a los productores de bloques. A los productores de bloques se les concede una autoridad limitada y verificada para congelar cuentas, actualizar aplicaciones defectuosas y proponer cambios difíciles para el protocolo subyacente.

Incrustar en el software EOS.IO es la elección de los productores de bloques. Antes de que se pueda realizar cualquier cambio en el blockchain, estos productores de bloque deben aprobarlo. Si los productores de bloque se niegan a realizar los cambios deseados por los poseedores de las fichas (tokens) entonces ellos pueden ser retirados mediante una votación. Si los productores de bloques hacen cambios sin el permiso de los poseedores de las fichas(tokens) entonces todos los otros validadores de nodos completos no productores(servicios de intercambio de criptomonedas, etc) rechazarán el cambio.

## Congelación de Cuentas

A veces un contrato inteligente se comporta en una forma aberrante o impredecible y no funciona como se pretende; otras veces una aplicación o cuenta puede descubrir un exploit que le permite consumir una cantidad irrazonable de recursos. Cuando esos problemas inevitablemente ocurren, los productores de bloques tienen el poder para rectificar esas situaciones.

Los productores de bloques en todas las Blockhains tienen el poder de seleccionar cuales transacciones son incluidas en bloques lo que les da la capacidad de congelar cuentas. Una Blockchain usando el software EOS.IO formaliza esta autoridad sometiendo el proceso de congelación de una cuenta a un voto de 17/21 de los productores activos. Si los productores abusan de su poder ellos pueden ser retirados a través del voto y la cuenta será descongelada.

## Cambiar el Código de Cuenta

Cuando todo lo demás falla y una "aplicación indetenible" actúa de una forma impredecible, una Blockchain usando el software de IOS.IO permite a los productores reemplazar el código de la cuenta sin tener que actualizar la Blockchain entera. Parecido al proceso de congelar las cuentas, este reemplazo del código requiere de una votación 17/21 de los productores de bloques elegidos.

## Constitución

El software EOS.IO permite a las Blockchains establecer términos de servicio P2P (usuario-usuario) o un contrato vinculando entre aquellos que lo firme, referido como una "constitución". El contenido de esta constitución define las obligaciones entre los usuarios que no pueden aplicarse enteramente por código y facilita la resolución de conflictos al establecer una jurisdicción y ley junto con otras reglas mutualmente aceptadas. Cada transacción emitida en la red debe incorporar el hash de la constitución como parte de la firma y, por lo tanto, vincula explícitamente al firmante con el contrato.

La constitución también define la intención de que el protocolo de código fuente sea leído por humanos. Esta intención es usada para identificar la diferencia entre un error y una función cuando se producen errores y guía a la comunidad en que arreglos son los adecuados o los inadecuados.

## Actualizando el Protocolo & la Constitución

El software EOS.IO define un proceso por el cual el protocolo definido por el código fuente canónico y su constitución pueden ser actualizados utilizando el siguiente proceso:

1. Los productores de bloques proponen un cambio a la constitución y obtiene aprobación 17/21.
2. Los productores de bloques mantienen la aprobación 17/21 por 30 días consecutivos.
3. Todos los usuarios están obligados a firmar las transacciones utilizando el hash de la nueva constitución.
4. Los productores de bloques adoptan los cambios en el código fuente para reflejar el cambio en la constitución y lo proponen a la blockchain usando el hash de un compromiso git.
5. Los productores de bloques mantienen la aprobación 17/21 por 30 días consecutivos.
6. Los cambios en el código entran en vigencia 7 días después, dando a todos los nodos completos 1 semana para actualizar después de la ratificación del código fuente.
7. Todos los nodos que no se actualicen al nuevo código se apagarán automáticamente.

Por una configuración predeterminada del software EOS.IO, el proceso de actualización de la blockchain para agregar nuevas características lleva de 2 a 3 meses, mientras que las actualizaciones para corregir errores no críticos que no requieren cambios en la constitución pueden tomar de 1 a 2 meses.

### Cambios de Emergencia

Los productores de bloques pueden acelerar el proceso si se requiere un cambio de software para solucionar un error dañino o un ataque de seguridad que pueda estar dañando activamente a los usuarios. Generalmente hablando podría estar en contra de la constitución realizar actualizaciones aceleradas para introducir nuevas funciones o arreglar errores inofensivos.

# Máquinas Virtuales & Scripts

El software EOS.IO será ante todo una plataforma para coordinar la entrega de mensajes autenticados a cuentas. Los detalles del lenguaje de scripts y la máquina virtual son implementaciones con detalles específicos en su mayoría independientes del diseño de la tecnología EOS.IO. Cualquier lenguaje o máquina virtual que sea determinista y propiamente aislada con un rendimiento suficiente puede ser integrada al API del software EOS.IO.

## Mensajes Definidos por Esquema

Todos los mensajes enviados entre cuentas son definidos por un esquema que es parte del estado de consenso de la Blockchain. Este esquema permite la perfecta conversión entre binario y representación JSON de los mensajes.

## Base de Datos definida por esquema

El estado de la base de datos es también definido utilizando un esquema similar. Esto asegura que todos los datos almacenados por todas las aplicaciones estén en un formato que pueda ser interpretado como de alto nivel(leído por humanos) de tipo JSON pero almacenado y manipulado con la eficiencia del binario.

## Separando Autenticación de Aplicación

Para maximizar las oportunidades de paralelización y minimizar la deuda computacional asociada con la regeneración del estado de la aplicación del registro de transacciones, el software EOS.IO separa la validación lógica en tres secciones:

1. Validar que un mensaje es internamente consistente;
2. Validad que todas las precondiciones son válidas; y
3. Modificar el estado de la aplicación.

La validación de la consistencia interna de un mensaje es solamente leíble y requiere que no se tenga acceso al estado de la Blockchain. Esto significa que puede realizarse con paralelismo maximo. La validación de las precondiciones, como los fondos requeridos, es solamente leíble y, por lo tanto también se puede beneficiar del paralelismo. Solo modificaciones al estado de la aplicación requieren acceso de escritura y deben ser procesados secuencialmente por cada aplicación.

Autenticación es el proceso de solo lectura de verificar que un mensaje pueda ser aplicado. La aplicación esté realmente haciendo el trabajo. Ambos cálculos requieren ser realizados en tiempo real, sin embargo una vez una transacción es incluida en la Blockchain ya no es necesario realizar las operaciones de autenticación.

## Arquitectura Independiente de la Máquina Virtual

Es la intención de la blockchain basada en el software EOS.IO que múltiples máquinas virtuales puedan ser sostenidas y nuevas máquinas virtuales añadidas con el tiempo que sea necesario. Por esta razón este trabajo no discutirá los detalles de un lenguaje particular o máquina virtual. Dicho esto, hay dos máquinas virtuales que actualmente están siendo evaluadas para su uso con las blockchains basadas en el software EOS.IO.

### Web Assembly (WASM)

¨Web Assembly" es un estándar web emergente para la construcción de aplicaciones web de alto rendimiento. Con algunas pequeñas modificaciones Web Assembly puede volverse determista y aislado. El beneficio de Web Assembly es el apoyo generalizado de la industria y que permite que contratos sean desarrollados en lenguajes familiares como C o C++.

Desarrolladores de Ethereum ya han comenzado a modificar Web Assembly para proveer un ambiente aislado y determinista con su [Web Assembly (WASM) Ethereum](https://github.com/ewasm/design). Este enfoque puede ser fácilmente adoptado e integrado con el software EOS.IO.

### Máquina Virtual de Ethereum (MVE)

Esta máquina virtual ha sido usada por la mayoría de los contratos inteligentes existentes y pueden ser adaptadas para trabajar dentro de una blockchain EOS.IO. Es concebible que los contratos MVE puedan correr dentro de su propio ambiente aislado en una Blockchain basada en el software EOS.IO y que con algunas adaptaiones los contratos MVE puedan comunicarse con otras aplicaciones de la blockchain basada en el software EOS.IO.

# Comunicación Inter Blockchain

El software EOS.IO está diseñada para facilitar la comunicación Inter-Blockchain. Esto se logra haciendo que sea sencillo generar la prueba de la existencia del mensaje y prueba de la secuencia del mensaje. Estas pruebas combinadas con una arquitectura de aplicación diseñada alrededor de envíos de mensajes permite que los detalles de intercomunicación entre las Blockchain y pruebas de validación estén escondidos de los desarrolladores de la aplicación.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

## Pruebas Merkle para Validación de Cliente Ligero (VCL)

Integrarse con otras blockchains es mucho más fácil si los clientes no necesitan procesar todas las transacciones. Después de todo, un servicio de intercambio solo le interesan las transferencias que entran y salen de sus mercados y nada más. También seria ideal si la blockchain del servicio de intercambio podría utilizar pruebas ligeras de merkle de depósito en lugar de tener que confiar en sus propios productores de bloques totalmente. Al menos los productores de bloque de una blockchain les gustaría mantener la menor sobrecargposible al sincronizarse con otra blockchain.

El objetivo de VCL es habilitar la generación de pruebas de existencia relativamente ligeras que puedan ser validadas por cualquiera que siga un conjunto de datos relativamente ligero. En este caso el objetivo es probar que una transacción fue incluida en un bloque particular y que ese bloque es incluido en la historia verificada de una Blockchain en particular.

Bitcoin sostiene la validación de transacciones asumiendo que todos los nodos tengan acceso a la historia completa de los principales bloques lo cual asciende a 4mb de bloques principales por año. A 10 transacciones por segundo, una prueba válida requiere alrededor de 512 bytes. Esto funciona bien para una Blockchain con un intervalo de bloques de 10 minutos, pero no sería "ligero" para unas Blockchains con intervalos de bloques de 3 segundos.

El software de EOS.IO permite pruebas ligeras para cualquiera que tenga un bloque principl irreversible luego del punto en el cual la transacción fue incluida. Usando la estructura hash-ligado mostrada a continuación es posible probar la existencia de cualquier transacción con una prueba de menos de 1024 bytes en tamaño. Si se asume que los nodos de validación se mantienen con los bloques principales en el día pasado (2mb de data), entonces probar estas transacciones solo requiere pruebas de 200 bytes de longitud.

Hay pocos incrementos en los gastos asociados con la producción de bloques con el enlace de hashes adecuados para permitir estas pruebas lo que significa que no hay razón para no generar bloques de esta manera.

Cuando llega el momento de validar pruebas en otras blockchains hay una gran variedad de optimizaciones de ancho de banda/tiempo/espacio que pueden hacerse. Rastrear todos los bloques principales (420mb/año) mantendrá el tamaño de las pruebas pequeño. Rastrear solo los últimos bloques principales ofrece una ventaja entre almacenamiento a largo plazo y tamaño de la prueba. Alternativamente, una blockchain puede utilizar un enfoque de evaluación perezosa donde recuerda hashes intermedias de pruebas anteriores. Nuevas pruebas solo tienen que incluir enlaces al árbol disperso conocido. El enfoque exacto usado necesariamente dependerá del porcentaje de bloques extranjeros que incluyan la transacción referenciada en la prueba merkle.

Después de una cierta densidad de interconexión se vuelve más eficiente simplemente hacer una sola blockchain que contenga el historial entero del bloque de otra blockchain y así eliminar la necesidad de hacer pruebas. Por motivos de rendimiento, es ideal para reducir al mínimo frecuencia de las pruebas inter-blockchains.

## Latencia de la Comunicación Interblockchain

Al comunicarse con otra blockchain externa, los productores de bloques deben esperar hasta que haya certeza al 100% que una transacción ha sido confirmada irreversiblemente por la otra blockchain antes de aceptarla como una entrada válida. Usando una blockchain basada en el software de EOS.IO y Pruebas Delegadas de Participación con bloques de 3 segundos y 21 productores, esto tomaria aproximadamente 45 segundos. Si los productores de una cadena de bloques no esperan por la irreversibilidad sería como si un servicio de intercambio aceptando un depósito que luego fue devuelto y podría afectar la validez de consenso de la blockchain.

## Prueba de Integridad

Cuando se utiliza pruebas de merkle de blockchains externa, hay una diferencia significativa entre conocer que todas las transacciones procesadas son válidas y que ninguna ha sido saltada u omitida. Si bien es imposible provar que todas las transacciones más recientes son conocidas, es posible para probar que no ha habido ninguna brecha en el historial de transacciones. El software EOS.IO facilita esto al asingnar una secuencia de números a cada mensaje entregado a cada cuenta. Un usuario puede usar esta secuencia de números para probar que todos los mensajes dirigidos a una cuenta en particular han sido procesados y que fueron procesados en orden.

# Conclusión

El software EOS.IO es diseñado desde la experiencia con conceptos probados y las mejores prácticas, y representa avances fundamentales en la tecnología blockchain. El software es parte de un plan integral para una sociedad basada en la blockchain globalmente escalable en la que las aplicaciones descentralizadas puedan fácilmente ser desplegadas y gobernadas.