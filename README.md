#!/usr/bin/env python3
"""
setup_profile.py  -  builds the whole GitHub profile by itself.

Run:     python3 setup_profile.py
Result:  folder "Mohamed-Hamed5" with
           README.md
           assets/banner.svg
           assets/terminal.svg
           .github/workflows/snake.yml
Then upload that folder's contents to your public repo "Mohamed-Hamed5".
"""
import os, random
from xml.sax.saxutils import escape

ROOT = "Mohamed-Hamed5"


def save(path, text):
    full = os.path.join(ROOT, path)
    os.makedirs(os.path.dirname(full), exist_ok=True)
    with open(full, "w", encoding="utf-8", newline="\n") as f:
        f.write(text)
    print("created", full)


# ------------------------------------------------------------------ banner.svg
def make_banner():
    W, H = 1000, 300
    PURPLE, CYAN, PINK = "#7C3AED", "#00F5FF", "#F472B6"
    nodes = {
        "me":  (500, 150, 16, CYAN,   "MOHAMED"),
        "ai":  (280, 68, 10, PURPLE, "AI"),
        "be":  (720, 68, 10, PURPLE, "BACKEND"),
        "cpp": (250, 215, 9, CYAN,   "C++"),
        "sys": (750, 215, 9, CYAN,   "LINUX"),
        "alg": (130, 140, 11, PINK,  "ALGORITHMS"),
        "net": (870, 140, 11, PINK,  "NETWORKING"),
        "sql": (500, 255, 8, PURPLE, "SQL"),
        "py":  (500, 42, 8, PURPLE,  "PYTHON"),
    }
    edges = [("me","ai"),("me","be"),("me","cpp"),("me","sys"),("me","sql"),("me","py"),
             ("ai","alg"),("ai","py"),("be","net"),("be","py"),("cpp","alg"),("sys","net"),
             ("sql","alg"),("sql","net"),("cpp","sql"),("sys","sql")]
    s = [f'<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 {W} {H}" width="100%" role="img" aria-label="Animated knowledge graph banner for Mohamed Hamed">',
         "<title>Mohamed Hamed</title>",
         "<defs>"
         '<linearGradient id="bg" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#05070F"/><stop offset="1" stop-color="#17152D"/></linearGradient>'
         '<radialGradient id="glow" cx="50%" cy="50%" r="50%"><stop offset="0" stop-color="#00F5FF" stop-opacity=".35"/><stop offset="1" stop-color="#00F5FF" stop-opacity="0"/></radialGradient>'
         '<filter id="blur"><feGaussianBlur stdDeviation="3"/></filter>'
         "<style>"
         ".e{stroke:#00F5FF;stroke-opacity:.35;stroke-width:1.2;stroke-dasharray:4 6;animation:flow 3s linear infinite}"
         "@keyframes flow{to{stroke-dashoffset:-40}}"
         ".n{animation:pulse 3s ease-in-out infinite;transform-box:fill-box;transform-origin:center}"
         "@keyframes pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.25)}}"
         ".l{font:600 10px 'JetBrains Mono',Consolas,monospace;fill:#C9D1D9;letter-spacing:1.5px}"
         ".t{font:800 40px 'JetBrains Mono',Consolas,monospace;fill:#fff;letter-spacing:4px;animation:glow 4s ease-in-out infinite}"
         "@keyframes glow{0%,100%{filter:drop-shadow(0 0 6px #00F5FF)}50%{filter:drop-shadow(0 0 16px #7C3AED)}}"
         ".s{font:500 15px 'JetBrains Mono',Consolas,monospace;fill:#00F5FF;letter-spacing:3px}"
         "</style></defs>",
         f'<rect width="{W}" height="{H}" fill="url(#bg)"/>']
    random.seed(7)
    for _ in range(60):
        x, y = random.randint(5, W-5), random.randint(5, H-5)
        d, b = round(random.uniform(2, 5), 1), round(random.uniform(0, 4), 1)
        s.append(f'<circle cx="{x}" cy="{y}" r="1" fill="#fff"><animate attributeName="opacity" values="0.1;0.8;0.1" dur="{d}s" begin="{b}s" repeatCount="indefinite"/></circle>')
    s.append('<circle cx="500" cy="150" r="170" fill="url(#glow)"/>')
    for i, (a, b) in enumerate(edges):
        ax, ay = nodes[a][:2]; bx, by = nodes[b][:2]
        dur = 3 + i % 3
        s.append(f'<line class="e" x1="{ax}" y1="{ay}" x2="{bx}" y2="{by}" style="animation-delay:-{i*0.4:.1f}s"/>')
        s.append(f'<circle r="2.5" fill="#fff"><animateMotion dur="{dur}s" begin="{i*0.35:.2f}s" repeatCount="indefinite" path="M{ax},{ay} L{bx},{by}"/>'
                 f'<animate attributeName="opacity" values="0;1;1;0" dur="{dur}s" begin="{i*0.35:.2f}s" repeatCount="indefinite"/></circle>')
    for k, (x, y, r, c, label) in nodes.items():
        dl = round(random.uniform(0, 2.5), 1)
        s.append(f'<circle cx="{x}" cy="{y}" r="{r*2.2:.1f}" fill="{c}" opacity=".18" filter="url(#blur)" class="n" style="animation-delay:{dl}s"/>')
        s.append(f'<circle cx="{x}" cy="{y}" r="{r}" fill="#080B16" stroke="{c}" stroke-width="2" class="n" style="animation-delay:{dl}s"/>')
        if k != "me":
            s.append(f'<text class="l" x="{x}" y="{y+r+16}" text-anchor="middle">{label}</text>')
    s.append('<rect x="255" y="112" width="490" height="76" rx="10" fill="#05070F" fill-opacity=".72"/>')
    s.append('<text class="t" x="500" y="153" text-anchor="middle">MOHAMED HAMED</text>')
    s.append('<text class="s" x="500" y="176" text-anchor="middle">CS STUDENT · AI · BACKEND · SYSTEMS</text>')
    s.append("</svg>")
    return "\n".join(s)


