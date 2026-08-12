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

<table>
<tr>
<td width="50%" valign="top">

<a href="https://github.com/why-xdd/inferkit"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-inferkit.svg" alt="inferkit — batched, cached, canary-routed model serving. 16× throughput."/></a>

#### [`⬢` inferkit](https://github.com/why-xdd/inferkit)

The serving layer around a model you already have. Dynamic batching, result
caching keyed on model *version*, canary routing that rolls itself back when a
candidate exceeds its error budget.

**16× throughput and 13× lower p99** in a benchmark you can re-run — the README
prints the command.

</td>
<td width="50%" valign="top">

<a href="https://github.com/why-xdd/voicedata"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-voicedata.svg" alt="voicedata — prepare and audit speech datasets."/></a>

#### [`⬢` voicedata](https://github.com/why-xdd/voicedata)

Raw recordings into a dataset you can train on: resample, slice on speech,
normalise to **ITU-R BS.1770-4** loudness with true-peak limiting, find the
duplicates, and say what is wrong with the rest.

Near-duplicates are found by how a clip *sounds*, so a re-encode still matches.

</td>
</tr>
<tr>
<td width="50%" valign="top">

<a href="https://github.com/why-xdd/askdocs"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-askdocs.svg" alt="askdocs — local hybrid retrieval with a measured eval."/></a>

#### [`⬢` askdocs](https://github.com/why-xdd/askdocs)

Ask your own documents, locally. BM25 and vectors fused by reciprocal rank, so
it finds both `PAY_1004` and *"what happens when I reuse an idempotency key"*.

Ships `askdocs eval`, because "hybrid retrieval helps" is a claim, and the
numbers in the README are the measurement — including where it only ties.

</td>
<td width="50%" valign="top">

<a href="https://github.com/why-xdd/slowq"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-slowq.svg" alt="slowq — rank Postgres queries by the time they actually cost."/></a>

#### [`⬢` slowq](https://github.com/why-xdd/slowq)

One static Go binary over `pg_stat_statements`. Ranks by **total** time, because
the query worth fixing is almost never the slowest one — it is the 0.8 ms query
called five million times.

Explains each finding, and proposes indexes with the columns in the right order.

</td>
</tr>
<tr>
<td width="50%" valign="top">

<a href="https://github.com/why-xdd/botkit"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/card-botkit.svg" alt="botkit — an aiogram 3 starter with the boring parts already correct."/></a>

#### [`⬢` botkit](https://github.com/why-xdd/botkit)

The aiogram 3 starter I wish I had had: FSM forms you can cancel from any state,
i18n resolved in middleware with a CI parity check, roles that compare rather
than enumerate, and a broadcast paced under Telegram's rate limit.

Middleware order is documented, because it is load-bearing.

</td>
<td width="50%" valign="top">

<a href="https://queueviz.vercel.app"><img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/queueviz-demo.gif" alt="queueviz running at 150% offered load: three tenant lanes filling at different rates under weighted fair queueing"/></a>

#### [`⬢` queueviz](https://github.com/why-xdd/queueviz) &nbsp;·&nbsp; [**live →**](https://queueviz.vercel.app)

Queueing theory is taught as algebra and experienced as an outage. This is the
same thing as a live simulation: watch a queue build at 95% load, then watch it
*not* build once backpressure is on.

Above: fair queueing at 150% load, recorded from the running page. Seven
presets, zero runtime dependencies, 8.5 kB gzipped. **Live at
[queueviz.vercel.app](https://queueviz.vercel.app)** — no install, no signup.

</td>
</tr>
</table>

<img width="100%" src="https://raw.githubusercontent.com/why-xdd/why-xdd/main/assets/divider.svg" alt=""/>

## ⟡ &nbsp;Also

<table>
<tr>
<td width="50%" valign="top">

#### [`⬢` VoxShift](https://github.com/why-xdd/voxshift)

Real-time voice changer: phase-vocoder pitch shifting, effect presets, VU
meters, and virtual-mic output through VB-Cable so the changed voice works in
Discord and games.

<img src="https://skillicons.dev/icons?i=python&theme=dark" height="28" alt="Python"/>

</td>
<td width="50%" valign="top">

#### `⬢` FriendCards &nbsp;<sub>private · in production</sub>

A collectible-card Telegram bot with an in-game economy, trading, mini-games, an
admin panel and a WebApp frontend. Deployed on a VPS with live users and
zero-downtime schema migrations.

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
