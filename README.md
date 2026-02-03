# Copilot Workspace - Release Automation Tools

This repository contains GitHub Copilot agents and tools for automating release management processes.

## Overview

An automated agent for creating PreCAB (Pre-Change Advisory Board) release documentation by pulling information from Jira tickets and Confluence templates.

## Contents

- `.github/agents/agent.md` - Precab Release Assistant agent definition
- `precab-release-*.md` - Generated precab release pages

## Usage

### Creating a Precab Release Page

1. **Invoke the agent** using the `/precab` command followed by the ticket number:
   ```
   /precab 4687
   ```

2. **Review the generated file** and fill in placeholders for deployment steps, dates, and artifacts

3. **Submit to CAB** once all operational details are complete

## Customizing for Your Squad

To adapt this agent for your team:

1. **Edit `.github/agents/agent.md`** - Update the agent configuration:
   - Change `cloudId` to your Atlassian cloud instance
   - Update Confluence parent page ID (search location for templates)
   - Modify content guidelines (background length, summary bullets)
   - Adjust product names and release contact defaults

2. **Configure MCP Servers:**
   - Set up Atlassian MCP with your Jira/Confluence credentials
   - Optionally add GitHub MCP for PR analysis

3. **Customize templates:**
   - Update risk level guidelines
   - Modify release step templates
   - Adjust quality metrics format

## Prerequisites

- GitHub Copilot with agent support
- Atlassian MCP Server configured with your Jira/Confluence access
- GitHub MCP Server (optional, for PR analysis)

## Content Guidelines

### Background Section
- **2-3 sentences maximum**
- Focus on business context and value
- Avoid technical implementation details

### Summary Section
- 3-5 bullet points
- High-level changes or features

### Quality Metrics
- Include test coverage details
- Reference test files from GitHub PRs

---

**Last Updated:** February 4, 2026
