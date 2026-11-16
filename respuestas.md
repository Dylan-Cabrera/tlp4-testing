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

## 5. Corregir la función de cálculo

 Se modificó solo precios.ts: antes de calcular se valida que el precio sea un número finito mayor o igual a 0 y que el descuento sea un número finito entre 0 y 100, si no se cumple se lanza un RangeError. La fórmula para entradas válidas se conservó. Ahora pasan las pruebas.

## 6. Agregar pruebas unitarias

 Se agregaron tres pruebas: descuento del 100 % devuelve 0, precio -100 lanza RangeError y descuento -10 lanza RangeError.
 Desafío adicional: se agregó una prueba con NaN como precio y la validación actual lo rechaza, porque Number.isFinite(NaN) es false.

## 7. Primera prueba de integración HTTP

 Se agregó la prueba que envía descuento 150 y espera status 400 y un objeto con la propiedad error.
 Es de integración porque participan la ruta de Express (app.ts) y el servicio de cálculo (precios.ts) juntos: la ruta recibe el JSON, llama al servicio, captura el RangeError y lo convierte en una respuesta 400.

## 8. Diferenciar pruebas y verificación de tipos

 typecheck analiza los tipos sin ejecutar el código y npm test ejecuta el código y comprueba su comportamiento. Conviene usar ambos porque detectan clases distintas de problemas: Node ejecuta los .ts sin comprobar tipos, y los tipos correctos no garantizan que se cumpla la regla de negocio.

## 9. Integración continua

 El workflow pruebas.yml descarga el repositorio, prepara Node, instala dependencias, ejecuta typecheck y ejecuta las pruebas. Como package-lock.json ya existe, se cambió npm install por npm ci para tener instalaciones reproducibles.

## 10. Registro de resultados

| Comprobación | Resultado | Observación |
| --- | --- | --- |
| Respuesta de `GET /healthy` | Ruta no encontrada | En app.ts la ruta definida es `/salud`, que responde `{"estado":"ok"}` |
| Primera ejecución de `npm test` | 4 pasan, 1 falla | Falla la prueba del descuento del 150 % |
| Corrección del servicio | Correcto | Se agregaron validaciones con RangeError |
| Nuevas pruebas unitarias | Correcto | 7 pruebas en precios.test.ts pasan |
| Prueba de integración HTTP 400 | Correcto | 3 pruebas en api.test.ts pasan |
| Verificación `npm run typecheck` | Correcto | Sin errores de tipos |

## 11. Preguntas de cierre

1. La prueba manual la hace una persona cada vez y depende de su criterio, la automatizada está escrita en código, se ejecuta con un comando y da resultados repetibles.
2. La función calcularPrecioFinal no validaba el porcentaje de descuento, por lo que con 150 % devolvía -500 en lugar de lanzar un RangeError.
3. Porque verifican que el sistema rechace entradas que incumplen las reglas de negocio, y no solamente que funcione con los datos esperados.
4. precios.test.ts comprueba la función calcularPrecioFinal de forma aislada, api.test.ts comprueba la ruta HTTP de Express junto con el servicio de cálculo.
5. Permiten detectar rápidamente regresiones, es decir, fallas introducidas al modificar algo que antes funcionaba.
