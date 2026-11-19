# Diagramas de Mermaid

Este documento contiene ocho diagramas elaborados con Mermaid para representar el funcionamiento, la estructura y los procesos de un sistema de gestión de una clínica veterinaria.

---

## Ejemplo: Tipos de flecha

```mermaid
flowchart TD
    a1[A] --> b1[B]
    a2[A] --- b2[B]
    a3[A] -.-> b3[B]
    a4[A] ==> b4[B]
    a5[A] -->|etiqueta| b5[B]
```

---

## Diagrama 1. Diagrama de contexto

Representa las interacciones entre los usuarios, el sistema de la clínica y los servicios externos.

```mermaid
flowchart TD
    rec([Recepcionista])
    vet([Veterinario])
    sis["Sistema de la clínica"]
    lab["Laboratorio externo"]
    pago["Pasarela de pagos"]

    rec -->|Agenda y cobra| sis
    vet -->|Consulta expedientes| sis
    sis -->|Envía estudios| lab
    sis -->|Cobra consultas| pago

    style sis fill:#ff5d00,color:#fff
```

---

## Diagrama 2. Diagrama de contenedores

Muestra los principales componentes tecnológicos del sistema y la comunicación entre ellos.

```mermaid
flowchart TD
    rec([Recepcionista])

    subgraph sistema["Sistema de la clínica"]
        web["App web<br/>React"]
        api["API<br/>Node.js"]
        db[("Base de datos<br/>PostgreSQL")]

        web -->|HTTPS / JSON| api
        api -->|Lee y escribe| db
    end

    lab["Laboratorio externo"]

    rec -->|Usa| web
    api -->|SFTP| lab

    style api fill:#ff5d00,color:#fff
```

---

## Diagrama 3. Diagrama de componentes

Representa los componentes funcionales de la API y sus relaciones con la aplicación web, la base de datos y el servicio de correo.

```mermaid
flowchart TD
    web["App web"]

    subgraph api["API del sistema"]
        agenda["Agenda<br/>Citas y horarios"]
        cobros["Cobros<br/>Cuentas y pagos"]
        exp["Expedientes<br/>Historial clínico"]
        notif["Notificador<br/>Interfaz de envío"]
    end

    db[("Base de datos")]
    correo["Servicio de correo"]

    web --> agenda
    web --> cobros
    web --> exp

    cobros -->|Consulta el servicio atendido| agenda
    cobros -->|Envía el recibo| notif

    notif --> correo

    agenda --> db
    cobros --> db
    exp --> db

    style cobros fill:#ff5d00,color:#fff
```

---

## Diagrama 4. Diagrama de flujo

Describe el proceso para validar y aplicar un abono a una cuenta de paciente.

```mermaid
flowchart TD
    ini([Inicio])
    val{"¿El monto es mayor a cero?"}
    exc{"¿El monto excede el saldo?"}
    apl["Aplicar el abono"]
    err1["Monto inválido"]
    err2["El monto excede el saldo"]
    fin([Fin])

    ini --> val

    val -->|No| err1
    val -->|Sí| exc

    exc -->|Sí| err2
    exc -->|No| apl

    err1 --> fin
    err2 --> fin
    apl --> fin

    style apl fill:#ff5d00,color:#fff
```

---

## Diagrama 5. Diagrama de secuencia

Muestra la comunicación entre la recepcionista, la aplicación web, los componentes de cobros y agenda, y la base de datos durante el registro de un abono.

```mermaid
sequenceDiagram
    actor Rec as Recepcionista
    participant Web as App web
    participant Cob as Cobros
    participant Age as Agenda
    participant BD as Base de datos

    Rec->>Web: Registra un abono de 500
    Web->>Cob: abonar(cuenta, 500)
    Cob->>Age: Consulta el servicio atendido
    Age-->>Cob: Consulta general
    Cob->>BD: Guarda el pago
    BD-->>Cob: Pago guardado correctamente
    Cob-->>Web: Pago registrado
    Web-->>Rec: Muestra el nuevo saldo
```

---

## Diagrama 6. Diagrama de clases

Representa las clases principales relacionadas con los pacientes, sus cuentas y los pagos registrados.

```mermaid
classDiagram
    class CuentaDePaciente {
        -double saldo
        -List~Pago~ pagos
        +abonar(monto) string
        +consultarSaldo() double
    }

    class Pago {
        -double monto
        -Date fecha
    }

    class Paciente {
        -string nombre
        -string especie
    }

    Paciente "1" --> "1" CuentaDePaciente : tiene
    CuentaDePaciente "1" --> "*" Pago : registra
```

---

## Diagrama 7. Diagrama de estados

Describe los posibles estados de una cita veterinaria desde que se agenda hasta que finaliza o se cancela.

```mermaid
stateDiagram-v2
    [*] --> Agendada

    Agendada --> Confirmada: El dueño confirma
    Agendada --> Cancelada: Se cancela a tiempo

    Confirmada --> Atendida: Llega a consulta
    Confirmada --> NoAsistio: No se presenta

    Atendida --> [*]
    Cancelada --> [*]
    NoAsistio --> [*]
```

---

## Diagrama 8. Diagrama entidad-relación

Representa las relaciones entre los pacientes, las citas, los expedientes clínicos, las cuentas y los pagos.

```mermaid
erDiagram
    PACIENTE ||--o{ CITA : tiene
    PACIENTE ||--|| CUENTA : tiene
    CITA ||--o| EXPEDIENTE : genera
    CUENTA ||--o{ PAGO : registra

    PACIENTE {
        int id
        string nombre
        string especie
    }

    CITA {
        int id
        date fecha
        string motivo
    }

    EXPEDIENTE {
        int id
        string diagnostico
        string tratamiento
    }

    CUENTA {
        int id
        decimal saldo
    }

    PAGO {
        int id
        decimal monto
        date fecha
    }
```

---

**Fin del documento.**
