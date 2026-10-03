# Arquitectura Onion (Laravel)

## Veredicto Ejecutivo
La arquitectura implementa un modelo Onion estricto de 4 capas para garantizar la separación de responsabilidades:
1. **Domain**: Núcleo puro (PHP puro). Entidades, Value Objects, Interfaces de Repositorios, Excepciones. Cero dependencias de Laravel.
2. **Application**: Casos de uso (Use Cases) y DTOs. Depende de Domain. No sabe de infraestructura (ej. no usa `DB::transaction` directamente).
3. **Infrastructure**: Implementaciones concretas. Modelos Eloquent, Mappers, Repositorios concretos, `LaravelTransactionManager`. Depende de Domain y Application.
4. **Presentation**: Controladores HTTP, FormRequests, JsonResources. Delega rápidamente en Application.

## Regla de Oro
> **Las dependencias de código apuntan hacia el centro. Las implementaciones concretas quedan afuera.**

## Dilema Arquitectónico: Transacciones
Application no debe conocer a `DB::transaction()`.
Solución: Implementación de un puerto `TransactionManagerInterface` en Domain/Application, implementado por `LaravelTransactionManager` en Infrastructure.

## Errores Críticos a Evitar
1. **Domain contaminado con Laravel**: Prohibido importar `Illuminate\*` o extender de `Model` en el Dominio.
2. **`DB::transaction()` dentro del UseCase**: Esto viola la independencia de la infraestructura.
3. **Romper la propiedad del esquema**: Las migraciones son dueñas de la API, no de la infraestructura (Docker).
