# ATVS Ticket Management System

An internal web application that manages the full employee lifecycle and the day-to-day operations of an organization. Scope starts with a department requesting a new employee and ends with approved exit, access revocation, asset return and settlement handoff. The ticketing / service desk is one module of a broader internal operations platform.

> Status: requirements baseline (v2.0, 05 October 2026). This repository tracks the build and testing of that baseline.

## Table of Contents
1. [Overview](#overview)
2. [Lifecycle Coverage](#lifecycle-coverage)
3. [Modules](#modules)
4. [Common Transaction Design](#common-transaction-design)
5. [Roles and Permissions](#roles-and-permissions)
6. [Tech Stack and Architecture](#tech-stack-and-architecture)
7. [Security and Reliability](#security-and-reliability)
8. [Delivery Phases](#delivery-phases)
9. [Testing and Acceptance](#testing-and-acceptance)
10. [Out of Scope](#out-of-scope)
11. [Getting Started](#getting-started)
12. [Project Structure](#project-structure)
13. [Open Decisions](#open-decisions)
14. [Contributing](#contributing)

## Overview
The system connects HR, recruitment, managers, IT, administration, finance, project teams and employees through shared records, approval workflows, service requests and reports. The employee ID is the common reference across all modules. Approval levels, attendance rules, retention periods and salary components are configurable to company policy.

## Lifecycle Coverage
| Stage | Outcome |
|---|---|
| Plan and hire | Requisition, budget/headcount approval, recruitment, interviews, selection, approved offer |
| Join and enable | Preboarding, employee record, onboarding checklist, email/app access, equipment, induction |
| Work and support | Attendance, leave, projects, timesheets, tickets, expenses, payroll inputs, performance |
| Change and exit | Transfers, access changes, resignation, handover, clearance, account closure, assets, settlement inputs |

## Modules
- Organization, masters and setup
- Employee / manpower requisition
- Recruitment and interview management
- Offer approval and preboarding
- Employee onboarding
- Email and application access allocation
- Employee master and HR lifecycle
- Assets, devices and software licenses
- Attendance, leave and work modes
- Projects, development and quality
- Resource planning and timesheets
- Payroll inputs, benefits and employee pay
- Travel, expenses and reimbursements
- Internal tickets and service requests
- Ticket workflow, testing and SLA
- Procurement, vendors and office admin
- Performance, probation and learning
- Policies, knowledge base and confidential HR cases
- Resignation, exit and full clearance
- Roles, approvals and notifications
- Dashboards and reports
- Central help desk and maintenance

## Common Transaction Design
Every request has an auto-generated number, date/time, requester, organization scope, owner, current status, attachments, approval history, comments and audit history.

Example sequences: `REQ-2026-000001`, `CAND-2026-000001`, `EMP-000001`, `ONB-2026-000001`, `TKT-2026-000001`, `EXIT-2026-000001`. Numbers are generated server-side with no duplicates, even under simultaneous saves.

Ticket flow: New > Assigned > Acknowledged > In Progress > Waiting for Testing / Testing > Resolved > requester confirms > Closed (plus Waiting for User, Waiting for Development, Reopened, Cancelled).

Example SLA targets (configurable):

| Priority | Response | Resolution |
|---|---|---|
| Critical | 30 minutes | 4 hours |
| High | 1 hour | 8 hours |
| Medium | 4 hours | 24 hours |
| Low | 8 hours | 48 hours |

## Roles and Permissions
Employee, Manager / Project Lead, HR / Recruiter, IT / Security / Admin, Finance / Payroll, Support / Developer / Tester, Management / System Admin. Access is enforced by function, record ownership, team, company and sensitivity. Self-approval is prevented, and material changes after approval trigger re-approval. Confidential HR cases, compensation and bank data have separate permissions.

## Tech Stack and Architecture
- Frontend: Vue.js (responsive)
- Backend: Laravel REST API
- Database: MySQL (transactional, foreign keys, effective-dated records)
- Private file storage for attachments
- Background jobs for notifications, SLA evaluation, scheduled tasks, integrations and exports

## Security and Reliability
- Authentication and permission checks on every API and file download, including exports
- MFA/SSO where supported, least privilege, HTTPS, secret management
- No plaintext passwords stored or displayed
- File type/size allowlist, malware scanning, expiring download links
- Full audit trail (actor, time, action, old/new values, reason)
- Idempotent integrations with controlled retries
- Proposed targets: 100 concurrent users, 95% of list/detail requests within 3 seconds, restore within 4 hours, maximum 24 hours data loss

## Delivery Phases
1. Foundation and lifecycle: masters, users/roles, approvals, requisitions, recruitment/offer, employee master, onboarding, access allocation, assets, core tickets, manual exit tracking.
2. Daily operations: attendance/leave, projects/timesheets, payroll inputs, expenses, procurement, help desk/maintenance, performance/training, policies/knowledge, full reports.
3. Automation and integration: provider-backed provisioning, automated SLA escalation, biometric/payroll/finance connectors, optional WhatsApp and mobile app, AI-assisted ticket features.

## Testing and Acceptance
Essential end-to-end scenarios:
1. Requisition > recruitment > offer > employee record.
2. Onboarding with unique email account and laptop allocation, plus employee acknowledgement.
3. Data persistence, no duplicate IDs or duplicate allocation under simultaneous requests.
4. Leave, timesheet and expense approvals with balances, audit history and controlled corrections.
5. Ticket lifecycle (assign, develop, fail/pass test, resolve, reopen, close) with SLA checked against a known calendar.
6. Exit with asset return, handover and verified access revocation, with settlement tracked separately.

Permission and failure checks: cross-employee data access through screens, APIs and file links; manager scope; confidential HR access; pay segregation; rejected approvals; provider failures; duplicate retries; expired access; cancelled joining; urgent termination; export-vs-filter accuracy; one backup restore.

QA deliverables: traceable test scenarios, test results, UAT defect log.

## Out of Scope
- Native gross-to-net payroll engine, statutory filing and bank payment execution
- General ledger, taxation, vendor payment execution and financial statements
- Full CRM (leads/proposals are optional integrations)
- External client/vendor self-service portals (optional)

## Getting Started
> Replace with real commands once code exists.

```bash
git clone <repo-url>
cd atvs-ticket-management-system

# backend
cd backend
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve

# frontend
cd ../frontend
npm install
npm run dev
```

Use synthetic data only. No real employee data or production integrations should be assumed.

## Project Structure
```
atvs-ticket-management-system/
  backend/      Laravel REST API, migrations, workflows, jobs
  frontend/     Vue.js application
  docs/         Requirements PDF, API spec, user/admin guides
  qa/           Test scenarios, traceability matrix, UAT defect log
  README.md
```

## Open Decisions
Headcount/usage, entities and locations, email provider and domains, approval matrix, attendance/leave rules, payroll boundary and provider, integration credentials, access templates, SLA calendars, document templates, retention rules.

## Contributing
1. Create a feature branch from `main`.
2. Link every change to a requirement section and a test case.
3. Open a pull request for review.

## License
Internal use only.
