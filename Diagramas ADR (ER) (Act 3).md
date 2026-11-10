# Diagramas de Mermaid de Express Restaurante.

Este documento tiene los 8 diagramas de Mermaid pero con la información del sistema de Express Restaurante.

---

## Diagrama 1. Diagrama de contexto

```mermaid
flowchart TD
    mes([Mesero])
    coc([Personal de Cocina])
    caj([Encargado de Caja])
    sis["Sistema POS/KDS Express"]
    imp["Impresora térmica de caja"]

    mes -->|Captura y envía comanda| sis
    coc -->|Actualiza estado de platillos| sis
    caj -->|Solicita cierre y cobra| sis
    sis -->|Envía comprobante de cobro| imp

    style sis fill:#ff5d00,color:#fff
```

---

## Diagrama 2. Diagrama de contenedores

```mermaid
flowchart TD
    mes([Mesero / Operador])

    subgraph sistema["Sistema POS/KDS Express"]
        web["App web / Tablet<br/>React"]
        api["API del Sistema<br/>Node.js"]
        db[("Base de datos local<br/>PostgreSQL")]

        web -->|HTTPS / JSON| api
        api -->|Lee y escribe| db
    end

    imp["Impresora térmica"]

    mes -->|Usa interfaz táctil| web
    api -->|Comando de impresión| imp

    style api fill:#ff5d00,color:#fff
```

---

## Diagrama 3. Diagrama de componentes

```mermaid
flowchart TD
    web["App web (Tablet / KDS / Caja)"]

    subgraph api["API y Módulos del Sistema"]
        ordenes["Módulo de Órdenes<br/>Mesas y platillos"]
        cocina["Módulo de Cocina<br/>Pantalla KDS"]
        caja["Módulo de Facturación<br/>Cuentas e impuestos (IVA 16%)"]
    end

    db[("Base de datos")]
    impresora["Impresora térmica"]

    web --> ordenes
    web --> cocina
    web --> caja

    ordenes -->|Envía comanda| cocina
    caja -->|Valida estado de platillos| ordenes
    caja -->|Envía ticket de cobro| impresora

    ordenes --> db
    cocina --> db
    caja --> db

    style caja fill:#ff5d00,color:#fff
```

---

## Diagrama 4. Diagrama de flujo

```mermaid
flowchart TD
    ini([Inicio de Cierre])
    val{"¿Existen platillos pendientes en cocina?"}
    err["Bloquear emisión: Platillos pendientes"]
    calc["Calcular subtotal, IVA (16%) y total"]
    imp["Imprimir ticket en caja"]
    fin([Fin])

    ini --> val

    val -->|Sí| err
    val -->|No| calc

    err --> fin
    calc --> imp
    imp --> fin

    style calc fill:#ff5d00,color:#fff
```

---

## Diagrama 5. Diagrama de secuencia

```mermaid
sequenceDiagram
    actor Caj as Encargado de Caja
    participant Web as App web (Caja)
    participant CajaMod as Módulo Facturación
    participant OrdMod as Módulo Órdenes
    participant BD as Base de datos

    Caj->>Web: Solicita cierre de mesa
    Web->>CajaMod: cerrarMesa(mesaId)
    CajaMod->>OrdMod: Validar estado de platillos
    OrdMod-->>CajaMod: Todos los platillos listos
    CajaMod->>BD: Consultar consumos y calcular IVA (16%)
    BD-->>CajaMod: Desglose financiero obtenido
    CajaMod-->>Web: Muestra cuenta exacta
    Web-->>Caj: Imprime ticket en impresora térmica
```

---

## Diagrama 6. Diagrama de clases

```mermaid
classDiagram
    class Mesa {
        -int id
        -int numero
        +consultarEstado() string
    }

    class Orden {
        -int id
        -string horaIngreso
        +enviarComanda()
    }

    class Platillo {
        -string nombre
        -string especificaciones
        -string estado
        +actualizarEstado(nuevoEstado)
    }

    class Cuenta {
        -decimal subtotal
        -decimal iva
        -decimal total
        +calcularTotal() decimal
        +validarPendientes() bool
    }

    class Ticket {
        -int id
        -date fecha
        +imprimir()
    }

    Mesa "1" --> "1" Cuenta : tiene
    Mesa "1" --> "*" Orden : genera
    Orden "1" --> "*" Platillo : contiene
    Cuenta "1" --> "1" Ticket : emite
```

---

## Diagrama 7. Diagrama de estados

```mermaid
stateDiagram-v2
    [*] --> Recibida : Mesero presiona Enviar

    Recibida --> EnPreparacion : Cocina inicia platillo
    EnPreparacion --> Listo : Cocina termina platillo

    Listo --> Entregado : Platillo servido al comensal
    Entregado --> [*]
```

---

## Diagrama 8. Diagrama entidad-relación

```mermaid
erDiagram
    MESA ||--o{ ORDEN : genera
    MESA ||--|| CUENTA : tiene
    ORDEN ||--|{ PLATILLO : incluye
    CUENTA ||--|| TICKET : emite

    MESA {
        int id PK
        int numero
        string estado
    }

    ORDEN {
        int id PK
        int mesa_id FK
        datetime hora_ingreso
    }

    PLATILLO {
        int id PK
        int orden_id FK
        string nombre
        string especificaciones
        string estado
    }

    CUENTA {
        int id PK
        int mesa_id FK
        decimal subtotal
        decimal iva
        decimal total
    }

    TICKET {
        int id PK
        int cuenta_id FK
        datetime fecha_emision
    }
```

---

