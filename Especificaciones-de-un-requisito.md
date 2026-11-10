# Especificación de requisitos

> **Plantilla del curso · Ingeniería de Software I · ISIS3407**

---

**Sistema:** POS/KDS Express Restaurante (Sistema de Punto de Venta y Pantallas Digitales de Cocina)  
**Autor:** Oliver Sebastián Albores Ramírez
**Fecha de la última actualización:** 1 de Octubre de 2026  
**Repositorio:** `docs/especificacion-requisitos.md`  

---

## 1. Propósito y alcance

**Propósito del documento:**
El propósito de este documento es definir y especificar de manera formal los requisitos funcionales y no funcionales del sistema POS/KDS Express Restaurante. Sirve como guía de desarrollo, diseño de prototipos, pruebas y validación del sistema, asegurando la alineación entre las necesidades operativas del restaurante (meseros, cocineros y cajeros) y el software a entregar.

**Alcance del sistema:**
- **Transmisión rápida de comandas:** Envío de datos de órdenes desde la tableta del mesero hacia la pantalla digital de cocina (KDS) en menos de 2 segundos.
- **Despliegue claro de especificaciones de preparación:** Visualización en la pantalla KDS del número de mesa, platillos solicitados, hora de ingreso y especificaciones/modificaciones especiales (ej. "sin cebolla", "término medio").
- **Cálculo automático e inmediato de la cuenta:** Cómputo automático del subtotal neto, los impuestos aplicables (IVA 16%) y el total definitivo a pagar al solicitar el cierre de una mesa.
- **Gestión operativa del estado del pedido:** Permite al personal de cocina cambiar el estado de cada platillo en la pantalla KDS entre "En preparación" y "Listo".
- **Generación e impresión de comprobantes de consumo:** Envío de la orden de cobro a la impresora térmica de caja para emitir el ticket físico final para el cliente.

**Fuera del alcance:**
- **Procesamiento de pagos electrónicos ni cobros bancarios:** No se conecta a terminales TPV (PIN pads) ni procesa cobros con tarjetas de crédito/débito directamente en la aplicación.
- **Control y descuento automático de inventarios por insumos:** No lleva el control de stock en almacén ni resta gramos o piezas de ingredientes al enviar una orden.
- **Recepción de reservaciones en línea de clientes:** No cuenta con portal web ni aplicación para que los comensales agenden mesas con anticipación.

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
| --- | --- | --- |
| Mesero | Anota las órdenes a mano en libretas de papel, camina físicamente hasta la cocina para entregar la comanda, confía en el recuerdo o notas al margen para especificaciones especiales, y suma manualmente la cuenta con una calculadora física. | Capturar pedidos de forma rápida e intuitiva desde una tableta táctil, registrar especificaciones especiales con facilidad y generar la cuenta exacta al instante sin cálculos manuales. |
| Personal de Cocina (Cocinero/Chef) | Recibe comandas de papel (frecuentemente ilegibles) entregadas por los meseros y gestiona mentalmente los tiempos de preparación sin control sistematizado. | Visualizar en la pantalla KDS las órdenes de manera clara, priorizadas por número de mesa y hora de ingreso, con notas de preparación legibles y cambio simple de estado ("En preparación", "Listo"). |
| Encargado de Caja / Cajero | Recibe notas de consumo manuscritas, realiza la suma manual de importes en calculadora y redacta o emite el comprobante de cobro. | Recibir automáticamente el desglose de la cuenta con impuestos e imprimir de inmediato el comprobante final en la impresora térmica de caja. |

**Conflictos identificados entre usuarios:**
El mesero prefiere una interfaz en la tableta con múltiples botones, menús desplegables y campos libres para anotar todo tipo de peticiones personalizadas del cliente. Sin embargo, si el mesero envía comandas sobrecargadas de texto no estandarizado, la pantalla de cocina (KDS) se satura de información densa, haciendo que los cocineros pierdan tiempo leyendo párrafos largos y aumenten el riesgo de cometer errores bajo presión.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
| --- | --- | --- | --- |
| RF-001 | Transmisión de comanda a pantalla KDS | Imprescindible | Visión del Producto / Entrevista Operativa |
| RF-002 | Despliegue de orden y especificaciones en KDS | Imprescindible | Visión del Producto / Entrevista Operativa |
| RF-003 | Cálculo automático de cuenta e impuestos | Imprescindible | Regla de Negocio 2 / Visión del Producto |
| RF-004 | Cambio de estado de platillos en KDS | Imprescindible | Visión del Producto / Entrevista Operativa |
| RF-005 | Bloqueo de emisión de ticket con platillos pendientes | Imprescindible | Regla de Negocio 3 / Visión del Producto |

