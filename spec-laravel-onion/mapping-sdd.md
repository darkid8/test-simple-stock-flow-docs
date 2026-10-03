# Mapeo de Arquitectura Hexagonal a Onion

El sistema "Simple Stock Flow" transiciona los conceptos de Puertos y Adaptadores a las 4 capas de Onion Architecture con Laravel:

| Hexagonal (Ports & Adapters) | Onion Architecture (Laravel) |
|------------------------------|------------------------------|
| **Domain (Core)** | **Domain Layer**: Entidades de negocio puras, Value Objects, y Excepciones de dominio. PHP sin dependencias. |
| **Driving Ports (Inbound)** | **Application Layer**: Interfaces de Casos de Uso (Use Cases) y DTOs de entrada/salida. |
| **Driving Adapters** | **Presentation Layer**: Controladores de Laravel, FormRequests y JsonResources HTTP. |
| **Driven Ports (Outbound)** | **Domain/Application Layer**: Interfaces de Repositorios y `TransactionManagerInterface`. |
| **Driven Adapters** | **Infrastructure Layer**: Repositorios Eloquent, Mappers (Model -> Entity), Base de datos, HTTP Clients. |

## Puntos Críticos del Mapeo
- **Mappers Obligatorios**: Evitan que los atributos técnicos de persistencia (como la concurrencia `version`) contaminen la Entidad pura de Dominio.
- **TransactionManager como Puerto**: Permite operaciones atómicas desde el Caso de Uso, posibilitando cumplir con ACID sin que la aplicación se acople a `Illuminate\Support\Facades\DB`.
