### Jonathan Alemandi

I keep a real business running on software, and I build the infrastructure that
lets AI agents operate it without breaking the accounting.

Those two things are the same job. Most of what I know about writing careful
code came from being the person who gets called when invoicing stops working on
a Monday morning.

---

### What I'm building

**[ALdía](https://github.com/jonalemndi2/ALdia)** — an open-source business
engine designed to be operated by AI agents through MCP.

It started as a replacement for a VB6 + Access system running a shop, and turned
into an attempt to answer a harder question: *what does a business system need
before it's safe to let an agent move real money through it?*

48 MCP tools, 237 tests, Apache 2.0. Runs on the shop's own PC, with the
internet down.

---

### Three decisions that say more than a résumé

**Money is never a float.** Every amount is an integer number of cents.

```python
sum([0.10] * 10)   # 0.9999999999999999
1234.56 * 0.21     # 259.25759999999997   ← the tax on a real invoice
```

That second line is a VAT amount that will not reconcile, on an invoice that
was already filed. → [`dinero.py`](https://github.com/jonalemndi2/ALdia/blob/master/backend/dinero.py)

**An agent's retry must not bill someone twice.** The obvious implementation —
check whether the operation ran, run it, then record it — leaves a window where
two simultaneous retries both pass the check. I wrote the test that proves it,
watched two payments land, and then closed it by reserving the operation id
*before* executing.
→ [`idempotencia.py`](https://github.com/jonalemndi2/ALdia/blob/master/backend/idempotencia.py)

**A migration that refuses to run is doing its job.** Rebuilding sixteen tables
on a live database, the safety check found a problem and rolled everything back
untouched. The bug was mine — a unique constraint that SQLAlchemy emitted as a
separate index, which the rebuild never executed. The database was fine because
the check existed.
→ [`migraciones.py`](https://github.com/jonalemndi2/ALdia/blob/master/backend/migraciones.py)

---

### How I work

I write down *why*, not *what*. The comments in that codebase explain which
alternatives were rejected and what broke before — because the next person to
touch it will otherwise "simplify" the thing that was load-bearing.

I state limits out loud. The US sales tax support in ALdía is one manual rate,
and the system says so in its own API response rather than letting someone
believe their filing is handled.

---

📍 Córdoba, Argentina · IT 
💬 Spanish / English · Issues and PRs welcome on any of my repos
