# Spec Delta: `database-and-mcp-tools`

## Purpose

Defines the PostgreSQL `toolbox_db` schema, demo seed population, and MCP Toolbox SQL tools the support agent uses to read orders and record audited customer intents.

## ADDED Requirements

### Requirement: Order Database Schema and Seed Population
The system SHALL provision PostgreSQL tables `users`, `customer_orders`, and `actions_log` and SHALL seed 10 demo shoppers and 17 orders spanning all five order statuses.

#### Scenario: Seed file defines required tables and demo accounts
- **WHEN** `mcp_toolbox/seed.sql` is inspected
- **THEN** it creates `users`, `customer_orders`, and `actions_log` and inserts 10 users (4 premium, 6 standard) and 17 orders

### Requirement: Tenant-Scoped MCP Order Lookup Tools
The MCP tool catalog SHALL expose parameterized SQL tools `get-order-status`, `find-customer-orders`, `action-log`, and `update-order-status`, and `get-order-status` MUST filter by both `order_id` and `customer_email`.

#### Scenario: get-order-status requires customer email
- **WHEN** `mcp_toolbox/tools.yaml` is loaded
- **THEN** `get-order-status` includes parameters `order_id` and `customer_email` and SQL `WHERE order_id = $1 AND customer_email = $2`

#### Scenario: Restricted write tool remains in the catalog
- **WHEN** toolset `cs_agent_tools` is listed
- **THEN** it includes `update-order-status` for compliance testing while `action-log` remains the intended mutation path
