# Proyecto Métodos Numéricos :pencil2:

## Requerimentos y detalles
Este código resuelve y grafica ecuaciones ingresadas por el usuario, utiliza el método de bisección, Newton - Raphson, secante y falsa posición.

**Aviso:** de preferencia ejecutar desde el entorno de desarrollo ya que el ejecutable puede no funcionar al 100% por problemas de compatibilidad
- Se necesita una versión 19 o superior del JDK para que funcione el código
- El ejecutable se encuentra en la capeta `out\artifacts\MetodosNum_jar`
- Al usar el ejecutable este puede no funcionar al 100% por problemas de compatibilidad

## Descripción del código

### Menú
La ventana que aparece al iniciar es un menú con las opciones de escoger cualquier metodo, ver los créditos, o salir de la aplicación, además de una serie de instrucciones para usar correctamente la aplicación.

### Escoger función
Al escoger cualquier función se abrira la ventana para que escojas o ingreses una función, el unico requerimento que debe tener está es que sea escrita en terminos de x, por ejemplo: `x^2 - 3`

### Graficación de funciones
Las funciones se grafican en base a la que escojamos, respeta los limites que nosotros ingresamos, excepto en el metodo de Newton, donde como no ingresamos limites solo la gráfica desde -10 a 10 en el eje x.

### Método de bisección
Este método nos pide que ingresemos un limite a y un limite b, realiza las válidaciones necesarias ya que como el método nos menciona f(a) y f(b) deben tener signos opuestos o el método no tendría sentido.

### Método de Newton - Raphson
Al ejecutar esté método lo primero que hace es calcular la derivda de la función que elegimos para que el usuario pueda visualizarla, nos pide una Xo *( Aproximación inicial )*, ejecutamos y nos dibuja la gráfica en un intervalo de -10 a 10 en el eje x.

### Método de falsa posición
Bastante similar al de bisección pero en este no pedimos la validación de si f(a) y f(b) son de signo opuesto ya que el valor de Xi lo obtenemos de manera distinta.

### Método de secante
En este obtenemos nuestro valor de Xi de manera bastante similar al de falsa posición pero aquí los criterios de como cambiar nuestros datos es diferente, ya que en cada iteración a tomará el valor de b, y b el valor de nuestra Xi.

## Créditos :star:

- ### Cortes Aguilar Aaron
- ### Macias Caracosa Dante
- ### Barrera Reyes David
- ### Barriga Morales Jesús Bladimir
- ### Alejos Soria Salvador