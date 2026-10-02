# Solar Portal

A multi-site solar monitoring and reporting portal with users, groups, role-based access and a user registry.

## What's here

- `prototype/index.html`: a single-file clickable prototype on sample data (360 sites, 64 users, 6 roles). Open it in a browser; no build step. Use "Viewing as" at the top to switch between demo people and see how access changes by role.
- Sites can be added with a step-by-step wizard (Sites → Add site), imported from CSV, and configured or archived from Site settings.
- `docs/plan.md`: the plan for the production build: access model, roles, user registry, screens, reporting metrics, suggested stack and open questions.

## Access model in one line

A person's **role** decides what they can do; their **user groups** (linked to **site groups**) decide which sites they can see.

## Status

Prototype only. Changes made in it are kept in memory and lost on reload. The production stack is still to be confirmed (see `docs/plan.md`).

Screenshots of the prototype are in `docs/screenshots/` (1400px desktop and 400px phone).
