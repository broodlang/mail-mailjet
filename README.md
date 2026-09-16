# mail-mailjet — folded into `mail`

This lived as its own package for one day (0.1.0 on the registry, kept because
releases are immutable). From `mail` 0.2.0 it is the built-in module `mail/mailjet`:
https://github.com/broodlang/mail. Depend on `[mail :version "^0.2.0"]` and replace
`mail-mailjet/` with `mail/mailjet/` at the call sites.
