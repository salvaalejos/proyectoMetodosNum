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

```
    public static void graficarFuncion(String funcion, double a, double b, JPanel panel) {
        XYSeries series = new XYSeries("f(x)");

        for (double x = a; x <= b; x += 0.1) {
            double y = evaluarFuncion(funcion, x);
            if (!Double.isNaN(y)) {
                series.add(x, y);
            }
        }
        XYSeriesCollection dataset = new XYSeriesCollection();
        dataset.addSeries(series);

        JFreeChart chart = ChartFactory.createXYLineChart(
                "Gráfica de la Función",
                "x",
                "f(x)",
                dataset,
                PlotOrientation.VERTICAL,
                true, true, false);

        panel.removeAll();
        panel.setLayout(new BorderLayout());
        panel.add(new ChartPanel(chart), BorderLayout.CENTER);
        
        panel.revalidate();
        panel.repaint();
        
    }
```

### Método de bisección
Este método nos pide que ingresemos un limite a y un limite b, realiza las válidaciones necesarias ya que como el método nos menciona f(a) y f(b) deben tener signos opuestos o el método no tendría sentido.

```
    public static List<String> biseccion(String funcion, double a, double b, double ans, XYSeries series) {
            List<String> iterador = new ArrayList<>();

            double fa = evaluarFuncion(funcion, a);
            double fb = evaluarFuncion(funcion, b);

            if (fa * fb >= 0) {
                JOptionPane.showMessageDialog(null,
                        "El método de bisección no puede aplicarse porque los signos de f(a) y f(b) no son opuestos.",
                        "Error",JOptionPane.ERROR_MESSAGE);
                return iterador;
            }
            
            double m = a;
            double error = Double.MAX_VALUE;
            int i = 0;
            double prevm = m;

            while (error > ans && i < 100 ) {
                i++;
                m = (a + b) / 2.0;
                double fm = evaluarFuncion(funcion, m);

                if (Double.isNaN(fm)) {
                    JOptionPane.showMessageDialog(null,
                        "Error al evaluar la función en el punto medio.",
                        "Error",JOptionPane.ERROR_MESSAGE);
                    break;
                }

                error = Math.abs((m - prevm) / m);  // Error 
                iterador.add(String.format("%-4d %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f", 
                    i, a, fa, b, fb, m, fm, error));

                if (fm == 0) {
                    break; // c es la raíz exacta
                } else if (fa * fm < 0) {
                    b = m; // La raíz está en [a, m]
                    fb = fm;
                } else {
                    a = m; // La raíz está en [m, b]
                    fa = fm; // Actualizar f(a) para la siguiente iteración
                }

                prevm = m; // Actualizar valor anterior de m

                series.add(a, fa);// Gráfica
                series.add(b, fb);// Gráfica
            }

            return iterador;
    }
```

### Método de Newton - Raphson
Al ejecutar esté método lo primero que hace es calcular la derivda de la función que elegimos para que el usuario pueda visualizarla, nos pide una Xo *( Aproximación inicial )*, ejecutamos y nos dibuja la gráfica en un intervalo de -10 a 10 en el eje x.

```
    private void desarrollarNewtonRaphson(String funcion, String derivada, double x0, double tolerancia) {
        double errorAbsoluto;
        int iteracionesMaximas = 100;
        double x = x0;
        double fx, fpx;

        DefaultTableModel modelo = (DefaultTableModel) tablaIteraciones.getModel();
        modelo.setRowCount(0); // Limpiar las filas actuales

        // Crear un formateador para mostrar solo 4 decimales
        DecimalFormat df = new DecimalFormat("#.####");

        for (int i = 0; i < iteracionesMaximas; i++) {
            fx = evaluarFuncion(funcion, x); // Evaluar F(x)
            fpx = evaluarFuncion(derivada, x); // Evaluar F'(x)
            // Calcular nuevo valor de x
            double xNuevo = x - fx / fpx;

            // Calcular el error relativo
            errorAbsoluto = Math.abs(xNuevo - x);

            // Agregar fila a la tabla
            modelo.addRow(new Object[]{
                i + 1, // Iteración
                df.format(x), // Valor de x
                df.format(fx), // F(x)
                df.format(fpx), // F'(x)
                df.format(xNuevo), // Nuevo valor de x
                df.format(errorAbsoluto) // Valor absoluto de F(x)
            });

            // Preparar para la próxima iteración
            x = xNuevo;

            // Verificar si el error es menor que la tolerancia
            if (errorAbsoluto < tolerancia) {
                break;
            }
        }
    }
```

