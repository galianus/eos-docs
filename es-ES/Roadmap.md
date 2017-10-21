## Hoja de ruta de EOS.IO

Este documento describe el plan de desarrollo a alto nivel y se irá actualizando a medida que se vaya avanzando hacia la versión 1.0. Cabe señalar que esta hoja de ruta se aplica solo al componente Blockchain del software y no a las otras herramientas y utilidades, como wallets y exploradores de bloques, que tendrán sus propios equipos de desarrollo y hojas de ruta dedicados, una vez que se complete la Fase 1.

***Todo lo que se incluye en este documento está en forma de borrador, está sujeto a cambios en cualquier momento y se proporciona únicamente con fines informativos. Block.one no garantiza la exactitud de la información contenida en esta hoja de ruta y la información se proporciona "tal cual" sin representaciones ni garantías, expresas o implícitas.***

# Fase 1 - Entorno de Pruebas Mínimo Viable - Verano del 2017

El objetivo de esta fase es establecer las API que los desarrolladores necesitarán para comenzar a construir y probar aplicaciones en EOS.IO. Para que los desarrolladores comiencen a probar sus aplicaciones requerirán que se implemente lo siguiente:

### Nodo autónomo (Dan & Nathan)

Un nodo autónomo opera una cadena de bloques de prueba y produce bloques exponiendo una API. Este nodo no necesita utilizar código de red P2P.

### Contratos nativos (Nathan)

El software EOS.IO cuenta con varios contratos nativos. Estos son contratos que administran las operaciones centrales de la blockchain y son ajenos a la interfaz Web Assembly. Estos contratos incluyen:

1. @eos: gestiona las transferencias de tokens de EOS
2. @stake - gestiona el Bloqueo de los Tokens de EOS, las Votaciones y la Elección del Productor
3. @system: gestiona permisos, mensajes y las actualizaciones de los códigos de contacto

### API de la Máquina Virtual (Dan)

Los contratos compilados son convertidos a WebAssembly (WASM) y el código WASM interactúa con la blockchain a través de una API definida. Este API, es del que los desarrolladores dependen para crear aplicaciones y será relativamente estable antes de que los desarrolladores realmente puedan comenzar a construir en EOS.

### Interface RPC (Arhag, Nathan)

Se proporcionará una interfaz JSON RPC simple sobre HTTP que permitirá a los desarrolladores difundir transacciones y consultar los estados de las aplicaciones. Esto es crítico tanto para publicar como para interactuar con aplicaciones de prueba.

### Herramientas de línea de comando (Arhag)

Las herramientas de línea de comando facilitan la integración de la interfaz RPC con los entornos de construcción de los desarrolladores.

### Documentación básica del Desarrollador (Josh)

Documents that teach developers how to get started with building on EOS.IO blockchains. This includes documentations of the WASM API, RPC Interface, and Command Line Tools.

# Phase 2 - Minimal Viable Test Network - Fall 2017

Everything in Phase 1 assumes a trusted environment that only runs the developer's own code. Before a test network can be deployed several additional features need to be implemented and tested.

### P2P Network Code (Phil)

This is a plugin that is responsible for synchronizing the blockchain state between two standalone nodes.

### WASM Sanitation & CPU Sandboxing (Brian)

The WASM code needs to be sanitized to check for non-deterministic behavior such as floating point operations and infinite loops.

### Resource Usage Tracking & Rate Limiting (Arhag)

To prevent abuse the resource monitoring and usage tracking rate limits users according to staked EOS.

### Genesis Import Testing (DappHub)

Tools need to be developed to export data from the EOS Token Distribution state and create a genesis configuration file. This will enable anyone participating in the Token Distribution to acquire some initial test EOS (TEOS).

### Interblockchain Communication (Nathan)

This feature involves verifying the Merkle hashing of transactions is proper.

# Phase 3 - Testing & Security Audits - Winter 2017, Spring 2018

During this phase the platform will undergo heavy testing with a focus on finding security issues and bug. At the end of Phase 3 version 1.0 will be tagged.

### Develop Example Applications

Example applications are critical to proving the platform provides the features required by real developers.

### Bounties for Successfully Attacking Network

Attacking the network with spam, virtual machine exploits, and bug crashes, and non-deterministic behavior will be a heavily involved process but necessary to ensure that version 1.0 is stable.

### Language Support

Adding support for additional languages to be compiled to WASM: C++, Rust, etc.

### Documentation & Tutorials

# Phase 4 - Parallel Optimization Summer / Fall 2018

After getting a stable 1.0 product released, we will move toward optimizing the code for parallel execution.

# Phase 5 - Cluster Implementation The Future