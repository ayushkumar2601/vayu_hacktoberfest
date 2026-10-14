# VAYU — Autonomous Cyber Reasoning System (CRS)
# Complete "How to Use" & Live Demo Playbook

> **Target Audience:** Hackathon Judges, Security Engineers, Open-Source Contributors, and Enterprise Evaluators.  
> **Repository Root:** `/Users/ayush/Desktop/hackto/vayu`  
> **Version:** 1.0.0 Production MVP  
> **Status:** 179 Automated Tests Passing  

---

## Table of Contents

1. [Executive Summary & The 30-Second Pitch](#1-executive-summary--the-30-second-pitch)
2. [Prerequisites & One-Time Environment Setup](#2-prerequisites--one-time-environment-setup)
3. [Demo Matrix: What to Run, Where to Run, and What to Say](#3-demo-matrix-what-to-run-where-to-run-and-what-to-say)
   - [Act 1: Autonomous CRS Terminal Loop (`vayu scan`)](#act-1-autonomous-crs-terminal-loop)
   - [Act 2: VAYU Skills CLI & Animated Installer (`vayu skills`)](#act-2-vayu-skills-cli--animated-installer)
   - [Act 3: Real-Time 3D Web Dashboard (Next.js & WebGL)](#act-3-real-time-3d-web-dashboard)
   - [Act 4: Model Context Protocol (MCP) Server for AI IDEs](#act-4-model-context-protocol-mcp-server)
   - [Act 5: Automated Verification Test Suite (179 Tests)](#act-5-automated-verification-test-suite)
4. [Cheat-Sheet: Copy-Paste Terminal Runbook](#4-cheat-sheet-copy-paste-terminal-runbook)
5. [Judge Q&A Defense: Hard Questions & How to Answer](#5-judge-qa-defense-hard-questions--how-to-answer)
6. [Troubleshooting Guide](#6-troubleshooting-guide)

---

## 1. Executive Summary & The 30-Second Pitch

### What is VAYU?
VAYU is an autonomous, local-first **Cyber Reasoning System (CRS)** inspired by the DARPA AI Cyber Challenge (AIxCC). Unlike traditional SAST scanners that only dump alerts into a developer's backlog, VAYU autonomously closes the loop:

$$\textbf{FIND} \longrightarrow \textbf{REASON} \longrightarrow \textbf{PATCH} \longrightarrow \textbf{VERIFY}$$

### The 30-Second Elevator Pitch (What to Say)
> *"Modern AI coding assistants are dangerous when applied to security: they hallucinate, rewrite entire files unpredictably, and never verify their fixes. Traditional static scanners, on the other hand, just drown developers in false-positive alerts.*  
>  
> *VAYU solves this. It's an autonomous Cyber Reasoning System where the AI **never has the final say**. VAYU finds the vulnerability with AST precision, reasons about exploitability using local models, synthesizes a minimal single-line patch, and proves the fix in an isolated 4-gate sandbox. If the code breaks tests or the vulnerability persists, VAYU rejects it fail-closed. Finally, it signs a cryptographic SLSA provenance attestation proving the fix is authentic."*

---

## 2. Prerequisites & One-Time Environment Setup

Open your terminal and ensure your local environment is configured:

### Step 1: Ensure Local Python & Node.js are Available
- Python **3.11+** installed (`python3 --version`)
- Node.js **18+** installed (`node --version`)
- Git installed (`git --version`)

### Step 2: Set Up Virtual Environment & Dependencies
In the repository root (`/Users/ayush/Desktop/hackto/vayu`):

```bash
# 1. Navigate to repository root
cd /Users/ayush/Desktop/hackto/vayu

# 2. Activate existing virtualenv
source venv/bin/activate

# 3. Ensure editable installation is active
pip install -e .
```

### Step 3: Configure Environment Variables
Verify or create `.env.local` in the project root:

```bash
# Display your local environment configuration
cat .env.local
```

Ensure `.env.local` contains:
```bash
export AIKAVACH_LLM_PROVIDER=ollama
export AIKAVACH_OLLAMA_URL=http://localhost:11434
export AIKAVACH_OLLAMA_MODEL=llama3  # or qwen2.5-coder:7b-instruct
```

Load the environment variables:
```bash
source .env.local
```

### Step 4: Ensure Ollama is Running (Optional for Model Mode)
If demonstrating with live LLM reasoning:
```bash
# In a background tab or daemon:
ollama run llama3
```
*(Note: If Ollama is offline or unavailable, VAYU seamlessly falls back to its deterministic offline rule engine without breaking the verification pipeline.)*

---

## 3. Demo Matrix: What to Run, Where to Run, and What to Say

---

### Act 1: Autonomous CRS Terminal Loop

This demonstrates VAYU's core continuous security loop running directly on a vulnerable target repository.

| Parameter | Details |
|---|---|
| **Where to Run** | Terminal 1 — Project Root (`/Users/ayush/Desktop/hackto/vayu`) |
| **Prerequisites** | `source venv/bin/activate && source .env.local` |
| **Command** | `vayu samples/vulnerable/command_injection`<br>*(or: `python -m crs.vayu samples/vulnerable/command_injection`)* |
| **Duration** | ~10 to 20 seconds |

#### What You Will See on Screen:
1. Large orange ASCII `VAYU` brand logo and model information (`Model: gemma 4`).
2. **Panel 1 — Vulnerability Detected:** Highlights CWE-78 Command Injection in `samples/vulnerable/command_injection/app.py` with syntax-highlighted code context.
3. **Panel 2 — AI Reasoning:** Root Cause and Security Impact analysis.
4. **Panel 3 — AI Generated Patch Proposal:** Clean unified diff replacing `shell=True` execution with a safe `subprocess.run([...], shell=False)` list.
5. **Panel 4 — Verification Engine:** Live animated progress checking syntax, regression tests, exploit regression, and static rescan.
6. **Panel 5 — Remediation Complete Table:** Green `✓ VERIFIED` badge confirming zero regressions and unchanged original repository.

#### What to Say (Word-for-Word Script):
> *"Let's look at VAYU running in the terminal on a real Python microservice containing a remote command injection vulnerability.*  
>  
> *Notice what happens when I run `vayu samples/vulnerable/command_injection`. First, Semgrep identifies the exact AST pattern at line 14. But instead of stopping there, VAYU's reasoning engine determines why it's exploitable.*  
>  
> *Next, look at the patch proposal. VAYU did not hallucinate a complete rewrite of the file. It synthesized a surgical, single-site patch that replaces the dangerous shell call with parameterized arguments.*  
>  
> *Now watch the 4 verification gates run in a disposable sandbox. VAYU tests compilation, executes unit tests, executes an exploit test to prove the vulnerability is gone, and rescans the code. The patch passed all four checks, so VAYU marked it `VERIFIED` and cryptographically signed the provenance."*

---

### Act 2: VAYU Skills CLI & Animated Installer

This demonstrates the new Anthropic-inspired **Skills CLI**, enabling developers to install modular, production-ready AI agent skills into their own workspaces.

| Parameter | Details |
|---|---|
| **Where to Run** | Terminal 1 — Project Root (`/Users/ayush/Desktop/hackto/vayu`) |
| **Prerequisites** | `source venv/bin/activate` |
| **Duration** | ~1 to 2 minutes |

#### Sequence of Commands to Run:

#### 1. Display VAYU Skills Banner
```bash
vayu skills
```
**What to Say:**
> *"Just like Anthropic introduced `npx skills add`, VAYU features its own dedicated Skills CLI with a custom terminal UI and full ASCII branding. Typing `vayu skills` shows our command group."*

#### 2. List the Skills Catalog
```bash
vayu skills list
```
**What to Say:**
> *"Running `vayu skills list` discovers all bundled skills as well as skills already installed in the current workspace. We ship with 8 production-grade engineering skills grounded in VAYU's architecture—from vulnerability remediation and SAST workflows to prompt injection defense."*

#### 3. Search Skills Intelligently
```bash
vayu skills search prompt
vayu skills search remediate
```
**What to Say:**
> *"The search command uses token and stem matching. Whether a developer searches for 'prompt' or 'remediate', VAYU immediately finds the relevant agent instructions."*

#### 4. Install a Skill with Animated Progress
```bash
vayu skills add security-analysis
```
*(If already installed, show conflict handling and `--force`):*
```bash
vayu skills add security-analysis --force
```
**What to Say:**
> *"When we run `vayu skills add security-analysis`, watch the smooth orange terminal animation. VAYU resolves the skill, validates YAML frontmatter, prepares the `.agents/skills/` directory, installs the standardized `SKILL.md`, and confirms the file count and scope. Notice the conflict prevention: VAYU will never overwrite existing code unless the user explicitly passes `--force`."*

#### 5. Inspect Skill Instructions Preview
```bash
vayu skills info security-analysis
```
**What to Say:**
> *"Using `vayu skills info`, developers can inspect the version, author, and live preview of the skill's instructions before using them in their AI editor."*

#### 6. Run System Health Diagnostics (Doctor)
```bash
vayu skills doctor
```
**What to Say:**
> *"The `doctor` command inspects project root detection, validates the `.agents/skills` tree, verifies global user directories, checks catalog integrity, and confirms zero corruption across all installed skills."*

#### 7. Install an External Skill from Git
```bash
vayu skills add frontend-design --from https://github.com/anthropics/skills
```
**What to Say:**
> *"VAYU can also safely pull external skills from public or enterprise Git repositories. It uses a shallow clone, checks for path traversal vulnerabilities, validates that a legitimate `SKILL.md` exists, installs only the requested skill, and cleans up temporary directories automatically."*

---

### Act 3: Real-Time 3D Web Dashboard

This demonstrates the production-grade Next.js 3D Web Dashboard visualizing codebase terrain, vulnerability hot-zones, and live streaming execution.

| Parameter | Details |
|---|---|
| **Where to Run** | **Terminal 2:** Inside `vayu-nextjs/`<br>**Browser:** `http://localhost:3000/dashboard` |
| **Command** | `cd /Users/ayush/Desktop/hackto/vayu/vayu-nextjs && npm run dev` |
| **Port** | `http://localhost:3000` |
| **Duration** | ~2 to 3 minutes |

#### What You Will See on Screen:
1. **3D Codebase Terrain:** A Three.js/WebGL point-cloud elevation map representing the target repository's file structure and code complexity.
2. **Vulnerability Hot-Zone:** Bright glowing orange elevation hotspot marking the vulnerable file (`app.py`).
3. **Toggle "System View":** Shows live architecture simulation: Untrusted Input $\rightarrow$ Flask Handler $\rightarrow$ `subprocess.run` $\rightarrow$ System Shell.
4. **Live Execution Stream:** Click the **"Run VAYU"** button (`R` hotkey) to stream real-time events over NDJSON (`crs.stream`).
5. **Cooling Hot-Zone:** Once the patch is verified, the glowing orange hot-zone visually cools down to stable blue/green.
6. **Provenance & SLSA Tab:** Displays the SHA-256 hashes, SARIF finding, and signed cryptographic attestation.

#### What to Say (Word-for-Word Script):
> *"Now let's switch to the VAYU Web Dashboard running on Next.js and Three.js. This gives security teams a visual mission control.*  
>  
> *Here on the main viewport is the **Codebase Terrain**. Every point represents a code entity, and dense, complex files rise into highlands. Notice this glowing orange peak? That's our vulnerability hotspot in `app.py`.*  
>  
> *If I toggle to **System View**, VAYU maps the data-flow graph: untrusted user input entering the web route and escaping into the system shell.*  
>  
> *Now I'll hit 'Run VAYU'. The backend immediately spawns our streaming pipeline. The LLM reasons about the flaw, synthesizes the fix, and runs sandbox verification. Look at the terrain—the orange hot-zone cools down in real-time as the verified patch lands! Over on the right panel, we have downloadable SARIF reports and a signed SLSA v1.0 attestation."*

---

### Act 4: Model Context Protocol (MCP) Server for AI IDEs

This demonstrates VAYU functioning as an enterprise **MCP Server**, allowing developers to invoke VAYU's verified remediation directly from Claude Desktop, Cursor, or Google Antigravity.

| Parameter | Details |
|---|---|
| **Where to Run** | Terminal 1 or AI IDE (Claude Desktop / Cursor / Antigravity) |
| **Command to Test** | `python -m vayu_mcp.server`<br>*(or console script: `vayu-mcp`)* |
| **Config File** | `docs/mcp-clients.md` |
| **Duration** | ~1 minute |

#### What You Will See on Screen:
The FastMCP server boots cleanly over standard `stdio`. Logging is routed exclusively to `stderr` so JSON-RPC protocol frames over `stdout` remain uncorrupted.

#### Key Tools Exposed to AI Assistants:
- `run_crs_pipeline`: Full end-to-end autonomous remediate & verify cycle.
- `scan_repository`: AST discovery of CVE/CWE vulnerabilities.
- `verify_patch`: Deterministic 4-gate verification of any candidate diff.
- `get_slsa_attestation`: Retrieve cryptographic SLSA provenance for a run.
- `list_supported_vulnerabilities`: Returns catalog of detectable CWEs.

#### What to Say (Word-for-Word Script):
> *"Developers shouldn't have to switch contexts to stay secure. That's why VAYU includes a native Model Context Protocol (MCP) server built with Python's official MCP SDK.*  
>  
> *By simply adding VAYU to Claude Desktop, Cursor, or Antigravity's config, developers can ask their IDE assistant: 'Analyze this repo with VAYU and propose a verified fix.'*  
>  
> *Behind the scenes, VAYU enforces strict workspace path validation, isolates stdio channels, runs the sandbox verification, and returns the verified diff. The AI assistant cannot hallucinate a broken fix because VAYU's deterministic harness sits in the loop."*

---

### Act 5: Automated Verification Test Suite

This proves VAYU's engineering quality, reliability, and regression safety.

| Parameter | Details |
|---|---|
| **Where to Run** | Terminal 1 — Project Root (`/Users/ayush/Desktop/hackto/vayu`) |
| **Prerequisites** | `source venv/bin/activate` |
| **Command** | `venv/bin/python -m pytest -q` |
| **Duration** | ~15 seconds |

#### What You Will See on Screen:
```text
........................................................................ [ 40%]
........................................................................ [ 80%]
...................................                                      [100%]
179 passed, 1 warning in 15.12s
```

#### What to Say (Word-for-Word Script):
> *"Security tools must themselves be held to the highest standard of verification. Every single component of VAYU is covered by automated tests.*  
>  
> *Running `pytest` executes **179 automated unit and integration tests** in 15 seconds. This includes testing our AST scanner, prompt injection firewall, cryptographic ECDSA signatures, MCP server protocol handshakes, and all 18 test cases for our new Skills CLI and animated installer."*

---

## 4. Cheat-Sheet: Copy-Paste Terminal Runbook

Keep this cheat-sheet open on a second monitor during your live demo:

```bash
# ==============================================================================
# WINDOW 1: MAIN TERMINAL (Repository Root)
# ==============================================================================
cd /Users/ayush/Desktop/hackto/vayu
source venv/bin/activate
source .env.local

# 1. Run the Autonomous Scanner CLI
vayu samples/vulnerable/command_injection

# 2. Demonstrate the VAYU Skills CLI
vayu skills
vayu skills list
vayu skills search remediate
vayu skills add security-analysis --force
vayu skills info security-analysis
vayu skills doctor

# 3. Run the complete automated test suite
venv/bin/python -m pytest -q

# ==============================================================================
# WINDOW 2: NEXT.JS DASHBOARD (Dashboard Directory)
# ==============================================================================
cd /Users/ayush/Desktop/hackto/vayu/vayu-nextjs
npm run dev

# Open Browser: http://localhost:3000/dashboard
# Press 'R' to trigger autonomous live run
```

---

## 5. Judge Q&A Defense: Hard Questions & How to Answer

### Q1: *"How does VAYU differ from GitHub Copilot or Devin proposing a code fix?"*
**Your Answer:**  
> *"Copilot and general coding agents operate with unconstrained generation. They rewrite entire files, frequently hallucinate non-existent libraries, and have zero regression verification. Most critically, they have no security gate.*  
>  
> *In VAYU, the LLM is restricted to emitting a minimal `PatchEdit` intent (specifying target line and code). That intent is constructed into a diff by trusted Python code, applied inside an isolated temporary sandbox, and subjected to 4 deterministic gates: compile check, functional test suite, exploit test, and static rescan. If any check fails, the patch is rejected fail-closed. The AI never has the final say."*

### Q2: *"What stops a malicious repository from performing prompt injection against VAYU?"*
**Your Answer:**  
> *"VAYU implements **Invariant 2: Repository Content Is Untrusted Data**. We use our Evidence Builder and Prompt Firewall to wrap all extracted source code inside strict `<UNTRUSTED_REPOSITORY_CONTENT>` boundary markers with strict system directives instructing the model to treat all comments, variable names, and string literals as passive data, never executable instructions."*

### Q3: *"Does VAYU send proprietary source code to third-party cloud APIs?"*
**Your Answer:**  
> *"No. VAYU is local-first and air-gap compatible. By default, it interfaces with local Ollama instances (like Gemma 4, Llama 3, or Qwen 2.5 Coder) running entirely on localhost. In environments without any LLM, VAYU includes an offline deterministic rule engine that handles core vulnerability remediation with zero network connectivity."*

### Q4: *"What is the difference between `skills.md` and `SKILL.md`?"*
**Your Answer:**  
> *"`skills.md` in the project root is our persistent internal engineering handbook that enforces architecture, typing, and security invariants for developers working ON VAYU.*  
>  
> *In contrast, `SKILL.md` is the standard format used by our new Skills CLI (`vayu skills add`). When installed under `.agents/skills/<skill-name>/SKILL.md`, it provides portable, modular instructions that external AI coding agents (like Claude, Cursor, or Antigravity) use when working ON user repositories."*

---

## 6. Troubleshooting Guide

| Issue | Cause | Fix |
|---|---|---|
| `Configuration Error: Environment variables are not loaded` | `.env.local` was not sourced | Run `source .env.local` before launching CLI. |
| `Ollama Connection Error` | Ollama daemon is stopped | Run `ollama serve` or `ollama run llama3` in another terminal, or rely on offline mode. |
| `Skill already installed at ...` | Skill directory exists | Pass the `--force` flag: `vayu skills add <skill> --force`. |
| Port `3000` already in use | Previous Next.js dev server running | Kill process with `npx kill-port 3000` or run on custom port: `npm run dev -- -p 3001`. |
| `git clone failed` during external skill install | Network or URL issue | Check your internet connection and verify URL starts with `https://` or `git@`. |
