# mcm-solver-python Subagent Boldness Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make `mcm-solver-python` default to aggressive subagent parallelism so real usage better reflects the value of delegated work.

**Architecture:** Keep the change constrained to the skill definition and its agent metadata. Rewrite the subagent policy from conservative opt-in language to parallel-by-default language, define minimum staffing for each stage, and require visible subagent contribution summaries in final outputs.

**Tech Stack:** Markdown skill docs, YAML agent metadata, git

---

### Task 1: Rewrite the default subagent policy in the skill document

**Files:**
- Modify: `skills/mcm-solver-python/SKILL.md`

**Step 1: Write the failing review checklist**

Confirm the current wording is still too conservative:

```text
- subagent usage is described as enabled but not clearly parallel-by-default
- downgrade conditions are not framed as narrow exceptions
- no minimum staffing is defined for pre-confirmation and post-confirmation stages
```

**Step 2: Verify the checklist fails**

Run: `rg -n "默认启用 subagent|默认触发 subagent|若任务只是|继续分工规则" skills/mcm-solver-python/SKILL.md`
Expected: wording shows subagent use but not the new aggressive-default framing

**Step 3: Write the minimal implementation**

Edit `skills/mcm-solver-python/SKILL.md` to:

- add a default principle that the skill should parallelize whenever the task is not truly tiny
- rewrite the subagent strategy to make aggressive parallelism the default
- narrow downgrade conditions to explicit tiny-task exceptions
- define minimum staffing for pre-confirmation and post-confirmation stages
- define an expanded staffing recommendation for larger problems

**Step 4: Run a focused review pass**

Run: `sed -n '1,260p' skills/mcm-solver-python/SKILL.md`
Expected: the skill now reads as parallel-first rather than parallel-optional

**Step 5: Commit**

```bash
git add skills/mcm-solver-python/SKILL.md
git commit -m "docs: increase mcm solver subagent usage"
```

### Task 2: Make subagent value visible in outputs

**Files:**
- Modify: `skills/mcm-solver-python/SKILL.md`

**Step 1: Write the failing review checklist**

Check whether the output structure exposes delegated work:

```text
- final output does not clearly summarize which subagents contributed
- the main agent is not required to surface subagent findings in the final answer
```

**Step 2: Verify the checklist fails**

Run: `rg -n "默认输出|主 Agent 责任|subagent" skills/mcm-solver-python/SKILL.md`
Expected: subagents are mentioned, but the final output visibility requirement is incomplete

**Step 3: Write the minimal implementation**

Edit `skills/mcm-solver-python/SKILL.md` to:

- require the main agent to summarize subagent contributions in the final output
- add a subagent-summary section or equivalent structure to the default output template
- ensure this summary is high level and user-facing rather than internal chain-of-thought

**Step 4: Run a focused review pass**

Run: `sed -n '220,390p' skills/mcm-solver-python/SKILL.md`
Expected: final output guidance now makes delegated work visible

**Step 5: Commit**

```bash
git add skills/mcm-solver-python/SKILL.md
git commit -m "docs: expose mcm solver subagent outputs"
```

### Task 3: Align the default prompt with the new parallel-first behavior

**Files:**
- Modify: `skills/mcm-solver-python/agents/openai.yaml`

**Step 1: Write the failing review checklist**

Confirm the agent metadata does not yet enforce aggressive default parallelism:

```text
- short_description does not emphasize aggressive subagent parallelism
- default_prompt does not tell the agent to design parallel work first unless the task is tiny
```

**Step 2: Verify the checklist fails**

Run: `sed -n '1,120p' skills/mcm-solver-python/agents/openai.yaml`
Expected: the current metadata mentions subagents but not the stronger parallel-first policy

**Step 3: Write the minimal implementation**

Edit `skills/mcm-solver-python/agents/openai.yaml` to:

- update `short_description` to reflect aggressive default parallelism
- update `default_prompt` so the agent first decides the parallel staffing plan unless the task is tiny or the user explicitly declines parallelism

**Step 4: Run a focused review pass**

Run: `sed -n '1,120p' skills/mcm-solver-python/agents/openai.yaml`
Expected: metadata matches the revised skill behavior

**Step 5: Commit**

```bash
git add skills/mcm-solver-python/agents/openai.yaml
git commit -m "docs: align mcm solver prompt with parallel first flow"
```

### Task 4: Final consistency check

**Files:**
- Modify: `skills/mcm-solver-python/SKILL.md`
- Modify: `skills/mcm-solver-python/agents/openai.yaml`

**Step 1: Write the failing review checklist**

List the consistency points to verify:

```text
- aggressive parallelism is the default in both files
- downgrade conditions are clearly narrow exceptions
- minimum staffing is clearly stated
- final output visibility for subagent work is documented
```

**Step 2: Verify the checklist fails or is incomplete before cleanup**

Run: `rg -n "并行|subagent|最小编制|降级|默认输出" skills/mcm-solver-python/SKILL.md skills/mcm-solver-python/agents/openai.yaml`
Expected: identify any wording gaps that need cleanup

**Step 3: Write the minimal implementation**

Make final wording adjustments so both files use the same framing for:

- aggressive default parallelism
- downgrade exceptions
- stage staffing
- visible subagent summaries

**Step 4: Run the final verification**

Run: `rg -n "并行|subagent|最小编制|降级|默认输出" skills/mcm-solver-python/SKILL.md skills/mcm-solver-python/agents/openai.yaml`
Expected: all required concepts are visible and aligned

**Step 5: Commit**

```bash
git add skills/mcm-solver-python/SKILL.md skills/mcm-solver-python/agents/openai.yaml
git commit -m "docs: finalize mcm solver parallel first policy"
```
