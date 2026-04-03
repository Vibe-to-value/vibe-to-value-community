# Backlog

**Last updated:** [YYYY-MM-DD]

**Default rule: new ideas go to Later.** Move an item to Now or Next only if the product would fail without it or a committed dependency requires it. Resist the pull to start new work before Now items ship.

---

## Now

*Committed. Work in progress or next up this session.*

- [ ] [Item — be specific enough that you know when it's done]
- [ ] [Item]

---

## Next

*Queued. Will start when Now items ship. Order matters — top item is first.*

- [ ] [Item]
- [ ] [Item]
- [ ] [Item]

---

## Later

*Ideas and possibilities. Not in scope yet. No commitment implied.*

- [ ] [Item]
- [ ] [Item]
- [ ] [Item]
- [ ] [Item]

---

## Done

*Completed items with dates. Keep this section — it's your evidence of progress.*

| Item | Completed |
|------|-----------|
| [Item description] | [YYYY-MM-DD] |
| [Item description] | [YYYY-MM-DD] |

---

## Working With This Backlog

**Adding items:** New ideas go to **Later** by default. Ask: "Would the product fail without this?" If yes, it can move to Now or Next. If no, it stays in Later until Now is clear.

**Promoting items:** Move an item from Later → Next → Now one step at a time. Only pull into Now when the current Now list is empty or nearly empty.

**Completing items:** Move from Now to Done. Record the completion date.

**Pruning:** Review Later regularly. Delete items that are no longer relevant. A backlog with 50+ Later items is a graveyard — keep it honest.

---

## Prompt: How to generate this document

```
I need to create a project backlog.

Project: [project name]
Today's date: [YYYY-MM-DD]

Here are the items I want to track:

Now (in progress or committed next):
- [Item]

Next (queued after Now):
- [Item]

Later (ideas, not committed):
- [Item]

Done (completed work):
- [Item] — completed [YYYY-MM-DD]

Generate a backlog using the standard Now/Next/Later/Done format. Apply the default rule: if I've placed anything in Now or Next that doesn't clearly need to be there, note it and suggest moving it to Later.
```

---

## Prompt: How to update this document

```
Here is my current backlog:

[paste backlog]

Here are the updates I need to make:

Items completed (move to Done):
- [Item] — completed [YYYY-MM-DD]

New items to add:
- [Item] — suggested bucket: [Now / Next / Later]

Items to promote:
- Move [item] from Later to Next

Items to remove or defer:
- [Item] — reason: [why removing or moving back]

Apply the default rule when placing new items: if a new item would go into Now or Next, confirm it belongs there. Update the "Last updated" date. Return the full revised backlog.
```
