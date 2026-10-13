<h1 align="center">Lucas Lucachak</h1>

<p align="center">
  <strong>SOC analyst and backend developer, working on identity, access control and security tooling.</strong><br>
  Python · Java · Go — Keycloak, Microsoft Entra ID, Azure
</p>

<p align="center">
  <a href="https://portifolio-vercel-inky.vercel.app">Portfolio</a> ·
  <a href="https://linkedin.com/in/lucachak-dev">LinkedIn</a> ·
  <a href="mailto:lucas.lucachak9@gmail.com">Email</a>
</p>

---

## About

I'm a software engineer based in São Paulo, studying Computer Science at the
University of Debrecen (Hungary) on a merit scholarship. I currently work in a
**SOC**, and outside of that I build backend systems, the sign-in and
access-control layer around them, and my own security tools.

- Day job: security operations in a SOC.
- Production apps with real users: Java/Spring Boot and Python/Django, shipped with Docker.
- Identity: OpenID Connect SSO with Keycloak, Microsoft Entra ID roles driving application access, and Zero Trust SSH.
- Tooling: OSINT, recon and scanning tools written from scratch, plus CTFs and lab research.
- Hungarian citizen (no EU visa needed). Fluent in English and Portuguese, conversational Hungarian.
- **Open to** junior developer or security-adjacent roles, remote or in Europe.

<details>
<summary><strong>🇧🇷 Resumo em português</strong></summary>

<br>

Sou desenvolvedor de software em São Paulo, estudando Ciência da Computação na
Universidade de Debrecen (Hungria), com bolsa de mérito. Atualmente trabalho em
um **SOC** e, fora disso, construo sistemas backend, a camada de login, SSO e
controle de acesso ao redor deles, e minhas próprias ferramentas de segurança.

- Dia a dia: operações de segurança em um SOC.
- Aplicações em produção com usuários reais: Java/Spring Boot e Python/Django, com Docker.
- Identidade: SSO com OpenID Connect e Keycloak, roles do Microsoft Entra ID controlando o acesso à aplicação e SSH Zero Trust.
- Ferramentas: OSINT, reconhecimento e scanners escritos do zero, além de CTFs e laboratórios.
- Cidadão húngaro (sem necessidade de visto na UE). Inglês e português fluentes, húngaro conversacional.
- **Procuro** vagas de desenvolvedor júnior ou áreas próximas de segurança, remoto ou na Europa.

</details>

---

## Featured: identity & access

