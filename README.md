<div align="center">

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/header.svg" alt="Artem — backend and ML engineer. Python, Go, real-time audio."/>

<br/>

<a href="mailto:why.not.live.alone@gmail.com"><img src="https://img.shields.io/badge/email-why.not.live.alone%40gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white&labelColor=0D1117" alt="email"/></a>
<a href="https://github.com/why-xdd?tab=repositories"><img src="https://img.shields.io/badge/repositories-7C7CE0?style=flat-square&logo=github&logoColor=white&labelColor=0D1117" alt="repositories"/></a>
<img src="https://img.shields.io/badge/open%20to-internships-34D399?style=flat-square&labelColor=0D1117" alt="open to internships"/>
<img src="https://komarev.com/ghpvc/?username=why-xdd&style=flat-square&color=7C7CE0&label=views&labelColor=0D1117" alt="profile views"/>

</div>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

## ⟡ &nbsp;About

<img align="right" width="26%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/monogram.svg" alt=""/>

I build the parts of a system that decide whether it stays up — queues,
batching, backpressure, rollout, and the metrics that tell you which of them
just broke.

Most of my work is **Python** and **Go** on the server side, with a detour into
**real-time audio DSP** that I enjoyed far more than expected.

- **`⬢` Services** — FastAPI, asyncio, aiogram 3. Typed, async all the way down, one transaction per request.
- **`⬢` ML serving** — dynamic batching, result caching, canary rollout with automatic rollback, Prometheus.
- **`⬢` Data** — PostgreSQL, Redis, SQLite, pgvector. Alembic migrations, and query plans I have actually read.
- **`⬢` Audio & signal** — phase vocoder, EBU R 128 loudness, VAD, mel fingerprinting, all on numpy and scipy.
- **`⬢` Shipping** — Docker, GitHub Actions, health and readiness probes. If it is not measured, it is not done.

<br clear="right"/>

<div align="center">
<img width="72%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/terminal.svg" alt="whoami: backend developer, Python · Go · TypeScript · SQL, FastAPI · aiogram 3, PostgreSQL · Redis, Docker · GitHub Actions"/>
</div>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

## ⟡ &nbsp;How I build

<div align="center">

*The shape almost everything here takes — clients on the left, data on the right,<br/>
and an async, typed application layer in between with its instrumentation underneath.*

<br/>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/architecture.svg" alt="Architecture: clients, an async application layer with middleware, queue and workers, and a data layer, with metrics and health probes underneath."/>

</div>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

## ⟡ &nbsp;Projects

<div align="center">

*Every one of these is real, working code with tests and CI — not a template with the name changed.*
<br/>
*Each README explains **why** the code is shaped that way, including the bugs that shaped it.*

</div>

<br/>

### [`⬢` inferkit](https://github.com/why-xdd/inferkit)

**The serving layer around a model you already have.**

<a href="https://github.com/why-xdd/inferkit"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-inferkit.png" alt="inferkit benchmark: 214 requests per second without batching, 3579 with it, 10598 with batching and cache; p99 falls from 304 ms to 21"/></a>

<sub>The benchmark, on one laptop core. Same model, same 1200 requests: **214 → 3 579 requests per second**, and p99 from **304 ms to 21**.</sub>

<a href="https://github.com/why-xdd/inferkit"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-inferkit2.png" alt="The canary panel before and after: the candidate goes from 10 percent weight to zero, tripped, after nine failed requests"/></a>

<sub>A candidate that raises on every call. Nine requests is all it takes: weight to zero, `tripped: true`, and every later request lands on the primary — no operator, no restart.</sub>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-inferkit.svg" alt="inferkit — scattered requests collapsing into one batch, 16 times the throughput"/>

Dynamic batching, result caching keyed on the model *version*, and canary routing
that rolls itself back the moment a candidate spends its error budget.

The batching window is measured from **now**, not from when the first request
arrived — anchoring it to the oldest request is the bug that makes a batcher look
like it works while giving you nothing. That one is written up in the README.

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

### [`⬢` voicedata](https://github.com/why-xdd/voicedata)

**Raw recordings in, a dataset you can actually train on out.**

<a href="https://github.com/why-xdd/voicedata"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-voicedata.png" alt="voicedata report: five files audited, three pass, a clipped phone recording fails and a mostly-silence room tone warns, each with its measurements"/></a>

<sub>Five recordings audited. The clipped phone take fails, the room tone warns — each with the number that condemned it, not a bare verdict.</sub>

<a href="https://github.com/why-xdd/voicedata"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-voicedata2.png" alt="voicedata dedup: one duplicate group found, interview_02.wav kept, similarity 1.000"/></a>

<sub>`interview_02_copy.wav` is a re-encode, so its bytes differ. It is caught anyway: clips are compared by a mel fingerprint — by how they *sound*.</sub>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-voicedata.svg" alt="voicedata — a waveform being cut on its silences, normalised to −23 LUFS"/>

Resample, slice on speech, normalise to **ITU-R BS.1770-4** loudness with true-peak
limiting, find the near-duplicates, and say plainly what is wrong with the rest.

