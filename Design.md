# DESIGN.md

# CyberHub Design System

**Product:** CyberHub  
**Feature:** Dazy  
**Status:** Draft v2 — aligned to PRD v2  
**Audience:** Security-curious, non-technical users (Phase 1); cybersecurity-student / portfolio reviewers and hackathon judges also evaluate Phase 2+ (MITRE tagging, hardening)  
**Primary use case:** Help people understand their personal security exposure through one connected, plain-language story.
### Phase Alignment (read before using this doc)

This design system now spans more ground than Phase 1 of the PRD. Sections 1–20 describe the Phase 1 experience (Breach Checker, Phishing Detector, Attack Path, Dashboard, Dazy) and remain the baseline voice and visual system for everything that follows. **Section 21** adds design treatment for Phase 2 (MITRE tags, email header analyzer, link checker, password lab, defense layers, transparent risk formula) and Phase 3 (red/blue toggle, scenario picker, PDF report, judge mode). Nothing in Phase 1 should visually change to accommodate later phases — new modules extend the system, they don't redesign it.

## 1. Design Direction

### Design Concept

**A calm security briefing from a character who knows how to explain things.**

CyberHub should not look like:

- A hacker terminal
- A SOC dashboard
- A compliance report
- A generic AI chat interface
- A fear-based scam-warning page
- A dark neon “cyberpunk” template

It should feel like a **personal risk briefing**: credible, approachable, focused, and slightly memorable.

Dazy is the emotional bridge between technical signals and everyday action. The interface should make the user feel:

1. “I understand what this means.”
2. “This is about my situation, not an abstract score.”
3. “I know what I can do next.”
4. “I am not being judged.”

### Product Personality

- Clear, not clinical
- Reassuring, not complacent
- Specific, not alarmist
- Human, not childish
- Curious, not invasive
- Honest about uncertainty and mock data

### Visual Keywords

- Editorial briefing
- Soft utility
- Signal mapping
- Warm intelligence
- Personal security
- Calm urgency
- Illustrated companion
- Evidence over decoration

### Anti-Patterns To Avoid

- Purple-to-blue gradients
- Matrix rain, hacker grids, skulls, locks everywhere
- Neon green terminal text
- Excessive glassmorphism
- Cards nested inside cards
- Every section using a rounded rectangle
- Huge hero text with no immediate action
- Color-only risk communication
- Faux AI language such as “Your digital aura is compromised”
- Treating mock breach data as real
- Displaying three unrelated score cards with no connection between them

The visual system should be intentional and restrained. The product's novelty should come from the **attack-path narrative and Dazy**, not from decorative effects.

## 2. Core Experience

### Primary User Journey

```text
Landing / intro
  -> Check exposure
  -> Review breach results
  -> Test phishing awareness
  -> View combined attack path
  -> Receive Dazy's advice
  -> Take one recommended action
```

The experience should be progressive rather than dashboard-first. Users should not encounter an empty dashboard full of disabled cards.

### Primary Experience Principle

**Show one useful conclusion at every step.**

After the email check:

> “Your email appears in 2 simulated breach records. The exposed data types suggest password reuse may be your biggest risk.”

After the phishing test:

> “You identified 2 of 4 suspicious messages. You may be most vulnerable to urgency-based scams.”

After combining both:

> “The most realistic path is a fake password-reset email sent after a breach exposes your email address.”

This keeps the interface connected to the product's differentiator: chaining signals into a realistic scenario.

## 3. Information Architecture

### Phase 1 Screens

1. **Welcome / Start**
2. **Breach Checker**
3. **Breach Result**
4. **Phishing Detector**
5. **Phishing Result**
6. **Attack Path Reveal**
7. **Dashboard / Personal Briefing**
8. **Advice Detail**
9. **Error and Partial-Completion States**

### Navigation Model

Use a simple stepper rather than a persistent app sidebar.

```text
01 Exposure
02 Phishing
03 Attack path
04 Your next step
```

The stepper should be visible on desktop and condensed on mobile.

Do not make users navigate through a conventional dashboard before they have completed the inputs. The dashboard is the result of the analysis, not the starting point.

### Header

Desktop:

- Left: CyberHub wordmark
- Center or right: progress stepper
- Right: `What happens to my data?`

Mobile:

