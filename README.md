# BookMyShowClone

# Cinema Booking System

## Overview
This project is a reactive cinema booking system designed to allow users to browse movies, check showtimes, book seats, and make payments online. It leverages a microservices architecture built with Spring Boot and Spring WebFlux, is secured with Spring Security, and features a React-based frontend. Images, such as movie posters, are stored in Amazon S3.

## Architecture
The application is composed of several microservices, each handling a specific domain:

- **User Service**: Manages user registration, profiles, and authentication.
- **Movie Service**: Handles movie details, including titles, genres, cast, and reviews.
- **Booking Service**: Manages showtime schedules, seat reservations, and booking records.
- **Payment Service**: Processes payments for completed bookings.
- **Gateway**: Utilizes Spring Cloud Gateway to route requests to the appropriate microservices.

![Architecture Diagram](docs/architecture-diagram.png)

## Tech Stack
| Component       | Technology          |
|-----------------|---------------------|
| Backend         | Spring Boot, Spring WebFlux, Spring Security, Spring Cloud Gateway |
| Frontend        | React               |
| Database        | MySQL               |
| Image Storage   | Amazon S3           |

**Note**: The backend uses Spring WebFlux for reactive, non-blocking programming, enabling efficient handling of high concurrency.

## Prerequisites
Before setting up the project, ensure you have the following installed:
- Java 17 or higher
- Node.js 14 or higher
- MySQL 8.0 or higher
- An Amazon S3 bucket with valid credentials

## Setup Instructions
Follow these steps to set up and run the application locally:

