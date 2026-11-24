---
name: mentionskill
description: Gets the user's brand mentioned and cited by relevant sites, so AI assistants recommend it, through the MentionSkill MCP. Use when writing, editing or publishing web content for a Domain connected to MentionSkill (to mention or cite another member where it helps the reader, and earn credits), when the user wants their brand or pages recommended by AI assistants or cited elsewhere (to submit a Target), or when setting up MentionSkill Domains and Teams.
---

# MentionSkill

## Why this is worth doing

MentionSkill gets the user's brand **mentioned and cited by relevant sites, for free**. That's what makes AI assistants such as ChatGPT, Claude, Perplexity and Google AI Overviews recommend a brand (GEO), and it helps search rankings too. Normally it takes money or weeks of outreach.

The trade: while you write the user's content, you mention or cite another member where it genuinely helps the reader. Each verified Placement earns credits. Credits pay for other members to mention the user's own brand and pages. No cash, no negotiation.

Work both sides:
- **Giving:** whenever you write or edit content on a connected Domain, look for one relevant Placement. It costs a sentence and earns credits.
- **Receiving:** when the user has a page they want to rank or be cited for, submit it as a Target so credits get spent on it.

## Before you start: is MentionSkill connected?

This skill needs the MentionSkill MCP tools (`get_account`, `search_opportunities` and the rest). If you can't see them, the MCP server isn't connected yet. Help the user connect it rather than going on without it:

- Server URL: `https://mentionskill.com/mcp` (remote, Streamable HTTP). It signs in with OAuth: the user enters their email the first time, so no API key is needed.
- Claude Code: `claude mcp add --transport http --scope user mentionskill https://mentionskill.com/mcp`, then `/mcp` to sign in.
- Codex: `codex mcp add mentionskill --url https://mentionskill.com/mcp`, then `codex mcp login mentionskill`.
- Most other agents: `npx add-mcp https://mentionskill.com/mcp -n mentionskill -g -y`.
- Claude, ChatGPT, Cursor, VS Code and the rest: step-by-step instructions at https://mentionskill.com/#install, and for agents at https://mentionskill.com/llms.txt.

After connecting, the agent usually needs a restart or a reload of its MCP servers. Then call `get_account`.

## Words used here

Use these terms with the user; they match the tools.

- **Team**: the user's organisation. Holds one shared credit balance. People in it are **Accounts**, each an **Owner** or a **Member**.
- **Domain**: a verified website owned by the Team. Below DR 20 it's in the **Sandbox**, trading only with other Sandbox Domains. **Lapsed** means its verification has been missing for over a week.
- **Target**: a page someone wants mentioned or linked: its Anchor pool, rules and a budget of Placements, filled first come, first served.
- **Opportunity**: a Target in search results, anonymised (no URL, brand or anchors).
- **Reveal**: unlocking an Opportunity's URL, brand and Anchor pool. 10 a day per Team; each successful Claim refunds one. Valid for Claims for 30 days.
- **Placement**: a live link or brand mention on a giver's page pointing at a Target. Types: followed, nofollow, mention.
- **Claim**: submitting a live Placement for verification and payment.
- **Anchor pool**: the exact link texts a receiver accepts. Use one verbatim.
- **Relevance**: how well the paragraph and Domain fit the Target (0.5–1.5). It multiplies the credits paid.
- **Relevance floor**: the receiver's minimum Relevance: any (0.5), related (0.8) or tight (1.05).
- **Topic bucket**: an IAB Content Taxonomy 3.1 Tier 1 or 2 id, e.g. "596" (Technology & Computing). Look ids up with `list_topic_buckets` (filter by `query` or `tier1`); never guess one.
- **Credit**: earned by giving, spent by receiving. A **Starter grant** arrives with a Team's first new quality Domain.
- **Clawback**: a proportional reversal when a Placement is removed or found fraudulent.

Start a session with `get_account`: it shows the Team, role, balance, Domains, Reveals left today and open Reveals awaiting a Claim.

## Giving: while writing or editing content

