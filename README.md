# Sistema de Gestión de Proyectos de Graduación UNI

Plataforma web para registrar, revisar, aprobar y dar seguimiento a proyectos de graduación de la Universidad Nacional de Ingeniería (UNI).

> Proyecto académico de la asignatura Diseño de Sistemas en Internet.

## Arquitectura

La arquitectura se documenta con el Modelo C4 (niveles 1 y 2), complementado con diagramas de secuencia que describen las interacciones API-First.

### C4 Nivel 1 - Contexto

```mermaid
C4Context
    title Sistema de Gestión de Proyectos de Graduación UNI - Contexto

    Person(estudiante, "Estudiante", "Registra su propuesta, entrega documentos y consulta el avance de su proyecto")
    Person(tutor, "Tutor", "Revisa proyectos, registra observaciones y valida avances")
    Person(coordinador, "Coordinador", "Gestiona asignaciones, estados y seguimiento académico")
    Person(admin, "Administrador", "Administra usuarios, roles y catálogos")

    System(uni, "Sistema de Gestión de Proyectos de Graduación UNI", "Plataforma web para registrar, revisar, aprobar y dar seguimiento a proyectos de graduación")

    System_Ext(email, "Correo institucional", "Envía notificaciones académicas")
    System_Ext(registro, "Sistema de Registro Académico", "Aporta datos académicos del estudiante")

    Rel(estudiante, uni, "Registra propuestas, entrega documentos y consulta avances")
    Rel(tutor, uni, "Revisa proyectos y registra observaciones")
    Rel(coordinador, uni, "Gestiona proyectos, tutores y estados")
    Rel(admin, uni, "Administra usuarios y catálogos")

    Rel(uni, email, "Envía notificaciones", "SMTP / API")
    Rel(uni, registro, "Consulta datos académicos", "HTTPS/JSON")
```

### C4 Nivel 2 - Contenedores

```mermaid
C4Container
    title Sistema de Gestión de Proyectos de Graduación UNI - Contenedores

    Person(estudiante, "Estudiante", "Registra su propuesta y entrega documentos")
    Person(tutor, "Tutor", "Revisa proyectos y registra observaciones")
    Person(coordinador, "Coordinador", "Gestiona asignaciones y estados")
    Person(admin, "Administrador", "Administra usuarios y catálogos")

    System_Boundary(uni, "Sistema de Gestión de Proyectos de Graduación UNI") {
        Container(web, "Aplicación Web", "React", "Interfaz para estudiantes, tutores, coordinadores y administradores")
        Container(api, "API REST", "ASP.NET Core Web API / C#", "Autenticación, reglas de negocio y servicios HTTP")
        ContainerDb(db, "Base de Datos", "SQL Server", "Usuarios, proyectos, revisiones, estados y catálogos")
        Container(files, "Gestor de Documentos", "Almacenamiento de objetos", "Documentos de propuestas, avances y defensa")
    }

    System_Ext(email, "Correo institucional", "SMTP / proveedor de correo")
    System_Ext(registro, "Sistema de Registro Académico", "Datos académicos del estudiante")

    Rel(estudiante, web, "Usa", "HTTPS")
    Rel(tutor, web, "Usa", "HTTPS")
    Rel(coordinador, web, "Usa", "HTTPS")
    Rel(admin, web, "Usa", "HTTPS")

    Rel(web, api, "Consume API REST", "HTTPS/JSON")
    Rel(api, db, "Lee y escribe", "EF Core / SQL")
    Rel(api, files, "Sube y recupera documentos", "HTTPS")
    Rel(api, email, "Envía notificaciones", "SMTP / API")
    Rel(api, registro, "Consulta datos académicos", "HTTPS/JSON")
```

| Contenedor | Tecnología | Responsabilidad |
|---|---|---|
| Aplicación Web | React | Interfaz de usuario y consumo de la API |
| API REST | ASP.NET Core Web API / C# | Autenticación, reglas de negocio y endpoints HTTP |
| Base de Datos | SQL Server | Persistencia de usuarios, proyectos, revisiones y estados |
| Gestor de Documentos | Almacenamiento de objetos | Archivos y evidencias del proyecto |

## Diagramas de secuencia

### Registrar propuesta de proyecto

```mermaid
sequenceDiagram
    autonumber
    actor E as Estudiante
    participant W as Aplicación Web
    participant A as API REST
    participant DB as SQL Server
    participant F as Gestor de Documentos

    E->>W: Completa formulario de propuesta y adjunta documento
    W->>A: POST /api/projects (Bearer token)
    activate A
    A->>A: Validar token, DTO y reglas de negocio
    alt Token inválido o vencido
        A-->>W: 401 Unauthorized
        W-->>E: Solicitar iniciar sesión de nuevo
    else Datos inválidos
        A-->>W: 400 Bad Request + errores
        W-->>E: Mostrar errores en el formulario
    else Datos válidos
        A->>DB: Crear proyecto (estado = Propuesta)
        DB-->>A: Id del proyecto
        A->>F: Guardar documento
        F-->>A: URL / Id del documento
        A->>DB: Asociar documento al proyecto
        DB-->>A: Confirmación
        A-->>W: 201 Created + proyecto
        W-->>E: Mostrar número de proyecto
    end
    deactivate A
```

### Revision del tutor

```mermaid
sequenceDiagram
    autonumber
    actor T as Tutor
    participant W as Aplicación Web
    participant A as API REST
    participant DB as SQL Server
    participant M as Correo institucional

    T->>W: Abre proyecto asignado
    W->>A: GET /api/projects/{id} (Bearer token)
    activate A
    A->>DB: Consultar proyecto y documentos
    DB-->>A: Datos del proyecto
    A-->>W: 200 OK + JSON
    deactivate A
    W-->>T: Mostrar proyecto

    T->>W: Registra observaciones
    W->>A: POST /api/projects/{id}/reviews
    activate A
    A->>DB: Verificar que el tutor esté asignado al proyecto
    DB-->>A: Resultado
    alt Tutor no asignado
        A-->>W: 403 Forbidden
        W-->>T: Mostrar acceso denegado
    else Tutor asignado
        A->>DB: Guardar revisión
        DB-->>A: Revisión registrada
        A->>M: Notificar al estudiante
        M-->>A: Envío aceptado
        A-->>W: 201 Created
        W-->>T: Mostrar confirmación
    end
    deactivate A
```

## API principal

_Pendiente._

## Cómo ejecutar el proyecto

_Pendiente._