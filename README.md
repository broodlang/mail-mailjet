# mail-mailjet

[Mailjet](https://dev.mailjet.com/email/guides/send-api-v31/) as a
[mail](https://github.com/broodlang/mail) adapter: one email, one `POST /v3.1/send`,
authenticated with the account's API key and secret key.

```lisp
(def adapter (mail-mailjet/adapter {:api-key (os/env "MAILJET_API_KEY")
                                    :secret-key (os/env "MAILJET_SECRET_KEY")}))

(-> (mail/new :from "Hive <noreply@brood.fly.dev>" :to "ada@example.com"
              :subject "Your token" :text "…")
    (mail/deliver adapter))
;; → [:ok {:id 1234 :uuid "…" :href "https://api.mailjet.com/v3/REST/message/1234"}]
;;   [:error "mail-mailjet: 401 API key authentication/authorization failure. …"]
;;   [:error "mail-mailjet: 400 mj-0004 HTMLPart: Type mismatch. Expected type \"string\"."]
```

Everything `mail` can express is sent — To/Cc/Bcc, Reply-To, text and HTML parts, custom
headers, attachments and inline attachments (`:cid` becomes `ContentID`). Fields with
nothing in them are left out rather than sent empty.

## Options

Adapter: `:api-key`, `:secret-key`, `:sandbox?` (validate without sending), `:endpoint`,
`:transport` (a `(fn (url body headers) -> response)`, for tests).

Per email, through `mail/provider-option`: `:template-id`, `:template-language`,
`:variables`, `:custom-id`, `:custom-campaign`, `:deduplicate-campaign`, `:event-payload`,
`:url-tags`, `:track-opens`, `:track-clicks`, `:priority`, `:sandbox?` — each Mailjet's
field of the same name — and `:mailjet`, a map merged into the message as it is.

## Errors

The answer is read for what it is: a 200 carries a per-message `Status`; a 400 carries
`Errors` naming the field refused, and that text is what `[:error …]` carries; a 401 has
only a flat `ErrorMessage`. A connection failure or a non-JSON body is reported by status.

## Running

```bash
nest fetch
nest test tests/mail-mailjet_test.blsp
nest format
```
