# PRD: CyberHub (feat. Dazy)

**Status:** Draft v2
**Owner:** rudranshrautela9@gmail.com
**Timeline:** ~1 year, no hard deadline. Built in phases; early phases are hackathon-demo-ready, but no phase is scheduled against a fixed date.
**Pitch:** CyberHub models how an attacker chains your leaked data and phishing weakness into account takeover, maps it to MITRE ATT&CK, and shows how each defense breaks the chain.
---

## 1. Problem

People have no coherent picture of their own security exposure. Existing tools are single-purpose and disconnected — a breach checker here, a phishing quiz there — none of them chain the signals into "here is the realistic attack that would actually work on you, and here's what stops it." CyberHub's differentiator is that chain: exposure data + phishing susceptibility → a predicted, MITRE-mapped attack path → a visible defense layer that breaks it — explained by a character (Dazy), not a wall of jargon.

**Open risk carried from v1, now partly resolved:** the original plan assumed mock breach data because HaveIBeenPwned's lookup API now requires a paid key. **XposedOrNot** is a free, no-key alternative breach-check API and should be evaluated first — if its data quality is acceptable, it removes the mock-data compromise entirely. If it isn't, fall back to clearly-labeled mock data as before. This decision should be made in the first implementation hour of Phase 1, not deferred.

