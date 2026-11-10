# Descripción del sistema:
Un Sistema de Punto de Venta (POS) con Pantallas de Cocina (KDS).

# Problema " Restaurante de Alto Volumen "

## Lo que sucedio; 
La comida llega fría a las mesas, la cocina prepara platillos equivocados
frecuentemente y la rotación de mesas es muy lenta porque los meseros tardan en cobrar.

## El problema central; 
Los meseros toman la orden en libretas de papel y caminan hasta la cocina
para entregar la comanda. La letra ilegible causa errores, y si el mesero
olvida un ingrediente extra, no hay registro. Además, el cobro se hace 
calculando la cuenta en una calculadora.

## Lo que se tiene que hacer; 
Un Sistema de Punto de Venta (POS) con Pantallas de Cocina (KDS). Un software 
donde el mesero tome la orden en una tablet; al presionar "Enviar", la orden
aparecerá inmediatamente en una pantalla digital en la cocina, detallando las
especificaciones exactas. Al final, el sistema generará la cuenta automática y 
precisa al instante.

# Detalles del sistema.

## 1. Lo que el sistema hace actualmente:

Transmitir comandas: Envía los datos de la orden desde la tablet del mesero a 
la pantalla de cocina en menos de 2 segundos tras presionar "Enviar".

Desplegar especificaciones de preparación: Muestra en la pantalla KDS el número
de mesa, platillos solicitados, hora de ingreso y modificaciones especiales 
(ej. "sin cebolla", "término medio").

Calcular la cuenta total: Computa de forma automática el subtotal, los impuestos
aplicables y el total exacto a pagar al momento de solicitar el cierre de mesa.

Actualizar el estado del pedido: Permite al personal de cocina modificar en pantalla 
el estado operativo de cada platillo ("En preparación", "Listo").

Generar e imprimir el comprobante: Envía la orden de cobro final a la impresora 
térmica de caja para emitir el ticket de consumo del cliente.

## 2. Cosas que el sistema no puede hacer:

No procesa pagos electrónicos ni cobros bancarios: No se conecta a terminales 
TPV (PIN pads) ni procesa cobros con tarjeta de crédito/débito directamente en
la aplicación.

No descuenta inventario por insumos o ingredientes: No lleva el control de stock
en almacén ni resta gramos/piezas de ingredientes al enviar una orden.

No recibe reservaciones en línea de clientes: No cuenta con portal web ni 
aplicación para que los comensales agenden mesas con anticipación.

## 3. Justificación de exclusión (Control de inventarios por insumos):

Integrar la deducción automática de insumos exige construir un motor de recetas 
estructuradas, control de mermas y multialmacenes. Esta complejidad excede el objetivo
central del proyecto, que está enfocado exclusivamente en la velocidad del flujo 
operativo entre la comanda del mesero, la visualización en cocina y la emisión 
de la cuenta en caja.

## 4. Idea futura para una primera actualización:
   
Notificaciones en reloj inteligente (Smartwatch) para meseros: Enviar una alerta 
con vibración al smartwatch del mesero responsable únicamente en el momento exacto 
en que la cocina toque la pantalla KDS y marque el platillo como "Listo".


# Selección del Modelo de Ciclo de Vida del Software

## 1. Caracterización del Proyecto POS/KDS

| **¿Qué tan estable es el alcance?** | Variable. Aunque las 5 funciones principales están identificadas (transmisión de comandas, visualización en KDS, cálculo de cuenta, actualización de estado e impresión), la forma exacta en que los meseros interactúan con la Tablet y cómo los cocineros leen los tickets digitales bajo presión cambiará al probar la interfaz en la operación real.|

| **¿Disponibilidad del usuario final?** | Alta disponibilidad (frecuencia 
semanal). El personal operativo (meseros, cocineros y encargado de caja) se
encuentra en el sitio de trabajo y puede probar iteraciones cortas. Es posible 
realizar revisiones semanales al final de los turnos para observar cómo interactúan
con las pantallas provisionales y obtener retroalimentación directa.|

| **¿Mayor riesgo identificado?** | No entender exactamente lo que se necesita
a nivel de usabilidad (UI/UX). La complejidad técnica de enviar una orden por 
red local o sumar un ticket es baja; el verdadero riesgo es diseñar una pantalla 
de cocina saturada que confunda al cocinero en hora pico o un menú en Tablet 
que tarde más en navegarse que escribir en papel. |

| **¿Requiere auditoría o certificación?** | No requiere auditorías externas. 
El sistema no procesa transacciones bancarias directas (las TPV/tarjetas están 
fuera de alcance) ni maneja datos fiscales normados o auditorías de inventario
legal; se limita a la operación interna del restaurante y comprobantes locales. |

## 2. Justificación de Prototipado Rápido

Se selecciona el modelo de **Prototipado Rápido** debido a que la interfaz 
de usuario de las tabletas y la pantalla KDS es el factor crítico del sistema.
Dado que el personal del restaurante reconoce más fácilmente las fallas de 
flujo cuando interactúa con un modelo tangible que mediante descripciones 
abstractas, la creación de prototipos provisionales permite afinar los 
requisitos reales antes de construir la versión definitiva del software.

## 3. Modelos Descartados

* **Modelo en Cascada:** Exige fijar los requisitos de manera definitiva al
inicio del proyecto. En este POS/KDS, definir la interfaz sin validación
en cocina provocaría la entrega de un sistema rígido que, al llegar a las
mesas, ralentizaría el cobro o la preparación por errores de diseño no previstos.
  
* **Modelo en Espiral:** Centra sus iteraciones principalmente en la evaluación
 y mitigación de riesgos técnicos y arquitectónicos complejos. Dado que la
infraestructura del POS (envío de datos y el total o totales calculados) es de
bajo riesgo técnico, el sobrecoste metodológico de la Espiral resulta innecesario
para un problema enfocado primordialmente en la usabilidad del cliente.