- CyberHub wordmark
- Compact progress indicator
- Privacy link in an overflow or bottom section

The header should remain visually quiet. The product's main focus is the current question and Dazy's explanation.

## 4. Layout System

### Container

- Maximum width: `1180px`
- Reading width for explanatory content: `680px`
- Dashboard content width: `1100px`
- Horizontal page padding:
  - Desktop: `32px`
  - Tablet: `24px`
  - Mobile: `16px`

### Grid

Desktop dashboard:

```text
Main narrative column: 2fr
Supporting signal column: 1fr
```

Suggested layout:

```text
┌──────────────────────────────────────────────┐
│ Header / progress                            │
├───────────────────────────────┬──────────────┤
│ Attack path narrative         │ Dazy         │
│                               │ advice       │
├───────────────────────────────┴──────────────┤
│ Exposure signal │ Phishing signal │ Confidence │
├──────────────────────────────────────────────┤
│ Recommended actions                          │
└──────────────────────────────────────────────┘
```

### Vertical Rhythm

Use a generous but controlled spacing scale:

```css
--space-1: 4px;
--space-2: 8px;
--space-3: 12px;
--space-4: 16px;
--space-5: 24px;
--space-6: 32px;
--space-7: 48px;
--space-8: 64px;
--space-9: 96px;
```

Avoid using identical padding on every component. Large narrative sections should breathe more than compact metadata rows.

## 5. Visual Language

### Color Strategy

The interface should use a warm, light foundation with signal colors used sparingly.

```css
:root {
  --canvas: #f7f4ee;
  --surface: #fffdf8;
  --surface-muted: #eeeae1;
  --ink: #202421;
  --ink-muted: #68706b;
  --line: #d9d5ca;

  --teal-900: #123f3e;
  --teal-700: #1e6460;
  --teal-100: #d7ece8;

  --amber-700: #a96317;
  --amber-100: #f8e7c9;

  --coral-700: #ad493d;
  --coral-100: #f5d9d3;

  --green-700: #39704d;
  --green-100: #dcebdc;

  --focus: #1d69d5;
}
```

### Color Semantics

| Meaning | Color | Usage |
|---|---|---|
| Neutral / informational | Teal | Primary product color, progress, links |
| Low risk / action complete | Green | Positive findings and completed actions |
| Needs attention | Amber | Moderate exposure or uncertain results |
| High concern | Coral | High-risk findings and urgent actions |
| Mock data | Amber with label | Never communicate mock data through color alone |
| System error | Coral | API or validation errors |

Risk colors must always be paired with:

- A written label
- An icon or visual marker
- A supporting explanation

Never rely on red, yellow, or green alone.

### Surfaces and Borders

Use surfaces to separate content, not to create a stack of nested cards.

Preferred:

- One primary content surface
- One supporting surface for Dazy
- Flat sections separated by spacing and subtle rules
- Soft shadow only for elevated popovers or dialogs

Avoid:

- Three levels of nested cards
- Thick colored borders around every block
- Excessive pill-shaped containers

### Border Radius

```css
--radius-sm: 8px;
--radius-md: 14px;
--radius-lg: 22px;
--radius-pill: 999px;
```

Use:

- `8px` for inputs and compact controls
- `14px` for buttons and small panels
- `22px` for large narrative surfaces
- Pill shapes only for tags, statuses, and progress labels

## 6. Typography

### Recommended Type Pairing

Use a distinctive editorial display face with a highly readable sans-serif.

Suggested pairing:

- Display: `DM Serif Display` or `Fraunces`
- UI/body: `Manrope` or `Public Sans`

The display face should be reserved for:

- Main page titles
- Attack-path conclusion
- Dazy's key statements

The sans-serif should handle:

- Form labels
- Body copy
- Score details
- Metadata
- Buttons
- Error messages

Avoid using a monospace font as the primary brand typeface. Small monospace labels may be used for technical metadata, but CyberHub should not visually position itself as a terminal.

### Type Scale

```css
--text-xs: 12px;
--text-sm: 14px;
--text-md: 16px;
--text-lg: 20px;
--text-xl: 28px;
--text-2xl: 40px;
--text-3xl: 64px;
```

Suggested usage:

- Page title: `40px` desktop, `32px` mobile
- Section title: `28px`
- Attack-path conclusion: `32px` to `48px`
- Body: `16px` to `18px`
- Supporting text: `14px`
- Metadata: `12px` to `14px`

