# Diagramas del sistema de control de proyectos

**Proyecto:** Sistema web para el control de costos, gastos, facturación, pagos y ejecución de proyectos de SOINSOLAR S.A.S. BIC.  
**Estado:** diseño técnico del aplicativo.  
**Alcance:** arquitectura, comportamiento, datos, seguridad y despliegue.

## 1. Propósito

Este documento reúne los diagramas necesarios para comprender el aplicativo antes y durante su desarrollo. Los modelos representan la solución prevista y se mantienen coherentes con la base de datos PostgreSQL de Supabase, la autenticación, las políticas de seguridad por fila y la capa de acceso a datos implementada.

Los diagramas complementan las actividades del Objetivo 3:

| Diagrama | Actividades relacionadas | Finalidad |
|---|---|---|
| Contexto y arquitectura | 8 y 10 | Mostrar usuarios, interfaz, lógica, autenticación y base de datos |
| Casos de uso | 6, 8 y 10 | Delimitar lo que puede hacer cada rol |
| Flujo general | 6 y 8 | Explicar el proceso completo del sistema |
| Secuencia de registro mensual | 6, 7 y 10 | Mostrar la interacción entre usuario, aplicación y base de datos |
| Clases de acceso a datos | 8 y 10 | Documentar la organización del código |
| Entidad–relación | 6 y 7 | Representar tablas, claves y cardinalidades |
| Estados del periodo | 6 y 7 | Explicar apertura, cierre y bloqueo |
| Validación de facturas y pagos | 6 y 7 | Mostrar controles financieros |
| Seguridad | 7, 8 y 10 | Explicar autenticación, roles, RLS y auditoría |
| Despliegue | 8 y 10 | Separar demostración pública, código privado y servicios |

## 2. Diagrama de contexto y arquitectura

La solución es una aplicación web. El navegador presenta la interfaz; los servicios y repositorios aplican las reglas de la aplicación; Supabase autentica al usuario y PostgreSQL conserva la información. Las políticas RLS vuelven a comprobar los permisos en la base de datos.

~~~mermaid
flowchart TD
    U["Administrador y Gerencia"] --> UI["Aplicación web"]
    UI --> SR["Servicios y repositorios JavaScript"]
    SR --> AU["Supabase Auth y políticas RLS"]
    AU --> DB["PostgreSQL"]
    DB --> VW["Vistas y resúmenes financieros"]
    VW --> UI
~~~

### Responsabilidad de cada capa

| Capa | Responsabilidad |
|---|---|
| Interfaz web | Formularios, consultas, filtros, tableros y mensajes de validación |
| Servicios | Coordinar operaciones completas y cálculos que consume la interfaz |
| Repositorios | Leer y escribir en tablas o vistas específicas |
| Autenticación | Identificar al usuario y mantener una sesión segura |
| RLS | Autorizar cada operación según el rol y el registro |
| PostgreSQL | Conservar datos, relaciones, restricciones, auditoría y cálculos consolidados |

## 3. Diagrama de casos de uso

El sistema contempla dos perfiles iniciales. El administrador gestiona la configuración y la operación completa. Gerencia consulta, valida y puede realizar correcciones autorizadas, pero no elimina proyectos ni administra usuarios.

~~~mermaid
flowchart TD
    A["Administrador"] --> UC1["Gestionar proyectos y contratos"]
    A --> UC2["Gestionar periodos"]
    A --> UC3["Registrar movimientos financieros"]
    A --> UC4["Administrar usuarios"]
    A --> UC5["Consultar reportes e historial"]

    G["Gerencia"] --> UC6["Consultar proyectos e indicadores"]
    G --> UC7["Validar seguimiento mensual"]
    G --> UC8["Solicitar o registrar correcciones"]
    G --> UC5

    subgraph SISTEMA["Sistema de control"]
        UC1
        UC2
        UC3
        UC4
        UC5
        UC6
        UC7
        UC8
    end
