# Visión del producto

> **Plantilla del curso · Ingeniería de Software I · SIS3407**

---

**Autor:** Oliver Sebastián Albores Ramírez

**Fecha de la última versión:** 29 de Agosto de 2026

**Repositorio:** `docs/vision-del-producto.md`

---

## 1. Descripción del sistema

**Nombre del sistema:** POS/KDS Express Restaurante (Sistema de Punto de Venta y Pantallas Digitales de Cocina)

**Descripción:**
Es una solución tecnológica para restaurantes de alto volumen que digitaliza la toma y preparación de pedidos. Permite a los meseros ingresar las órdenes desde una tableta táctil y enviarlas de inmediato a pantallas digitales en la cocina. El sistema organiza los pedidos por número de mesa, muestra especificaciones exactas de preparación y calcula automáticamente la cuenta total para agilizar el cobro y acortar los tiempos de espera de los clientes.

---

## 2. Problema y usuarios

**El problema:**
En horas pico, la comida llega fría a las mesas, la cocina comete errores frecuentes preparando platillos equivocados y la rotación de mesas es muy lenta porque los meseros tardan en cobrar. Las comandas tomadas en libretas de papel causan confusión por letra ilegible o por no registrar ingredientes extra, mientras que el cálculo manual de la cuenta en calculadora genera demoras y posibles equivocaciones.

**Cómo se resuelve hoy sin el sistema:**
Los meseros anotan las órdenes a mano en libretas de papel y caminan hasta la cocina para entregar físicamente la comanda. Si hay especificaciones o ingredientes extra, confían en recordarlos verbalmente o los anotan al margen de la hoja. Al momento del cobro, el mesero o cajero suma manualmente cada importe utilizando una calculadora física para obtener la cuenta total.

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
| Mesero | Tomar pedidos de forma rápida e intuitiva en la tablet, registrar modificaciones especiales fácilmente y generar la cuenta exacta al instante sin cálculos manuales. | Que la interfaz de la tablet sea compleja o lenta durante momentos de alta afluencia, o que las notas especiales del cliente no se registren correctamente. |
| Personal de Cocina (Cocinero/Chef) | Visualizar en la pantalla KDS las órdenes de manera clara, ordenada por mesa y tiempo, con notas de preparación legibles y actualización simple de estado ("En preparación", "Listo"). | Que la pantalla KDS presente un diseño saturado o confuso que dificulte la lectura rápida a distancia durante la preparación de alimentos. |
| Encargado de Caja / Cajero | Recibir automáticamente el desglose de la cuenta con impuestos e imprimir de inmediato el comprobante final en la impresora térmica de caja. | Que existan discrepancias entre los platillos servidos y la cuenta cobrada, o que el proceso de impresión presente demoras que ralenticen la liberación de mesas. |

**Un conflicto entre usuarios:**
El mesero prefiere una interfaz en la tablet con múltiples botones, menús desplegables y campos libres para anotar todo tipo de peticiones personalizadas del cliente. Sin embargo, si el mesero envía comandas sobrecargadas de texto no estandarizado, la pantalla de cocina (KDS) se satura de información densa, haciendo que los cocineros pierdan tiempo leyendo párrafos largos y aumenten el riesgo de cometer errores bajo presión.

---

## 3. Alcance

### Dentro del alcance

- **Transmisión rápida de comandas:** Envío de los datos de la orden desde la tablet del mesero a la pantalla de la cocina en menos de 2 segundos tras presionar "Enviar".
- **Despliegue claro de especificaciones de preparación:** Visualización en la pantalla KDS del número de mesa, platillos solicitados, hora de ingreso y modificaciones especiales (ej. "sin cebolla", "término medio").
- **Cálculo automático e inmediato de la cuenta:** Cómputo automático del subtotal, los impuestos aplicables y el total exacto a pagar al solicitar el cierre de la mesa.
- **Gestión operativa del estado del pedido:** Permitir al personal de cocina cambiar el estado de cada platillo en la pantalla entre "En preparación" y "Listo".
- **Generación e impresión de comprobantes de consumo:** Envío de la orden de cobro a la impresora térmica de caja para emitir el ticket físico para el cliente.

### Explícitamente fuera del alcance