**Separately, Pwned Passwords (HIBP's k-anonymity password-check endpoint) is free and requires no key** — it's a different product from the breach-lookup API this PRD previously flagged as paywalled. It can be used for real, immediately, in the Password Lab (Phase 2).

## 2. Target Users

- **Primary (build/demo phase):** you, plus hackathon judges / early testers / cybersecurity-student peers — people evaluating both the *concept* and the *technical rigor* (this audience will check your MITRE IDs and your security hardening claims).
- **Primary (post-hackathon, ~6–12mo horizon):** security-curious but non-technical individuals who've heard of breaches and phishing but don't know their personal exposure.
- **Secondary (portfolio audience):** anyone reviewing this as a cybersecurity-student portfolio piece — recruiters, professors. This is why Phase 4 (hardening, README, CI badges) matters even though it adds no end-user feature.
- **Explicitly not targeted:** enterprises, anyone needing audit-grade/compliance-grade reporting, anyone needing this to detect real-time live phishing campaigns in production email flow.

## 3. Core Features, by Phase

### Phase 1 — Core (MVP)
- **Breach Checker** — email in → exposure data out (breach count, breached sites, data types leaked) → exposure score. Source: XposedOrNot if viable, else clearly-labeled mock data.
- **AI Phishing Detector** — user pastes an email or takes a quiz of real phishing samples → pretrained Hugging Face model (fine-tuning optional, evaluate pretrained quality first) scores phishing vs. safe, and scores the user's own detection ability, with suspicious words/phrases highlighted inline.
- **Attack Path Predictor** — deterministic, rule-based (not ML) logic combining both scores into a plain-English predicted attack scenario.
- **Dashboard + Dazy** — static illustrated character (4–5 expressions), reacts to results, delivers LLM-generated advice (free-tier API, templated fallback) in a speech bubble.
- **Terminal hack animation + attack graph** — visualizes the predicted attack path as a sequence/graph rather than only text.
- **Quiz, fix-it quests, rank card, demo persona "Alex"** — gamification layer and a safe pre-loaded demo identity so nobody has to type a real email live.

### Phase 2 — Security Depth
- **MITRE ATT&CK tags on every attack-graph node** (e.g., Phishing T1566, Valid Accounts T1078, Credential Stuffing T1110.004). **Every ID must be verified against attack.mitre.org before display** — this is a hard requirement, not a nice-to-have, given the audience.
- **Email header analyzer** — user pastes raw headers; parse SPF/DKIM/DMARC results, From-vs-Reply-To mismatch, and the Received chain. Combine with the ML phishing score into one verdict. Pure parsing, no external calls, no SSRF surface.
- **Link checker** — flags lookalike domains (edit-distance), shortened URLs, odd TLDs, IP-based links. **Constraint: no server-side following of redirects or live resolution of arbitrary user-submitted URLs** unless requests are restricted to an allowlist of known shortener domains — treat any "follow this link to check it" feature as a potential SSRF vector against your own backend, not just a feature.
- **Password Lab** — zxcvbn for strength scoring (client-side) + Pwned Passwords k-anonymity check (free, real, no key). "Time to crack" figures must carry a caveat — they routinely understate real-world GPU/ASIC cracking speed and will be scrutinized by this audience if presented as precise.
- **Defense layers** — each fix-it quest maps to a specific control (MFA, password manager, email filtering) and visibly shows which attack-graph edge it breaks.
- **Transparent risk formula** — the scoring weights are shown in the UI, not hidden, so the score is explainable rather than a black box.

### Phase 3 — Differentiation
- **Red team / Blue team toggle** — red shows the attack; blue shows the corresponding defenses/detections (what a SOC would see).
- **Multiple attack scenarios** — credential stuffing, phishing takeover, SIM-swap, each with its own graph/animation.
- **Downloadable PDF report** — score, MITRE-tagged attack chain, prioritized fixes.
- **Judge/demo mode** — "Try it live" button preloaded with the "Alex" persona so no real email is typed on stage.

### Phase 4 — Harden & Document (do this before calling anything "done," not after)
- Rate limiting, input validation (pydantic on the Flask/ML side; equivalent validation middleware on the Node side), strict CORS, security headers, HTTPS, no email storage or logging in Phase 1–3.
- Secrets in `.env`, never committed. `SECURITY.md` + a one-page STRIDE threat model in the README.
- CI: **split by language** — `bandit` + `pip-audit` for the Python/Flask ML bridge; `npm audit` + an eslint security plugin for the Node/Express backend. A handful of `pytest`/equivalent tests. A green CI badge is a cheap, strong signal — but only if it's actually covering both halves of the stack.
- README: GIF, one-line pitch, features, architecture diagram, MITRE mapping table, model metrics *with stated limitations*, threat model, security measures, live link, demo video, "what I learned."

### Phase 5 — Stretch (only if time remains; not scheduled)
Chrome extension for the link checker, a Telegram/Discord bot wrapping the phishing detector, a comparison of your fine-tuned model against the stock Hugging Face baseline.

## 4. User Stories

- As a user, I enter my email and see which breaches it appeared in (real via XposedOrNot, or clearly-labeled mock), so I understand my exposure at a glance.
- As a user, I take a short phishing quiz or paste a suspicious email, so I find out if I can actually spot a scam, with the suspicious parts highlighted.
- As a user, I paste raw email headers and get a plain-English verdict on whether they look spoofed, so I don't need to know what SPF/DKIM/DMARC mean.
- As a user, I see one combined "here's the attack that would likely work on you" graph with MITRE references, not disconnected scores, so the risk feels concrete and technically grounded.
- As a user, I see which specific action (MFA, password manager, etc.) breaks which step of the predicted attack, so the advice isn't generic.
- As a judge/demo viewer, I can click one button and see the full flow on a safe pre-loaded persona, without anyone typing a real email on stage.
- As a returning user (post-MVP), I want my previous scores saved, so I can see change over time. *(Requires a database — deferred, see Phase 6 note below.)*

## 5. Requirements

**Functional**
- Email input, basic format validation only (no ownership verification).
- Breach Checker: structured result (source, breach name, data types, date) from XposedOrNot or labeled mock data.
- Phishing Detector: phishing-likelihood score + user-skill score + inline highlighting of flagged phrases.
- Attack Path Predictor: deterministic rule table (documented, testable), tagged with verified MITRE ATT&CK IDs.
- Email header parser: SPF/DKIM/DMARC pass/fail, From/Reply-To mismatch, Received-chain summary — local parsing only, no outbound calls.
- Link checker: lookalike-domain detection (edit distance), TLD/IP heuristics — no live redirect-following unless allowlisted.
- Password Lab: zxcvbn (client-side) + Pwned Passwords k-anonymity (server relays hash prefix only, never full password).
- Dashboard: all scores + attack graph with MITRE tags + Dazy's reaction + LLM advice text + visible defense-layer mapping.

**Non-functional**
- No persistent storage of entered emails through Phase 1–3 (session-only, discarded on refresh/close).
- Visible "demo/simulated data" disclosure wherever mock data is used — not a footnote.
- LLM advice calls need a templated fallback on rate-limit/timeout.
- Rate limiting + input validation on every endpoint that accepts user text (phishing paste, headers, links) before any public deployment — unbounded text input on a free-tier/self-hosted backend is a cost and availability risk, not just a security one.
- CI security scanning split by language (see Phase 4).
- No server-side requests to arbitrary user-submitted URLs.

## 6. Edge Cases

- Empty/invalid email format → validation error, no API call.
- Email with zero breaches → Dazy still responds meaningfully (don't design only for the "bad news" path).
- Phishing quiz skipped, only breach checked (or vice versa) → Attack Path Predictor needs defined partial-input behavior (lower-confidence scenario, or a prompt to complete both).
- LLM API failure/timeout/rate-limit → templated advice fallback, keyed off score buckets.
- Large pasted text in phishing checker or header analyzer → needs a length cap.
- Malformed or incomplete email headers pasted → parser must fail gracefully, not crash the verdict.
- Shortened/obfuscated URL submitted to Link Checker → flagged by heuristic, never followed server-side.
- Mock data vs. real data confusion → visible, persistent "demo data" label if mock is used at all.
- MITRE ID mismatch or deprecated technique ID → verify against attack.mitre.org at build time; wrong citation is worse than omission in front of this audience.
- Privacy/legal exposure once public: processing real user emails (and potentially real phishing emails containing real third-party data) against external services. Needs a stated privacy note that is actually true, not aspirational.

## 7. MVP Scope (Phase 1)

**In scope:**
- Breach Checker (XposedOrNot if viable, else labeled mock)
- Phishing Detector (pretrained Hugging Face model, highlighted terms, quiz)
- Rule-based Attack Path Predictor with basic graph visualization
- Dashboard + Dazy + free-tier LLM advice with templated fallback
- Terminal hack animation, rank card, "Alex" demo persona
- No database, no auth, no persistence, session-only

**Explicitly deferred to later phases:**
- MITRE tagging, header analyzer, link checker, password lab, defense-layer mapping, risk-formula transparency (Phase 2)
- Red/blue toggle, multi-scenario attacks, PDF report, judge mode (Phase 3)
- Security hardening, CI scanning, threat model, portfolio README (Phase 4) — note: don't treat this as truly optional if the project will ever be public; treat it as "before public," not "if time remains"
- Browser extension, bot wrappers, model comparison (Phase 5)
- User accounts, saved history, database (Phase 6, see note below)

**Definition of done for Phase 1:** a user can enter any email, get a breach result + take the phishing quiz, see one combined attack-path narrative with a basic graph, and get advice from Dazy — end to end, no crashes, with an honest data-source disclosure visible on screen.

## 8. Note on Future Database (Phase 6, not yet scheduled)

No database through Phase 1–5. When one is added — triggered by wanting saved history, change-over-time tracking, or avoiding repeat paid-API calls — it will need, at minimum: user accounts/auth, per-session score history (breach score, phishing score, attack-path result, timestamp), and possibly a cache of breach-API responses. One addition to flag now: if the DB ever stores *which specific breaches* an email matched, that's more sensitive than the email alone — it's inferred personal security data, not just a login identifier — and should get its own access-control thinking when it's built, not retrofitted after.

## 9. Concrete Fixes (Implementation Notes)

These are the specific actions behind the risks flagged above. Each is tied to the phase it belongs to.

1. **MITRE ID verification (Phase 2).** Before any technique ID (e.g. T1566) goes on screen, look it up directly on attack.mitre.org and copy it from there — never from memory or an old list. Keep a small internal checklist/spreadsheet of every ID used in the app, so it's easy to re-check later if MITRE updates or deprecates one.

2. **Breach data source (Phase 1).** Test the XposedOrNot free API first with a handful of real email addresses and check the result quality. If it's solid, use it as the real data source for the Breach Checker. Only fall back to clearly-labeled mock data if it proves unreliable, rate-limited, or too thin.

3. **Password-check API (Phase 2).** Use the Pwned Passwords k-anonymity endpoint for real, no mocking needed. Implementation detail: hash the password locally, send only the first 5 characters of the hash to the API, and match the rest of the hash against the returned list — the full password and full hash never leave the user's side.

4. **Link checker scope limit (Phase 2).** Do not have the backend visit or follow submitted links. Keep detection passive: edit-distance checks for lookalike domains, regex/heuristics for odd TLDs, IP-based URLs, and known shortener patterns. This avoids turning the Link Checker into a server-side request forgery (SSRF) vector.

5. **Split security scanning by language (Phase 4).** Run `bandit` + `pip-audit` in CI for the Python/Flask ML bridge. Run `npm audit` plus an ESLint security plugin for the Node/Express backend. Both must be green before the CI badge is considered meaningful — one language's scan passing doesn't cover the other.

6. **Future DB field sensitivity (Phase 6, forward-looking only).** No action needed now. When the database is eventually built, treat any field that records *which breach* an email matched as more sensitive than the email itself — plan for encryption at rest and restricted read access on that field specifically, not just standard auth.

7. **"Time to crack" disclaimer (Phase 2).** Keep the zxcvbn-based estimate in the Password Lab, but add a short caption under it, e.g. *"Estimate only — real-world cracking can be significantly faster."* One line is enough to stay honest without reworking the feature.
