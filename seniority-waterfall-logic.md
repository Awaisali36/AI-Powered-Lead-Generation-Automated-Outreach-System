# Seniority Waterfall Logic

## Overview

When approved companies trigger contact research, the system applies a strict
seniority hierarchy to ensure outreach always reaches the highest-level
decision-maker available — never defaulting to mid-level contacts when
a C-suite or VP is reachable.

---

## Priority Tiers

### Priority 1 — C-Suite & Founders
```
Titles: CEO, Founder, Co-Founder, Chief Executive Officer,
        Owner, Managing Director, President
Target: First 5 contacts from this tier if available
```

### Priority 2 — VP & Director Level
```
Titles: VP, Vice President, Director, Head of, SVP,
        Senior Vice President, Group Director
Target: Fill remaining slots if Priority 1 yields < 5 contacts
```

### Priority 3 — Senior Management
```
Titles: Manager, Senior Manager, Lead, Principal,
        Senior [Function] Manager
Target: Fill remaining slots if Priority 1 + 2 yields < 5 contacts
```

---

## Waterfall Rules

```
max_contacts_per_company = 5

Step 1: Search Apollo for P1 titles at company
  → Found >= 5: take top 5, stop
  → Found < 5:  take all, proceed to Step 2

Step 2: Search Apollo for P2 titles at company
  → remaining = 5 - p1_count
  → Found >= remaining: take top [remaining], stop
  → Found < remaining: take all, proceed to Step 3

Step 3: Search Apollo for P3 titles at company
  → remaining = 5 - p1_count - p2_count
  → Take up to [remaining], stop regardless
```

---

## Why This Matters

Sending outreach to a Marketing Manager at a company where the CEO is reachable
wastes the campaign and burns the company. The waterfall ensures:

- Highest decision-making authority is always contacted first
- No slots are wasted on mid-level contacts when senior ones exist
- The 5-contact cap prevents saturation of a single company
- Tier logic is applied per-company — not per-batch

---

## Apollo.io Search Parameters

```json
{
  "person_titles": ["CEO", "Founder", "Co-Founder"],
  "organization_ids": ["{{approved_company_id}}"],
  "contact_email_status": ["verified", "likely to engage"],
  "per_page": 5
}
```

Repeated per tier with title list updated accordingly.
Email status filtering ensures only contactable leads are returned.