- **Procesamiento de pagos electrónicos ni cobros bancarios:** No se conecta a terminales TPV (PIN pads) ni procesa cobros con tarjetas de crédito/débito directamente en la aplicación.
- **Control y descuento automático de inventarios por insumos:** No lleva el control de stock en almacén ni resta gramos o piezas de ingredientes al enviar una orden.
- **Recepción de reservaciones en línea de clientes:** No cuenta con portal web ni aplicación para que los comensales agenden mesas con anticipación.

**Por qué queda fuera:**
Integrar la deducción automática de insumos exige construir un motor de recetas estructuradas, control de mermas y multialmacenes. Esta complejidad excede el objetivo central del proyecto, el cual está enfocado exclusivamente en maximizar la velocidad del flujo operativo entre la comanda del mesero, la visualización en cocina y la emisión de la cuenta en caja.

---

## 4. Tipo de sistema y restricciones

**Tipo de sistema:**
Sistema de información (operativo y transaccional en tiempo real local)

**Por qué es de ese tipo:**
Gestiona la captura, almacenamiento local, procesamiento y visualización de datos operativos (órdenes de servicio, estados de preparación de cocina y montos a cobrar). Su propósito principal es coordinar el flujo de información en tiempo real entre meseros, cocineros y caja dentro de la red local del restaurante.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| Desempeño / Latencia | La comanda debe reflejarse en la pantalla KDS en menos de 2 segundos tras ser enviada desde la tablet. | Si el envío tarda, la cocina no inicia la preparación a tiempo, perpetuando el problema de comida fría y lentitud. |
| Usabilidad / Facilidad de uso | La interfaz de la tablet y del KDS debe ser clara, de botones amplios y lectura sencilla a distancia. | Los meseros tardarán más en capturar que escribiendo en papel y la cocina se confundirá al leer las pantallas en horas pico. |
| Confiabilidad / Disponibilidad local | El sistema debe mantenerse operativo y sincronizado sin caídas durante todo el turno de trabajo del restaurante. | Una falla en el sistema paraliza la toma de comandas, impide saber qué se está preparando y bloquea el cobro en caja. |

**Reglas de negocio que ya identifiqué:**

1. **Inmutabilidad de la orden enviada a cocina:** Una vez presionado el botón "Enviar", cualquier modificación o cancelación de un platillo debe requerir confirmación explícita para evitar descuadres entre lo que cocina está preparando y lo que figura en la cuenta.
2. **Desglose obligatorio de impuestos en la cuenta total:** El cálculo de la cuenta debe separar automáticamente el subtotal neto de los impuestos aplicables antes de generar el monto total definitivo en el ticket térmico.
3. **Cierre de mesa vinculado al estado de la orden:** No se puede emitir el ticket de cobro definitivo de una mesa si existen platillos pendientes que no hayan pasado por el flujo de preparación ("En preparación" / "Listo") en la pantalla de cocina.

---

## 5. Ciclo de vida elegido

**Modelo elegido:**
Prototipado Rápido

**Por qué le conviene a este proyecto:**
La interfaz de usuario de las tabletas y la pantalla KDS es el factor crítico del sistema. Dado que el personal del restaurante reconoce más fácilmente las fallas de flujo cuando interactúa con un modelo tangible que mediante descripciones abstractas, la creación de prototipos provisionales permite afinar los requisitos reales antes de construir la versión definitiva del software. La disponibilidad semanal del personal operativo al final de los turnos facilita probar iteraciones cortas y obtener retroalimentación directa.

### Alternativas descartadas

**Alternativa 1:** Modelo en Cascada

*Por qué la descarté:* Exige fijar los requisitos de manera definitiva al inicio del proyecto. En este POS/KDS, definir la interfaz sin validación en cocina provocaría la entrega de un sistema rígido que, al llegar a las mesas, ralentizaría el cobro o la preparación por errores de diseño no previstos.

**Alternativa 2:** Modelo en Espiral

*Por qué la descarté:* Centra sus iteraciones principalmente en la evaluación y mitigación de riesgos técnicos e arquitectónicos complejos. Dado que la infraestructura del POS (envío de datos en red local y cálculo de totales) es de bajo riesgo técnico, el sobrecoste metodológico de la Espiral resulta innecesario para un problema enfocado primordialmente en la usabilidad del cliente.
