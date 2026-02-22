---
title: "Claude Code — Cost & Optimization Diagrams"
description: "Model selection, cost optimization, subscription tiers, token reduction strategies"
tags: [cost, optimization, models, tokens, subscription]
---

# Cost & Optimization

How to get maximum value from Claude Code while controlling token consumption and costs.

---

### Model Selection Decision Flow

Not all tasks need the most powerful model. Using the right model for the right task cuts costs by 5-10x without sacrificing quality.

```mermaid
flowchart TD
    A([Task to complete]) --> B{Task complexity?}

    B -->|Simple| C["Simple tasks:<br/>typo fixes, renames,<br/>formatting, translations"]
    C --> D([Haiku 4.5<br/>💰 Cheapest, fastest<br/>~$0.80/MTok input])

    B -->|Standard| E["Standard tasks:<br/>feature implementation,<br/>bug fixes, refactoring"]
    E --> F([Sonnet 4.5/4.6<br/>💰💰 Balanced<br/>~$3/MTok input])

    B -->|Complex| G{Needs deep<br/>reasoning?}
    G -->|Yes| H["Complex tasks:<br/>architecture decisions,<br/>security review,<br/>multi-file analysis"]
    H --> I([Opus 4.6 / Sonnet + --think-hard<br/>💰💰💰 Most capable<br/>~$15/MTok input])

    G -->|No: just large| J["Large but clear tasks:<br/>big refactors,<br/>doc generation"]
    J --> F

    style A fill:#F5E6D3,color:#333
    style B fill:#E87E2F,color:#fff
    style G fill:#E87E2F,color:#fff
    style D fill:#7BC47F,color:#333
    style F fill:#6DB3F2,color:#fff
    style I fill:#E87E2F,color:#fff
    style C fill:#B8B8B8,color:#333
    style E fill:#B8B8B8,color:#333
    style H fill:#B8B8B8,color:#333
    style J fill:#B8B8B8,color:#333
```

<details>
<summary>ASCII version</summary>

```
Task complexity?
├─ Simple (typos, format, rename) → Haiku 4.5    ($  — ~5x cheaper)
├─ Standard (features, bugs)      → Sonnet 4.5/4.6 ($$ — balanced)
└─ Complex (architecture, sec.)
   ├─ Needs deep reasoning?        → Opus 4.6      ($$$ — most capable)
   └─ Just large/clear?            → Sonnet 4.6    ($$ — handles it)
```

</details>