Maintain readable line lengths. Explanatory content should generally stay below `70ch`.

## 7. Dazy Character System

### Role

Dazy is not a mascot placed beside the interface. Dazy is the product's interpreter.

Dazy should:

- Explain results
- Signal emotional tone
- Make uncertainty visible
- Encourage the next action
- Reduce the intimidation of security concepts

Dazy should not:

- Pretend certainty where the system has partial inputs
- Make jokes about a user's risk
- Use fear or shame
- Deliver long unstructured AI responses
- Hide the mock-data disclosure
- Replace the actual result with personality

### Required Expressions

Phase 1 needs five static illustrations:

1. **Welcome**
   - Open posture
   - Curious, attentive expression

2. **Concerned**
   - Used for high exposure or high phishing risk
   - Calm concern, not panic

3. **Thinking**
   - Used while the attack path is being generated
   - Visualizes analysis without implying human-like certainty

4. **Relieved**
   - Used for low exposure or strong phishing performance
   - Still includes practical next steps

5. **Focused**
   - Used for the final action plan
   - Direct, confident, constructive

### Dazy Composition

Desktop:

```text
┌────────────────────────────────┐
│ Dazy illustration              │
│                                │
│ “Here’s what I think matters.” │
│                                │
│ Advice text                    │
│                                │
│ [Take this action]             │
└────────────────────────────────┘
```

Mobile:

- Illustration above or beside the opening sentence
- Speech bubble becomes a normal reading surface
- Keep Dazy visible without consuming the full viewport

### Speech Bubble Rules

Dazy's content should have three layers:

1. **Headline**
   - One sentence
   - Plain English
   - No jargon

2. **Reason**
   - One or two sentences explaining the signal chain

3. **Action**
   - One recommended action
   - Optional secondary action behind “See other steps”

Example:

> **A password-reset scam is the most realistic risk here.**  
> Your email appears in simulated breach records, and the phishing test suggests urgent account messages may be harder for you to spot.  
> **Start by turning on two-factor authentication for your main email account.**

## 8. Breach Checker

### Intro Copy

Avoid starting with a frightening claim.

Preferred:

> Check whether this email appears in CyberHub's demo breach data.

Supporting text:

> Phase 1 uses simulated data. It is designed to show how CyberHub works and does not confirm whether your email appeared in a real breach.

### Form

Fields:

- Email address
- Primary action: `Check exposure`
- Optional privacy link: `How we handle this`

Form behavior:

- Validate format locally
- Do not submit invalid values
- Never show the email back in full after submission
- Display a masked identifier when needed, such as `j•••@example.com`

### Loading State

Use a short, informative sequence:

```text
Checking the demo records
Comparing exposed data types
Preparing your exposure summary
```

Do not use fake progress percentages. Use an indeterminate indicator or stepped status text.

### Result Structure

Show the conclusion first:

```text
Moderate simulated exposure
2 simulated breach records
```

Then provide evidence:

- Breach name
- Simulated breach date
- Data types
- Source label: `CyberHub demo dataset`

Every result must include a persistent disclosure:

> Demo data: these results are simulated and do not confirm a real-world breach.

This should be visible in the main result region, not hidden in a tooltip or footer.

### Zero-Result State

Zero breaches should still feel useful.

Example:

> No matches in the demo dataset. That is a good signal, but it is not proof that your email has never appeared in a real breach.

Dazy should remain present and offer the next step:

> “Let’s test how well you spot the messages attackers might still send.”

## 9. Phishing Detector

### Entry Options

Present two clear modes:

- `Take the quick quiz`
- `Paste a suspicious message`

The quiz should be the primary path for the demo because it produces a user-skill score consistently.

### Quiz Design

Each question should show one message at a time.

Required controls:

- Message content
- Sender information
- Links shown as text, not automatically opened
- Choice: `Phishing` / `Probably safe`
- Optional `Why?` explanation after answering
- Progress indicator: `Question 2 of 4`

Do not overload the screen with a complete email client simulation. The goal is to test recognition, not create visual noise.

### Paste Mode

Input behavior:

- Large text area
- Visible character limit
- Count characters while typing
- Reject or truncate excessively large content
- Preserve pasted text locally only for the current session
- Never echo pasted sensitive content into analytics or logs

