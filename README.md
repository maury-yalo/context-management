# Context Management for Yalo Code

Single source of truth for per-account context. Any Yalo Code agent loads from here to operate accounts with execution quality — no tribal knowledge required.

See full architecture: [Notion — Context Management for Yalo Code](https://www.notion.so/36dd53382b23819b9f7fc7b970ee2fbb)

---

## Folder structure

| Folder | What it holds | Owner |
|--------|--------------|-------|
| `products/` | Yalo product knowledge — capabilities, what each product supports, how it's meant to be used. | Product team |
| `accounts-and-flows/` | Per-account technical details — flows, integrations, Cloud Functions, custom configs, quirks. | Delivery + PMs |
| `customers/` | End-customer view — audiences, segmentations, campaign patterns, journeys, behavioral data. | CSMs + Ops |

---

## Accounts

| Account | customers/ | accounts-and-flows/ |
|---------|-----------|---------------------|
| Unilever HPC Ecuador | ✅ | — |
| Unilever ICE Ecuador | ✅ | ✅ knowledge-base |

---

## Products

`products/` is a git submodule pointing to [yalo-help-center](https://github.com/andressilva-yalo/yalo-help-center).

```bash
# Clone with submodule
git clone --recurse-submodules https://github.com/andressilva-yalo/context-management

# Update submodule to latest
git submodule update --remote products/
```