# ---------------------------------------------------------------- terminal.svg
def make_terminal():
    lines = [
        ("$ whoami",          "Mohamed Hamed, CS student @ Benha University"),
        ("$ cat focus.txt",   "AI  |  Backend  |  Linux & Networking"),
        ("$ ls projects/",    "banking-knowledge-graph/  warehousepro-ai/  second-chance/"),
        ("$ ./status --now",  "learning... building... improving...  [ OK ]"),
    ]
    CY, TW, TH = 15.0, 820, 290

    def typed(txt, t0, dt):
        out = []
        for k, ch in enumerate(txt):
            a = t0 + k * dt; b = a + 0.04
            kt = f"0;{a/CY:.4f};{b/CY:.4f};{(CY-0.5)/CY:.4f};1"
            out.append(f'<tspan>{escape(ch)}<animate attributeName="fill-opacity" values="0;0;1;1;0" keyTimes="{kt}" dur="{CY}s" repeatCount="indefinite"/></tspan>')
        return "".join(out)

    t = [f'<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 {TW} {TH}" width="{TW}" role="img" aria-label="Animated terminal introducing Mohamed Hamed">',
         "<title>Mohamed Hamed terminal</title>",
         "<defs><style>"
         ".m{font:500 15px 'JetBrains Mono',Consolas,'Courier New',monospace;white-space:pre}"
         ".p{fill:#00F5FF}.o{fill:#C9D1D9}.g{fill:#7EE787}"
         ".cur{animation:blink 1s steps(1) infinite}@keyframes blink{50%{opacity:0}}"
         "</style></defs>",
         f'<rect x="1" y="1" width="{TW-2}" height="{TH-2}" rx="12" fill="#0D1117" stroke="#7C3AED" stroke-width="2"/>',
         f'<path d="M1 13a12 12 0 0 1 12-12h{TW-26}a12 12 0 0 1 12 12v22H1z" fill="#161B22"/>']
    for i, c in enumerate(["#FF5F56", "#FFBD2E", "#27C93F"]):
        t.append(f'<circle cx="{26+i*22}" cy="20" r="6" fill="{c}"/>')
    t.append(f'<text class="m o" x="{TW/2}" y="25" text-anchor="middle" font-size="12" opacity=".7">mohamed@portfolio: ~</text>')
    y0, step = 72, 46
    for i, (cmd, out) in enumerate(lines):
        y = y0 + i * step; s0 = 0.5 + i * 3.0
        t.append(f'<text class="m p" x="24" y="{y}" xml:space="preserve">{typed(cmd, s0, 0.06)}</text>')
        so = s0 + len(cmd) * 0.06 + 0.25
        t.append(f'<text class="m {"g" if i == 3 else "o"}" x="24" y="{y+22}" xml:space="preserve">{typed(out, so, 0.02)}</text>')
    ly = y0 + 4 * step + 4
    t.append(f'<text class="m p" x="24" y="{ly}">$</text><rect class="cur" x="40" y="{ly-14}" width="9" height="18" fill="#00F5FF"/>')
    t.append("</svg>")
    return "\n".join(t)


