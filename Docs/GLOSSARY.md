# Cinema Management System - Glossary of Key Terms

## Hall (Cinema Hall)
A physical space within a cinema where movies are screened. Each hall has a unique name, seating capacity, and can host multiple showtimes. Administrators manage hall configurations including capacity limits and scheduling constraints.

## Showtime (Movie Showtime)
A scheduled screening of a specific movie in a particular hall at a defined date and time. Showtimes are the core booking units in the system, linking movies to halls and time slots.

## Film Title
The name of the movie being screened during a showtime. This field is validated for length and required for all showtime records.

## Capacity
The maximum number of seats available in a cinema hall. Validated to ensure it falls within acceptable limits (1-300 seats) and is considered when scheduling showtimes.

## Controller
A REST API component that handles HTTP requests related to halls and showtimes. Responsible for request validation, response formatting, and error handling for client interactions.

## DTO (Data Transfer Object)
Objects used to transfer data between application layers, particularly between controllers and services. HallDto and ShowtimeDto provide optimized data structures for API responses, often excluding sensitive or unnecessary entity details.

## Entity
The persistent domain objects (Hall and Showtime) that represent business concepts and are stored in the database. These objects contain business logic and validation rules.

## Repository
A data access abstraction layer that provides CRUD operations for entities. HallRepository and ShowtimeRepository interface with the database using Spring Data JPA patterns.

## Service
Business logic components (HallService, ShowtimeService) that contain the core application rules, coordinate between repositories, handle transactions, and implement caching strategies.

## Cache (ShowtimeCache)
An in-memory data store used to improve performance by temporarily storing frequently accessed showtime data, reducing database load for read operations.

## Validation
The process of ensuring data integrity through constraints and rules. Includes field-level validation (name length, capacity range) and business rule validation (showtime scheduling conflicts).

## Bulk Creation
The process of creating multiple halls or showtimes in a single operation, often implemented as a transactional batch process with error handling for individual items.

## Filtering
The capability to query and retrieve showtimes based on specific criteria such as date ranges, film titles, or hall availability. Implemented through custom repository queries.

## Cascade Operations
Automatic propagation of entity state changes to related entities. For example, deleting a hall triggers deletion of all associated showtimes through cascade configuration.

## REST API
The architectural style used for system interfaces, providing stateless, resource-oriented endpoints for hall and showtime management operations.

## JPA (Jakarta Persistence API)
The Java specification for object-relational mapping, used to manage persistence and object-relational mapping between Hall/Showtime entities and database tables.

## Transaction
A unit of work that treats a series of database operations as a single action. Ensures data consistency through ACID properties (Atomicity, Consistency, Isolation, Durability).

## Exception Handling
The systematic approach to managing errors and exceptional conditions in the application, including custom exceptions like ResourceNotFoundException for API error responses.

## Swagger/OpenAPI
The documentation framework used to describe and document the REST API endpoints, providing interactive documentation for developers and API consumers.

## Mermaid Diagrams
The diagramming tool used to create visual representations of system components, processes, and data flows in documentation.

## Database Schema
The structure of the database including tables (halls, showtimes), columns, data types, constraints, and relationships that define how data is stored and organized.