### Método de falsa posición
Bastante similar al de bisección pero en este no pedimos la validación de si f(a) y f(b) son de signo opuesto ya que el valor de Xi lo obtenemos de manera distinta.

```
    public List<String> falsaPosicion(String funcion, double a, double b, double ans, XYSeries series) {
        List<String> iterador = new ArrayList<>();

        double fa = evaluarFuncion(funcion, a);
        double fb = evaluarFuncion(funcion, b);

        
        
        double m = a;
        double error = Double.MAX_VALUE;
        int i = 0;
        double prevm = m;

        while (error > ans && i < 100) {
            i++;
            m = b-((fb*( b - a)) / (fb - fa));
            
            double fm = evaluarFuncion(funcion, m);

            if (Double.isNaN(fm)) {
                JOptionPane.showMessageDialog(null,
                    "Error al evaluar la función.",
                    "Error",JOptionPane.ERROR_MESSAGE);
                break;
            }

            error = Math.abs(m - prevm);  // Error 
            iterador.add(String.format("%-4d %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f", 
                i, a, fa, b, fb, m, fm, error));

            if (fm == 0) {
                break; // c es la raíz exacta
            } else if (fa * fm < 0) {
                b = m; // La raíz está en [a, m]
                fb = fm;
            } else {
                a = m; // La raíz está en [m, b]
                fa = fm; // Actualizar f(a) para la siguiente iteración
            }

            prevm = m; // Actualizar valor anterior de m

            series.add(a, fa);// Gráfica
            series.add(b, fb);// Gráfica
        }

        return iterador;
    }
```

### Método de secante
En este obtenemos nuestro valor de Xi de manera bastante similar al de falsa posición pero aquí los criterios de como cambiar nuestros datos es diferente, ya que en cada iteración a tomará el valor de b, y b el valor de nuestra Xi.

```
    public static List<String> secante(String funcion, double a, double b, double ans, XYSeries series) {
        List<String> iterador = new ArrayList<>();

        double fa = evaluarFuncion(funcion, a);
        double fb = evaluarFuncion(funcion, b);
        
        double m = a;
        double error = Double.MAX_VALUE;
        int i = 0;
        double prevm = m;

        while (error > ans && i < 100) {
            i++;
            m = b-((fb*( a - b)) / (fa - fb));
            double fm = evaluarFuncion(funcion, m);

            if (Double.isNaN(fm)) {
                JOptionPane.showMessageDialog(null,
                    "Error al evaluar la función.",
                    "Error",JOptionPane.ERROR_MESSAGE);
                break;
            }

            error = Math.abs(m - prevm);  // Error 
            iterador.add(String.format("%-4d %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f %-10.6f", 
                i, a, fa, b, fb, m, fm, error));

            if (fm == 0) {
                break; // c es la raíz exacta
            } else {
                a = b; // La raíz está en [m, b]
                fa = fb; // Actualizar f(a) para la siguiente iteración
                b = m;
                fb = fm;
            }

            prevm = m; // Actualizar valor anterior de m

            series.add(a, fa);// Gráfica
            series.add(b, fb);// Gráfica
        }

        return iterador;
    }
```

## Créditos :star:

- ### Cortes Aguilar Aaron
- ### Macias Caracosa Dante
- ### Barrera Reyes David
- ### Barriga Morales Jesús Bladimir
- ### Alejos Soria Salvador