### 3.2 Fichas

#### RF-001. Transmisión de comanda a pantalla KDS

| Campo | Contenido |
| --- | --- |
| **Descripción** | El sistema transmite la comanda capturada desde la tableta del mesero hacia la pantalla de cocina (KDS) de forma inmediata al presionar el botón de envío. |
| **Origen** | Visión del producto (Sección 3: Alcance dentro del sistema) y validación operativa. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al capturar una orden en la tableta y presionar el botón "Enviar", se verifica que la orden aparezca en el listado activo de la pantalla KDS en menos de 2 segundos. |
| **Relacionado con** | RF-002, RNF-REN-001, RNF-CON-001 |

#### RF-002. Despliegue de orden y especificaciones en KDS

| Campo | Contenido |
| --- | --- |
| **Descripción** | El sistema despliega en la pantalla KDS el número de mesa, los platillos solicitados, la hora de ingreso y las modificaciones especiales de la orden. |
| **Origen** | Visión del producto (Sección 3: Alcance dentro del sistema) y entrevistas con cocineros. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al registrar una orden para la Mesa 4 con el detalle "sin cebolla", se comprueba que la pantalla KDS proyecte la Mesa 4, la hora exacta de ingreso, el platillo y la etiqueta visible "sin cebolla" en la tarjeta de la orden. |
| **Relacionado con** | RF-001, RF-004, RNF-USA-001 |

#### RF-003. Cálculo automático de cuenta e impuestos

| Campo | Contenido |
| --- | --- |
| **Descripción** | El sistema calcula automáticamente el subtotal neto, los impuestos aplicables (IVA) y el monto total exacto a pagar al solicitar el cierre de una mesa. |
| **Origen** | Regla de negocio 2 del restaurante y Visión del producto. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al solicitar la cuenta de una mesa con un consumo subtotal neto de $100.00, se verifica que el sistema desglose $16.00 de IVA (16%) y calcule un total exacto a pagar de $116.00. |
| **Relacionado con** | RF-005, RNF-CON-001 |

#### RF-004. Cambio de estado de platillos en KDS

| Campo | Contenido |
| --- | --- |
| **Descripción** | El sistema permite al personal de cocina cambiar el estado operativo de cada platillo en la pantalla entre "En preparación" y "Listo". |
| **Origen** | Visión del producto (Sección 3: Alcance dentro del sistema) y consulta con el equipo de cocina. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al presionar o tocar la tarjeta de un platillo en la pantalla KDS, se comprueba que su etiqueta de estado cambie de forma visible e inmediata entre "En preparación" y "Listo". |
| **Relacionado con** | RF-002, RF-005, RNF-USA-001 |

#### RF-005. Bloqueo de emisión de ticket con platillos pendientes

| Campo | Contenido |
| --- | --- |
| **Descripción** | El sistema impide emitir el ticket de cobro definitivo de una mesa si existen platillos registrados en estado pendiente que no hayan sido procesados en la pantalla de cocina. |
| **Origen** | Regla de negocio 3 del restaurante. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al intentar generar el ticket de cierre de una mesa cuyo pedido contenga platillos en estado "En preparación" o pendiente, el sistema bloquea la acción y muestra un mensaje de advertencia en caja. |
| **Relacionado con** | RF-003, RF-004 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
| --- | --- | --- | --- | --- |
| RNF-REN-001 | Rendimiento | Tiempo de transmisión y despliegue de comanda | Imprescindible | Visión del Producto |
| RNF-USA-001 | Usabilidad | Facilidad de captura y tiempo de aprendizaje | Imprescindible | Visión del Producto |
| RNF-CON-001 | Confiabilidad | Disponibilidad local y sincronización continua | Imprescindible | Visión del Producto |

