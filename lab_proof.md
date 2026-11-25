# Lab 6 — GDPR Self-Audit of Retune

**Audit date:** 25 November 2026  
**System reviewed:** Retune, the bilingual wellbeing and routine-tracking app in the Lovable project named “Activities Hub”  
**Code version reviewed:** `a912852896a89dc367dfe967ae3a1c8efaa0331d`  
**Scope:** The built MVP and the pilot use confirmed by the project owner. Real pilot users entered personal data and used the group-sharing feature. The number of users and the exact data each user entered are not confirmed. This review does not cover future AI features.

## Data-processing brief for the project operator

Retune is a bilingual web app for tracking daily activities and personal routines. Users sign in with Google. They can create activities and goals, record progress, and enter sleep, energy, mood, and reflection information. The app calculates personal progress summaries and patterns from these entries. It does not provide a medical diagnosis or treatment recommendation. No AI model or AI API integration was found in the reviewed code.

The app also has a “Together” feature. The owner confirmed that pilot users used it. Users can join groups and choose activities to share. The shared check-in records the activity name, completion status, and date. Group profiles, membership, invitation codes, and supportive reactions are also stored. The in-app privacy text says that activity logs, mood, energy, goals, reflections, and badges are private unless the user chooses to share specific items. The code does not indicate that sleep, mood, or private notes are included in a shared check-in.

The app writes data to a Supabase/PostgreSQL backend. The code shows a Supabase project reference, but direct access to that project through the user’s Supabase account was denied. It may be managed through Lovable Cloud; this must be confirmed in the Lovable project’s Cloud settings. The backend’s region, processor agreement, transfer arrangements, and provider-side retention are unknown. No automatic deletion schedule or full account-deletion flow was found in the reviewed app code. Some individual activity logs and shared check-ins can be deleted.

**Conclusion:** Retune processes personal data. Some activity names, free-text entries, mood, sleep, and energy information could reveal health-related information. Whether any pilot entry is special-category health data under GDPR Article 9 requires contextual legal review. Browser storage also holds onboarding state and temporary onboarding answers before they are saved to the account.

## Evidence and confidence labels

- **Code-confirmed:** observed in the source code or database migrations for the reviewed commit.
- **Owner-confirmed:** stated by the project owner in this audit conversation.
- **Unknown:** not established by the source code or owner confirmation; verify before making a compliance claim.

## Phase 1 — Personal-data inventory

| Data category | Source | Purpose | Retention | Crosses or may cross the EEA? |
|---|---|---|---|---|
| Account identifier and email | Google sign-in and authenticated account | Sign-in, account access, displaying the signed-in email | Unknown; no app-level account-deletion flow found | Unknown; Google and backend locations/transfer terms must be checked |
| Activities and goals | Entered by the user: activity name/category, schedule, importance, versions, goal text, adaptation notes | Personal routine planning, reminders and progress summaries | No automatic expiry found; individual activity deletion exists | Unknown backend region |
| Activity logs | Entered by the user: date, completion status, duration, free-text note, completion type, energy level | Daily tracking, charts, milestone calculations and personal insights | No automatic expiry found; individual log deletion exists | Unknown backend region |
| Daily wellbeing entries | Entered by the user: sleep hours/quality, energy and mood | Personal wellbeing tracking and comparisons over time | No automatic expiry found | Unknown backend region; assess possible health-data processing |
| Reflection entries | Entered by the user: feeling, next step, what helped, activity link and timestamp | Personal reflection and journey summaries | No expiry or deletion period verified; the `reflections` table is used in code but its migration was not found in the reviewed repository | Unknown backend region |
| Group and sharing data | User profile name and optional avatar URL; group membership; invitation code; selected shared activity name/status/date; reactions | Group membership, user-selected sharing and supportive responses | No schedule found; users can delete some check-ins and reactions | Unknown backend region and any provider-side processing |
| Browser and technical data | Browser local storage for onboarding and authentication; external font request; possible Lovable runtime error reporting in preview | Remember onboarding, maintain a sign-in session, render fonts and diagnose errors | Browser data persists until cleared; provider log retention unknown | Unknown; external service locations and log retention need checking |

The owner confirmed that real pilot users entered data and used group sharing. The exact number of users and which of the wellbeing fields they completed remain unknown. The application code proves that the fields are supported; it does not prove that every field was used in the pilot.

## Phase 2 — Role map

