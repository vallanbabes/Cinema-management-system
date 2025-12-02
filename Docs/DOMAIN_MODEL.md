# Cinema Management System - Domain Model Class Diagram

## Overview
This document describes the key entities and their relationships within the Cinema Management System application domain, based on the database schema and system design. This model serves as a conceptual foundation for the system's object-oriented implementation and database structure.

## Class Diagram Schema

![Cinema Domain Class Diagram](/Docs/schema/uml_diagramm_class.png)

## Class Diagram Description
The core domain of Cinema Management System revolves around cinema halls, showtimes, and their scheduling. The main entities are:

**Hall**: Represents a cinema hall with seating capacity and name.
**Showtime**: Represents a movie showtime scheduled in a specific hall at a particular date and time.

## Key Relationships Explained:

### Hall - Showtime (One-to-Many):
- Each **Hall** can have multiple **Showtime** entities scheduled in it.
- Each **Showtime** is associated with exactly one **Hall**.
- This relationship is managed through a foreign key `hall_id` in the `showtimes` table referencing the `halls` table.

## Database Schema:

### Halls Table:
- `id` bigint (PRIMARY KEY, IDENTITY/AUTO_INCREMENT)
- `name` character varying(10) - Name of the cinema hall (max 10 characters)
- `capacity` integer - Seating capacity of the hall (1-300 seats)

### Showtimes Table:
- `id` bigint (PRIMARY KEY, IDENTITY/AUTO_INCREMENT)
- `film_title` character varying(100) - Title of the movie (max 100 characters)
- `date_time` timestamp with time zone - Date and time of the showtime
- `hall_id` bigint (FOREIGN KEY referencing `halls.id`)

## Entity Attributes:

### Hall Entity:
- `id` (Long, PK) - Unique identifier for the hall
- `name` (String) - Name of the cinema hall, validated to be not blank and max 10 characters
- `capacity` (Integer) - Seating capacity, validated to be between 1 and 300
- `showtimes` (List<Showtime>) - Collection of showtimes scheduled in this hall

### Showtime Entity:
- `id` (Long, PK) - Unique identifier for the showtime
- `filmTitle` (String) - Title of the movie, validated to be not blank and max 100 characters
- `dateTime` (LocalDateTime) - Date and time of the showtime
- `hall` (Hall) - Reference to the hall where this showtime is scheduled