Helper copy:

> Paste the message text here. Remove personal details first if you prefer.

### Result Structure

Show two distinct concepts:

1. **Message assessment**
   - How likely the pasted message is phishing

2. **User detection skill**
   - How well the user identified quiz examples

Do not combine these into one ambiguous score.

Example:

```text
Message risk
Likely phishing

Your detection skill
2 of 4 identified correctly
```

The distinction should be explained in plain language:

> The first score describes the message. The second describes your answers.

### Phishing Result Dazy Copy

Weak result:

> “You are not bad at this. The tricky part was urgency language.”

Strong result:

> **Urgent messages are the pattern to slow down for.**  
> You identified the obvious scam, but messages that create time pressure were harder to judge.

Avoid shaming language such as “you failed” or “you are vulnerable.”

## 10. Attack Path Predictor

### Purpose

This is the emotional and conceptual center of CyberHub.

The attack path should transform separate measurements into a concrete, understandable sequence.

### Visualization

Use a horizontal path on desktop and a vertical path on mobile.

```text
Exposure
  ↓
Opportunity
  ↓
Message
  ↓
Likely action
```

Example:

```text
Your email appears in 2 demo breach records
  ↓
An attacker knows where to reach you
  ↓
They send a fake password-reset message
  ↓
Urgency makes the message easier to trust
```

Each step should include:

- A short title
- One explanatory sentence
- An icon or simple illustration
- A confidence label if applicable

### Narrative Headline

Use human language:

> The most realistic attack path is a fake account-reset message.

Avoid:

- “Threat vector identified”
- “Attack surface compromised”
- “AI-generated attack chain”
- “Critical cyber vulnerability”

### Partial Input

If only one module is complete, do not manufacture a complete attack path.

Display:

> We can make a better prediction after you complete the phishing check.

Then show a partial interpretation:

> So far, your demo exposure looks moderate. We still need to understand how you respond to suspicious messages.

The UI should clearly label the result as:

- `Partial assessment`
- `Lower confidence`
- `Complete the second check`

### Confidence

Confidence should be expressed in words, not fake numerical precision.

Use:

- `Early signal`
- `Moderate confidence`
- `Strongest available signal`

Do not display `87.4% likely` unless the underlying model genuinely supports that level of calibration.

## 11. Dashboard

### Dashboard Goal

The dashboard is a concise briefing, not an analytics workspace.

The first viewport should answer:

1. What is my biggest realistic risk?
2. Why does CyberHub think that?
3. What should I do now?

### Dashboard Hierarchy

1. Overall conclusion
2. Attack path
3. Dazy's explanation
4. Two supporting signals
5. Recommended actions
6. Method and privacy details

### Score Presentation

Use compact signal rows rather than three large decorative cards.

```text
Exposure
2 simulated records
Moderate signal

Phishing awareness
2 of 4 correct
Needs practice

Attack path
Password-reset message
Moderate confidence
```

The attack-path result should visually dominate the other scores because it is the product's differentiator.

### Recommended Actions

Actions should be concrete and ordered.

Example:

1. Turn on two-factor authentication for your primary email.
2. Change passwords reused across old accounts.
3. Pause before clicking urgent account messages.

Each action should have:

- A verb-led title
- One sentence of context
- Optional external link or instructions
- Completion state for the current session only

Avoid a generic list of “improve your security” tips.

## 12. Advice Generation And Fallback

### Generated Advice

LLM-generated advice should be treated as a content variation layer, not as the source of truth.

The interface must always have a valid fallback response.

### Fallback States

If the advice request:

- Times out
- Is rate-limited
- Returns invalid content
- Fails due to network issues

Show:

> Dazy's custom advice is taking longer than expected. Here is a practical next step based on your results.

Then display templated advice from the score buckets.

Do not show:

- Empty speech bubbles
- Raw API errors
- “Something went wrong” as the only message
- A spinner that never resolves

### Advice Length

Target:

- Headline: 8 to 14 words
- Explanation: 25 to 50 words
- Primary action: 1 clear action

Long generated paragraphs should be truncated or replaced with a concise summary.

## 13. Trust And Privacy

### Persistent Disclosure

Phase 1 must display the demo-data disclosure in at least three places:

1. Breach Checker before submission
2. Breach results
3. Dashboard source or methodology section

