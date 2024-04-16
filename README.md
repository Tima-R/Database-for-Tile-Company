# AlvandTileCompanyDatabase
A MySQL database project for managing operations of the Alvand Tile manufacturing company, including production, personnel, and environmental data.

## Overview
This repository contains the SQL scripts, documentation, and additional resources for a database designed to manage the operations of Alvand Tile & Ceramic Industries. The project, completed two years ago, includes a MySQL database schema for operational details, production capacities, employee management, and environmental impact assessments. As of the time of writing, I no longer have access to the actual company data used during the project's development. Therefore, the repository includes generalized or anonymized data structures and test data, designed to illustrate the database functionalities without revealing sensitive information.

## Getting Started

### Prerequisites

MySQL Server (Version 8.0 or newer recommended): This project was initially developed using MySQL Server 8.0. This version introduced significant updates and features that may be necessary for optimal performance and compatibility with the project scripts.
MySQL Workbench (Version 8.0 or newer recommended): To manage the database schema and execute SQL scripts, MySQL Workbench 8.0 or newer is recommended due to enhancements that support newer SQL standards and improved usability features.

### Installation

1. **Clone the repository**:
    ```
    git clone https://github.com/Tima-R/AlvandTileCompanyDatabase.git
    ```

2. **Create the database** by running the `schema.sql` script in MySQL Workbench. This script will create the `AlvandTileCompanyDatabase` database along with all required tables.

3. **Populate the database** with initial test data by executing the `insert_data.sql` script.

### Directory Structure

- `/sql` - Contains SQL scripts for creating the database schema (`schema.sql`) and inserting test data (`insert_data.sql`).
- `/docs` - Contains the project documentation, including the Entity-Relationship Diagram (ERD) and other relevant documentation.

## Usage

After setting up the database, to start querying the data or inserting new records. Here are some example operations to perform:

### Querying the Database

```sql
-- Read (select)
SELECT * FROM OperationalDetails;
SELECT FactoryID, DailyProductionCapacity FROM CapacityAndProduction;

-- Create (Insert)
INSERT INTO OperationalDetails (FactoryCode, Location, YearEstablished, OperationalStatus, Shifts, TechnologyLevel, EnergySource) 
VALUES ('ATC002', 'Qazvin', 2006, 'Active', 'Triple', 'Moderate', 'Electricity');

-- Update
-- Update the TechnologyLevel of a specific factory
UPDATE OperationalDetails
SET TechnologyLevel = 'Moderate'
WHERE FactoryCode = 'ATC003';

-- Delete
-- Delete a record from the OperationalDetails table
DELETE FROM OperationalDetails
WHERE FactoryCode = 'ATC003';

--- Quick Start Query
-- Quickly get the production capacity of all factories
SELECT FactoryCode, Location, DailyProductionCapacity 
FROM OperationalDetails
JOIN CapacityAndProduction ON OperationalDetails.FactoryID = CapacityAndProduction.FactoryID;

-- Specific Operations
-- Generate a detailed report of each factory including operational details and environmental scores
SELECT 
    OperationalDetails.FactoryCode, 
    OperationalDetails.Location, 
    OperationalDetails.Shifts, 
    CapacityAndProduction.DailyProductionCapacity, 
    EnvironmentalImpactScore.EnvironmentalImpactScore
FROM OperationalDetails
JOIN CapacityAndProduction ON OperationalDetails.FactoryID = CapacityAndProduction.FactoryID
JOIN EnvironmentalImpactScore ON OperationalDetails.FactoryID = EnvironmentalImpactScore.FactoryID;

-- Update Production Capacity Based on Environmental Score
-- Increase the production capacity for factories with an environmental score above 80
UPDATE CapacityAndProduction
SET DailyProductionCapacity = DailyProductionCapacity * 1.1
WHERE FactoryID IN (
    SELECT FactoryID 
    FROM EnvironmentalImpactScore
    WHERE EnvironmentalImpactScore > 80
);

-- Find Factories Needing Review
-- Find factories that have not been reviewed in over a year
SELECT FactoryCode, Location, YearEstablished 
FROM OperationalDetails
WHERE YEAR(CURDATE()) - YearEstablished > 1 AND OperationalStatus = 'Active';





