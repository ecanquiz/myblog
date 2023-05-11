---
layout: "@layouts/ArticleLayout.astro"
title: Modelo Vista Controlador
date: 08 May 2023
image: /myblog/images/model-view-controller/mvc.png
tags:
  - architecture
  - design
  - patterns
  - web-app 

description: How a description would look like if added
---

---

<h2>Para hablar de MVC hay que definir primero en qué consiste una Arquitectura de Software</h2>

>Una _Arquitectura de Software_ consiste, en un conjunto de patrones de diseño, que proporcionan un marco definido y claro para interactuar con el código fuente del software. Dichos patrones, tratan los problemas que se repiten y que se presentan en situaciones particulares, con el fin de proponer soluciones a ellas. Por lo tanto, _'los patrones de diseño son soluciones exitosas a problemas comunes'_.

Al referirnos a _MVC_, decimos que se trata de un _patrón arquitectónico_ de diseño de software, muy utilizado en sistemas web, que separa en tres (3) capas ( _modelo_, _vista_ y _controlador_ ) el código fuente de la aplicación.
                    
Dicho patrón separa los datos y la lógica del negocio ( es decir: el _modelo_ ),  de la interfaz del usuario ( osea: la _vista_ ) y también del módulo encargado de gestionar los eventos (en otras palabras: el _controlador_ ).

<strong>Recapitulando</strong>
<br/><br/> 
• El <strong>Modelo</strong> se encarga de interactuar con la base de datos y también ejecuta las reglas de negocio. 
<br/><br/> 
• La <strong>Vista</strong> consiste en lo que se le muestra al usuario, con información proveniente del modelo, a través del controlador. 
<br/><br/> 
• El <strong>Controlador</strong> procesa las peticiones del usuario a través de la vista, y envía estos datos al modelo, para que esta le devuelva la información adecuada para mostrarla en la vista.

---

<h3>¿Porqué utilizar MVC?</h3>

Por que hace que el código sea más escalable, facilita su mantenimiento y su refactorización, al estar separadas las distintas capas y procesos según su tipo y lenguaje.

Para una mejor percepción, a través de un ejemplo, muestro la transformación de una  lista de artículos de un blog.

Este código es lo necesario para cumplir con el propósito de listar los artículos:

```php
<?php
 $cn = pg_connect(
     "host=localhost dbname=prueba user=usuario password=clave"
 );

 $resultado = pg_query( $cn, "SELECT fecha, titulo FROM articulo;" );
?>

 <h1>Listado de Artículos</h>
 <table>
   <tr> <th>Fecha</th> <th> Título</th> </tr>
   
   <?php
     while ($fila = pg_fetch_array( $resultado, NULL, PGSQL_ASSOC)) {
         echo "<tr>";
         echo "<td>" . $fila['fecha'] . "</td>";
         echo "<td>" . $fila['titulo'] . "</td>";
         echo "</tr>";
     }
   ?>
   
 </table>

 <?php
     pg_close( $cn );
 ?>
```

<strong>Observe que:</strong>
 
• En la misma página nos conectamos al servidor y seleccionamos una base de datos. Note que solo funciona para <i>PostgreSQL</i>.
<br/>
• Realizamos una consulta ( no hay manejo de errores y/o excepciones).
<br/>
• Pintamos el código <i>HTML</i>, combinando código <i>HTML </i>dentro del código <i>PHP</i> y dejando los tags ( <i>&lt;tr&gt;</i>, <i>&lt;td&gt;</i> ) ilegibles para la persona que implemente los estilos.

---

<h3>Utilizando el Controlador y la Vista</h3>

El ejemplo anterior los separaremos en dos (2) archivos, uno se llamará controlador y el otro vista.
<br/><br/> 
<strong>controlador.php</strong>

```php
<?php
 $cn = pg_connect(
   "host=localhost dbname=prueba user=usuario password=clave"
 );
 $resultado = pg_query( $cn, "SELECT fecha, titulo FROM articulo;" );
 $articulos = array();

 while ( $articulo = pg_fetch_array( $resultado, NULL, PGSQL_ASSOC ) ) {
     $articulos[] = $articulo;
 }

 pg_close( $cn );

 require( "vista.php" );
?>
```