Recommended wording:

> Phase 1 uses simulated breach data for demonstration. These results do not confirm whether an email appeared in a real-world breach.

### Privacy Copy

Use direct language:

> We do not save your email after this session. CyberHub uses it only to produce the current demo result. If real breach data is added later, this privacy explanation must be reviewed before launch.

This statement should only be shown if the implementation actually satisfies it.

### Methodology Drawer

Include a collapsible section:

> How this assessment works

Contents:

- Breach results are simulated in Phase 1
- Phishing skill comes from quiz answers
- Attack path is rule-based
- Advice may be generated by an external language model
- Results are not a professional security audit

This is especially important because the UI should not imply that the attack-path logic is an autonomous intelligence system.

## 14. Interaction And Motion

Motion should clarify state changes and provide a sense of progression. It should not turn the security experience into entertainment.

### Recommended Motion

- Page entrance: subtle opacity and vertical translation
- Step transitions: short horizontal or crossfade transition
- Score reveal: number or bar animates once after content appears
- Attack path: path nodes reveal sequentially
- Dazy expression: crossfade or small scale transition
- Speech bubble: fade and rise slightly into place
- Success state: restrained highlight, no confetti

### Timing

```css
--motion-fast: 140ms;
--motion-standard: 220ms;
--motion-slow: 360ms;
```

Use `ease-out` for entrances and reveals.

Avoid:

- Bounce animations
- Continuous floating illustrations
- Pulsing warnings
- Excessive parallax
- Animating every score independently
- Delaying access to content for dramatic effect

### Reduced Motion

When `prefers-reduced-motion: reduce` is active:

- Remove path sequencing
- Remove scale transitions
- Use instant state changes or opacity only
- Keep all content and status changes visible

## 15. Responsive Behavior

### Mobile Principles

On mobile:

- Keep one question per viewport
- Convert horizontal attack paths to vertical timelines
- Move Dazy above the main advice text
- Keep the primary action visible without excessive scrolling
- Avoid side-by-side score cards
- Preserve the demo-data disclosure near the result

### Breakpoints

Suggested breakpoints:

```css
--breakpoint-sm: 480px;
--breakpoint-md: 768px;
--breakpoint-lg: 1024px;
```

At widths below `768px`:

- Stack dashboard columns
- Use full-width buttons
- Reduce page title size
- Make result surfaces less padded
- Keep form inputs at least `44px` tall

### Mobile Dashboard Order

```text
Overall conclusion
Dazy advice
Attack path
Exposure signal
Phishing signal
Recommended actions
Methodology and privacy
```

## 16. Accessibility

### Requirements

- WCAG AA contrast minimum
- Visible keyboard focus states
- Form labels always visible
- Error text associated with the relevant field
- No color-only risk indicators
- Minimum `44px` touch targets
- Logical heading hierarchy
- Screen-reader labels for score visuals
- `aria-live` for loading, completion, and error states
- Alt text for Dazy illustrations
- Do not expose private pasted content in accessible labels unnecessarily

### Score Accessibility

Visual:

```text
Moderate simulated exposure
```

Accessible equivalent:

```text
Exposure signal: moderate. Two simulated breach records were found.
```

Charts and meters should include text summaries. Avoid requiring users to interpret a gauge without a label.

### Form Errors

Use specific messages:

- `Enter an email address.`
- `Check the format and try again.`
- `This message is too long. Remove some text and try again.`

Avoid:

- `Invalid input`
- `Something went wrong`
- `That doesn't look right`

## 17. Component Inventory

### Core Components

- `BrandMark`
- `ProgressStepper`
- `EmailField`
- `PrimaryButton`
- `SecondaryButton`
- `DisclosureBanner`
- `DazyPanel`
- `SpeechBubble`
- `SignalRow`
- `ScoreLabel`
- `BreachResultList`
- `PhishingMessage`
- `QuizChoice`
- `AttackPath`
- `AttackPathNode`
- `ConfidenceLabel`
- `ActionList`
- `MethodologyDrawer`
- `ErrorNotice`
- `LoadingStatus`

**Phase 2+ components** (defined in detail in Section 21, listed here for inventory completeness):

