# Deployment History

This document tracks all deployments performed by the ITO team, including:
- package versions
- configuration files
- JIRA tickets used to request the deployment
- any relevant operational notes

Use this file as the single source of truth for redeployments, audits, and environment reconstruction.

---

## Latest Deployment Summary

| Date | Environment | Component | Version / File | JIRA Ticket | Notes |
|------|-------------|-----------|----------------|-------------|-------|
| —    | PROD        | —         | —              | —           | —     |
| —    | STAGING     | —         | —              | —           | —     |
| —    | DEV         | —         | —              | —           | —     |


---

## Full Deployment Log

| Date       | Environment | Component       | Version / File   | JIRA Ticket | Notes           |
|------------|-------------|-----------------|------------------|-------------|-----------------|
| 2026‑02‑10 | PROD        | app-service     | 1.14.7           | ITO‑4821    | Hotfix applied  |
| 2025‑11‑03 | PROD        | config-service  | config-prod.yaml | ITO‑4310    | Updated DB URLs |
| 2025‑08‑27 | STAGING     | payment-service | 3.2.0            | ITO‑3922    | Major update    |
| —          | —           | —               | —                | —           | —               |

---

### **Deployment Request Template**

**Environment:** PROD / STAGING / DEV  
**Component:** app-service / config-service / ...  
**Version or File:** 1.14.7 or config-prod.yaml  
**JIRA Ticket:** ITO‑XXXX  
**Description:** Short summary of what changed and why  