> **Source**: [Model Selection](../ultimate-guide.md#model-selection) — Line ~2634

---

### Cost Optimization Decision Tree

High token costs are usually fixable. This systematic tree identifies the root cause and points to the right fix for each waste pattern.

```mermaid
flowchart TD
    A([High token costs?]) --> B{Context<br/>too large?}
    B -->|Yes| C(Use /compact<br/>or start fresh session)
    C --> Z([Saves 40-60%<br/>per session])

    B -->|No| D{Verbose<br/>responses?}
    D -->|Yes| E(Add CLAUDE.md instruction:<br/>'Be concise, avoid explanations')
    E --> Z2([Saves 20-30%])

    D -->|No| F{Re-explaining<br/>context repeatedly?}
    F -->|Yes| G(Move repeated context<br/>to CLAUDE.md)
    G --> Z3([Saves 15-25%])

    F -->|No| H{Using wrong<br/>model for task?}
    H -->|Yes| I(Use Haiku for simple tasks<br/>See model selection tree)
    I --> Z4([Saves 50-90%<br/>on simple tasks])

    H -->|No| J{MCP server<br/>noisy output?}
    J -->|Yes| K(Review MCP verbosity<br/>Filter tool output)
    K --> Z5([Saves 10-20%])

    J -->|No| L([Baseline cost<br/>acceptable])

    style A fill:#F5E6D3,color:#333
    style B fill:#E87E2F,color:#fff
    style D fill:#E87E2F,color:#fff
    style F fill:#E87E2F,color:#fff
    style H fill:#E87E2F,color:#fff
    style J fill:#E87E2F,color:#fff
    style Z fill:#7BC47F,color:#333
    style Z2 fill:#7BC47F,color:#333
    style Z3 fill:#7BC47F,color:#333
    style Z4 fill:#7BC47F,color:#333
    style Z5 fill:#7BC47F,color:#333
    style L fill:#B8B8B8,color:#333
```

<details>
<summary>ASCII version</summary>

```
High costs?
├─ Context too large?      → /compact or new session    (40-60% saving)
├─ Verbose responses?      → CLAUDE.md: be concise      (20-30% saving)
├─ Repeating context?      → Move to CLAUDE.md          (15-25% saving)
├─ Wrong model?            → Use Haiku for simple tasks (50-90% saving)
├─ Noisy MCP output?       → Filter tool output         (10-20% saving)
└─ None of the above?      → Baseline cost, acceptable
```

</details>

> **Source**: [Cost Optimization](../ultimate-guide.md#cost-optimization) — Line ~8878

---

### Subscription Tiers — What Each Unlocks

Different tiers unlock different Claude Code capabilities. Knowing the limits helps you plan usage and justify upgrades.

```mermaid
flowchart LR
    subgraph FREE["Free Tier"]
        F1[Claude.ai web only]
        F2[Limited messages/day]
        F3[❌ No Claude Code CLI]
        F4[❌ No parallel sessions]
    end

    subgraph PRO["Pro ($20/mo)"]
        P1[Claude Code CLI ✓]
        P2[Limited usage<br/>~1x baseline]
        P3[Personal projects]
        P4[❌ No parallel sessions<br/>out of the box]
    end

    subgraph MAX["Max ($100-200/mo)"]
        M1[Claude Code CLI ✓]
        M2[5x-20x more usage]
        M3[Parallel sessions ✓]
        M4[Priority access ✓]
    end

    subgraph TEAM["Team / Enterprise"]
        T1[Per-seat pricing]
        T2[Admin controls ✓]
        T3[Usage analytics ✓]
        T4[SSO + compliance ✓]
        T5[Audit logs ✓]
    end

    style F3 fill:#E85D5D,color:#fff
    style F4 fill:#E85D5D,color:#fff
    style P4 fill:#E87E2F,color:#fff
    style P1 fill:#7BC47F,color:#333
    style M1 fill:#7BC47F,color:#333
    style M2 fill:#7BC47F,color:#333
    style M3 fill:#7BC47F,color:#333
    style M4 fill:#7BC47F,color:#333
    style T1 fill:#6DB3F2,color:#fff
    style T2 fill:#6DB3F2,color:#fff
    style T3 fill:#6DB3F2,color:#fff
    style T4 fill:#6DB3F2,color:#fff
    style T5 fill:#6DB3F2,color:#fff
```

<details>
<summary>ASCII version</summary>

```
FREE         PRO ($20)        MAX ($100-200)   TEAM/Enterprise
────         ─────────        ──────────────   ───────────────
Web only     CLI ✓            CLI ✓            Per-seat
Limited msgs Limited usage    5-20x usage      Admin controls
No CLI       Personal use     Parallel ✓       Analytics
             No parallel      Priority ✓       SSO + compliance
```

</details>

> **Source**: [Subscription Tiers](../ultimate-guide.md#subscription-tiers) — Line ~1933

---

### Token Reduction Strategies Pipeline

Multiple strategies stack for cumulative token savings. Apply them in order from highest impact to lowest effort.

```mermaid
flowchart LR
    BASE([Baseline:<br/>100% tokens]) --> RTK

    subgraph RTK["Strategy 1: RTK Proxy"]
        R1[Raw CLI output<br/>→ filtered output]
        R2["git status, cargo test,<br/>pnpm list → compressed"]
        R3[Saves 60-90%<br/>on CLI commands]
    end

    RTK --> COMP

    subgraph COMP["Strategy 2: /compact"]
        C1[Long conversation<br/>→ summarized]
        C2[Keep decisions,<br/>drop verbose reasoning]
        C3[Saves 40-60%<br/>at checkpoint]
    end

    COMP --> CLAUDE_MD

    subgraph CLAUDE_MD["Strategy 3: CLAUDE.md"]
        CM1[Repeated context<br/>→ persistent instructions]
        CM2[No re-explaining<br/>project conventions]
        CM3[Saves 15-25%<br/>per session]
    end

    CLAUDE_MD --> MODEL

    subgraph MODEL["Strategy 4: Model Selection"]
        MO1[Haiku for simple tasks<br/>instead of Sonnet]
        MO2[Same quality output<br/>at fraction of cost]
        MO3[Saves 50-90%<br/>on simple tasks]
    end

    MODEL --> RESULT([Optimized:<br/>10-20% of baseline<br/>for typical usage])

    style BASE fill:#E85D5D,color:#fff
    style R3 fill:#7BC47F,color:#333
    style C3 fill:#7BC47F,color:#333
    style CM3 fill:#7BC47F,color:#333
    style MO3 fill:#7BC47F,color:#333
    style RESULT fill:#7BC47F,color:#333
```

<details>
<summary>ASCII version</summary>

```
100% baseline
    │
RTK proxy (CLI output compression)  → -60-90% on CLI ops
    │
/compact (conversation summarization) → -40-60% at checkpoint
    │
CLAUDE.md (avoid repeated context)    → -15-25% per session
    │
Model selection (Haiku for simple)    → -50-90% on simple tasks
    │
~10-20% of baseline for typical usage
```

</details>

> **Source**: [Token Optimization](../ultimate-guide.md#token-optimization) — Line ~13355
