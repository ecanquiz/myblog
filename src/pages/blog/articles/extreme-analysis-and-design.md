---
layout: "@layouts/ArticleLayout.astro"
title: Análisis y Diseño eXtremo
date: 11 May 2023
image: /myblog/images/extreme-analysis-and-design/extreme-analysis-and-design.png
tags:
- agile
- kamban
- lean
- methodology
- scrum
- xp

description: Lo ideal en todo proyecto es que el Dueño del Producto tuviese una imagen clara y concisa de lo que quiere y de cómo lo quiere.
---

>Lo ideal en todo proyecto es que el [Dueño del Producto](https://caribestic.github.io/scrum/roles/product-owner.html) tuviese una imagen clara y concisa de lo que quiere y de cómo lo quiere. Desgraciadamente lo mas habitual en todo proyecto es que el usuario sabe lo que quiere en un dominio de su negocio pero carece del conocimiento para expresar esas funcionalidades en un ambiente algo mas técnico o propicio para los desarrolladores.

Tanto el análisis como el diseño son tareas muy importantes en el desarrollo de software, pero deben realizase con un enfoque más ligero y transparente. El análisis es parte fundamental, intentando recoger en él todas las necesidades del usuario. De él surgen las [Historias de Usuario](https://caribestic.github.io/scrum/artifacts/user-stories.html), que nos servirán para empezar a comenzar a desarrollar nuestro sistema.

El análisis, en el Desarrollo eXtremo, es una labor común entre los desarrolladores y el propio [usuario principal](https://caribestic.github.io/scrum/roles/product-owner.html). La parte de análisis se transforma en la exposición por parte del Dueño del Producto en las Historias de Usuarios que elabora y pasa a exponer en las [Reuniones de Planificación](https://caribestic.github.io/scrum/events/sprint-planning.html) con el [Equipo de Desarrollo](https://caribestic.github.io/scrum/roles/development-team.html). De esta forma, lo que se intenta es que todo el equipo tenga claro lo que se quiere hacer. Una vez que todos los miembros del equipo han comprendido lo que se va a realizar, podemos decir que la primera parte del análisis ha concluido. A continuación le sigue una parte de diseño global del sistema, en el que se profundiza hasta el nivel necesario para que los desarrolladores sepan exactamente que van a tener que hacer.

Esta parte de diseño global se realiza intentando lograr entre todos un cuadro general del sistema. Los miembros del equipo intentan detectar todas las tareas necesarias para desarrollar las Historias de Usuarios. Por regla general, nos encontramos con que el equipo ha encontrado una solución correcta, que implica una extensión de las funcionalidades de la última versión desarrollada. Otras veces, encontramos la existencia de varias aproximaciones, por la que el equipo debe elegir la más simple, acorde con la metodología que siempre se sigue. En otras ocasiones, no se encuentra ninguna solución factible a priori. Estas son las ocasiones típicas en las que se debe iniciar una iteración experimental, que nunca debe durar más de un día o dos, intentando ver cuál es una posible solución. Aquí nunca se resolverá el problema, se debe encontrar únicamente la manera, pero sin profundizar más allá de lo necesario para saber qué hacer.

---

Una vez que se tiene una visión global del sistema a desarrollar en la iteración en cuestión, se dividen las tareas estimándolas, de manera que ayudan al [Gestor del Proyecto](https://caribestic.github.io/scrum/roles/project-manager.html) a la hora de la estimación del esfuerzo y consiguen cierta libertad al desarrollar en un plazo de tiempo en el que ellos creen.

El agilismo se orienta a la parte práctica, intentando eliminar todo aquello innecesario y poniéndonos a implementar lo más pronto posible. Aún así, y pese a que la parte de análisis es mucho más ligera, la parte de diseño sigue siendo parecida a las labores de diseño más o menos habituales en otras metodologías.

Sin embargo, una de las diferencias radicales en el agilismo consiste en que el Equipo de Desarrollo después de haber escuchado con atención las
necesidades (Historias de Usuario) del Dueño del Producto, entonces diseñará el prototipo de pantalla (vista o interfaz del usuario) directamente en el mismo lenguaje de programación en que se construirá el software. Según la aprobación del usuario principal, de dicho prototipo, se diseñará y construirá inmediatamente el modelo de base de datos. De esta manera, mientras se va diseñando a su vez se va construyendo. Entonces, los posibles cambios o modificaciones que vayan surgiendo se harán directamente en el propio código fuente y la base de datos real. Eliminando así, diseños en herramientas gráficas que al pasar el tiempo se convierten solo en papeleos innecesarios y trámites burocráticos.

---

>Tanto Scrum como XP requieren que los equipos completen algún tipo de producto potencialmente liberable al final de cada iteración. Estas iteraciones están diseñadas para ser cortas y de duración fija. Este enfoque en entregar código funcional cada poco tiempo significa que los equipos Scrum y XP no tienen tiempo para teorías. No persiguen dibujar el modelo UML perfecto en una herramienta CASE, escribir el documento de requisitos perfecto o escribir código que se adapte a todos los cambios futuros imaginables. En vez de eso, los equipos Scrum y XP se enfocan en que las cosas se hagan. Estos equipos aceptan que puede que se equivoquen por el camino, pero también son conscientes de que la mejor manera de encontrar dichos errores es dejar de pensar en el software a un nivel teórico de análisis y diseño y sumergirse en él, ensuciarse las manos y comenzar a construir el producto.

_**Mike Cohn**: Autor de Agile Estimating and Planning y User Stories Applied for Agile Software Development._

Es necesario dejar claro, que el diseño de la base de datos es uno de los documentos importante que no debería ser pasado por alto y que al sufrir alguna modificación debe ser actualizado, garantizando siempre una eficiente y oportuna documentación del sistema. Recordemos, que una documentación del sistema, simple y a la disposición del equipo, cumple con los dos primeros valores del agilismo (comunicación y simplicidad).

---

<h3>Proyectos con requisitos inciertos e inestables</h3>

>¿Por qué predecir la versión definitiva de algo que va a estar evolucionando de forma continua?

El agilismo considera a la inestabilidad como una premisa, y adopta técnicas de trabajo para facilitar la evolución sin degradar la calidad de la arquitectura y permitir que también evolucione durante el desarrollo. Durante la construcción se depura el diseño y la arquitectura, y no se cierran en una primera fase del proyecto. Las distintas fases que el desarrollo en cascada (según la metodología predictiva) realiza de forma secuencial, en el agilismo se solapan de forma continua y simultánea, convirtiéndose como consecuencia en actividades.

<h3>Consejos prácticos</h3>

- "Primero resuelve el problema. Entonces, escribe el código" -- John Johnson
- "Programar sin una arquitectura o diseño en mente es como explorar una gruta sólo con una linterna: no sabes dónde estás, dónde has estado ni
hacia dónde vas“ -- Danny Thorpe
- “No empieces a desarrollar si no tienes antes (como mínimo) un pequeño diagrama del modelo del negocio;- Se pueden plantear más de un diseño,
que no necesariamente son “la única solución posible” y esto quedará a discusión del equipo de trabajo y ajustándose a un contexto determinado;-
Un diseño está atado siempre a un contexto, si cambia el contexto, muy probablemente deberá cambiar el diseño;- No existen diseños que sirvan
para absolutamente todos los contextos posibles, por lo tanto, es recomendable hacer un diseño concreto para un contexto concreto.” -- Enrique Place

<h2>Esquema organizacional</h2>

En muchas direcciones de informática o departamentos de sistemas (o como se denomine), suelen separar el área de “análisis y diseño” del área de
“desarrollo”. 

![extreme-analysis-and-design](/myblog/images/extreme-analysis-and-design/organizational-scheme-1.jpg)

Si hay algo en que las metodologías predictivas y las ágiles han estado siempre de acuerdo, es que las fases o actividades del desarrollo de software son las mismas para ambos (levantamiento, análisis, diseño, programación, pruebas e implementación), solo que con dos enfoque distintos. No obstante, el separar estas áreas (en oficinas, gerencias, coordinaciones, etc.) trae como consecuencia dos grandes obstáculos, generando incongruencia y mucho más retardo en el desarrollo de software:

1. Falta de comunicación efectiva durante el desarrollo del proyecto (el “programador” no conversa cara a cara con el usuario).
2. Establecimiento de normativas burocráticas innecesarias (barreras o parcelas de poder), que deben cumplirse para desarrollar el producto.

En este sentido, para garantizar el agilismo en una oficina de desarrollo (como su nombre lo indica) de software, debe estar conformada no solo por
simples programadores, sino mas bien por desarrolladores integrales. En otras palabras, cada miembro del Equipo de Desarrollo, además de saber programar, debe también aprender a conversar y escuchar todo lo que se le dice, tener mucha capacidad de análisis y el suficiente talento para lograr diseños que establezcan nuevas e innovadoras soluciones al modelo del negocio.

---

Por otra parte, en una dirección de informática, también es recomendable contar con una oficina encargada de maximizar la eficiencia y efectividad en la gestión de los proyectos de software. Así como además, encargados de las pruebas continuas (control de calidad), inducciones que asesoren a los usuarios finales y documentación que sea verdaderamente útil como elaboración de manuales del sistema y reportes de la gestión del proyecto de software, etc.

![extreme-analysis-and-design](/myblog/images/extreme-analysis-and-design/organizational-scheme-2.jpg)

Es importante aclarar que no confundamos la “Oficina de Gestión de Proyectos” (de software) con la “Oficina de Planificación de Proyectos”. Esta
última está orientada más hacia los proyectos propios de la Dirección de Informática, tales como:

- Adquisición de nuevas herramientas tecnológicas
- Construcción de la estructura de la red (cableado y/o wifi)
- Capacitación tecnológica del personal
- Entre otros

Finalmente, el objetivo de contar con una ágil estructura organizacional, es por un lado, impulsar el extremo desempeño del Equipo de Desarrollo en la construcción del software, apoyándolo en lo que se necesite y removiéndole cualquier tipo de obstáculos, y por el otro, garantizar la adecuada gestión del proyecto y la retroalimentación efectiva con el Dueño del Producto.