| Entity | Likely role | Processing activity | DPA / transfer status |
|---|---|---|---|
| Retune project operator | Likely controller for the app’s purposes and means. Confirm whether the university or another organisation decided the pilot’s purposes or essential means. | Defines the app, collects pilot data, decides how users share check-ins | Controller identity and any university role are unverified |
| Lovable Cloud / Supabase backend | Likely processor for app database, authentication and hosting, if the project uses Lovable Cloud | Stores account and app records; authenticates users; serves database operations | Direct Supabase dashboard access was denied. Confirm backend type, DPA, sub-processors, region and transfer terms in Lovable Cloud settings |
| Google | Authentication provider; its precise GDPR role depends on the service terms and processing purpose | Google OAuth sign-in; may receive authentication and browser request data | Terms, role and transfer mechanism not checked |
| Google Fonts | External font service used by the app shell | Browser requests font assets; may receive connection metadata such as IP address | Role, transfer details and applicable disclosure not checked |
| Pilot users and group members | Data subjects; users may see check-ins that another user deliberately shares with their group | Enter personal data and, when chosen, receive shared check-ins and reactions | User notice and pilot records of sharing choices need verification |

## Phase 3 — Lawful-basis assessment

The app source contains a short privacy explanation but no complete privacy notice, terms, or documented lawful-basis assessment. The following are candidates to review, not legal conclusions. GDPR requires a lawful basis for each purpose; special-category data also requires an Article 9 condition where applicable.

| Purpose | Candidate basis | One-line assessment | Legal review? |
|---|---|---|---|
| Sign-in and core account features | Article 6(1)(b), contract, if valid terms make the processing necessary; otherwise reassess | The code uses Google sign-in, but no terms or contract explaining necessity were found. | **TBD — legal review** |
| Activity, goal and wellbeing tracking | Article 6(1)(b) or consent, depending on the actual service terms and user choice | Users enter the information for personal tracking; the app does not explain a legal basis or a defined retention period. | **TBD — legal review** |
| Optional group sharing | Article 6(1)(a), consent, is a candidate because users select which activity check-ins to share | Confirm that users received clear information, could refuse without losing core features, and could withdraw sharing. If shared entries reveal health data, assess an Article 9 condition separately. | **TBD — legal review** |
| Security and error diagnostics | Article 6(1)(f), legitimate interests, may be a candidate | Identify the actual events collected, assess necessity and impact, and complete a three-part legitimate-interests assessment before relying on this basis. | **TBD — legal review** |

## Phase 4 — Risk and rights analysis

**Special-category data (Article 9):** The app supports mood, energy, sleep, therapy-related activity names and free-text reflections. These are personal and potentially health-related. They are not automatically Article 9 data in every context, but entries may reveal a person’s health. The operator should identify the actual pilot fields and assess whether they reveal health information. If they do, a separate Article 9 condition is required; the current project evidence does not establish one.

**Automated decisions (Article 22):** The reviewed code calculates progress counts, milestones and simple rule-based insights. No decision with legal or similarly significant effects on a person was found. The app does not appear to make solely automated consequential decisions.

**DPIA screening:** Potential criteria include processing highly personal or possibly sensitive wellbeing data, evaluating a person’s routine over time, and systematic longitudinal tracking. Pilot scale and user age are unknown. Two or more criteria may apply, so document a DPIA screening and complete a DPIA before expanding the pilot if the screening confirms likely high risk. The EDPB explains that, in most cases, processing meeting two of its criteria should be assessed through a DPIA.

**Rights and operational friction:** Users can view and edit parts of their records and delete some activity logs and shared check-ins. The settings code provides sign-out but no full account-deletion or data-export flow. No documented process was found for access requests, correction, objection, or completing requests within one month. A manual process and a complete account-erasure procedure need to be defined and tested with synthetic data.

## Phase 5 — Law-stacking check

- **EU AI Act:** No AI system, model API, or AI feature was identified in the reviewed build. Treat the AI Act as not applicable to this version and reassess if AI is added.
- **ePrivacy:** The app uses browser local storage for onboarding state and pending answers, and uses an external Google Fonts request. No analytics SDK, tracking pixel, or cookie-consent flow was found in the reviewed source. Assess whether each storage item is strictly necessary for the requested service and provide clear information; check the external font request separately.
- **Data Act:** No connected product or IoT data was identified. The connected-product data provisions appear not applicable. Cloud portability and switching terms should still be checked with the backend provider.

## Phase 6 — Recommendation memo

**To: Project operator / legal counsel**

Retune should **proceed only with conditions** before collecting more pilot data. The current build handles identifiable account data, activity histories, free-text reflections, and daily mood, energy, and sleep entries. The project owner confirmed that real pilot users entered data and used group sharing. The code supports a user-selected sharing flow for activity names, completion status, and dates. The current record does not establish the lawful basis, any required Article 9 condition, the backend region, provider terms, or a retention and account-erasure policy.

First, identify the controller for the pilot and document each processing purpose, lawful basis, and any Article 9 condition. Confirm whether the university or another organisation helped determine the pilot’s purposes or essential means. Review the actual pilot notice and sharing language; the app’s short privacy screen does not by itself establish that users received all required information.

Second, confirm in the Lovable Cloud settings whether the backend is Lovable-managed or connected to a separately owned Supabase project. Record the database region, processor and sub-processor terms, international-transfer arrangements, and service log retention. The Supabase project reference in source code does not establish that the project belongs to the operator’s Supabase account.

