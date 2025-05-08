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
- **Instance**: Represents a specific object based on a defined Schema, with data conforming to the Schema's JSON Schema.
- **Relation**: Defines the type of relationship between two Schemas.
- **InstanceRelation**: Represents a specific link between two Instances based on an existing Relation.

## Development Phases Status

| Phase | Description                                                                                                                                                                | Status      |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| 1     | [Foundation & Meta-Data Definition (Schemas & Relations)](#phase-1-foundation--meta-data-definition-schemas--relations)                                                    | Completed   |
| 2     | [Schema and Instance Management](#phase-2-schema-and-instance-management)                                                                                                  | Completed   |
| 3     | [Relationship Management (InstanceRelations)](#phase-3-relationship-management-instancerelations)                                                                          | Not Started |
| 4     | [Data Change Capturing/Tracking to Inventory Entities](#phase-4-implement-data-change-capturingtracking-to-inventory-entities)                                             | Not Started |
| 5     | [Integrations of Inventory to Specific Features & Improve Search and Filter](#phase-5-implement-integrations-of-inventory-to-specific-features--improve-search-and-filter) | Not Started |
| 6     | [Role Browser Mappings and Access Control for Inventory](#phase-6-implement-role-browser-mappings-and-access-control-for-inventory)                                        | Not Started |
| 7     | [Deep-Dive/Advanced GenAI Integration](#phase-7-deep-diveadvanced-genai-integration)                                                                                       | Not Started |
| 8     | [Sync Mechanism with Other Repository Systems](#phase-8-implement-sync-mechanism-with-other-repository-systems)                                                            | Not Started |
| 9     | [Visualization for Inventory Relations Management](#phase-9-visualization-for-inventory-relations-management-tbd)                                                          | Not Started |
| 10    | [Testing & Documentation](#phase-10-testing--documentation)                                                                                                                | Not Started |

## Development Phases

### Phase 1: Foundation & Meta-Data Definition (Schemas & Relations)

- Define Mongoose schemas for Schema, Instance, Relation, and InstanceRelation entities.
- Select and integrate a JSON Schema validation library (e.g., Ajv) for validating schema definitions and instance data.

### Phase 2: Schema and Instance Management

- **2.1:** Implement backend CRUD routes for the Schema entity.
- **2.2:** Implement backend CRUD routes for the Instance entity.
- **2.3:** Develop frontend CRUD UI for the Schema entity and integrate with backend APIs.
- **2.4:** Implement dynamic form generation in the frontend based on the JSON Schema for creating new Instance entities, including data validation, and integrate with backend.
- **2.5:** Develop frontend UI for listing, viewing details, updating, and deleting Instance entities, and connect with backend APIs.

### Phase 3: Relationship Management (InstanceRelations)

- **3.1:** Develop backend CRUD API endpoints for the Relation entity.
- **3.2:** Develop backend CRUD API endpoints for the InstanceRelation entity.
- **3.3:** Implement validation logic for InstanceRelation creation:
  - Fetch the Relation using `relationId`.
  - Fetch `fromInstance` and `toInstance`.
  - Verify that the source instance matches the relation's source schema.
  - Verify that the target instance matches the relation's target schema.
  - Ensure the uniqueness of the InstanceRelation.
- **3.4:** Implement cascading delete logic:
  - Handle related Instances and Relations when a Schema is deleted.
  - Handle related InstanceRelations when an Instance is deleted.
  - Handle related InstanceRelations when a Relation is deleted.
- **3.5:** Develop frontend UI for creating and deleting Relation entities and integrate with backend.
- **3.6:** Develop frontend UI for creating and deleting InstanceRelation entities, ensuring validation, and integrate with backend.

### Phase 4: Implement Data Change Capturing/Tracking to Inventory Entities

- **4.1:** Select and integrate a Mongoose plugin for capturing data changes (e.g., mongoose-audit-trail).
- **4.2:** Apply the change capture plugin to the four core entities: Schema, Instance, Relation, and InstanceRelation.
- **4.3:** Develop backend API routes to query the change log data.
- **4.4:** Implement frontend UI to view the change log and integrate with backend APIs.

### Phase 5: Implement Integrations of Inventory to Specific Features & Improve Search and Filter

- **5.1:** Define and design the user flow from model, staging, and other workflows to the Inventory system.
- **5.2:** Implement backend logic to integrate the Inventory system with other specified features.
- **5.3:** Define the logic and mechanism for categorizing and traversing inventory items.
- **5.4:** Implement a tree browser UI component for navigating hierarchical inventory structures.
- **5.5:** Conduct discussions and gather feedback on the current Inventory system to identify improvements and add more details to UI screens (e.g., related instances, helpful links).
- **5.6:** (Optional) Provide basic integration of a GenAI service into the Inventory system.

### Phase 6: Implement Role Browser Mappings and Access Control for Inventory

- **6.1:** Define the logic for mapping roles to inventory permissions.
- **6.2:** Develop frontend UI for managing role mappings and implement corresponding backend logic.
- **6.3:** Implement access control logic in the backend to enforce permissions on inventory operations.
- **6.4:** Develop frontend UI for managing access control settings and integrate with backend APIs.

### Phase 7: Deep-Dive/Advanced GenAI Integration

- **Tasks:** To be determined.

### Phase 8: Implement Sync Mechanism with Other Repository Systems

- **Tasks:** To be determined.

### Phase 9: Visualization for Inventory Relations Management (TBD)

- **Tasks:** To be determined.

### Phase 10: Testing & Documentation

- **10.1:** Write unit tests for:
  - Mongoose models, including validation and methods.
  - JSON Schema validation for schema definitions and instance data.
  - Relation validation logic, including cardinality and schema matching.
  - Individual service layer functions.
- **10.2:** Write integration tests for:
  - API endpoints.
  - Core workflows that involve multiple API calls or service functions.
- **10.3:** Document API endpoints using tools like Swagger/OpenAPI, including request/response formats, parameters, and authentication requirements.
- **10.4:** Document the data model, including diagrams or descriptions of the four entities and their relationships, as well as validation rules and constraints.