The loudness path is the standard one, built from the analog prototype filters and
gated on 400 ms blocks, because a dataset normalised the naive way is quietly
inconsistent in exactly the way that hurts training.

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

### [`⬢` askdocs](https://github.com/why-xdd/askdocs)

**Ask your own documents. Locally, with nothing leaving the machine.**

<a href="https://github.com/why-xdd/askdocs"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-askdocs.png" alt="askdocs eval: lexical 88 percent recall at MRR 0.724, dense 94 at 0.731, hybrid 94 at 0.809"/></a>

<sub>The measurement, not the claim. **Hybrid ties dense on recall and wins on MRR** — the same answers, ranked higher, which is what survives a context window.</sub>

<a href="https://github.com/why-xdd/askdocs"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-askdocs2.png" alt="askdocs ask with ranks: onboarding.md is first at bm25 #2 and vector #1, while BM25's own top hit is pushed to second place"/></a>

<sub>The question lexical search cannot do. BM25 put `deployment.md` first; the vector ranking knew better, and fusion moved `onboarding.md` to the top.</sub>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-askdocs.svg" alt="askdocs — two rankings feeding a fusion box, then the merged list"/>

BM25 and embeddings fused by **reciprocal rank** — ranks, not scores, because BM25
scores and cosine similarities are not on the same scale and their spread changes
per query, so "0.6 lexical, 0.4 dense" describes nothing reproducible.

`askdocs eval` ships as a command so the claim can be checked on *your* corpus.
An earlier README claimed 100% here; that number turned out to be a hashing bug,
and the README now says so and reports the real one.

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

### [`⬢` slowq](https://github.com/why-xdd/slowq)

**One static Go binary that reads `pg_stat_statements` and tells you where to start.**

<a href="https://github.com/why-xdd/slowq"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-slowq.png" alt="slowq output: a 0.81 millisecond query taking 64 percent of all execution time, ranked above an 8.6 second report, each finding explained with a suggested index"/></a>

<sub>Ranked by **total** time. The query at the top runs in 0.81 ms — and accounts for **64% of everything the database does**, because it runs 4.8 million times.</sub>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-slowq.svg" alt="slowq — ranked by total time, the dominant bar being a fast query called constantly"/>

The query worth fixing is almost never the slowest one, and every tool that sorts by
mean duration hides it. Each finding is explained in a sentence you could paste into
a ticket, and index suggestions put equality predicates first and the `ORDER BY`
column last, so the B-tree can seek and the sort comes free.

Reads a live database or a JSON snapshot, so it also works where you cannot connect.

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

### [`⬢` botkit](https://github.com/why-xdd/botkit)

**The aiogram 3 starter I wish I had had.**

<a href="https://github.com/why-xdd/botkit"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-botkit.png" alt="The botkit feedback flow: category buttons, the message prompt, a confirmation screen and the thank-you, with a cancel button at every step"/></a>

<sub>A form with a way out of every state, including the confirmation — the state that starters usually forget.</sub>

<a href="https://github.com/why-xdd/botkit"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/shot-botkit2.png" alt="The botkit admin panel: role counts, a broadcast preview naming the audience size, and a delivery report of delivered, blocked and failed"/></a>

<sub>The admin side: roles that compare rather than enumerate, and a broadcast that previews before it sends, paces itself under Telegram's rate limit, and reports what actually landed.</sub>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-botkit.svg" alt="botkit — three FSM states lighting in sequence, with the cancel route beneath"/>

FSM forms, i18n resolved in middleware with a CI check that the locale files stay in
parity, role checks that compare rank instead of listing every role, and a broadcast
that survives contact with Telegram's limits.

Both screenshots are rendered from `locales/en.json` at build time, so the pictures
cannot drift from the strings the bot actually sends.

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

### [`⬢` queueviz](https://github.com/why-xdd/queueviz) &nbsp;·&nbsp; [**queueviz.vercel.app →**](https://queueviz.vercel.app)

**Queueing theory is taught as algebra and experienced as an outage.**

<a href="https://queueviz.vercel.app"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/queueviz-demo.gif" alt="queueviz at 150 percent offered load: three tenant lanes filling at different rates under weighted fair queueing, p99 latency passing 45 seconds"/></a>

<sub>Weighted fair queueing at **150% offered load**, recorded from the live page. Three tenants, three lanes — the one flooding grows its own queue and not the others'.</sub>

<a href="https://queueviz.vercel.app"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/queueviz-still.png" alt="queueviz at 90 percent load: queue depth 13, p50 777 milliseconds, p99 1.49 seconds"/></a>

<sub>And the knee. At **90% load nothing is "overloaded"** — yet p99 is already twice p50 and climbing. This is the curve capacity plans are written straight past.</sub>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-queueviz.svg" alt="queueviz — arrivals flowing through a queue into workers"/>

A live simulation you can push around: seven presets, four queue disciplines, four
admission-control strategies. Watch a queue build at 95% load, then watch it *not*
build once backpressure is on.

**8.5 kB gzipped, zero runtime dependencies, no framework.** Every run is seeded, so
the same settings always produce the same trace and two people can argue about the
same picture. No install, no signup — the link above is the whole product.

