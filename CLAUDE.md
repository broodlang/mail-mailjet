# mail-mailjet — guidance for Claude

Mailjet's Send API v3.1 as a `mail` `Adapter`. The builder, validation and the delivery
contract are `mail`'s — do not add any here. The module is `request` (the body),
`headers` (Basic auth) and `outcome` (reading the answer).

- Field names are Mailjet's, capitalised, as STRING keys (`"HTMLPart"`); `json/decode` on
  the way back yields keyword keys (`:Messages`, `:Status`), so the answer tests read
  `:MessageID` while the request tests read `"TemplateID"`.
- An absent field, never an empty one: Mailjet refuses `"HTMLPart": ""`.
- A 400's `Errors` array is the useful message; keep surfacing it verbatim.
- The `impl` names the ability bare via `(:use mail :only [Adapter])` — qualified it
  registers under a name dispatch never sees.
- Run the one test file (the full suite is blocked on this machine) and `nest format`
  before committing. No trailing `!`, no abbreviations.