~~~

### Casos de uso principales

| Código | Caso de uso | Administrador | Gerencia |
|---|---|---:|---:|
| CU-01 | Iniciar y cerrar sesión | Sí | Sí |
| CU-02 | Crear y editar proyectos | Sí | Corrección autorizada |
| CU-03 | Registrar contratos, adiciones y deducciones | Sí | Corrección autorizada |
| CU-04 | Abrir, cerrar o reabrir periodos | Sí | Consulta |
| CU-05 | Registrar seguimiento mensual | Sí | Sí |
| CU-06 | Registrar facturas | Sí | Sí |
| CU-07 | Registrar pagos asociados a facturas | Sí | Sí |
| CU-08 | Registrar costos y gastos | Sí | Sí |
| CU-09 | Validar el seguimiento mensual | Sí | Sí |
| CU-10 | Consultar saldos, avance e histórico | Sí | Sí |
| CU-11 | Generar reportes y aplicar filtros | Sí | Sí |
| CU-12 | Anular movimientos conservando trazabilidad | Sí | Según autorización |
| CU-13 | Administrar usuarios y roles | Sí | No |
| CU-14 | Eliminar un proyecto | Sí, con restricciones | No |

## 4. Diagrama de flujo general

El proyecto y el contrato forman la base del control. Los movimientos se registran dentro de un periodo abierto. La aplicación consolida facturación, pagos, costos, gastos, saldo y avance, y luego permite su validación y consulta.

~~~mermaid
flowchart TD
    I["Inicio de sesión"] --> P["Seleccionar o crear proyecto"]
    P --> C["Registrar o consultar contrato vigente"]
    C --> M["Seleccionar periodo abierto"]
    M --> T{"Tipo de registro"}

    T --> S["Seguimiento mensual"]
    T --> F["Factura"]
    T --> CG["Costo o gasto"]
    F --> PG["Pago asociado"]

    S --> V["Validar reglas financieras"]
    F --> V
    PG --> V
    CG --> V

    V -->|Cumple| G["Guardar y auditar"]
    V -->|No cumple| E["Rechazar y explicar el error"]
    G --> R["Actualizar indicadores y reportes"]
    R --> X["Consulta o validación gerencial"]
    E --> T
~~~

## 5. Diagrama de secuencia del registro mensual

Este diagrama muestra la operación más representativa: registrar la ejecución de un periodo, recalcular el resumen y someterlo a validación. La base de datos no acepta el movimiento si el periodo está cerrado o si se incumplen límites contractuales.

~~~mermaid
sequenceDiagram
    actor AD as Administrador
    participant UI as Interfaz web
    participant DA as Servicio de datos
    participant SB as Supabase
    actor GE as Gerencia

    AD->>UI: Selecciona proyecto y periodo
    UI->>DA: Solicita resumen vigente
    DA->>SB: Consulta contrato y movimientos
    SB-->>DA: Datos autorizados por RLS
    DA-->>UI: Resumen financiero

    AD->>UI: Registra avance, factura, pago o costo
    UI->>DA: Envía datos normalizados
    DA->>SB: Inserta o actualiza
    SB->>SB: Valida periodo, relaciones y límites

    alt Registro válido
        SB-->>DA: Movimiento guardado y auditado
        DA-->>UI: Indicadores actualizados
        UI-->>AD: Confirmación
        GE->>UI: Consulta y valida el periodo
        UI->>SB: Registra validación autorizada
        SB-->>GE: Resultado actualizado
    else Regla incumplida
        SB-->>DA: Error controlado
        DA-->>UI: Explicación del incumplimiento
        UI-->>AD: Solicita corrección
    end
~~~

## 6. Diagrama de clases

La capa de acceso a datos evita que la interfaz consulte tablas de forma desorganizada. DataService reúne los repositorios; cada repositorio se responsabiliza de un grupo funcional y utiliza el cliente autenticado de Supabase. El módulo Models normaliza y valida los datos antes de enviarlos.

