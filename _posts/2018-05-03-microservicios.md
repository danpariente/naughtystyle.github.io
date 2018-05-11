---
layout: post
title: Microservicios
permalink: /microservicios/
tags: ["Software Architecture"]
---

# Microservicios

## Una definición de este nuevo termino arquitectural


El término "Arquitectura de Microservicios" ha surgido en los últimos años para describir una forma particular de diseñar aplicaciones de software como conjuntos de servicios de despliegue independiente. Si bien no existe una definición precisa de este estilo arquitectónico, existen ciertas características comunes en torno a la organización acerca de la capacidad operativa, la distribución automatizada, la inteligencia en los endpoints y el control descentralizado de lenguajes y datos.

"Microservicios" - otro nuevo término en las concurridas calles de la arquitectura de software. Aunque nuestra inclinación natural es pasar por alto estas cosas con una mirada despectiva, este trozo de terminología describe un estilo de sistemas de software que encontramos cada vez más atractivos. Hemos visto muchos proyectos usar este estilo en los últimos años, y los resultados hasta ahora han sido positivos, hasta el punto de que para muchos de nuestros colegas se está convirtiendo en el estilo por defecto para la creación de aplicaciones empresariales. Lamentablemente, sin embargo, no hay mucha información que describa el concepto de microservicio y cómo implementarlo.

En resumen, el estilo de arquitectura de microservicios[1] es un enfoque para desarrollar una única aplicación como un conjunto de pequeños servicios, cada uno de los cuales se ejecuta en su propio proceso y se comunica con mecanismos ligeros, a menudo una API de recursos HTTP. Estos servicios se construyen en torno a las capacidades operativas y pueden desplegarse de forma independiente mediante maquinaria de despliegue completamente automatizada. Existe un mínimo de manejo centralizado de estos servicios, el cual puede ser escrito en diferentes lenguajes de programación y usar diferentes tecnologías de almacenamiento de datos.

Para empezar a explicar el estilo de los microservicios es conveniente compararlo con el estilo monolítico: una aplicación monolítica construida como una sola unidad. Las aplicaciones empresariales suelen constar de tres partes principales: una interfaz de usuario del lado del cliente (que consta de páginas HTML y javascript que se ejecutan en un navegador en el equipo del usuario), una base de datos (que consta de muchas tablas insertadas en un sistema de gestión de bases de datos común y, por lo general, relacional) y una aplicación del lado del servidor. La aplicación del lado del servidor manejará las peticiones HTTP, ejecutará la lógica del dominio, recuperará y actualizará los datos de la base de datos, y seleccionará y completará las vistas HTML que se enviarán al navegador. Esta aplicación del lado del servidor es un monolito - un único ejecutable lógico[2]. Cualquier cambio en el sistema implica la creación e implementación de una nueva versión de la aplicación del lado del servidor.

Un servidor monolítico de este tipo es una forma natural de abordar la construcción de un sistema de este tipo. Toda su lógica para manejar una solicitud se ejecuta en un solo proceso, permitiéndole usar las características básicas de su lenguaje para dividir la aplicación en clases, funciones y espacios de nombres (namespaces). Con un poco de cuidado, puede ejecutar y probar la aplicación en el portátil de un desarrollador y utilizar un canal de despliegue para asegurarse de que los cambios se probaron e incorporaron correctamente en producción. Se puede escalar horizontalmente el monolito ejecutando muchas instancias detrás de un balanceador de carga (load-balancer).

Las aplicaciones monolíticas pueden tener éxito, pero cada vez son más las personas que sienten frustraciones con las mismas, especialmente a medida que se despliegan más aplicaciones en la nube. Los ciclos de cambio están unidos - un cambio hecho en una pequeña parte de la aplicación, requiere que todo el monolito sea reconstruido y desplegado. Con el tiempo, a menudo es difícil mantener una buena estructura modular, lo que hace más difícil mantener los cambios que sólo deberían afectar a un módulo dentro de ese módulo. El escalado requiere escalar toda la aplicación en lugar de partes de ésta que requieren mayores recursos.

<img src="/images/microservices/sketch.png" alt="Monolitos & Microservicios">

Estas frustraciones han llevado al estilo arquitectónico del microservicio: construir aplicaciones como conjuntos de servicios. Además del hecho de que los servicios son desplegables y escalables de forma independiente, cada servicio también proporciona un límite de módulo sólido, permitiendo incluso que se escriban diferentes servicios en diferentes lenguajes de programación. También pueden ser gestionados por diferentes equipos.

No afirmamos que el estilo de microservicio sea novedoso o innovador, sus raíces se remontan al menos a los principios de diseño de Unix. Pero sí creemos que no hay suficiente gente que considere una arquitectura de microservicio y que muchos desarrollos de software estarían mejor si la usaran.

## Características de una arquitectura de microservicios

