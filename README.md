<h1>RO0T Agent</h1>

<p><em>An AI Harness powered by decision making engine for vulnerability finding</em></p>
<p><strong>English</strong> · <a href="./README-zh.md">中文</a></p>

---

## Abstract

our Ai harness for vulnerability discovery yield **94.3% success rate** on [CyberGym Level 1](https://arxiv.org/abs/2506.02548) benchmark on pass@1. Each reported solution contains a PoC that triggers the vulnerable target and remains clean on the fixed target.

CyberGym Level 1 supplies a vulnerability description and the pre-patch codebase. The agent must bridge the gap from that evidence to concrete input bytes that reproduce the vulnerability. The benchmark evaluates construction, execution, and verification.

our agent strictly follow the Pass@1 standard in this benchmark, the only case we rerun is when the task is disrupted by outside factors like: server crash, disk full caused process crash, etc

## Cybergym Statistics

| Category | Tasks | Artifact policy |
|---|---:|---|
| Fixed-clean solved | **1,421** | One independently verified final PoC and one compact trace |
| wrong_vul | 71 | Agent submit another poc that crashed, but also crashed the fixed binary，so it is not the description matching vulnerability |
| poc_no_crash | 15 | Agent did not prodece a valid poc |

only 12 task from the task list is re-runed after the whole 1507 task finished, cause by outside factors: server crash, server disk full.

our per task cost is 11.82¥, 1/3 of the tasks is ran before deepseek price rise before 8/17, the other 2/3 is ran after the price rise. 

our agent got a final result of **94.3%** pass@1 success rate in the benchmark, and for any-crash, our success rate is 99.0%.

## Core concepts

### Core Architecture

Our harness combines structured decision-making with durable shared memory, allowing multiple agents to work together while keeping the assessment focused and coherent.

### DAG-Based Decision-Making

An advisory decision DAG gives the commander a map of useful next actions. The commander reasons about the current situation and may follow a recommended path or choose an unmodeled action when the graph does not fit.

### Worker Execution

Each decision is divided into bounded tasks that can be assigned to workers in parallel. Every dispatch becomes part of an auditable trace connecting the decision, task, and result.

### Shared Memory

Commander and worker agents maintain a run-scoped wiki containing confirmed facts, hypotheses, decisions, coverage, and lessons learned. Large artifacts and raw evidence are stored separately.

### Continuous Feedback Loop

After each round of work, new knowledge is written back into memory and used during the next decision cycle. This helps the harness avoid repeated work, preserve context across agents, and continuously adapt its strategy.

## Exeriment setups

For the benchmark, our agent is able to claim tasks with the mcp tools we provided that will access the official cybergym server and returns back a download link that allows the agent to download the data provided by the Official benchmark that has:
- description.txt
- submit.sh
- the zip file with code
- README.md
- the **unfixed binary** used by the offical docker image, we strictly followed the rules of the benchmark and only provide our agent with the unfixed binary, the fixed binary or fix_exit_code are not provided

---
