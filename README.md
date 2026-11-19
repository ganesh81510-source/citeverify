Citelens — Scientific Claim & Citation Verification System

Live Demo: https://ganesh81510-source.github.io/citeverify/

Demo Login Credentials: Username: expertise | Password: citeverify (or click Auto-Fill Demo Credentials on the login screen)

Problem Statement

Traditional academic integrity tools detect copied prose, not fabricated or misrepresented evidence. As AI-assisted writing increases the rate of hallucinated DOIs, inflated statistics, misattributed references, and cited retractions, reviewers need a transparent workflow to verify whether cited literature actually supports the claims in a document.

Solution Overview

Citelens (Prototype v2.4) is a privacy-first, client-side scientific claim and citation verification web application. Users upload a .pdf/.txt manuscript or paste cited text to cross-check inline claims and bibliography entries live against Crossref, OpenAlex, and DataCite, inspect side-by-side abstract evidence, and complete an auditable Human-in-the-Loop (HITL) review.

Key Features

Multi-Format Citation & Bibliography Parsing: Extracts claims linked to numeric brackets ([1], [1-3]) paired with a References / Bibliography / Works Cited section, Author-Year/APA citations ((Jumper et al., 2021)), inline DOIs (10.xxxx/...), and study attributions. Preserves line breaks during client-side PDF.js extraction so multi-line reference lists bind accurately.

Reference-Required Enforcement: Built strictly for scholarly citation auditing rather than general-knowledge trivia. Submissions without citations or with unmatched bracket markers ([1] without a References list) display a clear inline warning prompting the user to attach references instead of matching unrelated papers.

Multi-Registry Live API Lookup (Zero Exposed Keys): Resolves DOIs in parallel across Crossref, OpenAlex, and DataCite (Promise.allSettled), allowing arXiv/DataCite DOIs (10.48550/arXiv.*) to resolve cleanly while catching fabricated DOIs that return HTTP 404 across registries.

Misattributed & Irrelevant DOI Detection: Strips inline DOIs and URLs before extracting numeric figures (preventing DOI digits from being misread as statistics) and flags resolved DOIs whose title and abstract share virtually no topical overlap with the claim as Unsupported.

5-Status Verification Engine:

Supported — Verified work whose open abstract aligns with claim terminology and metrics.

Partly Supported — Verified work with missing/mismatched numeric figures (including percentages and comma-formatted counts) or publisher-restricted abstracts.

Unsupported — Fabricated/invalid DOI (404 across Crossref, OpenAlex, and DataCite), misattributed/irrelevant DOI, missing bibliography entry, or unindexed reference.

Contradictory — Directional polarity mismatch where the claim asserts efficacy or benefit while the abstract reports opposing, cautionary, or null-effect findings.

Possibly Outdated — Formally retracted publication (OpenAlex is_retracted / Crossref retraction metadata) or source older than 10 years.

Evidence & Audit Inspector: Displays the Verdict Banner alongside five structured sections: (1) Extracted Claim, (2) Matched Citation, (3) Source Evidence Passage (reconstructed from OpenAlex abstract_inverted_index or Crossref JATS XML), (4) Calibrated Confidence & Uncertainty disclosure, and (5) Protocol-validated Source Trace link.

Human-in-the-Loop Controls & Exportable Reports: Reviewers can Confirm Flag, Override / Mark Valid, add Reviewer Notes, inspect the timestamped Review History log, and export a formatted print/PDF Verification Report branded with the Citelens logo.

Built-In Stress-Test Demos & Accessibility: Includes one-click Demo 1 (5 embedded citation traps covering all 5 verdict types) and Demo 2 (cleanly cited excerpt), an animated brand welcome splash, 6 color themes (including Dark Mode and High Contrast), and full prefers-reduced-motion support.

Architecture & Security Controls

Tech Stack: Zero-backend, single-file static web app (HTML5, Tailwind CSS, Vanilla JavaScript, Lucide Icons, PDF.js) hosted on GitHub Pages. Uploaded documents are parsed locally in the browser; only extracted citation metadata/queries are sent to scholarly APIs.

No Hardcoded API Secrets: Uses public keyless academic REST endpoints (api.crossref.org, api.openalex.org, api.datacite.org).

XSS & Link Sanitization: All user input, reviewer notes, and external API metadata pass through escapeHTML(); all outbound links are protocol-validated (http:/https:) via sanitizeURL() and opened with rel="noopener noreferrer".

Input & Network Guards: Enforces .pdf/.txt validation, a 5 MB file-size cap, a 50,000-character limit, and 8-second AbortController timeouts per API request.

Authentication Scope (Disclosure): The entry login gate uses salted pure-JS SHA-256 hashing in browser storage with 5-attempt rate limiting (15-second cooldown) strictly as a local prototype session gate; production deployment replaces this with an OAuth2/JWT backend.

Scope, Limitations & Roadmap

Current Limitations: Verification runs at the abstract and metadata level using deterministic lexical, numeric, recency, retraction, and polarity heuristics. Paywalled full-text tables are flagged with lowered confidence rather than guessed, and uncited common-knowledge statements are intentionally rejected.

Next Phase: Add a server-side semantic Natural Language Inference (NLI) verification service, full-text open-access PDF ingestion (Unpaywall / CORE), OAuth2/JWT authentication, and LMS integrations (Moodle, Canvas, Google Classroom).

How to Test Locally or Online

Open the live demo at https://ganesh81510-source.github.io/citeverify/ (or open index.html directly in any modern browser).

Sign in with expertise / citeverify (or click Auto-Fill Demo Credentials).

Click Load Demo 1 to explore all 5 verdict types, or paste any cited passage (with [1] + References:, (Author, Year), or DOIs) and click Verify Citations.
