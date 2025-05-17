# Day 31 – ATL Airport Database Schema Design (CS50 SQL)

Even while sick, I showed up today.

After finishing Lesson 2 of CS50’s Introduction to Databases, I completed the ATL assignment: designing a full relational schema for Hartsfield-Jackson International Airport (ATL), the busiest airport in the world.

## 🧱 What I Built

This schema includes 4 main tables:

- **passengers**: First name, last name, and age
- **airlines**: Airline name and concourse (A–F, T)
- **flights**: Flight number, airline, origin, destination, departure/arrival time
- **check_ins**: Links passengers to flights and logs check-in time

## 🔧 Commands Used

```bash
# Create project directory and file
mkdir atl
cd atl
code schema.sql

CREATE TABLE IF NOT EXISTS "passengers" (
    "id" INTEGER PRIMARY KEY,
    "first_name" TEXT NOT NULL,
    "last_name" TEXT NOT NULL,
    "age" INTEGER NOT NULL CHECK ("age" > 0)
);

CREATE TABLE IF NOT EXISTS "airlines" (
    "id" INTEGER PRIMARY KEY,
    "name" TEXT NOT NULL UNIQUE,
    "concourse" TEXT NOT NULL CHECK ("concourse" IN ('A', 'B', 'C', 'D', 'E', 'F', 'T'))
);


CREATE TABLE IF NOT EXISTS "flights" (
    "id" INTEGER PRIMARY KEY,
    "flight_number" TEXT NOT NULL,
    "airline_id" INTEGER NOT NULL,
    "origin" TEXT NOT NULL,
    "destination" TEXT NOT NULL,
    "departure" NUMERIC NOT NULL,
    "arrival" NUMERIC NOT NULL,
    FOREIGN KEY ("airline_id") REFERENCES "airlines"("id")
);


CREATE TABLE IF NOT EXISTS "check_ins" (
    "id" INTEGER PRIMARY KEY,
    "passenger_id" INTEGER NOT NULL,
    "flight_id" INTEGER NOT NULL,
    "time" NUMERIC NOT NULL,
    FOREIGN KEY ("passenger_id") REFERENCES "passengers"("id"),
    FOREIGN KEY ("flight_id") REFERENCES "flights"("id")
);

# Create database and run schema
sqlite3 atl.db
.read schema.sql
.tables
