# Lucas Lucachak

Backend engineer and security researcher. I build the attack first; the defense follows from understanding it.

[LinkedIn](https://linkedin.com/in/lucachak-dev) · [Email](mailto:lucas.lucachak9@gmail.com) · [Portfolio](https://portifolio-vercel-inky.vercel.app/)

```json
{
  "role": "Backend Engineer & Security Researcher",
  "now": "SOC & Automation Engineer, studying for eJPT and CompTIA Security+",
  "based": "São Paulo, BR (EU passport, remote-ready)",
  "education": "B.Sc. Computer Science, University of Debrecen (merit scholarship)",
  "abroad": ["Ireland, 4 years", "Hungary, 3 years"],
  "speaks": ["pt_BR", "en_US", "hu_HU", "ru_RU"],
  "method": "build → break → understand → rebuild"
}
```

## In production

| Project | What it does | Built with |
|---|---|---|
| [**FinGen**](https://fingen-app.onrender.com/) | Personal and household finance. Tracks payables and receivables by scope (house, personal, business), recurring bills and free cash flow, and uses LLMs to analyze statements and suggest budget goals. | Java 21, Spring Boot 3, Docker, Gemini, OpenRouter |
| [**Cambiomatic**](https://cambiomatic.onrender.com/) | ERP for a mechanic workshop: service orders with customer and vehicle in one step, paginated search, KPI dashboard. | Java 21, Spring Boot, Thymeleaf, PostgreSQL (Neon), Docker |
| **EletricBike** | Workshop management plus e-commerce for e-bike conversion kits. Orders are kept separate from payments so cash flow stays auditable. | Django, HTMX, Stripe Checkout and webhooks, PostgreSQL |
| **VMA Contabilidade** | Site and client portal for an accounting firm. | Next.js, React, TypeScript, Tailwind, Supabase, Vercel |

## Security

| Project | What it does | What I wanted to learn |
|---|---|---|
| [**Dirpy**](https://github.com/lucachak/Dirpy) | Directory brute-forcer in pure Python, no third-party packages. HTTP and HTTPS written against the RFCs. | What DIRB and Gobuster actually do before trusting them |
| [**OSIMPY**](https://github.com/lucachak/OSIMPY) | Async OSINT and dorking suite with fallback across search engines, for assessing your own organization's exposure. | How leaks surface in public search, and how to find them first |
| [**Arachne**](https://github.com/lucachak/Arachne) | Browser automation that fills job applications from a single resume model. Selenium and undetected-chromedriver, Gaussian delays to behave like a person. | How far bot detection can be evaded, and where it can't |
| **pam-poc** | PAM module for SSH. An issuer signs short-lived access tokens, the VM verifies them against its public key. LDAP provider plugin, installer, and a lab of five systemd containers (CA and panel, three endpoints, one operator). | Zero-trust access without a middleman |
| **Orbit** | Django behind Keycloak. OIDC authorization code flow, JWKS validation, realm, client and Entra roles mapped to permissions, per-role dashboards. Tokens never reach the browser's JavaScript. | Doing OIDC and RBAC correctly end to end |

CTF and labs: HackTheBox, TryHackMe Jr. Pentester path.

## Also building

- **Onda**: music app in Portuguese. React Native, TypeScript, Expo.
- **Plataforma de Ensino**: courses app with student and teacher areas. React Native and FastAPI.
- **CourierIQ**: delivery route optimizer. Heuristic multi-stop routing, a Random Forest ETA model trained in notebooks (exploration, feature engineering, training), served by FastAPI. Python, scikit-learn.

## What I reach for

- **Backend**: Java and Spring Boot (custom security filters, JWT in HttpOnly cookies, service-layer architecture). Python with Django, FastAPI and SQLAlchemy.
- **Systems**: Go and C for the PAM work. PostgreSQL, MySQL, SQLite.
- **Front end**: HTMX, Next.js and TypeScript, React Native.
- **Infra**: Docker in production, Nginx, VMware, VirtualBox and QEMU with GPU passthrough. Arch Linux and Neovim every day.
- **Security by default**: OWASP threat modeling at the start, input validation and rate limiting as defaults, not patches.

## Currently

Exploring LoRA and QLoRA fine-tuning on security datasets (offensive techniques, CVE patterns, countermeasures) with PyTorch.

---

*Complexity is the enemy of execution. Keep the code clean, the architecture secure, the deploy automated.*