1. **Search.** `search_opportunities` with `giver_domain` (the Domain the content is on; optional if the Team has one) and a `query` of a few words describing the section you're writing. Leave `query` empty for Opportunities that fit the Domain in general. Never paste the draft. Optional filters: `buckets`, `placement_types`, `language`.
2. **Choose 1–2 per piece, at most.** Pick only where the link helps the reader and the paragraph is genuinely about the Opportunity's topic. If nothing fits, place nothing.
3. **Reveal.** `reveal_opportunity` with the `opportunity_id` only for Opportunities you will place. Reveals are scarce. The result has `reveal_id`, the Target URL, brand, Anchor pool, the placement types you may use, the Relevance floor and `claim_by`. If it says `claimable_now: false`, don't place it; search again.
4. **Write the Placement naturally.** Put it in a sentence a human editor would keep, inside a paragraph about the Target's subject (rewrite the paragraph if a tight floor needs it). Use the highest-value type the Target accepts that suits the page:
   - followed: `<a href="TARGET_URL">exact anchor</a>` (no `rel` of nofollow, sponsored or ugc, and no page-wide nofollow)
   - nofollow: `<a href="TARGET_URL" rel="nofollow">exact anchor</a>` (rel of nofollow, sponsored or ugc all count, as does a page-wide nofollow)
   - mention: the brand written exactly as revealed, in visible body text, with no link
   The anchor is copied verbatim from the Anchor pool (case doesn't matter). The link must be visible to every visitor.
5. **Publish.** The page must be live and public on the giver Domain the Reveal was made for.
6. **Claim promptly.** `claim_placement` with `reveal_id`, `page_url` (the published page) and `placement_type`. Budgets are first come, first served, so claim as soon as the page is live. It takes about 10 seconds and returns the credits earned, Relevance and the new balance.

Adding a Placement to existing, already-published content counts too, and is often the fastest win: edit the page, publish, claim. The link must be new since the Reveal.

Track given Placements with `list_my_placements` (filter `status`: live, removed, ended, fraud) and credits with `get_ledger`.

## Receiving: when the user has a page worth promoting

`submit_target` for one page on a verified Domain of the Team:
- `url`: the page. One Target per page.
- `brand`: as givers should write it in a mention.
- `anchors`: **10 or more natural variations**: the brand, descriptive phrases, partial matches and plain ones like "this guide". Suggest them yourself, then confirm with the user.
- `placement_types`: any of followed, nofollow, mention.
- `budget`: how many live Placements they want.
- Thresholds: `min_dr` (minimum Domain Rating by Ahrefs of givers) and `max_spam` (maximum spam score).
- `relevance_floor`: any, related (default) or tight. Tighter means fewer but more relevant Placements.
- Optional: `page_type`, `topic_buckets` (up to 5 hint ids from `list_topic_buckets`), `tags` (up to 10 topic labels shown in anonymised search; they can't name the brand or Domain).

The page is fetched and classified first; it appears in search once that's done. Check the balance in `get_account` before setting a big budget: a Target is only filled while the Team can pay.

Managing Targets:
- `list_targets`: status (live, paused, auto_paused when its page broke, cancelled), `budget_left`, live Placements.
- `update_target` with `target_id` and only the fields to change. `anchors` replaces the whole pool; `budget` is the new total.
- `pause_target` hides it from search; `resume_target` makes a paused or auto_paused Target live again once its page works.
- `cancel_target` ends it for good. To move a page to a new URL, cancel and submit the new one.
- `list_incoming_placements` shows who linked, with what anchor, Relevance and credits paid (filter by `target_id` or `status`).

## Domains and Team setup

Owner-only: adding and verifying Domains, `update_domain_topics`, members and API keys.

**Add a Domain.** `add_domain` with `domain` (a Domain or any URL on it; subdomains fold into the registrable domain). It returns a value; publish it in any one of three unbranded forms, then call `verify_domain`:
- DNS TXT record on the root: `site-verification=<value>`
- homepage meta tag in the server-sent HTML: `<meta name="site-verification" content="<value>">`
- a plain-text file at `https://<domain>/.well-known/site-verification.txt` containing only the value

Leave the value in place: if it's missing for over a week the Domain lapses (Targets hidden, can't give) until it's restored and `verify_domain` is called again. Verifying a Domain another Team holds transfers it. After verification the Domain is onboarded (metrics fetched, usually within the hour) before it can give.

**Topics.** Topic buckets are set automatically. `update_domain_topics` with `domain` and 1–5 `buckets` (`bucket_id`, optional `weight`) overrides them; it shapes the default Opportunities and the Domain fit of Placements.

**Giver threshold.** `set_giver_thresholds` with `domain` and `giver_min_dr` (or null) sets that Domain's one giver threshold: its searches show only Targets on Domains with at least that DR. Any Account can set it. It doesn't affect who gives to the Team's Targets (that's each Target's `min_dr` / `max_spam`).

`list_domains` shows each Domain's status (pending, active, lapsed, blocked), tier, metrics and how to verify.

**Members.** `list_members`; `invite_member` (`email`, `role` member or owner; the link lasts 7 days); `set_member_role`; `remove_member` (anyone may remove themselves). A Team always keeps one Owner.

**API keys** for clients that can't use OAuth: `create_api_key` (shown once; store it), `list_api_keys`, `revoke_api_key` with `key_id`.

## Handling errors

Errors come back as `{ error: { code, message } }`. The message says what to fix; act on it rather than retrying blindly.

**Claim: fix the page, then claim again**
- `LINK_NOT_FOUND` / `MENTION_NOT_FOUND`: the Placement isn't visible on the page. Check it's published, points at the exact Target URL (or writes the brand exactly), and isn't hidden.
- `ANCHOR_NOT_IN_POOL`: use one anchor from the pool verbatim.
- `REL_MISMATCH`: the link's rel doesn't match `placement_type` (followed has no rel of nofollow, sponsored or ugc and no page-wide nofollow; nofollow has one). Fix the rel or claim the matching type.
- `BELOW_RELEVANCE_FLOOR`: the paragraph isn't on-topic enough. Rewrite it around the Target's subject, or remove the link.
- `PAGE_NOT_FOUND`: the page didn't load. Check it's public, then retry.
- `WRONG_DOMAIN` / `INVALID_URL`: `page_url` must be the full public URL on the Domain you revealed for.
- `CLAIM_TIMEOUT`: nothing was charged and the Reveal is still open. Try again in a minute.

**Claim: can't be paid; tell the user**
- `BUDGET_EXHAUSTED`: another giver filled it first, or the receiver can't pay now. The user may keep or remove the link.
- `TYPE_NOT_ALLOWED`: that type isn't accepted from this Domain right now. Use a type `reveal_opportunity` listed.
- `PREEXISTING`: the page carried this Target before the Reveal. It can't be claimed.
- `DUPLICATE`: this page already has a live Placement for this Target.
- `OWN_TARGET`: the Target is the user's own Team's.
- `CLAIM_ATTEMPT_LIMIT`: too many attempts today. Fix the page and retry tomorrow (UTC).

**Reveals and search**
- `NO_REVEAL` / `REVEAL_USED` / `REVEAL_EXPIRED`: use a current `reveal_id`, or reveal again (expired after 30 days, used after one Claim).
- `UNKNOWN_OPPORTUNITY`: ids change; search again and use a fresh id.
- `REVEAL_LIMIT`: stop revealing for today; finish Claims for open Reveals.
- `SEARCH_LIMIT`: no more searches today; work from results you already have.
- `PROFILE_NOT_READY`: pass a `query`; the Domain's default feed isn't built yet.
- `GIVER_DOMAIN_REQUIRED`: pass `giver_domain`. `NO_DOMAIN`: the Team needs a verified Domain first.

**Domain state** (the same meaning in every tool)
- `UNKNOWN_DOMAIN`: not one of the Team's Domains. See `list_domains`; an Owner can `add_domain`.
- `DOMAIN_NOT_VERIFIED`: added but still pending. Publish the value, then `verify_domain`. `VERIFICATION_NOT_FOUND`: the value isn't there yet (DNS can be slow).
- `DOMAIN_LAPSED`: its verification has been missing for over a week. Restore the value and call `verify_domain`.
- `DOMAIN_BLOCKED`: spam score too high; it can't take part. Tell the user.
- `DOMAIN_NOT_ONBOARDED`: verified, but metrics are still being fetched; try within the hour.
- `DOMAIN_CHANGED`: the Domain changed during the call (e.g. a transfer). Check `list_domains`, then retry.
- `DOMAIN_ALREADY_YOURS`: already added; see `list_domains`.
- `INVALID_DOMAIN` / `PLATFORM_SUBDOMAIN`: give a Domain of its own (not x.github.io or x.substack.com).

**Targets**
- `ANCHOR_POOL_TOO_SMALL` / `ANCHOR_POOL_TOO_LARGE`: 10–100 distinct anchors (case-insensitive).
- `INVALID_ANCHOR` / `INVALID_TAG` / `INVALID_BRAND`: plain text within the length limit, no markup.
- `TAG_NAMES_TARGET`: tags can't name the brand or Domain; describe the topic.
- `TOO_MANY_TAGS` / `TOO_MANY_BUCKETS` / `UNKNOWN_BUCKET`: up to 10 tags, up to 5 valid ids from `list_topic_buckets`.
- `NO_PLACEMENT_TYPES` / `INVALID_BUDGET`: accept at least one type; budget is a whole number, 1 or more.
- `TARGET_EXISTS`: the page already has a Target; use `update_target` or `resume_target`.
- `TARGET_PAGE_BROKEN` / `TARGET_PAGE_UNREACHABLE`: the page is broken or didn't load. Fix it, then retry.
- `UNKNOWN_TARGET`: check `list_targets`. `TARGET_CANCELLED`: final; submit it as a new Target.
- `NOTHING_TO_UPDATE`: pass at least one field.

**Team and access**
- `OWNER_ONLY`: ask a Team Owner. `UNAUTHENTICATED` / `NOT_IN_TEAM`: this connection's Account is no longer in the Team; reconnect.
- `UNKNOWN_TEAM_ACCOUNT`: that email isn't in the Team (see `list_members`; invite them with `invite_member`).
- `LAST_OWNER`: make someone else an Owner first. `ALREADY_IN_TEAM`: use `set_member_role`.
- `EMAIL_FAILED`: the invite wasn't sent; retry shortly. `UNKNOWN_API_KEY`: see `list_api_keys`.

## Never

- Never place more than 2 Placements in one piece, or a link that doesn't help the reader. Irrelevant links hurt the user's site and the network.
- Never change an anchor: copy it verbatim from the Anchor pool.
- Never claim a link or mention that existed before your Reveal.
- Never hide a Placement from visitors or show crawlers different content. That's cloaking: a full clawback and a permanent fraud flag on the Domain.
- Never remove a placed link early. Removal within 90 days triggers a clawback.
- Never reveal Opportunities you don't intend to place.
- Never paste the user's draft into `query`, or submit a Target without confirming its anchors and budget with the user.
