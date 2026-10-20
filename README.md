# SwiftChat System Redesign: System Analysis & Design

A full analysis and design for rebuilding **SwiftChat**, a messaging and file-management platform used by medium-sized organisations. The project starts with business problems (lost audit logs, permission errors, security gaps, manual onboarding) and ends with a costed, Scrum-planned design. The design covers requirements, UML models, a normalised SQL schema and UI prototypes.

> **Type:** University group project, System Analysis and Design (ISYS 10242), Nottingham Trent University
> **Team:** 4 people working in Scrum
> **My role:** Business Analyst
> **Scope:** Analysis and design only. No production code was built; the deliverables are the design artefacts in this repo.

---

## The problem

As SwiftChat grew, its existing system (a local server with weak data storage) began to fail:

- **Lost audit logs**, which damaged client trust and accountability
- **Permission management errors**, so users could access data they shouldn't
- **Security vulnerabilities** and data-loss risk
- **Manual onboarding** of organisations and users, which took up staff time

## The proposed solution

An automated messaging and collaboration platform with:

- **Cloud-hosted storage** (Azure / AWS) for scalable, backed-up data
- **Role-based access control (RBAC)** across three portals: Organisation, Employee and Administrator
- **Integrated audit logging** of all administrative actions
- **Self-service onboarding**: organisations register, are approved and get a workspace created automatically
- **Usage analytics and reporting** for administrators and organisations
- **Encryption** of passwords and billing details

---

## My contribution (Business Analyst)

- **Requirements gathering:** designed the interview plan (group interviews followed by one-to-one interviews with the manager, system admin, database admin, finance and development staff) and the anonymous questionnaires for each portal
- **Requirements definition:** developed the functional and non-functional requirements, prioritised as Mandatory / Desirable / Optional / Improvement
- **Methodology:** wrote the case for **Scrum** over XP, RAD and Waterfall, based on the project's budget, timeline and team size
- **Feasibility and risk:** reviewed and revised the technical, economic and organisational feasibility study and the risk register
- **Design:** co-designed the database with two teammates and wrote the descriptions for the use-case diagram, class diagram and UI screens
- **Coordination:** managed the team workflow, then structured and edited the final report

---

## Repository contents

```
SwiftChat-System-Design/
├── README.md
├── sql/
│   └── schema.sql                  # 11-table MySQL schema
└── docs/
    ├── requirements/
    │   └── interview-questions.png # Stakeholder interview question set
    ├── diagrams/
    │   ├── use-case-diagram.png
    │   ├── class-diagram.png
    │   └── er-diagram.png
    └── ui/
        ├── employee-chat.jpg
        ├── admin-dashboard.jpg
        └── organisation-portal.jpg
```

---

## 1. Methodology: why Scrum

| Constraint | How Scrum addresses it |
|---|---|
| Fixed £60,000 budget | Feature prioritisation each sprint keeps spending under control |
| 5–6 month timeline | Short sprints deliver working increments early |
| Unfamiliar technologies | Regular retrospectives catch problems early |
| Need for schedule visibility | Sprint reviews and weekly progress tracking |

**Alternatives rejected:**

- **XP:** too little documentation for a system with compliance and audit needs
- **RAD:** only works when requirements are fully known at the start
- **Waterfall:** too rigid to handle changing stakeholder feedback

## 2. Feasibility

- **Technical:** replace the local server with a managed DBMS (PostgreSQL / MySQL), a Node.js or Django backend, and Azure / AWS cloud hosting
- **Economic:** £60,000 budget against an estimated £80,000 return, giving a positive ROI and NPV
- **Organisational:** medium risk, mainly from staff adapting to automation. Mitigated with training and a gradual rollout

## 3. Requirements

Requirements were gathered through **two rounds of stakeholder interviews** and **anonymous questionnaires** for each portal.

**Functional requirements (examples)**

| ID | Requirement | Priority |
|---|---|---|
| 1.1.1 | Organisations self-register without manual email requests | Mandatory |
| 1.1.2 | Workspace created automatically after admin approval | Mandatory |
| 1.2.2 | Employees can search chat history efficiently | Mandatory |
| 1.3.1 | Admins approve organisations and manage user permissions | Mandatory |
| 1.3.4 | Admins can retrieve logs and reports | Mandatory |
| 1.1.8 | Integrations with email, calendar and reminder tools | Optional |

**Non-functional requirements (examples)**

| Area | Requirement |
|---|---|
| Performance | Respond within **3 seconds** under normal load; scale as organisations and users grow |
| Security | Secure login, **role-based data access**, **encryption** of passwords and billing data, **audit trail** of admin actions |
| Availability | Available 24/7 outside maintenance windows, with failure recovery |
| Usability | New users learn core functions with minimal training |

## 4. Use-case diagram

There are three actors. **Organisation** and **Employee** are the primary actors and **Administrator** is the supporting actor. The diagram uses `<<include>>` and `<<extend>>` relationships; for example, login is required before most actions, and payment processing extends subscription management.

![Use-case diagram](docs/diagrams/use-case-diagram.png)

## 5. Class diagram

A `User` superclass is specialised into `Organization`, `Employee` and `Administrator`. Supporting classes are `Workspace`, `Channel`, `Subscription`, `Payment`, `ChatHistory`, `Report` and `Notification`, with associations and multiplicities between them (for example, one Organisation has many Workspaces).

![Class diagram](docs/diagrams/class-diagram.png)

## 6. Database design

The ER diagram was converted into an **11-table relational schema** ([`sql/schema.sql`](sql/schema.sql)):

- `Users` is the central table, extended by the role tables `Organization`, `Employee` and `Administrator`
- `Workspace` → `Channel` models the one-to-many structure of the collaboration space
- `Subscription` → `Payment` holds billing
- `Report`, `Notification` and `Permission` cover reporting, alerts and access control
- **Foreign-key constraints** enforce referential integrity: a payment can't exist without a subscription, and a channel can't exist without a workspace

![ER diagram](docs/diagrams/er-diagram.png)

## 7. UI prototypes

| Employee chat | Admin dashboard | Organisation portal |
|---|---|---|
| ![Employee chat](docs/ui/employee-chat.jpg) | ![Admin dashboard](docs/ui/admin-dashboard.jpg) | ![Organisation portal](docs/ui/organisation-portal.jpg) |

Each screen maps directly to the database. For example, the admin dashboard's revenue card reads from `Payment`, and its activity log reads from `Notification`.

---

## What I'd improve next

Reviewing the design after submission, these are the changes I would make in a next iteration:

- **Add a `Message` table.** `ChatHistory` appears in the class diagram but has no table, so messages aren't actually persisted in the schema.
- **Strengthen RBAC.** `Permission` only links to `Administrator`. A `Role` table plus a `UserRole` link table would support the role-based access the requirements call for.
- **Add a dedicated `AuditLog` table** (actor, action, target, timestamp) instead of relying on `Notification`, because lost audit logs were the client's main problem.
- **Store password hashes** (e.g. bcrypt) rather than a plain `password` column, to meet the encryption requirement.
- **Prototype the backend** as a REST API against this schema to test the 3-second response-time requirement.

---

## Skills demonstrated

Requirements engineering · Stakeholder interviews and questionnaires · Functional / non-functional requirements · Agile / Scrum · Feasibility and risk analysis · UML (use-case, class) · ER modelling · SQL schema design and normalisation · UI/UX prototyping · Technical writing

## Acknowledgements

Built with three teammates who held the roles of Team Leader & UI Designer, System Designer, and Project Manager & Database Designer.