- `MitreTag`
- `HeaderAnalysisPanel`
- `LinkCheckResult`
- `PasswordStrengthMeter`
- `DefenseLayerMap`
- `RiskFormulaDrawer`
- `RedBlueToggle`
- `ScenarioPicker`
- `PdfReportButton`
- `JudgeModeBanner`

### Component Rules

Components should support:

- Loading
- Empty
- Success
- Error
- Partial-input
- Reduced-motion
- Mobile layout

Avoid creating one-off visual treatments for each screen. The product should feel coherent even though each module has a different task.

## 18. Content Rules

### Voice

Dazy and the product should speak like a knowledgeable friend who works in security.

Use:

- “Here is what this means”
- “The strongest signal is”
- “This is worth checking”
- “The practical next step is”
- “We cannot confirm that from this result”

Avoid:

- “You have been hacked”
- “Your identity is at risk”
- “Threat detected”
- “Your digital life is exposed”
- “The AI has determined”
- “Critical vulnerability”

### Numbers

Always provide context for scores.

Bad:

> Risk score: 72

Better:

> Moderate exposure: 2 simulated breach records

If numeric scores are required internally, translate them into user-facing buckets:

- Low
- Moderate
- High
- Incomplete

### Uncertainty

Use honest qualifiers:

- `Based on the demo dataset`
- `This is an early signal`
- `We found no match in this dataset`
- `This does not prove your email was never breached`
- `The strongest available scenario is`

## 19. Implementation Guidance

### State Model

The UI should distinguish these states:

```text
idle
validating
loading-breach
breach-complete
loading-phishing
phishing-complete
partial-assessment
building-attack-path
attack-path-complete
loading-advice
advice-complete
advice-fallback
error
```

Do not infer state from missing data alone. Explicit state makes it easier to handle API failures and partial completion without rendering broken intermediate screens.

### Source Labels

Every breach result must carry a source identifier:

```text
source: "mock"
label: "CyberHub demo dataset"
```

When Phase 2 introduces real breach data, the visual language should change explicitly. The source label depends on which API ends up in use — the PRD's current plan is to try XposedOrNot (free, no key) first and fall back to HaveIBeenPwned (paid key) only if that proves insufficient. Both are valid "real" sources; what matters is that the label reflects whichever one is actually live:

```text
source: "real"
label: "XposedOrNot"
```

or

```text
source: "real"
label: "Have I Been Pwned"
```

Do not silently swap the source behind the same UI, and never show a "real" label if the backend is still serving mock data for that request (e.g. a fallback during an API outage).

### Analytics

Do not record:

- Raw email addresses
- Full pasted phishing messages
- Generated advice containing private text

If product analytics are added, prefer event categories such as:

- `breach_check_started`
- `breach_check_completed`
- `phishing_quiz_completed`
- `attack_path_viewed`
- `fallback_advice_shown`

## 20. Design Acceptance Checklist

### Trust

- [ ] Mock breach data is labeled before the user submits an email.
- [ ] Mock breach data is labeled in the result.
- [ ] Dashboard explains that Phase 1 is simulated.
- [ ] The product does not imply that the user has definitely been breached.
- [ ] Privacy copy matches actual implementation behavior.

### Usability

- [ ] The first screen has one obvious action.
- [ ] Invalid email input does not trigger an API call.
- [ ] Users can complete the demo without creating an account.
- [ ] Users understand the difference between message risk and user skill.
- [ ] Partial completion has a defined visual state.
- [ ] API failure never leaves an empty advice bubble.
- [ ] Huge pasted messages are capped with a clear message.

### Design Quality

- [ ] Attack path is visually more important than individual scores.
- [ ] Dazy explains rather than decorates.
- [ ] The interface does not rely on cyberpunk clichés.
- [ ] No screen is composed entirely of nested cards.
- [ ] Color is not the only risk signal.
- [ ] Typography supports both personality and readability.
- [ ] Motion is short, purposeful, and reduced when requested.

### Responsive And Accessible

- [ ] The full journey works at mobile widths.
- [ ] Attack path becomes a vertical sequence on mobile.
- [ ] Focus states are visible.
- [ ] Score summaries are available to screen readers.
- [ ] Error and loading states are announced.
- [ ] Touch targets are at least `44px`.
- [ ] Contrast meets WCAG AA.

### Phase 2 / Phase 3 (see Section 21)

