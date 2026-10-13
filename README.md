<div align="center">

<img src="Static/Img/mark.svg" alt="" width="56" height="56">

# Tally

**Accounts, subscriptions and billing for a SaaS, ready to rebrand.**

Django 5.2 · Stripe · PostgreSQL · HTMX · Tailwind CSS v4

</div>

---

Most SaaS projects start by rebuilding the same four things: sign-in, plans, checkout and
"what is this person allowed to do now". Tally is that foundation, finished and tested, so
the first commit of your product is the product.

You manage plans in the Django admin, Stripe mirrors them, customers pay through Stripe
Checkout, and their access follows the subscription status without any glue code from you.

## What you get

| Area | Included |
| --- | --- |
| **Sign-in** | Email and password, GitHub and Google login, email verification, TOTP two-factor, password reset, idle logout. Built on django-allauth. |
| **Billing** | Plans and prices created in the admin and mirrored to Stripe. Stripe Checkout, customer portal, cancel and resume, webhook sync. |
| **Access control** | Each plan carries permissions and groups. They are applied and removed as the subscription changes. |
| **Interface** | Tailwind CSS v4, HTMX, light and dark themes, English and Portuguese (pt-BR). Branded 400, 403, 404 and 500 pages. |
| **Staff tools** | Searchable user list, workspace numbers, and a Django admin styled with Unfold. |

## How billing works

```
Admin                Stripe                 Your app
─────                ──────                 ────────
Create plan  ──────► Product + Price
                            │
Customer clicks "Choose plan"
                            ▼
                     Checkout session  ◄──── /checkout/plan/<id>/
                            │ pays
                            ▼
                     Webhook event  ───────► /checkout/webhook/
                                                    │
                                                    ▼
                                     Subscription/services.py
                                     status + plan + permissions
```

The webhook and the checkout return page go through the same service code, so handling an
event twice is harmless. `Core/billing.py` is the only module that imports Stripe, which
keeps it easy to read, mock and replace.

## Quick start

You need Python 3.10 or newer and Node.js. The project is developed on Python 3.14.

```bash
git clone https://github.com/lucachak/Store.git
cd Store

python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
npm install && npm run build:css

cp env.example .env        # set SECRET_KEY, DATABASE_URL and the Stripe keys
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Open <http://localhost:8000>. While you work on styles, run `npm run watch:css` in a second
terminal.

`DATABASE_URL` accepts any URL that dj-database-url supports, for example
`postgres://user:pass@localhost:5432/saas`. Leave it empty to use a local SQLite file.

## Connect Stripe

1. Put `STRIPE_SECRET_KEY` and `STRIPE_PUBLISHABLE_KEY` in `.env`. Use `sk_test_...` keys
   locally.
2. In the admin, open **Subscription > Subscriptions** and create a plan. Saving a plan
   creates the Stripe product. Saving a price creates the Stripe price.
   Stripe prices are immutable, so to change an amount, add a new price.
3. Forward events to your machine and copy the signing secret into `STRIPE_WEBHOOK_SECRET`:

   ```bash
   stripe listen --forward-to localhost:8000/checkout/webhook/
   ```

4. In the Stripe dashboard, enable the customer portal so **Payment method and invoices**
   works.
5. Restart the server, then subscribe with the test card `4242 4242 4242 4242`.

In production, add a webhook endpoint at `https://your-domain/checkout/webhook/` for
`checkout.session.completed` and `customer.subscription.created`, `updated` and `deleted`,
and use that endpoint's own signing secret.

As a safety net for missed events, run this from cron:

```bash
python manage.py sync_billing
```

## Make it yours

| To change | Do this |
| --- | --- |
| Product name | Set `SITE_NAME` in `.env` |
| Colours and type | Edit the token block at the top of `frontend/app.css`, then `npm run build:css` |
| Landing page copy | `Templates/Home/home.html` |
| Plans and prices | Django admin, **Subscription** |
| Currency | Set `BILLING_CURRENCY` in `.env` |
| Social login | Add the GitHub or Google client ID and secret to `.env` |

## Project layout

| Path | What lives there |
| --- | --- |
| `Core/` | Settings, URLs, and `billing.py`, the only module that talks to Stripe |
| `Subscription/` | Plans, prices, user subscriptions, pricing and billing pages, sync services |
| `Checkout/` | Start checkout, return page, Stripe webhook |
| `Customers/` | Link between a user and a Stripe customer |
| `Auth/` | Account settings, staff user list, idle logout |
| `Home/` | Landing page and dashboard |
| `Templates/` | `base.html`, the `public.html` and `app.html` shells, per-app folders, `allauth/` theme |
| `frontend/app.css` | Design tokens and components, built to `Static/Css/app.css` |
| `locale/` | Translations |

## Translations

```bash
python manage.py makemessages -l pt_BR -i "node_modules/*" -i ".venv/*" -i "staticfiles/*"
python manage.py compilemessages -l pt_BR
```

## Tests

```bash
python manage.py test
```

Stripe is mocked, so the suite needs no network and no keys.

## Deploy

```bash
npm run build:css
python manage.py collectstatic --noinput
python manage.py migrate
gunicorn -c gunicorn.conf.py Core.wsgi:application
```

Before going live, set:

- `DEBUG=False`
- `ALLOWED_HOSTS` and `BASE_URL`
- a long, random `SECRET_KEY`
- live Stripe keys and the production webhook secret
- `BEHIND_PROXY=True`, if a proxy terminates TLS
- `SECURE_HSTS_SECONDS`, once HTTPS works for every subdomain. It is off by default.

The app refuses to start with an `sk_test_` key while `DEBUG` is off.

## Stack

Django 5.2, django-allauth, django-unfold, Stripe, PostgreSQL (SQLite for local use),
WhiteNoise, Gunicorn, HTMX, Tailwind CSS v4, Instrument Sans and JetBrains Mono.
