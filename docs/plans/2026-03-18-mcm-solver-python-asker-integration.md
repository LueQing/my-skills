# mcm-solver-python Asker Integration Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Update `mcm-solver-python` so it conditionally collaborates with `asker-collector` during idea generation, then requires user confirmation before subagent-driven implementation, execution, and validation.

**Architecture:** Keep the change documentation-first and limited to the skill definition plus its agent metadata. Rewrite the skill workflow so the pre-confirmation stage supports optional multi-source idea collection through `asker-collector`, while the post-confirmation stage explicitly covers implementation, runtime execution, and validation under main-agent control.

**Tech Stack:** Markdown skill docs, YAML agent metadata, git

---

### Task 1: Update the skill workflow document

**Files:**
- Modify: `skills/mcm-solver-python/SKILL.md`

**Step 1: Write the failing review checklist**

Create a temporary checklist while reading `skills/mcm-solver-python/SKILL.md` and verify these old behaviors still exist:

```text
- Idea selection is described as user-driven rather than main-agent-driven
- `asker-collector` is not mentioned
- Post-confirmation execution does not explicitly require run and validation phases
```

**Step 2: Verify the checklist fails**

Run: `rg -n "asker-collector|运行|检验|确认模型|向人类发起模型确认" skills/mcm-solver-python/SKILL.md`
Expected: no `asker-collector` references and incomplete post-confirmation execution wording

**Step 3: Write the minimal implementation**

Edit `skills/mcm-solver-python/SKILL.md` to:

- add conditional `asker-collector` collaboration to the opening description and principles
- rewrite the pre-confirmation steps so the main agent produces candidate ideas and asks for confirmation
- preserve local-only flow when the user explicitly declines collaboration
- change the workflow to `idea preparation -> user confirmation -> implementation -> execution -> validation -> delivery`
- update subagent sections so post-confirmation work includes execution and validation, not only coding

**Step 4: Run a focused review pass**

Run: `sed -n '1,260p' skills/mcm-solver-python/SKILL.md`
Expected: the new workflow reads coherently and old user-driven wording is removed

**Step 5: Commit**

```bash
git add skills/mcm-solver-python/SKILL.md
git commit -m "docs: revise mcm solver python workflow"
```

### Task 2: Update the agent default prompt

**Files:**
- Modify: `skills/mcm-solver-python/agents/openai.yaml`

**Step 1: Write the failing review checklist**

Confirm the current metadata does not mention conditional collaboration or post-confirmation execution/validation:

```text
- short_description lacks `asker-collector`
- default_prompt lacks run and validation stages
```

**Step 2: Verify the checklist fails**

Run: `sed -n '1,120p' skills/mcm-solver-python/agents/openai.yaml`
Expected: no mention of `asker-collector`, runtime execution, or validation

**Step 3: Write the minimal implementation**

Edit `skills/mcm-solver-python/agents/openai.yaml` to:

- update `short_description` to mention conditional collaboration and the execution lifecycle
- update `default_prompt` to instruct the agent to first decide whether `asker-collector` should be used, then pause for user confirmation, then proceed with subagent-driven implementation, execution, and validation

**Step 4: Run a focused review pass**

Run: `sed -n '1,120p' skills/mcm-solver-python/agents/openai.yaml`
Expected: metadata matches the revised skill workflow

**Step 5: Commit**

```bash
git add skills/mcm-solver-python/agents/openai.yaml
git commit -m "docs: align mcm solver agent prompt"
```

### Task 3: Consistency check across the skill

**Files:**
- Modify: `skills/mcm-solver-python/SKILL.md`
- Modify: `skills/mcm-solver-python/agents/openai.yaml`

**Step 1: Write the failing review checklist**

List the consistency points to verify:

```text
- conditional collaboration is described the same way in both files
- user confirmation is mandatory before implementation
- implementation, execution, and validation are all explicitly present
```

**Step 2: Verify the checklist fails or is incomplete before final cleanup**

Run: `rg -n "asker-collector|确认|实现|运行|检验" skills/mcm-solver-python/SKILL.md skills/mcm-solver-python/agents/openai.yaml`
Expected: identify wording mismatches or gaps to clean up

**Step 3: Write the minimal implementation**

Make final wording adjustments so both files use consistent terminology for:

- conditional collaboration
- user confirmation gate
- post-confirmation execution lifecycle

**Step 4: Run the final verification**

Run: `rg -n "asker-collector|确认|实现|运行|检验" skills/mcm-solver-python/SKILL.md skills/mcm-solver-python/agents/openai.yaml`
Expected: all required terms and stages appear in both files where appropriate

**Step 5: Commit**

```bash
git add skills/mcm-solver-python/SKILL.md skills/mcm-solver-python/agents/openai.yaml
git commit -m "docs: finalize mcm solver asker integration"
```
