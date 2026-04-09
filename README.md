# ai-scheduled-improvement

`ai-scheduled-improvement` is a minimal GitHub Actions setup for autonomous scheduled repository improvements using AI coding agents.

It is designed to be simple to fork, simple to understand, and inexpensive to run. The current setup uses **Kilo Code** and **OpenCode** because both support **non-interactive autonomous CLI usage** and currently offer **free model options** that make it easy to get started, including lightweight setups that can work even without adding API keys.

## What this repository does

This repository contains two scheduled GitHub Actions workflows that:

- check out your repository
- install an AI coding agent CLI
- configure the agent for unattended execution
- ask the agent to make **one very small improvement**
- have the agent commit and push the change back to the repository

The goal is not large refactors. The goal is continuous, low-cost, low-risk improvement over time.

## Why the changes are intentionally minimal

The prompt used by both workflows explicitly asks for the smallest possible useful change:

- one typo fix
- one tiny bug fix
- one small logic improvement
- one minimal code cleanup
- one narrow edit in a single file when possible

This token-saving approach is intentional.

The current implementation is optimized for **free-model usage** and for running **without API keys when possible**. Without API keys, access may be limited by shared IP-based rate limits on GitHub-hosted runners. Because GitHub Actions runners come from shared IP pools, larger prompts and larger rewrites are more likely to waste tokens or fail. Keeping each run minimal helps the automation stay cheap, practical, and more reliable.

## Current workflows

### Kilo Code workflow

File: `/home/runner/work/ai-scheduled-improvement/ai-scheduled-improvement/.github/workflows/kilo.yml`

Current behavior:

- runs daily at **00:00 UTC**
- also supports manual runs through `workflow_dispatch`
- installs `@kilocode/cli`
- configures Kilo to use `kilo/kilo-auto/free`
- passes `KILO_API_KEY` if you provide it in repository secrets
- asks Kilo to perform a minimal autonomous improvement and push the result

### OpenCode workflow

File: `/home/runner/work/ai-scheduled-improvement/ai-scheduled-improvement/.github/workflows/opencode.yml`

Current behavior:

- runs daily at **12:00 UTC**
- also supports manual runs through `workflow_dispatch`
- installs `opencode-ai`
- configures OpenCode with the `opencode/big-pickle` model
- writes `OPENCODE_API_KEY` auth data only if you provide the secret
- asks OpenCode to perform a minimal autonomous improvement and push the result

## Supported agents right now

The current implementation supports:

- **Kilo Code**
- **OpenCode**

These were chosen because they are practical for unattended CLI execution in GitHub Actions and are easy to try with free access paths.

## API keys are optional

The workflows are set up so you can start in a lightweight mode:

- with API keys, if you want more predictable authenticated access
- without API keys, if you want the easiest possible setup and the current free access paths are sufficient

If you want to add secrets, use:

- `KILO_API_KEY`
- `OPENCODE_API_KEY`

If you do not add secrets, the workflows still keep the configuration minimal and rely on the currently available free-model setup where supported.

## Important note about free models

This repository intentionally uses free models today, but **free model availability may change in the future**.

That means you may eventually need to update:

- the configured model name
- the provider configuration
- whether an API key is required
- the prompt size and frequency

Treat the current models as a convenient starting point, not a permanent guarantee.

## Why Kilo Code and OpenCode

This setup focuses on tools that are friendly to autonomous scheduled execution.

The main reasons for using Kilo Code and OpenCode here are:

- they support non-interactive CLI automation
- they can be run from GitHub Actions
- they currently provide accessible free-model options
- they are easy to try as a starting point for autonomous repository maintenance

References:

- Kilo Code docs: https://kilo.ai/docs
- OpenCode docs: https://opencode.ai/docs

## Extending and customizing the workflows

The repository is intentionally extensible. You can edit the workflows however you want for your own repository.

Common customizations include:

### 1. Change the schedule

You can update the cron expressions to run more or less often.

Examples:

- run only on weekdays
- run once per week instead of daily
- run both agents at different hours for your timezone
- disable schedule entirely and keep manual dispatch only

### 2. Change the prompt

You can make the agent focus on your preferred type of work.

Examples:

- documentation-only updates
- test-only improvements
- dependency maintenance
- lint fixes only
- bug fixes inside a specific directory
- changes limited to one package or service

### 3. Change the model or provider

You can swap the currently configured free model for another model if availability changes.

Examples:

- replace the current free model with another free tier
- switch to a paid model for higher reliability
- require API-key-backed providers only
- maintain separate models for docs tasks versus code tasks

### 4. Add validation steps

You can make the workflow safer by adding checks before the agent commits.

Examples:

- run tests before allowing a push
- run linting or formatting
- reject changes if protected files were modified
- require changes to stay within a specific directory

### 5. Adjust repository permissions and strategy

You can tailor the workflow to match how you want changes delivered.

Examples:

- push directly to a branch
- open pull requests instead of pushing straight to the default branch
- restrict permissions further
- pin dependency versions more aggressively

## Recommended usage pattern

A good default approach is:

1. start with the existing minimal prompts
2. observe the quality of several scheduled runs
3. add API keys if you want more stable access
4. tighten the scope if token usage or noisy changes become a problem
5. customize schedules, models, and validation steps to fit your repository

## Summary

This repository is a starting point for **autonomous scheduled improvement with AI coding agents on GitHub Actions**.

Today it is built around:

- Kilo Code and OpenCode
- free models
- optional API keys
- daily scheduled runs
- minimalist changes to save tokens
- easy workflow customization

If you want to adapt it, edit the workflow files directly. The setup is intentionally small so it is easy to extend.