~~~mermaid
classDiagram
    class DataService {
        +projects
        +contracts
        +periods
        +monthlyTracking
        +finance
    }

    class ProjectRepository {
        +list(filters)
        +getById(id)
        +create(input)
        +update(id, input)
        +remove(id)
        +getFinancialSummary(filters)
    }

    class ContractRepository {
        +listByProject(projectId)
        +getCurrent(projectId)
        +create(input)
        +update(id, input)
    }

    class PeriodRepository {
        +list()
        +getActive()
        +create(input)
        +close(id)
    }

    class MonthlyTrackingRepository {
        +list(filters)
        +upsert(input)
        +validate(id)
    }

    class FinanceRepository {
        +listInvoices(filters)
        +createInvoice(input)
        +listPayments(filters)
        +createPayment(input)
        +listCostsExpenses(filters)
        +createCostExpense(input)
    }

    class SupabaseClient {
        +auth
        +from(table)
        +rpc(function)
    }

    class Models {
        +normalizeProject(input)
        +normalizeContract(input)
        +normalizePeriod(input)
        +normalizeTracking(input)
        +normalizeInvoice(input)
        +normalizePayment(input)
        +normalizeCostExpense(input)
    }

    DataService o-- ProjectRepository
    DataService o-- ContractRepository
    DataService o-- PeriodRepository
    DataService o-- MonthlyTrackingRepository
    DataService o-- FinanceRepository
    ProjectRepository --> SupabaseClient
    ContractRepository --> SupabaseClient
    PeriodRepository --> SupabaseClient
    MonthlyTrackingRepository --> SupabaseClient
    FinanceRepository --> SupabaseClient
    ProjectRepository ..> Models
    ContractRepository ..> Models
    PeriodRepository ..> Models
    MonthlyTrackingRepository ..> Models
    FinanceRepository ..> Models
~~~

## 7. Diagrama entidad–relación de la base de datos

El modelo separa el contrato, el periodo, el seguimiento, las facturas, los pagos y los costos para evitar duplicidad y conservar el histórico. El mes no es un texto repetido en cada hoja: se representa con PERIODS y se relaciona con cada movimiento. El porcentaje de avance se obtiene a partir de la facturación acumulada respecto al valor contractual vigente.

