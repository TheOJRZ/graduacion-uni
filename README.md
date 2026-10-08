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
(pega aquí el código del diagrama, sin las comillas triples del bloque de arriba)
```

| Contenedor | Tecnología | Responsabilidad |
|---|---|---|
| Aplicación Web | React | Interfaz de usuario y consumo de la API |
| API REST | ASP.NET Core Web API / C# | Autenticación, reglas de negocio y endpoints HTTP |
| Base de Datos | SQL Server | Persistencia de usuarios, proyectos, revisiones y estados |
| Gestor de Documentos | Almacenamiento de objetos | Archivos y evidencias del proyecto |

### Diagramas de secuencia

_Pendiente._

## API principal

_Pendiente._

## Cómo ejecutar el proyecto

_Pendiente._