# BACON-AI Odoo MCP Server

**Orchestrating Intelligence for the Real World.**

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MPL 2.0](https://img.shields.io/badge/License-MPL_2.0-brightgreen.svg)](https://opensource.org/licenses/MPL-2.0)
[![PyPI](https://img.shields.io/pypi/v/mcp-server-odoo.svg)](https://pypi.org/project/mcp-server-odoo/)
[![Tests](https://img.shields.io/badge/tests-572%20passing-brightgreen.svg)](#test-coverage)

The most comprehensive MCP server for Odoo ERP. Connect AI assistants like Claude, GPT, and Gemini to your Odoo instance with **17 tools** covering CRUD operations and complete end-to-end business workflows — from quotation to manufacturing to delivery.

Built and maintained by [BACON-AI](https://bacon-ai.cloud) | Contact: [hello@bacon-ai.cloud](mailto:hello@bacon-ai.cloud)

> **Fork of [ivnvxd/mcp-server-odoo](https://github.com/ivnvxd/mcp-server-odoo)** with workflow tools, bug fixes, and 153 additional tests validated against Odoo 19.0.

---

## Why BACON-AI Odoo MCP?

| Capability | This Server | Typical Alternatives |
|-----------|-------------|---------------------|
| Core CRUD (search, create, update, delete) | 7 tools | 3-5 tools |
| Business Workflow Automation | 10 workflow tools | None |
| End-to-End Process Coverage | Sales, Purchase, Manufacturing, Inventory | CRUD only |
| YOLO Mode (no module install needed) | Read-only + Full access | Not available |
| Validated Test Suite | 572 tests (153 against live Odoo 19.0) | Minimal or none |
| PyPI Distribution | `pip install mcp-server-odoo` | Manual setup |

---

## End-to-End Business Workflows

What makes this MCP server unique is its ability to execute **complete business processes**, not just individual database operations. Every workflow below has been tested end-to-end against a real Odoo 19.0 instance.

### Order-to-Cash (Sales Workflow)

```
Customer Inquiry → Quotation → Sales Order → Delivery → Invoice
```

1. **Create Quotation** with customer, product lines, quantities, and pricing
2. **Confirm Quotation** to convert it into a sales order
3. **Deliver to Customer** by validating the outgoing shipment
4. **Track Workflow Status** to see deliveries, invoices, and related documents

### Procure-to-Pay (Purchase Workflow)

```
Demand → Purchase Order → Confirm → Receive Inventory
```

1. **Create Purchase Order** with vendor, product lines, and unit prices
2. **Confirm Purchase Order** to generate incoming shipments
3. **Receive Inventory** by validating the incoming receipt
4. **Track Workflow Status** to see receipts and payment status

### Plan-to-Produce (Manufacturing Workflow)

```
Bill of Materials → Manufacturing Order → Confirm → Material Assignment → Production
```

1. **Create Bill of Materials** defining product components and quantities
2. **Create Manufacturing Order** for a specific product and quantity
3. **Confirm Manufacturing Order** to assign materials and begin production
4. **Track Workflow Status** to monitor production progress

### Full Integrated Workflow

All three workflows connect seamlessly. A single AI conversation can:

```python
# 1. Create and confirm a sales quotation
quotation = create_quotation(customer_id=15, product_lines=[...])
order = confirm_quotation(quotation_id=quotation["quotation_id"])

# 2. Create manufacturing order for the sold product
mo = create_manufacturing_order(product_id=373, quantity=2.0, origin=order["order_name"])

# 3. Purchase raw materials from vendor
po = create_purchase_order(vendor_id=42, product_lines=[...])
confirm_purchase_order(po_id=po["po_id"])
receive_inventory(po_name=po["po_name"])

# 4. Confirm manufacturing and produce goods
confirm_manufacturing_order(mo_id=mo["mo_id"])

# 5. Deliver to customer
deliver_to_customer(so_name=order["order_name"])

# 6. Get complete workflow status showing the entire chain
status = get_workflow_status(order_id=order["order_id"], order_type="sale")
```

---

## Available Tools (17 Total)

### Core Operations (7 Tools)

| Tool | Description |
|------|-------------|
| `search_records` | Search any Odoo model with domain filters, field selection, pagination, and ordering |
| `get_record` | Retrieve a specific record by ID with smart field defaults |
| `list_models` | Discover all accessible models with field metadata |
| `create_record` | Create records with field validation and permission checks |
| `update_record` | Update specific fields on existing records |
| `delete_record` | Delete records respecting model-level permissions |
| `read_group` | Aggregate data with grouping and statistical functions |

### Workflow Tools (10 Tools)

| Tool | Description |
|------|-------------|
| `create_quotation` | Create sales quotation with customer, product lines, quantities, pricing |
| `confirm_quotation` | Convert quotation to confirmed sales order (triggers delivery creation) |
| `create_purchase_order` | Create PO with vendor, product lines, and pricing |
| `confirm_purchase_order` | Confirm PO (triggers incoming shipment creation) |
| `receive_inventory` | Validate incoming receipt by PO name or picking ID |
| `deliver_to_customer` | Validate outgoing delivery by SO name or picking ID |
| `create_manufacturing_order` | Create manufacturing order for a product (MRP module) |
| `confirm_manufacturing_order` | Confirm MO and assign materials for production |
| `create_bom` | Create Bill of Materials with component lines |
| `get_workflow_status` | Trace order through complete lifecycle with related documents |

---

## Test Coverage

This server is backed by one of the most thorough test suites of any Odoo MCP implementation.

### Test Summary

| Category | Tests | Status |
|----------|-------|--------|
| Workflow Unit Tests (mocked) | 69 | All passing |
| System Integration Tests (live Odoo 19.0) | 84 | All passing |
| Existing Unit & Integration Tests | 419 | All passing |
| **Total** | **572** | **All passing** |

### System Integration Test Categories (SIT)

All SIT tests run against a **real Odoo 19.0 instance** with actual XML-RPC calls.

| SIT | Category | Tests | What It Validates |
|-----|----------|-------|-------------------|
| SIT-1 | Sales End-to-End | 9 | Quotation creation, confirmation, delivery generation, multi-line orders, date handling |
| SIT-2 | Purchase End-to-End | 6 | PO creation, confirmation, receipt generation, multi-line orders |
| SIT-3 | CRUD Lifecycle | 7 | Create, read, search, update, delete with verification at each step |
| SIT-4 | Data Consistency | 7 | Field-level write/read verification, boolean/numeric fields, empty value handling |
| SIT-5 | Error Handling | 17 | Invalid models, nonexistent records, state violations, missing parameters, duplicate operations |
| SIT-6 | Model Discovery | 8 | Model listing, metadata structure, resource templates, YOLO mode indicators |
| SIT-7 | Workflow Status | 5 | Status tracking structure, delivery references, order name formats |
| SIT-8 | Connection & Auth | 7 | Authentication flow, server version, health check, reconnection, context manager |
| SIT-9 | Direct Connection Ops | 10 | Low-level search, read, write, unlink, fields_get, search_read, execute, ordering |
| SIT-10 | Manufacturing | 2 | MO creation and workflow status (conditional on MRP module) |
| SIT-11 | Search Features | 6 | String domains, pagination, ordering, smart defaults, empty results |

### Unit Test Coverage (Workflow Tools)

69 tests covering all 10 workflow handlers with cases for:
- Successful operations with full response validation
- Customer/vendor/product not found scenarios
- Access denied (permission) errors
- Input validation (missing required fields)
- Connection error handling and recovery
- URL generation and formatting
- Tool registration verification

---

## Quick Start

### Installation

```bash
pip install mcp-server-odoo
```

### Configuration

Add to your MCP client settings (Claude Desktop, Claude Code, Cursor, VS Code, etc.):

```json
{
  "mcpServers": {
    "odoo": {
      "command": "uvx",
      "args": ["mcp-server-odoo"],
      "env": {
        "ODOO_URL": "https://your-odoo-instance.com",
        "ODOO_API_KEY": "your-api-key-here",
        "ODOO_DB": "your-database-name"
      }
    }
  }
}
```

### YOLO Mode (No Module Install Needed)

Connect to **any standard Odoo instance** without installing additional modules:

```json
{
  "mcpServers": {
    "odoo": {
      "command": "uvx",
      "args": ["mcp-server-odoo"],
      "env": {
        "ODOO_URL": "http://localhost:8069",
        "ODOO_USER": "admin",
        "ODOO_PASSWORD": "admin",
        "ODOO_DB": "mydb",
        "ODOO_YOLO": "true"
      }
    }
  }
}
```

| Mode | Env Value | Description |
|------|-----------|-------------|
| Off (default) | `off` | Requires MCP module installed on Odoo |
| Read-Only | `read` | All read operations, no writes. Safe for demos. |
| Full Access | `true` | Full CRUD + workflows. Dev/test environments only. |

### Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `ODOO_URL` | Yes | Your Odoo instance URL |
| `ODOO_API_KEY` | Yes* | API key authentication (recommended) |
| `ODOO_USER` | Yes* | Username (if not using API key) |
| `ODOO_PASSWORD` | Yes* | Password (if not using API key) |
| `ODOO_DB` | No | Database name (auto-detected if not set) |
| `ODOO_YOLO` | No | YOLO mode: `off`, `read`, or `true` |

*Either `ODOO_API_KEY` or both `ODOO_USER` and `ODOO_PASSWORD` are required.

### Transport Options

| Transport | Use Case | Command |
|-----------|----------|---------|
| **stdio** (default) | Desktop AI apps (Claude Desktop, Cursor) | `uvx mcp-server-odoo` |
| **streamable-http** | Remote access, web clients | `uvx mcp-server-odoo --transport streamable-http --port 8000` |

---

## Usage Examples

Once connected, ask your AI assistant naturally:

**Search & Retrieve:**
- "Show me all customers from Germany"
- "Find products with stock below 10 units"
- "List today's sales orders over 1000 EUR"

**Create & Manage:**
- "Create a quotation for Acme Corp with 5 units of Widget Pro at 99.99 each"
- "Confirm quotation S00286 and check if a delivery was created"
- "Create a purchase order to restock 100 units of raw material from our supplier"

**Complete Workflows:**
- "Walk me through creating a full sales order, from quotation to delivery"
- "Set up a manufacturing order for 50 units of our flagship product"
- "Check the workflow status of order S00280 - where are we in the process?"

---

## Resources

Direct access to Odoo data through MCP resource URIs:

```
odoo://res.partner/record/1           - Get partner by ID
odoo://product.product/search?domain=[["qty_available",">",0]]  - Search products
odoo://sale.order/browse?ids=1,2,3    - Browse multiple records
odoo://res.partner/count?domain=[["customer_rank",">",0]]       - Count records
odoo://product.product/fields         - Field metadata
```

---

## About BACON-AI

**BACON-AI** develops self-learning AI frameworks that automate complex workflows across DevOps, robotics, and enterprise systems with deterministic control.

### What We Do

- **AI Business Consulting** -- Helping organizations harness AI to streamline operations, from strategy to implementation
- **Enterprise ERP Integration** -- Deep expertise in SAP and Odoo, connecting AI agents to real business processes
- **Agentic-Driven Development & Testing (ADDT)** -- Our evolution of TDD where AI agents design, analyze, build, and test solutions continuously
- **AI Orchestration** -- Coordinating complex multi-agent systems with deterministic control using our BACON-AI 12-phase framework
- **Robotics Integration** -- Smart AI frameworks for hardware and sensor control

### Our Expertise

With a foundation in SAP test architecture and enterprise systems, BACON-AI brings production-grade reliability to AI automation. We don't just build demos -- we build systems that work in the real world.

### Get in Touch

- **Website:** [bacon-ai.cloud](https://bacon-ai.cloud)
- **Email:** [hello@bacon-ai.cloud](mailto:hello@bacon-ai.cloud)
- **Live Demos & Workshops:** Available on request

---

## Contributing

Contributions are welcome! This is a fork of [ivnvxd/mcp-server-odoo](https://github.com/ivnvxd/mcp-server-odoo) with extended workflow tools and comprehensive testing.

```bash
# Clone and install
git clone https://github.com/BACON-AI-CLOUD/bacon-ai-odoo-mcp.git
cd bacon-ai-odoo-mcp
pip install -e ".[dev]"

# Run tests
pytest --cov
```

## License

This project is licensed under the Mozilla Public License 2.0 (MPL-2.0) - see the [LICENSE](LICENSE) file for details.

---

*Built with the [BACON-AI Framework](https://bacon-ai.cloud) -- Orchestrating Intelligence for the Real World.*
