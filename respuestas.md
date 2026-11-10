## 2. Conocer la aplicación

**Preguntas:** ¿qué archivo contiene la regla de cálculo? ¿Qué archivo define la ruta HTTP? ¿En qué se diferencian sus responsabilidades?
 El archivo con las regla de calculo es precios.ts, el que define las rutas es app.ts.

## 3. Ejecutar las pruebas disponibles

Leé la prueba que contiene `calcularPrecioFinal(1000, 150)`. ¿Qué comportamiento exige? ¿Qué hace realmente el código? **Todavía no modifiques la implementación.**
 La prueba exige que ocurra un error por el rango de descuento invalido, el codigo por su parte no realiza ninguna validacion sobre el porcentaje de descuento


## 4. Interpretar la falla

 1. ¿Qué total devuelve actualmente un descuento del 150 % sobre $1.000?
 2. ¿Por qué ese resultado no cumple la regla de negocio?
 3. ¿Por qué la prueba falla si la función terminó de ejecutarse?

1- Devuelve un total de -500
2- El resutado no cumple con la regla de negocio porque la funcion que se encarga de calcular los descuentos no realiza validaciones sobre el porcentaje de descuento
3- la prueba falla porque se espera un error al sobrepasar el 100% de descuento