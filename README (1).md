# Atlie Mart | AI-Assisted Supply Chain & Order Fulfillment Analytics

> **Project status:** In progress — tutorial-guided ingestion workflow implemented; extended SQL analysis and business recommendations are being developed.

## Business context

Atlie Mart is an organic food manufacturer operating in Ahmedabad and Vadodara, India, and New Jersey, USA. This educational case study explores order-fulfillment challenges that may contribute to customer dissatisfaction, including late deliveries and incomplete orders.

**Business objective:** Measure fulfillment reliability, locate performance gaps across markets, customers and products, and develop evidence-based recommendations. Incomplete fulfillment alone does not establish inventory shortages as the root cause; inventory and operational evidence would be needed to verify that hypothesis.

## Data ingestion automation

The tutorial-guided implementation uses **n8n** to extract incoming CSV attachments from Gmail and insert their records into a **Supabase-hosted PostgreSQL** database.

![Successful n8n workflow showing Gmail trigger, two CSV extraction branches and PostgreSQL inserts](screenshots/n8n-workflow.png)

*Figure 1. n8n execution: Gmail trigger → two CSV extraction nodes → PostgreSQL insert nodes. The screenshot shows 128 items in the upper branch and 226 in the lower branch, with successful execution indicators. These are execution item counts, not independently verified distinct business orders.*

### Pipeline

```text
Initial historical CSV files ────────────────┐
                                             ▼
                                      Supabase PostgreSQL
                                             ▲
Incoming CSV email → Gmail trigger → n8n extraction → column mapping → insert
                                             │
                                             ▼
                                  SQL validation & analysis
                                             │
                                             ▼
                                   Insights & recommendations
```

## Data model

| Table | Purpose | Typical grain |
|---|---|---|
| `fact_order_line` | Ordered and delivered product quantities | Order line |
| `fact_aggregate` | On-time, in-full and OTIF performance | Order |
| `dim_customers` | Customer and market attributes | Customer record |
| `dim_products` | Product attributes | Product |
| `dim_targets_orders` | Fulfillment targets | Target definition |

**Grain matters:** An order can contain several order lines. Order-level OTD, In-Full and OTIF rates must not be averaged over repeated order-line rows without first handling duplication.

## Initial KPI framework

| KPI | Definition |
|---|---|
| Total Orders | Distinct order IDs |
| Line Fill Rate | Completely fulfilled order lines ÷ all order lines |
| Volume Fill Rate | Delivered quantity ÷ ordered quantity |
| On-Time Delivery | Orders delivered by agreed date ÷ eligible orders |
| In-Full Delivery | Completely delivered orders ÷ eligible orders |
| OTIF | Orders delivered both on time and in full ÷ eligible orders |

*Final calculations depend on the dataset's exact business definitions and treatment of missing, cancelled, or over-delivered orders.*

## Planned independent analysis

- Profile and validate incoming and historical data, including duplicates, missing values, quantity anomalies and key relationships.
- Recalculate and verify the KPI framework in PostgreSQL SQL.
- Diagnose fulfillment performance by market, customer, product and time period.
- Quantify unfulfilled quantities and, where prices are available, **order value at risk** rather than assuming all shortfalls are permanently lost revenue.
- Evaluate possible causes and document what additional inventory, supplier or warehouse data would be needed to establish root causes.
- Present evidence-based operational recommendations and limitations.

## Tools

**Gmail · n8n · Supabase PostgreSQL · SQL · Quadratic AI (initial tutorial workflow)**

## Project origin and attribution

The **Atlie Mart scenario, dataset, initial KPI framework, n8n automation design and Quadratic setup** originate from a YouTube tutorial. The workflow shown here was implemented as part of that guided exercise. The additional SQL diagnostics, validation, analysis and recommendations are an independent extension in progress.

**Original tutorial:** [Add creator/channel and tutorial URL before publishing].

## Data and security

No database credentials, Gmail tokens, secrets or private customer data should be committed. Publish only data and workflow exports for which redistribution is permitted; sanitize any n8n workflow export before uploading.