### 4.2 Fichas

#### RNF-REN-001. Tiempo de transmisión y despliegue de comanda

| Campo | Contenido |
| --- | --- |
| **Atributo de calidad** | Rendimiento / Latencia |
| **Descripción** | El sistema despliega la comanda en la pantalla KDS en menos de 2 segundos tras ser enviada desde la tableta del mesero. |
| **Métrica** | Tiempo transcurrido medido con cronómetro desde que se presiona "Enviar" en la tableta hasta la aparición visual de la comanda en el KDS (debe ser < 2.0 segundos). |
| **Origen** | Derivado del tipo de sistema: operativo en tiempo real local con alto tráfico de comensales. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | La cocina requiere recibir la orden al instante. Si hay demoras en la red local, se retrasa la preparación de alimentos y la comida llega fría al cliente. |
| **Afecta a** | RF-001, RF-002 |

#### RNF-USA-001. Facilidad de captura y tiempo de aprendizaje

| Campo | Contenido |
| --- | --- |
| **Atributo de calidad** | Usabilidad / Facilidad de uso |
| **Descripción** | Un mesero sin capacitación previa completa la captura y envío de una comanda de tres platillos con especificaciones especiales en menos de 90 segundos. |
| **Métrica** | Tiempo de interacción medido desde que un usuario novel selecciona la mesa hasta que transmite la comanda completa (debe ser < 90 segundos). |
| **Origen** | Requisito de uso operativo en momentos de alta afluencia en el restaurante. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Si la interfaz de la tableta es compleja o confusa, el mesero tardará más en registrar la orden que escribiendo en papel, aumentando las demoras en el servicio. |
| **Afecta a** | RF-001, RF-002 |

#### RNF-CON-001. Disponibilidad local y sincronización continua

| Campo | Contenido |
| --- | --- |
| **Atributo de calidad** | Confiabilidad / Disponibilidad local |
| **Descripción** | El sistema mantiene la sincronización de órdenes entre tabletas, KDS y caja en la red local sin caídas ni pérdida de datos durante un turno operativo continuo de 12 horas. |
| **Métrica** | Cero caídas de conexión local y 100% de conservación de comandas transmitidas durante una prueba continua de 12 horas de operación. |
| **Origen** | Atributo de calidad indispensable para el correcto funcionamiento del restaurante. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Una falla o pérdida de red paraliza la toma de comandas, impide saber qué se está cocinando y bloquea la emisión de cuentas en caja. |
| **Afecta a** | RF-001, RF-003, RF-005 |

---

## 5. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
| --- | --- | --- | --- |
| **RF-001** | Visión del producto / Entrevista | CU-01 Tomar y enviar comanda | Botón "Enviar Comanda" en interfaz de tableta |
| **RF-002** | Visión del producto / Entrevista | CU-01, CU-02 | Tarjeta de orden en pantalla KDS de cocina |
| **RF-003** | Regla de Negocio 2 / Visión | CU-03 Cerrar mesa y emitir ticket | Pantalla de resumen de cuenta y desglose en caja |
| **RF-004** | Visión del producto / Entrevista | CU-02 Gestionar estado en cocina | Botón/Etiqueta de estado ("En preparación" / "Listo") en KDS |
| **RF-005** | Regla de Negocio 3 / Visión | CU-03 Cerrar mesa y emitir ticket | Alerta de bloqueo/Validación previa a impresión de ticket |
| **RNF-REN-001** | Atributo de Calidad (Rendimiento) | CU-01, CU-02 | Módulo de comunicación en red local (Socket/WebSockets) |
| **RNF-USA-001** | Atributo de Calidad (Usabilidad) | CU-01 Tomar y enviar comanda | Menú táctil simplificado de tablet con botones amplios |
| **RNF-CON-001** | Atributo de Calidad (Confiabilidad) | CU-01, CU-03 | Mecanismo de persistencia local y reintento de conexión |

---

## 6. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
| --- | --- | --- | --- |
| 01/10/2026 | Todos | Creación de la versión inicial (v1.0) de la especificación de requisitos. | Entrega correspondiente al avance del proyecto con prototipado rápido. |
