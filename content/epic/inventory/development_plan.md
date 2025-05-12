---
title: "Development Plan"
date: 2025-04-01T11:54:12+07:00
draft: false
weight: 5
---

## Overview

This document provides a detailed development plan for the Inventory Management Feature. The feature serves as a central repository for defining schemas and creating instances data based on those schemas. The development is broken down into multiple phases, each focusing on specific aspects of the feature, from foundational data models to advanced integrations and testing.

## Core Entities

- **Schema**: Defines the structure, fields, and validation rules for inventory items using JSON Schema.
- **Instance**: Represents a specific object based on a defined Schema, with data conforming to the rules and structure defined by the Schema.
- **Relation**: Defines the type of relationship between two Schemas.
- **InstanceRelation**: Represents a specific link between two Instances based on an existing Relation.

## Development Phases Status

| Phase | Description                                                                                                                                                                | Status                                  |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 1     | [Foundation & Meta-Data Definition (Schemas & Relations)](#phase-1-foundation--meta-data-definition-schemas--relations)                                                    | Completed Implementation. Test ongoing. |
| 2     | [Schema and Instance Management](#phase-2-schema-and-instance-management)                                                                                                  | Completed Implementation. Test ongoing. |
| 3     | [Relationship Management (InstanceRelations)](#phase-3-relationship-management-instancerelations)                                                                          | Ongoing                                 |
| 4     | [Data Change Capturing/Tracking to Inventory Entities](#phase-4-implement-data-change-capturingtracking-to-inventory-entities)                                             | Ongoing                                 |
| 5     | [Integrations of Inventory to Specific Features & Improve Search and Filter](#phase-5-implement-integrations-of-inventory-to-specific-features--improve-search-and-filter) | Ongoing                                 |
| 6     | [Role Browser Mappings and Access Control for Inventory](#phase-6-implement-role-browser-mappings-and-access-control-for-inventory)                                        | Ongoing                                 |
| 7     | [Deep-Dive/Advanced GenAI Integration](#phase-7-deep-diveadvanced-genai-integration)                                                                                       | Not Started                             |
| 8     | [Sync Mechanism with Other Repository Systems](#phase-8-implement-sync-mechanism-with-other-repository-systems)                                                            | Not Started                             |
| 9     | [Visualization for Inventory Relations Management](#phase-9-visualization-for-inventory-relations-management-tbd)                                                          | Not Started                             |
| 10    | [Testing & Documentation](#phase-10-testing--documentation)                                                                                                                | Ongoing                                 |

## Development Phases

### Phase 1: Foundation & Meta-Data Definition (Schemas & Relations)

- **Tasks**:
  1. Define Mongoose schemas for Schema, Instance, Relation, and InstanceRelation entities.
  2. Select and integrate a JSON Schema validation library (e.g., Ajv) for validating schema definitions and instance data.
- **Testing**:
  - Write **unit tests** to validate that the Mongoose schemas enforce the correct constraints (e.g., required fields, data types).
  - Write **unit tests** to verify that the JSON Schema validation library correctly validates schema definitions and instance data (e.g., valid vs. invalid inputs).

### Phase 2: Schema and Instance Management

- **Tasks**:
  1. Implement backend CRUD routes for the Schema entity.
  2. Implement backend CRUD routes for the Instance entity.
  3. Develop frontend CRUD UI for the Schema entity and integrate with backend APIs.
  4. Implement dynamic form generation in the frontend based on the JSON Schema for creating new Instance entities, including data validation, and integrate with backend.
  5. Develop frontend UI for listing, viewing details, updating, and deleting Instance entities, and connect with backend APIs.
- **Testing**:
  - Write **unit tests** for the backend CRUD routes for Schema and Instance entities (e.g., test each HTTP method).
  - Write **integration tests** for the API endpoints to ensure they interact correctly with the database (e.g., data persistence and retrieval).

### Phase 3: Relationship Management (InstanceRelations)

- **Tasks**:
  1. Develop backend CRUD API endpoints for the Relation entity.
  2. Develop backend CRUD API endpoints for the InstanceRelation entity.
  3. Implement validation logic for InstanceRelation creation:
     - Fetch the Relation using relationId.
     - Fetch fromInstance and toInstance.
     - Verify that the source instance matches the relation’s source schema.
     - Verify that the target instance matches the relation’s target schema.
     - Ensure the uniqueness of the InstanceRelation.
  4. Implement cascading delete logic:
     - Handle related Instances and Relations when a Schema is deleted.
     - Handle related InstanceRelations when an Instance is deleted.
     - Handle related InstanceRelations when a Relation is deleted.
  5. Develop frontend UI for creating and deleting Relation entities and integrate with backend.
  6. Develop frontend UI for creating and deleting InstanceRelation entities, ensuring validation, and integrate with backend.
- **Testing**:
  - Write **unit tests** for the backend CRUD routes for Relation and InstanceRelation entities.
  - Write **unit tests** for the validation logic in InstanceRelation creation (e.g., schema matching, uniqueness).
  - Write **unit tests** for the cascading delete logic (e.g., ensure related entities are deleted correctly).
  - Write **integration tests** for the Relation and InstanceRelation API endpoints to verify database interactions.

### Phase 4: Implement Data Change Capturing/Tracking to Inventory Entities

- **Tasks**:
  1. Select and integrate a Mongoose plugin for capturing data changes (e.g., mongoose-audit-trail).
  2. Apply the change capture plugin to the four core entities: Schema, Instance, Relation, and InstanceRelation.
  3. Develop backend API routes to query the change log data.
  4. Implement frontend UI to view the change log and integrate with backend APIs.
- **Testing**:
  - Write **unit tests** to verify that the change capture mechanism logs changes to the core entities (e.g., create, update, delete actions).
  - Write **unit tests** for the backend API routes that query the change log data (e.g., filtering by entity or timestamp).
  - Write **integration tests** to ensure the change log API endpoints correctly retrieve logged data from the database.

### Phase 5: Implement Integrations of Inventory to Specific Features & Improve Search and Filter

- **Tasks**:
  1. Define and design the user flow from model, staging, and other workflows to the Inventory system.
  2. Implement backend logic to integrate the Inventory system with other specified features.
  3. Define the logic and mechanism for categorizing and traversing inventory items.
  4. Implement a tree browser UI component for navigating hierarchical inventory structures.
  5. Conduct discussions and gather feedback on the current Inventory system to identify improvements and add more details to UI screens (e.g., related instances, helpful links).
  6. Provide basic integration of a GenAI service into the Inventory system.
- **Testing**:
  - Write **integration tests** for the user flows between the Inventory system and other features (e.g., data consistency across systems).
  - Write **unit tests** for the logic and mechanisms for categorizing and traversing inventory items (e.g., tree structure accuracy).
  - Write **unit tests** for the basic GenAI service integration (e.g., input/output validation).

### Phase 6: Implement Role Browser Mappings and Access Control for Inventory

- **Tasks**:
  1. Define the logic for mapping roles to inventory permissions.
  2. Develop frontend UI for managing role mappings and implement corresponding backend logic.
  3. Implement access control logic in the backend to enforce permissions on inventory operations.
  4. Develop frontend UI for managing access control settings and integrate with backend APIs.
- **Testing**:
  - Write **unit tests** for the role-to-permission mapping logic (e.g., role assignment accuracy).
  - Write **unit tests** for the access control logic in the backend (e.g., permission enforcement).
  - Write **integration tests** for the role mapping and access control API endpoints to verify database interactions and permission enforcement.

### Phase 7: Deep-Dive/Advanced GenAI Integration

- **Tasks**: To be determined (TBD).
- **Testing**: To be defined once tasks are specified. Will include **unit** and **integration tests** written at the end of the phase, tailored to the implemented features.

### Phase 8: Implement Sync Mechanism with Other Repository Systems

- **Tasks**: To be determined (TBD).
- **Testing**: To be defined once tasks are specified. Will include **unit** and **integration tests** written at the end of the phase, tailored to the implemented features.

### Phase 9: Visualization for Inventory Relations Management

- **Tasks**: To be determined (TBD).
- **Testing**: To be defined once tasks are specified. Will include **unit** and **integration tests** written at the end of the phase, tailored to the implemented features.

### Phase 10: Comprehensive Testing & Documentation

- **Tasks**:
  1. Execute all **unit** and **integration tests** developed in previous phases to ensure the entire system functions as expected.
  2. Write additional **unit** and **integration tests** for edge cases (e.g., rare user inputs), performance (e.g., load handling), security (e.g., unauthorized access), and any gaps not covered in phase-specific tests.
  3. Document API endpoints using tools like Swagger/OpenAPI, including request/response formats, parameters, and authentication requirements.
  4. Document the data model, including diagrams or descriptions of the four entities (Schema, Instance, Relation, InstanceRelation) and their relationships, as well as validation rules and constraints.
- **Testing Focus**:
  - Run the full test suite from all phases.
  - Address any remaining testing needs (e.g., cross-phase interactions, system-wide validation).