### Orbit — Django + Keycloak SSO on Azure
[`keycloak-poc`](https://github.com/lucachak/keycloak-poc)

A Django app authenticated through OpenID Connect (Authorization Code flow),
running with Keycloak on an Azure VM behind a TLS-terminating reverse proxy.

```text
Browser
   │
   ├── GET /login ───────────────► Keycloak ──► Microsoft Entra ID
   │                                  │         (organization tenant only)
   │◄──────── authorization code ─────┘
   │
   ├── GET /auth/callback ───────► Django
   │                                  ├── exchange the code for tokens
   │                                  ├── validate signature and claims (JWKS, RS256)
   │                                  ├── fetch userinfo
   │                                  └── store identity in the server-side session
   │
   ├── POST /api/session/sync ───► Django refreshes the token server-side
   │                               (roles and groups re-evaluated, every 15 s)
   │
   └── POST /logout ─────────────► Keycloak RP-initiated logout
                                      └── /logged-out
```

- Access tokens validated cryptographically against the realm JWKS (RS256 only).
- Tokens stay server-side; the browser only gets the Django session cookie.
- Roles consolidated from Keycloak realm roles, Keycloak client roles and Microsoft Entra ID app roles, then mapped to permissions.
- Per-role dashboards (`viewer`, `analyst`, `pentester`, `admin`) and live role refresh during the session.
- RP-initiated logout, Docker Compose deployment, NSG/firewall rules on the Azure VM.

**How Entra ID fit in:** Microsoft Entra ID was linked to Keycloak as an
external identity provider, so Keycloak remained the single issuer the app
trusted. Sign-in through Entra was limited to accounts in my organization's
tenant; outside accounts did not get a valid session. Users carried roles
assigned in Entra and Keycloak, and the app used them to decide access, for
example one user mapped to SOC access and another to INFRA. Unknown roles grant
nothing until explicitly mapped, and other clients' roles are ignored.

`Python` `Django` `Keycloak` `OIDC` `Microsoft Entra ID` `Azure VM` `RBAC` `JWKS` `Docker`

### ZTNA / PAM Access Lab — passwordless SSH in Go
*Proof of concept, private repository.*

A Linux PAM module written in Go (with cgo/C glue) for passwordless,
policy-driven SSH access.

- An issuer signs access; each VM enrolls with its own key.
- Every login is validated with an AES key, a timestamp and replay protection before the session opens.
- Installer, central/satellite configuration profiles and a control server that records session events.
- Docker lab (systemd containers) simulating one central node and three satellites.
- Designed for Azure VMs (admin SSH, NSG and firewall). SELinux enforcing on real VMs and Entra ID integration are not validated yet.

`Go` `cgo / C` `Linux PAM` `Zero Trust` `SSH` `AES` `Docker`

---

## In production

| Project | What it does | Stack | Links |
|---|---|---|---|
| **FinGen** | Turns bank statement PDFs into structured transactions with Gemini; budgets, recurring bills and an AI financial advisor | Java 21, Spring Boot, Thymeleaf, HTMX, Docker | [Live](https://fingen-app.onrender.com) · [Code](https://github.com/lucachak/FinGen) |
| **Cambiomatic** | Web ERP for auto repair shops: service orders from quote to invoice, inventory, customers, billing | Java 21, Spring Boot, PostgreSQL (Neon), Docker | [Live](https://cambiomatic.onrender.com) · [Code](https://github.com/lucachak/ERP) |
| **EletricBike** | E-bike workshop and e-commerce; invoices and payments (Stripe, Pix) kept consistent by webhooks | Django, Stripe, HTMX, PostgreSQL | [Code](https://github.com/lucachak/_) |
| **VMA Contabilidade** | Lead-generation site for an accounting firm: services, CLT vs PJ calculator, fiscal calendar | Next.js 16, React 19, TypeScript, Supabase | [Code](https://github.com/lucachak/VMA) |

---

## Security tooling & lab work

- [**OSIMPY**](https://github.com/lucachak/OSIMPY): async defensive OSINT suite with a four-level search-engine fallback and JSON export, for authorized assessments.
- [**Dirpy**](https://github.com/lucachak/Dirpy): directory scanner in pure Python, with HTTP/HTTPS implemented from scratch against the RFC.
- **CTFs and recon:** web exploitation, enumeration, JWT attack labs, APK static analysis in sandboxed VMs, network traffic analysis in isolated labs.

## Other projects

- [**CourierIQ**](https://github.com/lucachak/CourierIQ): multi-stop route optimization and ETA prediction behind a FastAPI service.
- **Onda**: music app in React Native (Expo) and TypeScript. *Private.*
- **Learning platform**: React Native client with a FastAPI backend and student/teacher roles. *Local project.*

---

## Stack

| Area | Tools |
|---|---|
| **Identity & access** | Keycloak, OpenID Connect, OAuth 2.0, Microsoft Entra ID, JWT/JWKS, RBAC, Linux PAM, Zero Trust |
| **Languages** | Python, Java 21, Go, C, TypeScript, SQL |
| **Backend** | Django, FastAPI, Spring Boot, REST APIs, HTMX, Celery |
| **Cloud & ops** | Azure VMs, NSG/firewall rules, Docker, Docker Compose, reverse proxy / TLS, GitHub Actions, Render, Vercel, Linux |
| **Data** | PostgreSQL, Neon, SQLite, Supabase |
| **Frontend & mobile** | Next.js, React, React Native (Expo), Tailwind CSS, Thymeleaf |
| **Security** | SOC operations, ethical hacking, OSINT, web recon, CTF, secure session handling |

---

## Contact

Hiring for backend, identity or security work? I'm available immediately,
full-time or part-time, and used to working across time zones including
Central Europe.

📧 [lucas.lucachak9@gmail.com](mailto:lucas.lucachak9@gmail.com) ·
💼 [linkedin.com/in/lucachak-dev](https://linkedin.com/in/lucachak-dev) ·
🌐 [Portfolio](https://portifolio-vercel-inky.vercel.app)