1. **Clone the Repository**  
   ```bash
   git clone https://github.com/Dharanes/BookMyShowClone.git

# Cinema Booking System Database Schema

This document outlines the database schema for a cinema booking system, detailing tables, columns, data types, descriptions, and relationships between entities.

## Table of Contents
1. [User & Booking Management](#user--booking-management)
2. [Movie Management](#movie-management)
3. [Cinema & Seating](#cinema--seating)
4. [Scheduling & Shows](#scheduling--shows)
5. [Entity Relationships](#entity-relationships)

---

## User & Booking Management

### User
| Column      | Type   | Description             |
|-------------|--------|-------------------------|
| `user_id`   | PK     | Unique user identifier  |
| `name`      | String | User's full name        |
| `email`     | String | Email address           |
| `phone`     | String | Phone number            |

### Booking
| Column          | Type     | Description                        |
|-----------------|----------|------------------------------------|
| `booking_id`    | PK       | Unique booking identifier          |
| `user_id`       | FK       | References User(user_id)           |
| `show_id`       | FK       | References Show(show_id)           |
| `booking_date`  | DateTime | Timestamp of booking                |
| `total_price`   | Decimal  | Total amount paid                  |
| `status`        | Enum     | Pending, Confirmed, Cancelled      |
| `payment_id`    | FK       | References Payment(payment_id)     |

### Booking_Seat
| Column       | Type  | Description                        |
|--------------|-------|------------------------------------|
| `booking_id` | FK    | References Booking(booking_id)     |
| `seat_id`    | FK    | References Seat(seat_id)           |
| `show_id`    | FK    | References Show(show_id)           |
| `screen_id`  | FK    | References Screen(screen_id)       |
| `class_id`   | FK    | References Class(class_id)         |
| `status`     | Enum  | Booked, Reserved                   |

### Payment
| Column              | Type     | Description                        |
|---------------------|----------|------------------------------------|
| `payment_id`        | PK       | Unique payment identifier          |
| `booking_id`        | FK       | References Booking(booking_id)     |
| `amount`            | Decimal  | Payment amount                     |
| `payment_method`    | String   | e.g., UPI, Card, Wallet            |
| `transaction_status`| Enum     | Pending, Completed, Failed         |
| `payment_date`      | DateTime | Date and time of payment           |

---

## Movie Management

### Movie
| Column          | Type   | Description                    |
|-----------------|--------|--------------------------------|
| `movie_id`      | PK     | Unique movie ID                |
| `name`          | String | Movie title                    |
| `duration`      | Integer| Duration in minutes            |
| `rating`        | Float  | Average rating                 |
| `certificate`   | String | Certification (U, A, etc)      |
| `release_date`  | Date   | Release date                   |
| `status`        | String | e.g., Running, Upcoming        |

### Director
| Column         | Type   | Description                |
|----------------|--------|----------------------------|
| `director_id`  | PK     | Unique director ID         |
| `name`         | String | Director's name            |

### Cast
| Column      | Type   | Description                |
|-------------|--------|----------------------------|
| `cast_id`   | PK     | Unique cast ID             |
| `name`      | String | Actor/Actress name         |

### Genre
| Column      | Type   | Description                |
|-------------|--------|----------------------------|
| `genre_id`  | PK     | Unique genre ID            |
| `name`      | String | Genre name                 |

### Mapping Tables
- **Movie_Directors** (`movie_id`, `director_id`)
- **Movie_Cast** (`movie_id`, `cast_id`)
- **Movie_Genre** (`movie_id`, `genre_id`)

### Review
| Column         | Type     | Description                        |
|----------------|----------|------------------------------------|
| `review_id`    | PK       | Unique review ID                   |
| `user_id`      | FK       | References User(user_id)           |
| `movie_id`     | FK       | References Movie(movie_id)         |
| `rating`       | Float    | User rating                        |
| `comment`      | Text     | Review text                        |
| `review_date`  | DateTime | Timestamp of the review            |

---

## Cinema & Seating

### Cinema
| Column        | Type   | Description                |
|---------------|--------|----------------------------|
| `cinema_id`   | PK     | Unique cinema ID           |
| `name`        | String | Cinema name                |
| `location`    | String | Cinema location            |

### Screen
| Column         | Type | Description                        |
|----------------|------|------------------------------------|
| `screen_id`    | PK   | Unique screen ID                   |
| `cinema_id`    | FK   | References Cinema(cinema_id)       |
| `screen_number`| Int  | Screen number inside cinema        |

### Seat
| Column        | Type | Description                        |
|---------------|------|------------------------------------|
| `seat_id`     | PK   | Unique seat ID                     |
| `screen_id`   | FK   | References Screen(screen_id)       |
| `seat_number` | Int  | Seat number                        |
| `class_id`    | FK   | References Class(class_id)         |

### Class
| Column      | Type   | Description                |
|-------------|--------|----------------------------|
| `class_id`  | PK     | Unique class ID            |
| `name`      | String | e.g., Gold, Silver         |

### Price
| Column       | Type   | Description                        |
|--------------|--------|------------------------------------|
| `price_id`   | PK     | Unique price ID                    |
| `cinema_id`  | FK     | References Cinema(cinema_id)       |
| `class_id`   | FK     | References Class(class_id)         |
| `price`      | Decimal| Price of ticket for that class     |

---

## Scheduling & Shows

### Schedule
| Column        | Type | Description                        |
|---------------|------|------------------------------------|
| `schedule_id` | PK   | Unique schedule ID                 |
| `movie_id`    | FK   | References Movie(movie_id)         |
| `screen_id`   | FK   | References Screen(screen_id)       |
| `start_date`  | Date | Schedule start date                |
| `end_date`    | Date | Schedule end date                  |

### Show
| Column         | Type | Description                        |
|----------------|------|------------------------------------|
| `show_id`      | PK   | Unique show ID                     |
| `schedule_id`  | FK   | References Schedule(schedule_id)   |
| `show_date`    | Date | Date of the show                   |
| `start_time`   | Time | Show start time                    |
| `end_time`     | Time | Show end time                      |

### Show_Seat
| Column      | Type | Description                        |
|-------------|------|------------------------------------|
| `show_id`   | FK   | References Show(show_id)           |
| `seat_id`   | FK   | References Seat(seat_id)           |
| `status`    | Enum | Available, Booked, Reserved        |

---

## Entity Relationships
- **User – Booking**: One-to-Many
- **Movie – Schedule – Show**: One-to-Many-to-Many
- **Cinema – Screen – Seat**: One-to-Many-to-Many
- **Show – Show_Seat – Booking_Seat**: Mapping seat availability and booking
- **Booking – Payment**: One-to-One
- **Movie – Director/Cast/Genre**: Many-to-Many via mapping tables