- [ ] Every MITRE ATT&CK ID shown has been verified against attack.mitre.org, not pulled from memory or an old list.
- [ ] The Link Checker UI never implies the app "visited" or "opened" the link — copy reflects passive pattern analysis only.
- [ ] The Password Lab's "time to crack" figure carries a visible "estimate only" caveat.
- [ ] The risk formula weights shown in the UI match the actual weights used server-side (no decorative/placeholder numbers).
- [ ] Defense-layer mapping visually ties each fix-it quest to a specific attack-graph edge, not a generic tip list.
- [ ] Red/Blue toggle and scenario picker are clearly optional explorations, not required steps to reach a conclusion.
- [ ] Judge mode is visually distinct enough that no one mistakes the "Alex" persona for a real user's data.

## 21. Phase 2 / Phase 3 Modules

These modules extend the Phase 1 system. They reuse the same tokens, type scale, color semantics, and Dazy voice — none of them introduce a new visual language.

### 22.1 MITRE ATT&CK Tags

Each `AttackPathNode` (Section 10) can carry a small `MitreTag`.

- Visual: a compact pill using `--radius-pill`, `--text-xs`, set in the UI sans-serif (never monospace — this is not meant to read as a raw technical code dump).
- Content: technique name first, ID in parenthetical — e.g. `Phishing (T1566)`, not `T1566` alone. The plain-English name stays primary; the ID is supporting evidence for anyone who wants to verify it.
- Placement: beneath the node's one-sentence explanation, not above it. The narrative sentence is always read first.
- Tooltip/expand on tap: one line linking the technique to attack.mitre.org by name (not as a raw pasted URL in the main flow).
- Never shown until verified — if a technique's ID can't be confirmed, omit the tag rather than guess.

### 22.2 Email Header Analyzer

New screen, inserted as an optional deep-dive reachable from the Phishing Detector result, not a required step in the main stepper.

