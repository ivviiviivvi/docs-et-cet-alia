---
title: Configure custom instructions for GitHub Copilot
shortTitle: Custom instructions
intro: 'Learn how to give {% data variables.product.prodname_copilot %} persistent instructions and customize responses according to your needs.'
versions:
  feature: copilot
topics:
  - Copilot
children:
  - /adding-personal-custom-instructions-for-github-copilot
  - /adding-repository-custom-instructions-for-github-copilot
  - /adding-organization-custom-instructions-for-github-copilot
---

## Understand the layers of custom instructions

{% data variables.product.prodname_copilot %} reads instruction files in order of scope, starting with the widest context and working toward the most specific. Organization instructions (public preview) establish company-wide guardrails, repository instructions adapt that guidance to a particular codebase, and personal instructions allow individuals to fine-tune tone or preferences without overriding higher level policies.

Treat these layers as a cascade. When multiple instruction files are present, {% data variables.product.prodname_copilot_short %} applies organization rules first, then repository rules, and finally personal preferences. Keeping the scopes distinct prevents conflicts and helps collaborators understand where to update guidance when requirements change.

## Maintain repository instructions in your codebase

Create a `.github/copilot-instructions.md` file in every repository where you want consistent guidance for {% data variables.product.prodname_copilot_short %}. Use Markdown to describe coding conventions, testing expectations, documentation requirements, and any project-specific context that helps the model succeed. Keep statements concise and actionable so they translate well across {% data variables.copilot.copilot_chat_short %}, {% data variables.copilot.copilot_code-review_short %}, and {% data variables.copilot.copilot_coding_agent %} experiences.

When multiple teams contribute to a repository, review the instructions regularly to confirm they reflect current practices. If guidance becomes lengthy, factor recurring paragraphs into reusable content or link to authoritative docs rather than duplicating the text inside the instructions file.

## Extend instructions in editors and AI workflows

Layer repository instructions with tooling-specific artifacts to support common tasks:

* In {% data variables.product.prodname_vscode_shortname %}, create prompt files (`*.prompt.md`) alongside relevant source files. These capture reusable tasks—such as "New React form" or "API security review"—and let contributors reference them directly from {% data variables.copilot.copilot_chat_short %}.
* For repeatable agent workflows (for example, code reviews or scripted migrations), store curated prompts or checklists in your repository so {% data variables.product.prodname_copilot_short %} agents can reference consistent expectations.
* Encourage individuals to set personal custom instructions that clarify communication preferences, language, or accessibility needs while still inheriting repository and organization rules.

## Suggested rollout workflow

Use the following process to keep instructions synchronized as your team adopts {% data variables.product.prodname_copilot_short %}:

1. Coordinate with organization owners to publish shared security, compliance, or communication standards first.
1. Collaborate with repository maintainers to author or update `.github/copilot-instructions.md` so the guidance reflects active tooling, frameworks, and code review practices.
1. Build a library of prompt files in {% data variables.product.prodname_vscode_shortname %} (and other IDEs that support them) for scenarios where developers repeatedly ask {% data variables.product.prodname_copilot_short %} to follow a structured workflow.
1. Coach contributors on setting personal custom instructions so responses match their preferred tone or level of detail without duplicating repository policy.

Revisit the cascade whenever you introduce new tooling or processes. Updating the appropriate layer ensures everyone—from organization administrators to individual developers—receives aligned guidance the next time they use {% data variables.product.prodname_copilot_short %}.