<strong>vista.php</strong>

```php
<h1>Listado de Artículos</h1>
<table>
    <tr> <th>Fecha</th> <th>Título</th> <tr>
    <?php foreach ( $articulos as $articulo ): ?>
    <tr>
        <td><?php echo $articulo['fecha'] ?></td>
        <td><?php echo $articulo['fecha'] ?></td>
    <tr>
    <?php endforeach; ?>
</table>
```

De esta manera tenemos separado en el archivo _controlador.php_ casi todo el código _PHP_ con la lógica de negocios, mientras que en el archivo _vista.php_ solo recorremos un arreglo con datos.

---

<h3>Utilizando el Modelo</h3>

Otro problema sería si queremos utilizar nuevamente el listar artículo en otra página, tendríamos que reescribir el <i>controlador</i> de nuevo. 
                     
Para esto separaremos el <i>controlador</i> en <i>modelo</i> y <i>controlador</i>. 

<strong>modelo.php</strong>

```php
<?php
    function getArticulos()
    {
        $cn = pg_connect(
            "host=localhost dbname=prueba user=usuario password=clave"
        ); 

        $resultado = pg_query($cn, "SELECT fecha, titulo FROM articulo;");
        $articulos = array();

        while ($articulo = pg_fetch_array( $resultado, NULL, PGSQL_ASSOC)){
            $articulos[] = $articulo;
        }

        pg_close($cn);
    }
?>;
```

<strong>controlador.php</strong>

```php
<?php
require("modelo.php");

$articulos = getArticulos();

require("vista.php");
?>
```

Después de esta separación el _controlador_ quedaría tan solo como un agente para pasar datos del _modelo_ hacia la _vista_, pero en aplicaciones mas complejas el controlador es quien realiza las tareas de autenticación de usuarios, manejo de sesiones, filtrar y validar entradas de datos por _GET_ o _POST_.

<h3>Cambiando el gestor de Base de Datos</h3>

La respuesta sería, cambiar todas las funciones del modelo, ( `pg_connect`, `pg_query` ) por las correspondientes, y eso nos tomaría mucho tiempo.

Para hacer un mejor uso de _MVC_ o mejor dicho, optimizando el patrón un poco, se podría separar el modelo en dos (2) capas:
                   
- La capa de _Abstracción de Datos_.
- La capa de _Abstracción de la Base de Datos_.

Si se diera el caso de cambiar de gestor de base de datos, solo tendríamos que actualizar la capa de _Abstracción de la Base de Datos_.

<strong>Ejemplo de las dos (2) capas:</strong>

```php
<?php
    function crearConexion( $servidor, $bd, $usuario, $clave )
    {
        return pg_connect(
            "host=$servidor dbname=$bd user=$usuario password=$clave"
        );
    }

    function cerrarConexion( $cn )
    {
        pg_close( $cn );
    }

    function consulta( $consulta, $cn )
    {
        return pg_query( $consulta, $cn );
    }

    function getResultado( $resultado, $row = NULL, $tipo = PGSQL_ASSOC )
    {
        return pg_fetch_array( $resultado, $row, $tipo );
    }
?>
```

<strong>Acceso a Datos</strong>

```php
<?php   
function</span> getTodosLosArticulos()
{
    $cn = crearConexion( "localhost", "", "usuario", "clave" );
    $resultado=consulta( "SELECT fecha, titulo FROM articulo;", "db", $cn);
    $articulos = array();
    while ( $articulo = getResultado( $resultado ) ) {
        $articulos[]  = $articulo;
    }

    cerrarConexion($cn);
    return $articulos;
}
?>
```

Ahora tenemos la capa de _Acceso a Datos_ sin ninguna dependencia de funciones de algún gestor de base de datos, y podremos reutilizar las funciones de la capa de Abstracción de la Base de Datos, en otras funciones de _Acceso a Datos_, de nuestro _modelo_.