~~~mermaid
erDiagram
    AUTH_USERS {
        uuid id PK
        string email
    }

    PROFILES {
        uuid id PK
        string full_name
        string role
        boolean active
    }

    PROJECTS {
        uuid id PK
        string cost_center UK
        string name
        string municipality
        string service_type
        decimal power_kwp
        string status
    }

    CONTRACTS {
        uuid id PK
        uuid project_id FK
        string contract_number
        decimal initial_value
        decimal additions_value
        decimal deductions_value
        decimal current_value
    }

    PERIODS {
        uuid id PK
        integer year
        integer month
        string status
        datetime closed_at
    }

    MONTHLY_TRACKING {
        uuid id PK
        uuid project_id FK
        uuid contract_id FK
        uuid period_id FK
        decimal recognized_value
        string validation_status
    }

    INVOICES {
        uuid id PK
        uuid project_id FK
        uuid contract_id FK
        uuid period_id FK
        string invoice_number
        date issue_date
        decimal amount
        string status
    }

    PAYMENTS {
        uuid id PK
        uuid invoice_id FK
        uuid project_id FK
        uuid contract_id FK
        uuid period_id FK
        string payment_reference
        date payment_date
        decimal amount
        string status
    }

    COSTS_EXPENSES {
        uuid id PK
        uuid project_id FK
        uuid period_id FK
        string movement_type
        string category
        date movement_date
        decimal amount
    }

    AUDIT_LOG {
        bigint id PK
        string table_name
        uuid record_id
        string action
        uuid changed_by FK
        datetime changed_at
    }

    AUTH_USERS ||--|| PROFILES : tiene
    AUTH_USERS ||--o{ AUDIT_LOG : realiza
    PROJECTS ||--o{ CONTRACTS : posee
    PROJECTS ||--o{ MONTHLY_TRACKING : registra
    CONTRACTS ||--o{ MONTHLY_TRACKING : controla
    PERIODS ||--o{ MONTHLY_TRACKING : agrupa
    PROJECTS ||--o{ INVOICES : factura
    CONTRACTS ||--o{ INVOICES : limita
    PERIODS ||--o{ INVOICES : contiene
    INVOICES ||--o{ PAYMENTS : recibe
    PROJECTS ||--o{ PAYMENTS : consolida
    CONTRACTS ||--o{ PAYMENTS : relaciona
    PERIODS ||--o{ PAYMENTS : contiene
    PROJECTS ||--o{ COSTS_EXPENSES : genera
    PERIODS ||--o{ COSTS_EXPENSES : contiene
~~~

### Cálculos financieros derivados

| Indicador | Cálculo |
|---|---|
| Valor contractual vigente | Valor inicial + adiciones − deducciones |
| Total facturado | Suma de facturas activas del proyecto |
| Total pagado | Suma de pagos activos asociados a facturas |
| Pendiente de pago | Total facturado − total pagado |
| Saldo contractual | Valor contractual vigente − total facturado |
| Avance financiero acumulado | Total facturado ÷ valor contractual vigente × 100 |
| Avance financiero mensual | Facturación del periodo ÷ valor contractual vigente × 100 |
| Costos y gastos | Suma de movimientos activos de costo y gasto |
| Resultado financiero simple | Total facturado − costos y gastos |

El valor pagado no reduce dos veces el saldo contractual. La factura consume el valor del contrato; el pago cancela total o parcialmente la cuenta originada por esa factura.

## 8. Diagrama de estados del periodo

Un periodo abierto permite registrar y corregir movimientos. Cuando el administrador lo cierra, los movimientos quedan disponibles para consulta, pero no pueden modificarse. Una reapertura es una acción administrativa y debe quedar auditada.

~~~mermaid
stateDiagram-v2
    [*] --> Abierto
    Abierto --> Abierto: Registrar o corregir movimientos
    Abierto --> Cerrado: Cierre por administrador
    Cerrado --> Cerrado: Consultar y reportar
    Cerrado --> Abierto: Reapertura administrativa
    Cerrado --> [*]: Conservación histórica
~~~

### Reglas de estado

- Solo un usuario autorizado puede cambiar el estado del periodo.
- El cierre registra fecha y usuario responsable.
- Las tablas financieras rechazan inserciones, ediciones y eliminaciones sobre periodos cerrados.
- La consulta histórica permanece disponible.
- Una reapertura debe borrar los datos de cierre correspondientes y generar evidencia en auditoría.

## 9. Flujo de validación de facturas y pagos

Las facturas representan el valor cobrado al cliente. Los pagos representan el dinero efectivamente recibido y siempre deben estar vinculados a una factura.

~~~mermaid
flowchart TD
    FI["Ingresar factura"] --> FP{"Periodo abierto y fecha válida"}
    FP -->|No| ER["Rechazar operación"]
    FP -->|Sí| FC{"Proyecto y contrato coinciden"}
    FC -->|No| ER
    FC -->|Sí| FL{"Facturación acumulada no supera contrato"}
    FL -->|No| ER
    FL -->|Sí| FS["Guardar factura"]

    FS --> PI["Ingresar pago de la factura"]
    PI --> PE{"Factura activa"}
    PE -->|No| ER
    PE -->|Sí| PL{"Pagos acumulados no superan factura"}
    PL -->|No| ER
    PL -->|Sí| PS["Guardar pago y actualizar pendiente"]
~~~

## 10. Diagrama de seguridad y auditoría

La clave pública del proyecto permite que el navegador se conecte, pero no concede acceso por sí sola. El usuario debe autenticarse y cada consulta pasa por las políticas RLS. Las credenciales administrativas o secretas no se incorporan al frontend.

~~~mermaid
flowchart TD
    AN["Visitante sin sesión"] --> DN["Acceso a datos denegado"]
    US["Usuario autenticado"] --> AU["Supabase Auth"]
    AU --> PR["Perfil activo y rol"]
    PR --> RL["Políticas RLS"]

    RL -->|Administrador| AD["Gestión completa autorizada"]
    RL -->|Gerencia| GE["Consulta, registro y validación controlada"]

    AD --> DB["Tablas y vistas"]
    GE --> DB
    DB --> LG["Registro de auditoría"]
~~~

### Matriz resumida de permisos

| Operación | Administrador | Gerencia | Usuario no autenticado |
|---|---:|---:|---:|
| Consultar información | Sí | Sí | No |
| Registrar movimientos | Sí | Sí | No |
| Corregir movimientos abiertos | Sí | Sí, según política | No |
| Validar seguimiento | Sí | Sí | No |
| Cerrar o reabrir periodos | Sí | No | No |
| Administrar usuarios | Sí | No | No |
| Eliminar proyectos | Sí, si las relaciones lo permiten | No | No |
| Consultar auditoría | Sí | Según política | No |

## 11. Diagrama de despliegue y publicación

Se separan tres productos: la aplicación real, la demostración pública y la documentación pública. La demostración usa información ficticia y no se conecta a la base de datos productiva.

~~~mermaid
flowchart TD
    DEV["Repositorio privado del aplicativo"] --> QA["Pruebas y revisión"]
    QA --> APP["Aplicación web autorizada"]
    APP --> SB["Supabase: Auth, RLS y PostgreSQL"]

    DEMO["Repositorio de demostración"] --> PAGES["GitHub Pages público"]
    DOCS["Repositorio de documentación"] --> PUBLIC["Documentación pública"]

    PAGES --> SAMPLE["Datos ficticios sin credenciales"]
    PUBLIC --> DIAG["Actividades y diagramas"]
~~~

## 12. Trazabilidad con la documentación

| Actividad | Diagramas que la respaldan |
|---|---|
| Actividad 6 — Base de datos | Flujo general, entidad–relación, estados y validación financiera |
| Actividad 7 — Integridad de datos | Entidad–relación, estados, facturas/pagos y seguridad |
| Actividad 8 — Conexión con Supabase | Arquitectura, secuencia, clases, seguridad y despliegue |
| Actividad 10 — Modelos de acceso a datos | Arquitectura, secuencia, clases, entidad–relación y seguridad |

## 13. Criterios de coherencia del diseño

- Todo movimiento financiero pertenece a un proyecto y a un periodo.
- Las facturas, pagos y seguimientos también se relacionan con el contrato que controla su límite.
- Cada pago pertenece a una factura concreta.
- El mes y el año se controlan mediante una entidad de periodos.
- El avance financiero no se captura como un porcentaje aislado: se calcula con valores facturados y el contrato vigente.
- Los costos y gastos se conservan separados de la facturación y los pagos.
- Los registros históricos no se sobrescriben al cambiar de mes.
- El cierre mensual impide modificaciones posteriores no controladas.
- Las operaciones relevantes generan trazabilidad de usuario y fecha.
- La interfaz nunca incorpora credenciales secretas ni reemplaza la autorización de la base de datos.

## 14. Referencias técnicas

- Supabase Auth: https://supabase.com/docs/guides/auth
- Row Level Security: https://supabase.com/docs/guides/database/postgres/row-level-security
- Seguridad de API: https://supabase.com/docs/guides/api/securing-your-api
- PostgreSQL: https://www.postgresql.org/docs/
