# My Jira Extension

An extension for [Gemini CLI](https://github.com/google/gemini-cli) that provides an interactive workflow for creating Jira issues with precise metadata mapping.

## Features

- **Interactive Workflow**: Prompts for project key, issue type, environment, and impact.
- **Smart Mapping**: Automatically maps selections to Jira custom fields (Urgency, Impact, Change Reason, Environment).
- **Default Metadata**: Sets default labels (`traiana`, `osttra`) and business units.
- **Plan Generation**: Automatically generates Test and Backout plans if left blank.

## Installation

```bash
gemini extensions install https://github.com/hoberoi1984/jira-atlassian-interactive
```

## Skills Included

- `jira-atlassian-interactive`: Specialized expert for Jira issue creation.