No podemos decir que exista una definición formal del estilo arquitectónico de los microservicios, pero podemos intentar describir lo que vemos como características comunes para las arquitecturas que se ajustan a la categoría. Al igual que con cualquier definición que describa características comunes, no todas las arquitecturas de microservicios tienen todas las características, pero esperamos que la mayoría de las arquitecturas de microservicios exhiban la mayoría de las características. Si bien nosotros, los autores, hemos sido miembros activos de esta comunidad bastante dispersa, nuestra intención es intentar una descripción de lo que vemos en nuestro propio trabajo y en esfuerzos similares por parte de equipos que conocemos. En particular, no estamos estableciendo ninguna definición a la que ajustarse.

Durante todo el tiempo que hemos estado involucrados en la industria del software, ha habido un deseo de construir sistemas mediante la integración de componentes, mucho en la forma en que vemos que las cosas se hacen en el mundo físico. Durante las últimas dos décadas hemos visto un progreso considerable con grandes compendios de bibliotecas comunes que forman parte de la mayoría de las plataformas lingüísticas.

Cuando hablamos de componentes nos encontramos con la difícil definición de lo que hace a un componente. Nuestra definición es que un componente es una unidad de software que es independientemente reemplazable y actualizable.

Las arquitecturas de microservicios utilizarán librerías, pero su principal forma de componer su propio software es dividiéndolo en servicios. Definimos las librerías como componentes que se enlazan a un programa y se llaman mediante llamadas a funciones en memoria, mientras que los servicios son componentes fuera de proceso que se comunican con un mecanismo como una solicitud de servicio web o una llamada a un procedimiento remoto. (Este es un concepto diferente al de un objeto de servicio en muchos programas de OO[3].

Una razón principal para utilizar los servicios como componentes (en lugar de librerías) es que los servicios se pueden implementar de forma independiente. Si tiene una aplicación[4] que consiste en múltiples librerías en un solo proceso, un cambio en cualquier componente resulta en tener que volver a desplegar toda la aplicación. Pero si esa aplicación se descompone en múltiples servicios, puede esperar que muchos cambios en un solo servicio sólo requieran la redistribución de ese servicio. Eso no es un hecho, algunos cambios cambiarán las interfaces de servicio resultando en cierta coordinación, pero el objetivo de una buena arquitectura de microservicio es minimizarlas a través de límites de servicio cohesivos y mecanismos de evolución en los contratos de servicio.

Otra consecuencia del uso de servicios como componentes es una interfaz de componentes más explícita. La mayoría de los idiomas no tienen un buen mecanismo para definir una Interfaz Publicada explícita. A menudo es sólo la documentación y la disciplina lo que evita que los clientes rompan el encapsulado de un componente, lo que conduce a un acoplamiento demasiado estrecho entre los componentes. Los servicios hacen más fácil evitarlo utilizando mecanismos explícitos de llamada remota.

Usar servicios como este tiene sus desventajas. Las llamadas remotas son más costosas que las llamadas en proceso y, por lo tanto, las API remotas deben ser de grano más grueso, lo que a menudo es más incómodo de usar. Si necesita cambiar la asignación de responsabilidades entre componentes, tales movimientos de comportamiento son más difíciles de hacer cuando está cruzando los límites del proceso.

En una primera aproximación, podemos observar que los servicios mapean los procesos en tiempo de ejecución, pero eso es sólo una primera aproximación. Un servicio puede consistir en múltiples procesos que siempre se desarrollarán e implementarán juntos, como un proceso de aplicación y una base de datos que sólo es utilizada por ese servicio.

## Organizado en torno a las capacidades de negocio

Cuando se busca dividir una aplicación grande en partes, a menudo la administración se centra en la capa de tecnología, lo que conduce a equipos de interfaz de usuario, equipos de lógica del lado del servidor y equipos de bases de datos. Cuando los equipos se separan en este sentido, incluso los cambios más sencillos pueden llevar a que un proyecto entre equipos requiera tiempo y aprobación presupuestaria. Un equipo inteligente optimizará en torno a esto y se inclinará por el menor de los dos males: sólo tiene que forzar la lógica en cualquier aplicación a la que tenga acceso. Lógica en todas partes, en otras palabras. Este es un ejemplo de la Ley de Conway[5] en acción.

> Cualquier organización que diseñe un sistema (definido ampliamente) producirá un diseño cuya estructura es una copia de la estructura de comunicación de la organización.
-- Melvyn Conway, 1967

<img src="/images/microservices/conways_law.png" alt="Ley de Conway">

El enfoque de la división desde el punto de vista de los microservicios es diferente, ya que se divide en servicios organizados en base a la capacidad empresarial. Tales servicios requieren una amplia implementación de software para esa área de negocio, incluyendo interfaz de usuario, almacenamiento persistente y cualquier colaboración externa. En consecuencia, los equipos son multifuncionales, incluyendo toda la gama de habilidades necesarias para el desarrollo: experiencia del usuario, base de datos y gestión de proyectos.

<img src="/images/microservices/prefer_functional_staff_organisation.png" alt="Organizacion de Equipos">

Una empresa organizada de esta manera es www.comparethemarket.com. Los equipos multifuncionales son responsables de la construcción y operación de cada producto y cada producto se divide en una serie de servicios individuales que se comunican a través de un bus de mensajes (message bus).

Las aplicaciones monolíticas de gran tamaño también pueden modularizarse en función de las capacidades empresariales, aunque no es el caso común. Ciertamente, nos gustaría instar a un gran equipo de construcción de una aplicación monolítica para dividirse a sí mismo a lo largo de las líneas de negocio. El principal problema que hemos visto aquí es que tienden a organizarse en demasiados contextos. Si el monolito se extiende a lo largo de muchos de estos límites modulares puede ser difícil para los miembros individuales de un equipo encajarlos en su memoria a corto plazo. Además, vemos que las líneas modulares requieren una gran disciplina para su aplicación. La separación necesariamente más explícita que requieren los componentes de servicio hace más fácil mantener claros los límites del equipo.

## Productos no Proyectos

La mayoría de los esfuerzos de desarrollo de aplicaciones que vemos utilizan un modelo de proyecto: donde el objetivo es entregar alguna pieza de software que luego se considera completada. Una vez finalizado, el software se entrega a una organización de mantenimiento y el equipo del proyecto que lo construyó se disuelve.

Los proponentes de los microservicios tienden a evitar este modelo, prefiriendo en cambio la noción de que un equipo debe poseer un producto durante toda su vida útil. Una inspiración común para esto es la noción de Amazon de "tú lo construyes, tú lo diriges", donde un equipo de desarrollo asume toda la responsabilidad del software en producción. Esto pone a los desarrolladores en contacto diario con el comportamiento de su software en la producción y aumenta el contacto con sus usuarios, ya que tienen que asumir al menos parte de la carga de soporte.

La mentalidad de producto, enlaza con la vinculación con las capacidades empresariales. En lugar de considerar el software como un conjunto de funcionalidades a completar, existe una relación continua en la que la cuestión es cómo puede el software ayudar a sus usuarios a mejorar la capacidad de negocio.

No hay razón por la que no se pueda adoptar este mismo enfoque con las aplicaciones monolíticas, pero la menor granularidad de los servicios puede facilitar la creación de relaciones personales entre los desarrolladores de servicios y sus usuarios.

## Puntos finales inteligentes y conexiones tontas

Al construir estructuras de comunicación entre los diferentes procesos, hemos visto muchos productos y enfoques que enfatizan la importancia de poner inteligencia significativa en el mecanismo de comunicación en sí. Un buen ejemplo de esto es el Enterprise Service Bus (ESB), donde los productos ESB a menudo incluyen instalaciones sofisticadas para el enrutamiento de mensajes, coreografía, transformación y aplicación de reglas de negocio.

La comunidad de microservicios favorece un enfoque alternativo: puntos finales inteligentes y conexiones tontas. Las aplicaciones construidas a partir de microservicios tienen como objetivo ser tan desacopladas y cohesivas como sea posible - poseen su propia lógica de dominio y actúan más como filtros en el sentido clásico de Unix - recibiendo una petición, aplicando la lógica apropiada y produciendo una respuesta. Estas son coreografiadas usando protocolos simples tipo REST en lugar de protocolos complejos como WS-Choreography o BPEL u orquestación por una herramienta central.

Los dos protocolos utilizados más comúnmente son HTTP request-response con recursos API y mensajería ligera[8]. La mejor expresión de la primera es

>Sea de la web, no detrás de la web
-- Ian Robinson

Los equipos de microservicios utilizan los principios y protocolos sobre los que se construye la World Wide Web (y en gran medida, Unix). Los recursos utilizados a menudo pueden almacenarse en caché con muy poco esfuerzo por parte de los desarrolladores o del personal de operaciones.

El segundo enfoque de uso común es la mensajería a través de un bus de mensajes ligero. La infraestructura elegida es típicamente tonta (tonta ya que actua sólo como un enrutador de mensajes) - las implementaciones simples como RabbitMQ o ZeroMQ no hacen mucho más que proveer una estructura asíncrona confiable - la parte inteligente todavía vive en los puntos finales que están produciendo y consumiendo mensajes; en los servicios.

En un monolito, los componentes se ejecutan durante el proceso y la comunicación entre ellos se realiza mediante la invocación del método o la llamada a la función. El mayor problema para transformar un monolito en microservicios radica en cambiar el patrón de comunicación. Una conversión ingenua de las llamadas de método en memoria a RPC conduce a comunicaciones de conversación que no funcionan bien. En su lugar, debe sustituir la comunicación de grano fino por un enfoque de grano más grueso.

```
*Microservicios y SOA*

Cuando hablamos de microservicios, una pregunta común es si se trata de una Arquitectura Orientada a Servicios (SOA)
como la que vimos hace una década. Hay mérito en este punto, porque el estilo de microservicio es muy similar a lo que algunos defensores de
SOA han estado a favor. El problema, sin embargo, es que SOA significa demasiadas cosas diferentes, y que la mayoría de las veces que nos
encontramos con algo llamado "SOA" es significativamente diferente al estilo que estamos describiendo aquí, usualmente debido a un enfoque en
ESBs usados para integrar aplicaciones monolíticas.

En particular, hemos visto tantas implementaciones fallidas de la orientación a servicios - desde la tendencia a ocultar la complejidad en los
ESB[6], a iniciativas plurianuales fallidas que cuestan millones y no aportan ningún valor, a modelos de gobierno centralizado que inhiben
activamente el cambio, que a veces es difícil ver más allá de estos problemas.

Ciertamente, muchas de las técnicas que se utilizan en la comunidad de microservicios han crecido a partir de las experiencias de los
desarrolladores que integran servicios en grandes organizaciones. El patrón de Lector Tolerante es un ejemplo de esto. Los esfuerzos por
utilizar la web han contribuido, el uso de protocolos sencillos es otro enfoque derivado de estas experiencias - una reacción lejos de los
estándares centrales que han alcanzado una complejidad que es, francamente, impresionante. (Cada vez que usted necesita una ontología para
manejar sus ontologías, se da cuenta de que está en serios problemas.

Esta manifestación común de SOA ha llevado a algunos defensores de los microservicios a rechazar la etiqueta de SOA por completo, aunque otros
consideran que los microservicios son una forma de SOA[7], tal vez la orientación a servicios bien hecha. De cualquier manera, el hecho de que
SOA signifique cosas tan diferentes significa que es valioso tener un término que defina más claramente este estilo arquitectónico.
```

## Gobernación descentralizada

Una de las consecuencias de la gobernanza centralizada es la tendencia a la normalización en plataformas tecnológicas concretas. La experiencia demuestra que este enfoque es restrictivo - no todo problema es un clavo y no toda solución un martillo. Preferimos utilizar la herramienta adecuada para el trabajo y aunque las aplicaciones monolíticas pueden aprovechar los diferentes lenguajes hasta cierto punto, no es tan común.

Al dividir los componentes del monolito en servicios, podemos elegir a la hora de construir cada uno de ellos. ¿Desea utilizar Node.js para poner de pie una página de reportes simple? Vamos. C++ para un componente particularmente nudoso y casi en tiempo real? Bien. ¿Desea intercambiar a un tipo diferente de base de datos que mejor se adapte al comportamiento de lectura de un componente? Tenemos la tecnología para reconstruirlo.

Por supuesto, el hecho de que pueda hacer algo no significa que deba hacerlo, pero particionar su sistema de esta manera significa que tiene la opción.

Los equipos que crean microservicios también prefieren un enfoque diferente de las normas. En lugar de utilizar un conjunto de estándares definidos escritos en algún papel, prefieren la idea de producir herramientas útiles que otros desarrolladores puedan usar para resolver problemas similares a los que están enfrentando. Estas herramientas son usualmente recolectadas de implementaciones y compartidas con un grupo más amplio, algunas veces, pero no exclusivamente, usando un modelo interno de código abierto. Ahora que git y github se han convertido en el sistema de control de versiones por defecto, las prácticas de código abierto son cada vez más comunes en las empresa.

Netflix es un buen ejemplo de una organización que sigue esta filosofía. Compartir código útil y, sobre todo, probado en el campo de batalla, ya que las librerías animan a otros desarrolladores a resolver problemas similares de forma similar, pero deja la puerta abierta para elegir un enfoque diferente si es necesario. Las librerías compartidas tienden a centrarse en problemas comunes de almacenamiento de datos, comunicación entre procesos y, como veremos más adelante, automatización de la infraestructura.

Para la comunidad de microservicios, los excesos son particularmente poco atractivos. Eso no quiere decir que la comunidad no valore los contratos de servicio. Todo lo contrario, ya que suelen ser muchos más. Es sólo que están buscando diferentes formas de manejar esos contratos. Patrones como Tolerant Reader y Consumer-Driven Contracts se aplican a menudo a los microservicios. Estos contratos de servicios de ayuda evolucionan de forma independiente. La ejecución de contratos impulsados por el consumidor como parte de su construcción aumenta la confianza y proporciona una rápida retroalimentación sobre si sus servicios están funcionando o no. De hecho, conocemos un equipo en Australia que impulsa la construcción de nuevos servicios con contratos orientados al consumidor. Utilizan herramientas sencillas que les permiten definir el contrato de un servicio. Esto se convierte en parte del build automatizado antes incluso de que se escriba el código para el nuevo servicio. El servicio se construye entonces sólo hasta el punto en que satisface el contrato - un enfoque elegante para evitar el dilema "YAGNI"[9] cuando se construye nuevo software. Estas técnicas y las herramientas que crecen en torno a ellas, limitan la necesidad de una administración central de contratos al disminuir el acoplamiento temporal entre servicios.

Tal vez el apogeo de la gobernanza descentralizada sea el espíritu de constrúyala / diríjala popularizado por Amazon. Los equipos son responsables de todos los aspectos del software que construyen, incluyendo la operación del software 24/7. La devolución de este nivel de responsabilidad no es la norma, pero vemos que cada vez más empresas trasladan la responsabilidad a los equipos de desarrollo. Netflix es otra organización que ha adoptado este ethos[11]. Ser despertado a las 3 de la madrugada todas las noches por su localizador es sin duda un poderoso incentivo para centrarse en la calidad a la hora de escribir su código. Estas ideas están lo más lejos posible del modelo tradicional de gobierno centralizado.

## Administración de Datos Descentralizada

La descentralización de la administración de datos se presenta de varias maneras diferentes. En el nivel más abstracto, significa que el modelo conceptual del mundo diferirá entre sistemas. Este es un problema común cuando se integra en una gran empresa, la vista del departamento de ventas hacia un cliente será diferente de la vista de soporte tecnico. Algunas cosas que se llaman clientes del punto de vista de ventas pueden no aparecer en absoluto de lado de soporte. Aquellos que puedan tener atributos diferentes y (peor aun) atributos comunes con semántica sutilmente diferente.

Este problema es común entre aplicaciones, pero también puede ocurrir dentro de las aplicaciones, especialmente cuando esa aplicación está dividida en componentes separados. Una forma útil de pensar sobre esto es la noción de Contexto Limitado de Diseño Dirigido por Dominio. DDD divide un dominio complejo en múltiples contextos limitados y mapea las relaciones entre ellos. Este proceso es útil tanto para arquitecturas monolíticas como de microservicio, pero existe una correlación natural entre los límites de servicio y contexto que ayuda a clarificar, y como describimos en la sección sobre capacidades de negocio, reforzar las separaciones.

Además de descentralizar las decisiones sobre modelos conceptuales, los microservicios también descentralizan las decisiones de almacenamiento de datos. Mientras que las aplicaciones monolíticas prefieren una única base de datos lógica para los datos persistentes, las empresas a menudo prefieren una única base de datos para toda una gama de aplicaciones - muchas de estas decisiones se toman a través de los modelos comerciales de los proveedores en torno a la concesión de licencias. Los microservicios prefieren dejar que cada servicio administre su propia base de datos, ya sea diferentes instancias de la misma tecnología de base de datos, o sistemas de base de datos completamente diferentes - un enfoque llamado Persistencia Políglota. Se puede utilizar la persistencia políglota en un monolito, pero aparece más frecuentemente con microservicios.

<img src="/images/microservices/decentralised_data.png" alt="Datos Descentralizados">

La responsabilidad de la descentralización de los datos en todos los microservicios tiene consecuencias para la gestión de las actualizaciones. El enfoque común para hacer frente a las actualizaciones ha consistido en utilizar las transacciones para garantizar la coherencia al actualizar múltiples recursos. Este enfoque se utiliza a menudo dentro de los monolitos.

El uso de transacciones de este tipo ayuda a la coherencia, pero impone un acoplamiento temporal significativo, lo que resulta problemático en múltiples servicios. Las transacciones distribuidas son notoriamente difíciles de implementar y, en consecuencia, las arquitecturas de microservicios hacen hincapié en la coordinación sin transacciones entre servicios, con el reconocimiento explícito de que la coherencia sólo puede ser una coherencia eventual y que los problemas se resuelven mediante operaciones compensatorias.

Decidir manejar las inconsistencias de esta manera es un nuevo desafío para muchos equipos de desarrollo, pero es uno que a menudo se ajusta a las prácticas comerciales. A menudo las empresas manejan un grado de inconsistencia con el fin de responder rápidamente a la demanda, mientras que tienen algún tipo de proceso de inversión para hacer frente a los errores. La compensación vale la pena siempre y cuando el costo de corregir errores sea menor que el costo de perder negocios bajo una mayor consistencia.

## Automatización de Infraestructura

Las técnicas de automatización de infraestructuras han evolucionado enormemente en los últimos años: la evolución de la nube y de AWS en particular ha reducido la complejidad operativa de la construcción, el despliegue y la operación de microservicios.

Muchos de los productos o sistemas que se están construyendo con microservicios están siendo construidos por equipos con amplia experiencia en la Entrega Continua (CD) y su precursor, la Integración Continua (CI). Los equipos que construyen software de esta manera hacen un uso extensivo de las técnicas de automatización de infraestructura. Esto se ilustra a continuación.

<img src="/images/microservices/basic_pipeline.png" alt="Build">

Como este no es un artículo sobre Entrega Continua, pondremos atención solo a un par de características clave. Queremos la mayor confianza posible en que nuestro software funciona, por lo que realizamos muchas pruebas automatizadas. La promoción del software de trabajo 'arriba' de la tubería significa que automatizamos la implementación en cada nuevo entorno.

Una aplicación monolítica será construida, testada y empujada a través de estos ambientes alegremente. Resulta que una vez que se ha invertido en automatizar el camino a la producción de un monolito, entonces desplegar más aplicaciones ya no parece tan aterrador. Recuerde, uno de los objetivos del CD es hacer que el despliegue sea aburrida, así que ya sea que se trate de una o tres aplicaciones, siempre y cuando sea aburrida no importa[12].

Otra área en la que vemos equipos que utilizan una amplia automatización de la infraestructura es en la administración de microservicios en la producción. En contraste con nuestra afirmación anterior de que mientras el despliegue sea aburrido no hay tanta diferencia entre monolitos y microservicios, el panorama operacional para cada uno puede ser notablemente diferente.

<img src="/images/microservices/micro_deployment.png" alt="Despliegue de Microservicios">

## Diseño para fallas

Una consecuencia del uso de los servicios como componentes es que las aplicaciones deben diseñarse de manera que puedan tolerar el fallo de los servicios. Cualquier llamada de servicio puede fallar debido a la falta de disponibilidad del proveedor, el cliente tiene que responder a esto tan elegantemente como sea posible. Esto es una desventaja en comparación con un diseño monolítico, ya que introduce complejidad adicional para manejarlo. La consecuencia es que los equipos de microservicios reflexionan constantemente sobre cómo las fallas en los servicios afectan la experiencia del usuario. El Ejército Simian de Netflix induce fallas en los servicios e incluso en los centros de datos durante la jornada laboral para probar tanto la resiliencia como la monitorización de la aplicación.

Este tipo de prueba automatizada en la producción sería suficiente para dar a la mayoría de los grupos de operación el tipo de escalofríos que normalmente preceden a una semana de ausencia del trabajo. Esto no quiere decir que los estilos arquitectónicos monolíticos no sean capaces de configuraciones de monitorización sofisticadas - es solamente menos común en nuestra experiencia.

Dado que los servicios pueden fallar en cualquier momento, es importante poder detectar los fallos rápidamente y, si es posible, restaurar el servicio automáticamente. Las aplicaciones de microservicios ponen mucho énfasis en la monitorización en tiempo real de la aplicación, comprobando tanto los elementos arquitectónicos (cuántas peticiones por segundo recibe la base de datos) como las métricas relevantes para el negocio (por ejemplo, cuántos pedidos por minuto se reciben). El monitoreo semántico puede proporcionar un sistema de alerta temprana de que algo va mal que desencadena que los equipos de desarrollo hagan un seguimiento e investiguen.

Esto es particularmente importante para una arquitectura de microservicios porque la preferencia de los microservicios por la coreografía y la colaboración en eventos conduce a un comportamiento emergente. Mientras que muchos expertos elogian el valor del surgimiento fortuito, la verdad es que el comportamiento emergente a veces puede ser algo malo. El monitoreo es vital para detectar rápidamente el mal comportamiento emergente y poder corregirlo.

Los monolitos pueden construirse para que sean tan transparentes como un microservicio; de hecho, deberían serlo. La diferencia es que es absolutamente necesario saber cuándo se desconectan los servicios que se ejecutan en procesos diferentes. Con las librerías dentro del mismo proceso, es menos probable que este tipo de transparencia sea útil.

Los equipos de microservicios esperarán ver configuraciones sofisticadas de monitoreo y registro para cada servicio individual, tales como cuadros de mando que muestren el estado ascendente/descendente y una variedad de métricas operativas y de negocios relevantes. Los detalles sobre el estado de los cortacircuitos, el rendimiento de corriente y la latencia son otros ejemplos que encontramos a menudo en la naturaleza.

## Diseño Evolutivo

Los practicantes de microservicios, por lo general, provienen de un contexto de diseño evolutivo y ven la descomposición de servicios como una herramienta más para permitir a los desarrolladores de aplicaciones controlar los cambios en sus aplicaciones sin ralentizar el cambio. El control del cambio no significa necesariamente la reducción del cambio - con las actitudes y herramientas correctas puede realizar cambios frecuentes, rápidos y bien controlados en el software.

Cada vez que intentas dividir un sistema de software en componentes, te encuentras con la decisión de cómo dividir las piezas - ¿cuáles son los principios sobre los que decidimos dividir nuestra aplicación? La propiedad clave de un componente es la noción de sustitución independiente y de capacidad de actualización[13]. lo que implica que busquemos puntos en los que podamos imaginar la reescritura de un componente sin afectar a sus colaboradores. De hecho, muchos grupos de microservicios van más allá y esperan explícitamente que muchos servicios sean desechados en lugar de evolucionar a largo plazo.

El sitio web The Guardian es un buen ejemplo de una aplicación que fue diseñada y construida como un monolito, pero que ha estado evolucionando en una dirección de microservicio. El monolito sigue siendo el núcleo del sitio web, pero prefieren añadir nuevas características mediante la creación de microservicios que utilizan la API del monolito. Este enfoque es particularmente útil para características que son intrínsecamente temporales, como páginas especializadas para manejar un evento deportivo. Esta parte del sitio web se puede crear rápidamente utilizando lenguajes de desarrollo rápido y eliminar una vez finalizado el evento. Hemos visto enfoques similares en una institución financiera donde se añaden nuevos servicios para una oportunidad de mercado y se descartan después de unos meses o incluso semanas.

Este énfasis en la reemplazabilidad es un caso especial de un principio más general de diseño modular, que consiste en impulsar la modularidad a través del patrón de cambio[14]. Con el fin de mantener las cosas que cambian al mismo tiempo en el mismo módulo. Las partes de un sistema que cambian raramente deberían estar en servicios diferentes a los que actualmente tienen muchos cambios. Si te encuentras cambiando repetidamente dos servicios juntos, eso es una señal de que deberían fusionarse.

La puesta en servicios de los componentes agrega una oportunidad para una planificación más granular en el planeamiento de un lanzamiento. Con un monolito cualquier cambio requiere una compilación e implementación completa de toda la aplicación. Sin embargo, con los microservicios, sólo necesita redistribuir el servicio o servicios que modificó. Esto puede simplificar y acelerar el proceso de lanzamiento. La desventaja es que usted tiene que preocuparse por los cambios en un servicio que quiebra a sus consumidores. El enfoque tradicional de integración consiste en tratar de resolver este problema utilizando el versionado, pero la preferencia en el mundo de los microservicios es utilizar el versionado sólo como último recurso. Podemos evitar una gran cantidad de versionado diseñando servicios que sean lo más tolerantes posible a los cambios en sus proveedores.

## ¿Son los Microservicios el futuro?

Nuestro principal objetivo al escribir este artículo es explicar las principales ideas y principios de los microservicios. Al tomarnos el tiempo para hacer esto, pensamos claramente que el estilo arquitectónico de los microservicios es una idea importante - una que vale la pena considerar seriamente para las aplicaciones empresariales. Recientemente hemos construido varios sistemas usando el estilo y sabemos de otros que han usado y favorecen este enfoque.

Aquellos de los que sabemos que de alguna manera son pioneros en el estilo arquitectónico incluyen Amazon, Netflix, The Guardian, the UK Government Digital Service, realstate.com.au, Forward y comparethemarket.com. El circuito de conferencias en 2013 estuvo lleno de ejemplos de compañías que se están moviendo a algo que se clasificaría como microservicios - incluyendo a Travis CI. Además, hay muchas organizaciones que han estado haciendo durante mucho tiempo lo que clasificaríamos como microservicios, pero sin usar nunca el nombre. (A menudo esto se etiqueta como SOA - aunque, como hemos dicho, SOA viene en muchas formas contradictorias. [15])

A pesar de estas experiencias positivas, no estamos argumentando que estemos seguros de que los microservicios son la dirección futura para las arquitecturas de software. Aunque nuestras experiencias hasta ahora son positivas en comparación con las aplicaciones monolíticas, somos conscientes del hecho de que no ha pasado suficiente tiempo para que podamos hacer un juicio completo.

A menudo las verdaderas consecuencias de sus decisiones arquitectónicas sólo son evidentes varios años después de haberlas tomado. Hemos visto proyectos en los que un buen equipo, con un fuerte deseo de modularidad, ha construido una arquitectura monolítica que se ha deteriorado con el paso de los años. Mucha gente cree que tal deterioro es menos probable con los microservicios, ya que los límites de los servicios son explícitos y difíciles de parchar. Sin embargo, hasta que no veamos suficientes sistemas con suficiente edad, no podremos evaluar realmente cómo maduran las arquitecturas de microservicios.

Ciertamente hay razones por las que uno podría esperar que los microservicios maduren pobremente. En cualquier esfuerzo de componentización, el éxito depende de lo bien que el software encaje en los componentes. Es difícil averiguar exactamente dónde deben estar los límites de los componentes. El diseño evolutivo reconoce las dificultades de establecer bien los límites y, por lo tanto, la importancia de que sea fácil refactorizarlos. Pero cuando sus componentes son servicios con comunicaciones remotas, entonces la refactorización es mucho más difícil que con las librerías en proceso. Mover código es difícil a través de los límites del servicio, cualquier cambio en la interfaz necesita ser coordinado entre los participantes, capas de retrocompatibilidad necesitan ser agregadas, y los testeos son más complicados.

Otro problema es que si los componentes no se componen limpiamente, todo lo que está haciendo es cambiar la complejidad del interior de un componente a las conexiones entre componentes. Esto no sólo mueve la complejidad, sino que la traslada a un lugar que es menos explícito y más difícil de controlar. Es fácil pensar que las cosas son mejores cuando se mira el interior de un componente pequeño y simple, mientras faltan conexiones desordenadas entre servicios.

Por último, está el factor de la habilidad del equipo. Las nuevas técnicas tienden a ser adoptadas por equipos más hábiles. Pero una técnica que es más efectiva para un equipo más hábil no necesariamente va a funcionar para equipos menos hábiles. Hemos visto muchos casos de equipos menos hábiles construyendo arquitecturas monolíticas desordenadas, pero lleva tiempo ver lo que ocurre cuando este tipo de lío ocurre con los microservicios. Un equipo pobre siempre creará un sistema pobre - es muy difícil saber si los microservicios reducen el desorden en este caso o lo empeoran.

Un argumento razonable que hemos escuchado es que no se debe empezar con una arquitectura de microservicios. En su lugar, comience con un monolito, manténgalo modular y divídalo en microservicios una vez que el monolito se convierta en un problema. (Aunque este consejo no es el ideal, ya que una buena interfaz en proceso no suele ser una buena interfaz de servicio.

Así que escribimos esto con cauteloso optimismo. Hasta ahora, hemos visto suficiente sobre el estilo del microservicio como para sentir que puede ser un camino que valga la pena recorrer. No podemos decir con seguridad dónde terminaremos, pero uno de los retos del desarrollo de software es que sólo se pueden tomar decisiones basadas en la información imperfecta que actualmente se tiene a mano.

### Notas a pie de página

[1] El término "microservicio" fue discutido en un taller de arquitectos de software cerca de Venecia en mayo de 2011 para describir lo que los participantes vieron como un estilo arquitectónico común que muchos de ellos habían estado explorando recientemente. En mayo de 2012, el mismo grupo se decidió por "microservicios" como el nombre más apropiado. James presentó algunas de estas ideas como un estudio de caso en marzo de 2012 en el 33º Grado en Cracovia en Microservicios - Java, el camino de Unix al igual que Fred George más o menos al mismo tiempo. Adrian Cockcroft en Netflix, describiendo este enfoque como "SOA de grano fino" fue pionero en el estilo a escala web, al igual que muchos de los otros mencionados en este artículo - Joe Walnes, Dan North, Evan Botcher y Graham Tackley.

[2] El término monolito ha sido usado por la comunidad Unix durante algún tiempo. Aparece en The Art of Unix Programming para describir sistemas que se hacen demasiado grandes.

[3] Muchos diseñadores orientados a objetos, incluidos nosotros mismos, utilizamos el término objeto de servicio en el sentido de diseño orientado a dominio (DDD) para un objeto que lleva a cabo un proceso significativo que no está vinculado a una entidad. Este es un concepto diferente de cómo estamos usando "servicio" en este artículo. Lamentablemente el término servicio tiene ambos significados y tenemos que vivir con la polisemia.

[4] Consideramos que una aplicación es una construcción social que une un código base, un grupo de funcionalidad y un cuerpo de financiamiento.

[5] El artículo original se puede encontrar en el sitio web de Melvyn Conway.

[6] No podemos resistirnos a mencionar la declaración de Jim Webber de que ESB significa "Egregious Spaghetti Box".

[7] Netflix hace explícito el enlace - hasta hace poco se refería a su estilo arquitectónico como SOA de grano fino.

[8] En los extremos de escala, las organizaciones a menudo pasan a protocolos binarios - protobufs por ejemplo. Los sistemas que utilizan estos sistemas todavía exhiben la característica de puntos finales inteligentes, conducciones tontas y transparencia de intercambio por escala. La mayoría de las propiedades web y, sin duda, la gran mayoría de las empresas no necesitan hacer esta compensación - la transparencia puede ser una gran victoria.

[9] "YAGNI" o "You are't going to need it" es un principio de XP y exhortación a no añadir características hasta que sepas que las necesitas.

[10] Es un poco poco insensato de nuestra parte afirmar que los monolitos son un solo lenguaje - para construir sistemas en la web de hoy en día, es probable que necesites saber JavaScript y XHTML, CSS, el lenguaje de tu servidor de elección, SQL y un dialecto ORM. Difícilmente un solo lenguaje, pero sabes a lo que nos referimos.

[11] Adrian Cockcroft menciona específicamente "developer self-service" y "Developers run what they wrote"(sic) en esta excelente presentación realizada en Flowcon en noviembre de 2013.

[12] Estamos siendo un poco insensatos aquí. Obviamente, desplegar más servicios, en topologías más complejas es más difícil que desplegar un único monolito. Afortunadamente, los patrones reducen esta complejidad; sin embargo, la inversión en herramientas sigue siendo una necesidad.

[13] De hecho, Dan North se refiere a este estilo como Arquitectura de Componentes Reemplazables en lugar de microservicios. Puesto que esto parece hablar de un subconjunto de las características, preferimos estas últimas.

[14] Kent Beck destaca esto como uno de sus principios de diseño en Patrones de Implementación.

[15] Y SOA no es la raíz de esta historia. Recuerdo que la gente decía "hemos estado haciendo esto durante años" cuando el término SOA apareció a principios de siglo. Un argumento fue que este estilo ve sus raíces en la forma en que los programas COBOL se comunicaban a través de archivos de datos en los primeros días de la informática empresarial. En otra dirección, se podría argumentar que los microservicios son lo mismo que el modelo de programación de Erlang, pero aplicados a un contexto de aplicación empresarial.
