# Database Design - Vehicle Onboarding

## Table of Contents
- [vehicles](#vehicles)
- [locations](#locations)

## Entity Relationship Diagram
```mermaid
erDiagram
    locations ||--o{ vehicles : "home location of"
```

## Tables

### vehicles
| Field | Data Type | Index | Constraint | Description |
| --- | --- | --- | --- | --- |
| id | UUID | Primary Key | NOT NULL, DEFAULT gen_random_uuid() | Unique identifier of the vehicle. |
| vin | TEXT | Unique Index | NOT NULL, UNIQUE | Vehicle identification number. |
| license_plate | TEXT | Unique Index | NOT NULL, UNIQUE | Vehicle license plate. |
| purchase_date | DATE | | NOT NULL | Date the vehicle was purchased. |
| purchase_cost | DECIMAL(15,2) | | NOT NULL | Purchase cost of the vehicle. |
| insurance_policy_number | TEXT | | NOT NULL | Insurance policy number. |
| insurance_expiry_date | DATE | | NOT NULL | Insurance policy expiry date. |
| odometer_reading | INTEGER | | NOT NULL, CHECK (odometer_reading >= 0) | Odometer reading at onboarding, in the standard unit (km). |
| brand | TEXT | | NOT NULL | Vehicle manufacturer brand. |
| model | TEXT | | NOT NULL | Vehicle model. |
| manufacturing_year | INTEGER | | NOT NULL | Year the vehicle was manufactured. |
| size | TEXT | Index | NOT NULL, CHECK (size IN ('small', 'medium')) | Vehicle size classification. |
| class | TEXT | Index | NOT NULL, CHECK (class IN ('economy', 'luxury')) | Vehicle class classification. |
| seats | INTEGER | | NOT NULL, CHECK (seats > 0) | Number of seats. |
| fuel_type | TEXT | Index | NOT NULL, CHECK (fuel_type IN ('gas', 'electric', 'hybrid')) | Vehicle fuel type. |
| ownership | TEXT | | NOT NULL, DEFAULT 'company' | Owner of the vehicle; always the company. |
| status | TEXT | Index | NOT NULL, CHECK (status IN ('Incoming', 'Active', 'Maintenance', 'Decommissioning', 'Sold')), DEFAULT 'Incoming' | Current lifecycle status of the vehicle. |
| home_location_id | UUID | Foreign Key | NOT NULL, REFERENCES locations(id) | Home location the vehicle is assigned to. |
| created_at | TIMESTAMP WITH TIME ZONE | | NOT NULL | Record creation timestamp. |
| updated_at | TIMESTAMP WITH TIME ZONE | | NOT NULL | Record last update timestamp. |
| deleted_at | TIMESTAMP WITH TIME ZONE | | | Record soft-deletion timestamp. |
| created_by | TEXT | | NOT NULL | User who created the record. |
| updated_by | TEXT | | NOT NULL | User who last updated the record. |
| deleted | BOOLEAN | | NOT NULL, DEFAULT false | Soft-delete flag. |

### locations
> Note: This table is a minimal reference used to assign a vehicle's home location. Full location and inventory management is out of scope for the Vehicle Onboarding TRD; see [Location & Inventory Management](../prd/prd-car-management.md#2-location--inventory-management).

| Field | Data Type | Index | Constraint | Description |
| --- | --- | --- | --- | --- |
| id | UUID | Primary Key | NOT NULL, DEFAULT gen_random_uuid() | Unique identifier of the location. |
| name | TEXT | Unique Index | NOT NULL, UNIQUE | Name of the location. |
| created_at | TIMESTAMP WITH TIME ZONE | | NOT NULL | Record creation timestamp. |
| updated_at | TIMESTAMP WITH TIME ZONE | | NOT NULL | Record last update timestamp. |
| deleted_at | TIMESTAMP WITH TIME ZONE | | | Record soft-deletion timestamp. |
| created_by | TEXT | | NOT NULL | User who created the record. |
| updated_by | TEXT | | NOT NULL | User who last updated the record. |
| deleted | BOOLEAN | | NOT NULL, DEFAULT false | Soft-delete flag. |