# ------------------------------------------------------------------ README.md
README = r'''<!--
  ╔══════════════════════════════════════════════════════════╗
  ║       MOHAMED HAMED // DEVELOPER PROFILE                 ║
  ║       AI • SOFTWARE ENGINEERING • SYSTEMS                ║
  ╚══════════════════════════════════════════════════════════╝
-->

<div align="center">

<img src="assets/banner.svg" width="100%" alt="Mohamed Hamed: animated knowledge-graph banner"/>

<br/>

<img src="assets/terminal.svg" width="720" alt="Animated terminal: whoami, focus, projects, status"/>

<br/>

<a href="https://github.com/Mohamed-Hamed5">
  <img src="https://img.shields.io/badge/GitHub-Mohamed--Hamed5-0D1117?style=for-the-badge&logo=github&logoColor=00F5FF" alt="GitHub Profile"/>
</a>
<img src="https://img.shields.io/badge/FOCUS-AI%20%7C%20SOFTWARE-17152D?style=for-the-badge&logo=probot&logoColor=00F5FF" alt="Current focus"/>
<img src="https://img.shields.io/badge/STATUS-LEARNING%20%26%20BUILDING-0D1117?style=for-the-badge&logo=githubsponsors&logoColor=7C3AED" alt="Learning status"/>
<img src="https://komarev.com/ghpvc/?username=Mohamed-Hamed5&label=Profile%20Views&color=7C3AED&style=for-the-badge" alt="Profile views"/>

<br/><br/>

*"Turning curiosity into code, and code into solutions."*

</div>

---

## `01 // WHO AM I?`

```python
class MohamedHamed:
    def __init__(self):
        self.role = "Computer Science Student"
        self.university = "Benha University, Egypt"
        self.interests = [
            "Artificial Intelligence",
            "Software Engineering",
            "Backend Development",
            "Problem Solving",
            "Linux & Networking"
        ]
        self.mindset = "Learn by building"
        self.current_goal = "Build meaningful projects"

    def next_step(self):
        return "Learn -> Build -> Test -> Improve"
```

Hey! I'm **Mohamed Hamed**, a fourth-year Computer Science student at Benha University, Egypt.

* 🧠 Exploring AI and Machine Learning, with a focus on practical implementation.
* 💻 Building my programming foundation through hands-on projects.
* 🌐 Learning web technologies and backend development.
* 🐧 Exploring Linux, networking, and the systems behind modern software.
* 🛠️ Interested in connecting algorithms, data, and software into useful solutions.

I believe the best way to learn technology is to build something with it.

---

## `02 // TECH ARSENAL`

### Programming Languages

<p>
<img src="https://skillicons.dev/icons?i=cpp,c,python,java,php,r,js&theme=dark" alt="Programming languages"/>
</p>

### Web Development & Databases

<p>
<img src="https://skillicons.dev/icons?i=html,css,sass,js,nodejs,mysql,sqlite&theme=dark" alt="Web development and databases"/>
</p>

### Tools, Systems & Design

<p>
<img src="https://skillicons.dev/icons?i=linux,git,github,vscode,figma,bash,matlab&theme=dark" alt="Development tools"/>
</p>

| Area                     | Experience / Focus                           |
| ------------------------ | -------------------------------------------- |
| C++ & C                  | Programming fundamentals, problem solving    |
| Python                   | Fundamentals; building toward AI/ML          |
| Java, PHP & JavaScript   | Learning and practical development           |
| HTML, CSS & Sass         | Web development fundamentals                 |
| SQL & Databases          | Database fundamentals                        |
| R                        | Statistical computing                        |
| Linux & Git              | Command line and version control             |
| Cisco Networking         | Networking fundamentals and NetAcad learning |
| MATLAB, SPSS & Excel     | Scientific and statistical tools             |
| Figma & Microsoft Office | Design, documentation and productivity       |

---

## `03 // CURRENT MISSION`

<div align="center">

### 🧠 Project: AI & Machine Learning

**From Python fundamentals to practical intelligent systems.**

</div>

My current learning priorities:

* 📘 Strengthen Python and data handling.
* 📊 Understand NumPy, Pandas, and data preprocessing.
* 🤖 Learn Machine Learning concepts and model evaluation.
* 🧠 Explore neural networks and PyTorch.
* 🔗 Understand how AI models integrate with real software applications.

```text
DATA -> PREPROCESSING -> MODEL TRAINING -> EVALUATION -> INTEGRATION -> REAL-WORLD APPLICATION
```

My objective isn't just to run an AI model. It's to understand what it does, evaluate its results, and integrate it into a working project.

---

## `04 // PROJECTS & EXPERIMENTS`

> A growing collection of practical work, experiments, and things I'm learning.

### 🔬 Featured Work

**🏦 Banking Knowledge Graph** *(Graduation Project)*

A graph-native architecture for Egyptian banking use cases, exploring how connected data can power smarter banking solutions.

`Graph Databases` `AI` `Data Modeling`

**📦 WarehousePro AI**

A full-stack SaaS inventory management platform with an AI assistant, combining a modern web application, database operations, and AI-assisted features.

`React` `Node.js` `MySQL` `Software Engineering`

**📊 Smart Statistical Analysis System**

A statistical analysis dashboard built with R Shiny, designed to support data exploration, descriptive statistics, visualizations, and reporting.

`R` `Shiny` `Statistics` `Data Visualization`

**🌱 Second Chance**

An AI-focused project where I'm developing my understanding of how intelligent components can be designed and integrated into a larger application.

`Python` `AI/ML` `Data Processing`

<div align="center">

<a href="https://github.com/Mohamed-Hamed5?tab=repositories">
  <img src="https://img.shields.io/badge/EXPLORE-ALL%20REPOSITORIES-00F5FF?style=for-the-badge&logo=github&logoColor=black" alt="Explore my repositories"/>
</a>

</div>

---

## `05 // GITHUB TELEMETRY`

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=Mohamed-Hamed5&show_icons=true&hide_border=true&bg_color=0D1117&title_color=00F5FF&icon_color=7C3AED&text_color=C9D1D9&rank_icon=github" width="49%" alt="GitHub profile statistics"/>
<img src="https://streak-stats.demolab.com/?user=Mohamed-Hamed5&theme=transparent&hide_border=true&background=0D1117&ring=00F5FF&fire=7C3AED&currStreakLabel=00F5FF&sideLabels=C9D1D9&dates=8B949E" width="49%" alt="GitHub contribution streak"/>

<br/><br/>

<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Mohamed-Hamed5&layout=compact&hide_border=true&bg_color=0D1117&title_color=00F5FF&text_color=C9D1D9" width="45%" alt="Most-used programming languages"/>

</div>

---

## `06 // CONTRIBUTION MATRIX`

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Mohamed-Hamed5/Mohamed-Hamed5/output/github-contribution-grid-snake-dark.svg"/>
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Mohamed-Hamed5/Mohamed-Hamed5/output/github-contribution-grid-snake.svg"/>
  <img alt="GitHub contribution snake animation" src="https://raw.githubusercontent.com/Mohamed-Hamed5/Mohamed-Hamed5/output/github-contribution-grid-snake.svg" width="100%"/>
</picture>

</div>

---

## `07 // BEYOND THE CODE`

* 🧩 Breaking complicated problems into smaller, understandable pieces.
* 🔍 Exploring how systems work under the hood.
* 📚 Learning through experimentation, debugging, and iteration.
* 🚀 Turning academic knowledge into practical software.

<div align="center">

`LEARN` → `EXPERIMENT` → `BUILD` → `DEBUG` → `REPEAT`

</div>

---

## `08 // LET'S CONNECT`

<div align="center">

<a href="https://github.com/Mohamed-Hamed5">
  <img src="https://img.shields.io/badge/GitHub-Connect-0D1117?style=for-the-badge&logo=github&logoColor=00F5FF" alt="GitHub"/>
</a>
<a href="mailto:mohammadhamed5359@gmail.com">
  <img src="https://img.shields.io/badge/Email-Say%20Hello-0D1117?style=for-the-badge&logo=gmail&logoColor=EA4335" alt="Email"/>
</a>

<br/><br/>

**Interested in software, AI, collaboration, and meaningful technical challenges.**

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:17152D,100:00F5FF&height=100&section=footer" width="100%" alt="Animated neon footer"/>

<sub>Designed with curiosity. Powered by persistence. Built one commit at a time.</sub>

</div>
'''

# ------------------------------------------------------- snake workflow (yml)
SNAKE = r'''name: Generate Snake

on:
  schedule:
    - cron: "0 */12 * * *"
  workflow_dispatch:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
'''

# ----------------------------------------------------------------------- run
save("README.md", README)
save("assets/banner.svg", make_banner())
save("assets/terminal.svg", make_terminal())
save(".github/workflows/snake.yml", SNAKE)

print()
print("Done. Next steps:")
print("1. On GitHub create a PUBLIC repo named exactly: Mohamed-Hamed5")
print("2. Upload everything inside the folder '" + ROOT + "' (keep the folders).")
print("3. Repo Settings > Actions > General > Workflow permissions > Read and write.")
print("4. Actions tab > Generate Snake > Run workflow (once).")
print("5. Open github.com/Mohamed-Hamed5 and hard refresh (Ctrl+Shift+R).")
