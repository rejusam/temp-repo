# Brainstorm: a "Protocol Explorer" for Dr Reju Sam John's field

## 1. What makes Protocol Explorer work

[protocol-explorer-alpha.vercel.app](https://protocol-explorer-alpha.vercel.app/), tagline *"Watch the internet work, packet by packet"*:

| Ingredient | How it shows up |
|---|---|
| **Lessons** | Sidebar of short lessons: DNS → TCP → TLS, each building on the last |
| **Live canvas** | Animated network of nodes with packets moving between them |
| **Event log** | Packet table: every message listed in time order |
| **Dissector** | Click any packet and see every byte labelled and explained |
| **Progress** | Lessons tick off as you finish them |
| **Reach** | 13 languages, works with no login, one-page app |

The core idea is to **make something invisible visible, one event at a time, and let people inspect each event.**

## 2. What fits your profile

Taken from your public GitHub (your site was blocked from this session):

- Computational epidemiology: SEIR models, metapopulation models (China), Lassa virus dynamics, bipartite contact networks
- Health AI evaluation: diabetic retinopathy screening simulation with an equity lens
- Aotearoa NZ focus: health equity, the ds-field-guide curriculum, healthcare quality dashboards
- Teaching: data science and AI courses, coders clubs
- Tools: Python, Jupyter, RAG over publications, TypeScript ("Ask My Papers")

Your version of "packets" could be **infections, contacts, patients or predictions**.

## 3. Ideas

### ⭐ Idea A: Outbreak Explorer *(recommended)*
> *"Watch an epidemic spread, one contact at a time."*

The same pattern as Protocol Explorer, applied to epidemics.

- **Canvas:** a contact network (households, schools, workplaces). Nodes change colour S → E → I → R. Each transmission is drawn as an animated "packet" along an edge.
- **Event table:** `t=3.2d | Node 14 → Node 22 | household | p=0.31 | infected`
- **Dissector:** click an event to see why it happened: contact type, transmission probability, how infectious the source was, susceptibility, and the β·S·I/N term behind it.
- **Lesson path:**
  1. One infection: what is R₀?
  2. SIR vs SEIR: why the latent period matters
  3. Networks beat averages: superspreaders and degree heterogeneity *(from your bipartite network work)*
  4. Herd immunity threshold: vaccinate nodes and watch chains break
  5. Interventions: isolation, distancing, contact tracing compared
  6. Metapopulations: spread between cities *(from your China SEIR repo)*
  7. Zoonotic spillover: rodent → human, Lassa *(from your Lassa repo)*
  8. Equity: same pathogen, unequal outcomes across NZ communities
- **Why it's strong:** it shows off your research, it teaches, it can be shared, and few people have built anything like it. A natural fit for your TasmanLab brand.

### Idea B: Model Dissector
> *"Open up an epidemic model, equation by equation."*

Each compartment in an SEIR or SEIRS model is a node, and people flow between them as particles. Click any flow to see the term (σE, γI…), its current value and what controls it. Sliders change parameters live, with the curves updating next to the animation. Less novel than A, but quicker to build and works well with lectures.

### Idea C: Screening Explorer (AI in the clinic)
> *"Follow 1,000 patients through an AI screening program."*

Based on your diabetic retinopathy evaluation repo. Patients pass through screen → AI grade → referral → specialist. Click a patient to see their features, the model score, the threshold and the outcome. Lessons cover sensitivity and specificity, base rates, threshold trade-offs, calibration, and **equity**: what happens when performance differs by ethnicity or region. Very relevant for health AI audiences and policymakers.

### Idea D: Base-Rate Explorer
> *"Why your intuition about probability is wrong."*

Based on the nz-car-theft and coin-toss repos. Each "packet" is one person or record passing through a test, with a dissector that applies Bayes' rule step by step. Lessons: false positives, Simpson's paradox, survivorship bias, regression to the mean. Broad appeal and good for the ds-field-guide.

### Idea E: Data Pipeline Explorer
> *"Watch a dataset travel from raw sensor to dashboard."*

Based on the cooling-system ETL repo. Records move through ingest → clean → feature → model → dashboard, and the dissector shows how each row changes at every step. More of an engineering showcase than a research one.

### Idea F: Research Explorer (portfolio)
> *"Explore my work as a network."*

Papers, methods, datasets and collaborators shown as a graph, with the RAG "Ask My Papers" built in. It would make a good portfolio centrepiece, but it's less of a teaching tool.

## 4. Comparison

| | Novelty | Shows off research | Teaching value | Build effort |
|---|---|---|---|---|
| **A Outbreak Explorer** | ★★★★★ | ★★★★★ | ★★★★★ | Medium-high |
| B Model Dissector | ★★★ | ★★★★ | ★★★★ | Low-medium |
| C Screening Explorer | ★★★★ | ★★★★ | ★★★★ | Medium |
| D Base-Rate Explorer | ★★★ | ★★ | ★★★★★ | Low |
| E Pipeline Explorer | ★★ | ★★★ | ★★★ | Medium |
| F Research Explorer | ★★ | ★★★★★ | ★★ | Medium |

**Recommendation:** build **A**, and design it so **B** and **C** can be added later as extra lesson tracks inside the same app ("Epidemics", "Models", "Health AI"). One platform with three tracks.

## 5. Outbreak Explorer: proposed MVP

**Stack:** Next.js (App Router) + TypeScript, Canvas/SVG for the network, a seeded simulation engine that runs entirely in the browser (stochastic Gillespie / discrete-time), deployed on Vercel or Cloudflare Pages. No backend.

**Layout (mirrors Protocol Explorer):**
```
┌───────────┬───────────────────────────────┬──────────────┐
│ Lessons   │   Network canvas (animated)   │  Overview    │
│ 1 R₀   ✓  │   ● ● ●──►●   ●               │  S  E  I  R  │
│ 2 SEIR    │      ●──►●  ●                 │  epi curve   │
│ 3 Networks│                               │  R_t gauge   │
├───────────┴───────────────────────────────┴──────────────┤
│ Event table: time | source → target | setting | p | ✓/✗  │
├──────────────────────────────────────────────────────────┤
│ Dissector: why did this transmission happen?             │
└──────────────────────────────────────────────────────────┘
```

**Milestones:**
1. **Week 1:** simulation engine (seeded RNG, SEIR on a network) + unit tests
2. **Week 2:** canvas animation + event table + play/pause/step controls
3. **Week 3:** dissector panel + lessons 1–3 + progress saved in localStorage
4. **Week 4:** interventions (vaccinate/isolate by clicking), lessons 4–5, share links, deploy

**Later:** metapopulation map of NZ regions, Lassa spillover track, te reo Māori + other languages, Screening Explorer track, "load your own network" (CSV edge list).

## 6. Open questions

1. Who is it mainly for: students, the public, or researchers and policymakers? This decides how mathematical the dissector should be.
2. Keep it fully separate, or make it part of the TasmanLab / personal site brand?
3. Should the lessons use real NZ data (e.g. regional populations), or stylised examples?
4. Name ideas: *Outbreak Explorer*, *Contagion Explorer*, *Transmission*, *R₀ Lab*, *Spread*.