- Entry copy: `Paste the raw headers from a suspicious email to check for spoofing signs.`
- Input: large monospace text area (this is the one place monospace is appropriate — it's genuinely technical, pasted content).
- Result structure, same three-layer pattern as Dazy's advice:
  1. **Headline** — plain-English verdict: `This looks spoofed` / `No spoofing signs found` / `We couldn't fully parse these headers`
  2. **Evidence** — SPF / DKIM / DMARC pass-fail shown as short labeled rows (not raw protocol dumps), From-vs-Reply-To mismatch flagged if present
  3. **Action** — one next step, same tone as existing Dazy advice
- Malformed/incomplete headers: never show a raw parser error. Use the existing `ErrorNotice` pattern with specific copy, e.g. `These headers look incomplete — we can't confirm SPF/DKIM from what's here.`

### 22.3 Link Checker

Reachable from the Phishing Detector result or the Header Analyzer, same optional-deep-dive placement as 22.2.

- Entry copy must not imply the app visits the link: `Paste a link to check it for common warning signs` — not `Check this link` (too action-ish) or anything implying live resolution.
- Result: a short list of matched heuristics only — lookalike domain, shortened URL, odd TLD, IP-based address — each with a one-line plain-English explanation (`This domain is one character off from paypal.com`).
- No "safe" verdict ever given — only `No common warning signs found in this link's pattern`, which is honest about the method's limits (pattern-matching, not a live safety check).
- This is the one module where the UI should actively communicate what it is *not* doing (visiting the link) — a short `How this works` note belongs directly on this screen, not buried in the methodology drawer.

### 22.4 Password Lab

Reachable as its own optional module, same tier as 22.2/22.3.

- Two inputs in sequence, not simultaneous: strength check (zxcvbn, client-side) first, then an opt-in breach check (Pwned Passwords k-anonymity) as a separate action — never auto-submit a password the moment it's typed.
- Strength meter: a simple horizontal bar, color-paired with a text label (`Weak` / `Fair` / `Strong`) per the existing color-semantics rule — never color alone.
- "Time to crack" display is secondary to the strength label, smaller type (`--text-sm`), and must carry a persistent caption directly beneath it: `Estimate only — real-world cracking can be significantly faster.` This caption is not optional and not collapsible.
- Breach check result uses the same honest-uncertainty language as the rest of the product: `This password was found in N known breaches` / `Not found in the breach sets we checked` — never `This password is safe.`
- Reinforce length-over-complexity guidance in one short supporting line, consistent with Dazy's plain-language voice.

### 22.5 Defense Layers

Extends `ActionList` (Section 11) rather than introducing a new list pattern.

- Each `ActionList` item gains an optional `DefenseLayerMap` connector: a thin visual line or tag linking the action back to the specific `AttackPathNode` it breaks (e.g. `MFA` tagged against the `Likely action` node).
- On the attack path visualization itself (Section 10), a defense layer can be shown as a small intercepting mark on the relevant edge — kept subtle, consistent with the "evidence over decoration" keyword from Section 1. This is the one place a slightly more technical, diagram-like treatment is appropriate, since it's illustrating a mechanism, not a decorative flourish.

### 22.6 Transparent Risk Formula

A `RiskFormulaDrawer`, structurally identical to the existing `MethodologyDrawer` (Section 13) — collapsed by default, same disclosure-first spirit.

- Content: the actual weights used (`Password leaked: +30`, `Breach under 2 years old: +15`, etc.) as a simple labeled list, not a formula or equation — the audience should be able to read it without doing math in their head.
- This drawer must reflect the real server-side weights. If the weights change, this drawer must be updated in the same change — it's a trust surface, not static copy.

### 22.7 Red / Blue Toggle

A two-state `RedBlueToggle` control, placed near the top of the attack-path visualization (Section 10), not in the main header — it's a lens on the existing result, not primary navigation.

- Red state: the existing attack-path narrative, unchanged.
- Blue state: the same path, re-labeled from a defender's point of view — what a SOC would see at each step (e.g. a login anomaly alert at the "Likely action" node). Reuses `AttackPathNode` with a blue-state content variant, not a separate component.
- Toggle must default to Red (the existing, validated experience) and never auto-switch.

### 22.8 Scenario Picker

A `ScenarioPicker` offered only after the core attack path (credential stuffing / phishing takeover / SIM-swap) has been generated once — it's an exploration of "what if," not a replacement for the primary, data-driven result.

- Visual: three labeled options, plain cards (not the large narrative surface used for the main attack path, to signal these are hypothetical explorations rather than personalized findings).
- Each scenario reuses the same `AttackPath` component with different node content — no separate visualization system per scenario.

### 22.9 PDF Report

A `PdfReportButton` on the Dashboard (Section 11), placed with the recommended actions, not with the primary navigation.

- Exported content mirrors the Dashboard hierarchy (Section 11.2): conclusion, attack path with MITRE tags, prioritized actions.
- The PDF must carry the same demo-data disclosure as the live UI if mock data was involved in that session — the disclosure obligation doesn't disappear once content leaves the browser.

### 22.10 Judge / Demo Mode

A `JudgeModeBanner`, visually distinct (a persistent, differently-colored thin bar, not a modal) whenever the "Alex" persona is active.

- Copy: `Viewing a pre-loaded demo (Alex) — no real data is being used.`
- This banner must be impossible to miss or dismiss while judge mode is active — the entire point is that nobody mistakes this for a real result, including the presenter mid-demo.

## 22. Design References

This direction applies the following principles from the referenced repositories:

- **Impeccable:** establish durable product truth, separate product context from surface styling, avoid generic AI-generated frontend patterns, and use explicit quality checks. ([github.com](https://github.com/pbakaus/impeccable?utm_source=openai))
- **Taste-Skill:** use deliberate visual direction, vary layout and density intentionally, and avoid default “SaaS template” composition. ([github.com](https://github.com/Leonxlnx/taste-skill?utm_source=openai))
- **Emil Kowalski’s skills:** use motion selectively, prefer short ease-out transitions, respect reduced motion, and use divergent exploration when evaluating interaction patterns. ([github.com](https://github.com/emilkowalski/skills?ref=toools&utm_source=openai))
- **UI/UX Pro Max:** treat design as a system covering typography, color, responsive behavior, accessibility, interaction, and implementation guidance rather than as a single visual mockup. ([github.com](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill/blob/main/.claude/skills/ui-ux-pro-max/SKILL.md?plain=1&utm_source=openai))
- **Ponytail:** use the referenced visual-design workflow as an additional input for shaping the product’s visual identity and interaction details.

The final implementation should preserve the core product truth: **CyberHub is not selling a scary score. It is helping a person understand one realistic security story and take one useful next step.**