<div align="center">

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/stats.svg" alt="Across six repositories: 229 tests all green, 8741 lines of source and 2725 of tests, Python 62 percent, TypeScript 22, Go 14."/>

</div>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

## ⟡ &nbsp;Also

<table>
<tr>
<td width="50%" valign="top">

#### [`⬢` VoxShift](https://github.com/why-xdd/voxshift)

Real-time voice changer: phase-vocoder pitch shifting, effect presets, VU meters, and virtual-mic output through VB-Cable so the changed voice works in Discord and games.

<img src="https://skillicons.dev/icons?i=python&theme=dark" height="28" alt="Python"/>

</td>
<td width="50%" valign="top">

#### `⬢` FriendCards &nbsp;<sub>private · in production</sub>

A collectible-card Telegram bot with an in-game economy, trading, mini-games, an admin panel and a WebApp frontend. Deployed on a VPS with live users and zero-downtime schema migrations.

Source stays private; happy to walk through the architecture.

</td>
</tr>
</table>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

## ⟡ &nbsp;Stack

<div align="center">

**`◈` Languages**

<img src="https://skillicons.dev/icons?i=python,go,ts,js,bash&theme=dark" alt="Python, Go, TypeScript, JavaScript, Bash"/>

**`◈` Backend**

<img src="https://skillicons.dev/icons?i=fastapi,nodejs,nginx,redis&theme=dark" alt="FastAPI, Node.js, nginx, Redis"/>
<img src="https://img.shields.io/badge/aiogram_3-0D1117?style=for-the-badge&logo=telegram&logoColor=2CA5E0" alt="aiogram 3"/>
<img src="https://img.shields.io/badge/asyncio-0D1117?style=for-the-badge&logo=python&logoColor=22D3EE" alt="asyncio"/>

**`◈` Data**

<img src="https://skillicons.dev/icons?i=postgres,sqlite&theme=dark" alt="PostgreSQL, SQLite"/>
<img src="https://img.shields.io/badge/SQLAlchemy_2-0D1117?style=for-the-badge&logo=sqlalchemy&logoColor=D71F00" alt="SQLAlchemy"/>
<img src="https://img.shields.io/badge/Alembic-0D1117?style=for-the-badge&logo=python&logoColor=A78BFA" alt="Alembic"/>
<img src="https://img.shields.io/badge/pgvector-0D1117?style=for-the-badge&logo=postgresql&logoColor=F472B6" alt="pgvector"/>

**`◈` ML & signal**

<img src="https://img.shields.io/badge/NumPy-0D1117?style=for-the-badge&logo=numpy&logoColor=22D3EE" alt="NumPy"/>
<img src="https://img.shields.io/badge/SciPy-0D1117?style=for-the-badge&logo=scipy&logoColor=8CAAE6" alt="SciPy"/>
<img src="https://img.shields.io/badge/scikit--learn-0D1117?style=for-the-badge&logo=scikitlearn&logoColor=F7931E" alt="scikit-learn"/>
<img src="https://img.shields.io/badge/Ollama-0D1117?style=for-the-badge&logo=ollama&logoColor=E6EDF3" alt="Ollama"/>
<img src="https://img.shields.io/badge/SimPy-0D1117?style=for-the-badge&logo=python&logoColor=34D399" alt="SimPy"/>

**`◈` Shipping**

<img src="https://skillicons.dev/icons?i=docker,githubactions,git,linux,vscode&theme=dark" alt="Docker, GitHub Actions, Git, Linux, VS Code"/>
<img src="https://img.shields.io/badge/Prometheus-0D1117?style=for-the-badge&logo=prometheus&logoColor=E6522C" alt="Prometheus"/>
<img src="https://img.shields.io/badge/pytest-0D1117?style=for-the-badge&logo=pytest&logoColor=0A9EDC" alt="pytest"/>

</div>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

## ⟡ &nbsp;Activity

<div align="center">

<img width="94%" src="https://github-readme-activity-graph.vercel.app/graph?username=why-xdd&theme=tokyo-night&hide_border=true&bg_color=0D1117&color=7C7CE0&line=22D3EE&point=F472B6&area=true" alt="Contribution activity"/>

<br/>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/why-xdd/why-xdd/output/github-snake-dark.svg"/>
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/why-xdd/why-xdd/output/github-snake.svg"/>
  <img width="94%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/output/github-snake.svg" alt="Contribution snake"/>
</picture>

</div>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

<div align="center">

### Get in touch

Currently a CS student in Moscow, looking for a backend or ML internship.

<a href="mailto:why.not.live.alone@gmail.com"><img src="https://img.shields.io/badge/why.not.live.alone@gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0D1117" alt="email"/></a>
<a href="https://github.com/why-xdd"><img src="https://img.shields.io/badge/why--xdd-181717?style=for-the-badge&logo=github&logoColor=white&labelColor=0D1117" alt="GitHub"/></a>

<br/><br/>

<sub><i>Every project above ships with tests and CI. 229 of them, at last count.</i></sub>

</div>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/footer.svg" alt=""/>