Third, set a clear retention schedule and create an end-to-end process for access, correction, export and deletion requests. The reviewed app has no automatic expiry or full account-deletion flow. Complete a DPIA screening for the wellbeing data and longitudinal tracking; if the screening confirms likely high risk, complete a DPIA before any wider pilot.

Residual risks include health information inferred from activity names or free text, uncertainty about provider location and backup retention, and disclosure within groups if users misunderstand what a shared check-in reveals. This memo is a first-pass assessment, not a legal opinion, DPIA, or certification. Obtain legal advice before relying on it for compliance decisions.

## Reinforce — Accountability evidence

| Document or control | Status from reviewed material |
|---|---|
| Records of processing activities | Not found; create or confirm whether an exemption applies |
| Privacy information | Short in-app privacy text exists; full information notice and proof it was shown to pilot users are unverified |
| Processor agreement / DPA | Unknown; verify for Lovable Cloud or connected Supabase and relevant sub-processors |
| Legitimate Interests Assessment | Not found; needed only if legitimate interests is selected |
| DPIA | Not found; complete screening and document the decision |
| Retention schedule | Not found; define one for app records and separately document provider backups/logs |
| Rights-request workflow | Not found; define owner, identity check, response steps and one-month deadline tracking |
| Breach-response procedure | Not found; document detection, escalation and the GDPR notification assessment |
| Transfer mechanism | Unknown until backend region, provider terms and recipient locations are confirmed |

## Stretch — Data protection by design

| Design principle | Current state | Change needed |
|---|---|---|
| Data minimisation | **Unknown** — the app supports detailed mood, sleep, energy and free-text entries | Confirm each field is necessary; make optional fields clearly optional and avoid asking for clinical details |
| Purpose binding | **Partial** — privacy text says most data stays private; sharing is user-selected | Match each field to a stated purpose and ensure only selected check-ins are shared |
| Access controls | **Partial** — migrations enable row-level security and define own-data/group policies | Verify policies on the live backend and test with synthetic accounts in separate groups |
| Retention enforcement | **Fail / unknown** — no automatic expiry or full account-deletion flow found in reviewed code | Set retention periods and implement deletion that covers account, app tables, shared data and provider backups where possible |
| Data-subject rights | **Fail / unknown** — some item-level deletion exists; no full export or account-erasure workflow found | Provide a documented access, export, correction and erasure process and test it end to end |
| Incident response | **Unknown** — no 72-hour assessment and notification process found | Document incident ownership, escalation, evidence preservation and notification decisions |

## Lab proof — input, evidence and limitation

**Input reviewed:** The current Lovable project source at commit `a912852896a89dc367dfe967ae3a1c8efaa0331d`, including `src/lib/data.ts`, `src/lib/daily.ts`, `src/lib/journey.ts`, `src/lib/social.ts`, `src/lib/prefs.ts`, `src/components/Onboarding.tsx`, `src/routes/auth.tsx`, `src/routes/_authenticated/settings.tsx`, Supabase client configuration, and database migrations.

**Method:** Read-only source and migration inspection. No live database rows were queried. No pilot user records or credentials were copied into this file.

**Evidence produced:** The source shows Google OAuth sign-in; writes to activity, daily-entry, preference, reflection, badge and group-sharing records; local browser storage for onboarding and authentication; and user-selected sharing of activity names, status and date. The project owner separately confirmed real pilot use and use of group sharing.

**Final output:** This first-pass inventory, role map, lawful-basis candidates, risk analysis, recommendation memo, accountability checklist, and design checklist.

**Failure / limitation:** Direct Supabase Dashboard access was denied, so the live backend type, region, real data counts, provider retention, backups, and account-deletion configuration could not be verified. The repository includes code using a `reflections` table, but no matching migration was found in the reviewed repository. These points remain **TBD**; they are not treated as confirmed safeguards.

## References

1. GDPR, Regulation (EU) 2016/679, especially Articles 2, 4–6, 9, 12–22, 25, 28, 30, 32, 35 and 44: <https://eur-lex.europa.eu/eli/reg/2016/679/oj/eng>
2. European Data Protection Board, “Be compliant” and DPIA criteria: <https://www.edpb.europa.eu/sme/be-compliant/be-compliant_en>
3. European Data Protection Board, “Data controller or data processor”: <https://www.edpb.europa.eu/sme/learn-the-basics/data-controller-or-data-processor_en>
4. ePrivacy Directive, Article 5(3): <https://eur-lex.europa.eu/eli/dir/2002/58/art_5/par_3/oj/eng>
5. Data Act, Regulation (EU) 2023/2854: <https://eur-lex.europa.eu/eli/reg/2023/2854/oj/eng>
6. Supabase guidance on projects managed through Lovable Cloud: <https://supabase.com/kb/troubleshooting/cant-access-supabase-project-lovable-cloud>
