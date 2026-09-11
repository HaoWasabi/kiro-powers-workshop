---
title: "Hands-on 2: Create Custom Power"
date: "2024-01-01"
weight: 5
chapter: false
pre: "<strong>5. </strong>"
---

#### Overview

In this hands-on lab, you will create a **Custom Kiro Power from scratch.**

A Power is a reusable package that combines domain expertise, best practices, and optionally, tools (MCP servers). After installation, Kiro automatically activates the Power when the conversation contains relevant keywords, helping keep the agent's context clean and focused.

By the end of this lab, you will:
- Understand the required file structure of a Power
- Write a valid `plugin.json` and `POWER.md`
- Import and test your Power in the Kiro IDE
- Optionally add MCP tools or skills

#### Task

We will create a simple but practical Power called `company-style-guide`.

**Purpose:** Provide the agent with internal coding standards, naming conventions, and a code review checklist for a fictional company, so every team member receives consistent guidance.

You can replace this example with your actual internal standards, API guidelines, or company deployment procedures later.

#### Prerequisites

- Kiro IDE installed and signed in
- Basic Markdown knowledge
- A text editor (or Kiro itself)

#### Step 1: Create the Power directory

Create a new directory on your computer with the following structure:

```text
company-style-guide/
├── plugin.json
└── POWER.md
```

> Tip: You can create this directory inside the current project or in a separate directory named `powers/`.

#### Step 2: Write the plugin.json file

This is the **manifest** file. It tells Kiro the Power's identity and activation keywords.

Create a `plugin.json` file with the following content:

```json
{
	"$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
	"name": "company-style-guide",
	"version": "1.0.0",
	"description": "Internal coding standards, naming conventions, and the company's code review checklist",
	"author": {
		"name": "Your name / Your team"
	},
	"keywords": [
		"style guide",
		"coding standard",
		"naming convention",
		"code review",
		"company standards",
		"naming rules",
		"best practice"
	]
}
```

Important fields:

- `name:` A unique identifier (use kebab-case with no spaces)
- `keywords:` Words or phrases that activate this Power
- `description:` A short description displayed in the Powers panel

#### Step 3: Write the POWER.md file

This is the most important file. It serves as the agent's instruction handbook.

Create a `POWER.md` file with the following content:

```md
# Company Style Guide Power

## Purpose
This Power provides the agent with the company's official internal coding standards, naming conventions, and code review checklist. Use it when the user asks about code style, naming, or code reviews.

## When to activate
Activate this Power when the conversation mentions:
- style guide / coding standard
- naming convention
- code review
- company standards
- codebase best practices

## Agent behavior

### Do
- Always follow the naming conventions defined below
- Prefer clarity over cleverness
- Suggest improvements that match the company's review checklist
- Point out violations politely and provide corrected examples

### Do not
- Invent naming rules that are not documented here
- Enforce personal preferences that conflict with company standards
- Skip the review checklist

## Naming conventions

| Item              | Convention             | Example                   |
|-------------------|------------------------|---------------------------|
| Variables         | camelCase              | `userId`, `totalAmount`   |
| Functions         | camelCase              | `getUserById()`           |
| Classes / Types   | PascalCase             | `UserService`             |
| Constants         | UPPER_SNAKE_CASE       | `MAX_RETRY_COUNT`         |
| File names        | kebab-case             | `user-service.ts`         |
| CSS classes       | kebab-case             | `.btn-primary`            |

## Code review checklist

When reviewing code, always check the following:

1. **Naming** - Does it follow the naming conventions?
2. **Readability** - Is the code easy to read and understand?
3. **Error Handling** - Are errors handled correctly?
4. **Security** - Are there hard-coded secrets or insecure patterns?
5. **Tests** - Are there meaningful unit or integration tests?
6. **Performance** - Are there N+1 queries or unnecessary loops?

## Golden path example

**User prompt:**
"Review this function according to our company standards."

**Expected agent behavior:**
1. Activate this Power
2. Check the naming conventions
3. Go through the review checklist in order
4. Suggest specific improvements with code examples
```

#### Step 4: Import the Power into Kiro

1. Open the Kiro IDE
2. Click the Powers icon in the left sidebar (Ghosty with a lightning bolt)
3. Click **Add Custom Power**
4. Select **Import power from a folder**
5. Select the `company-style-guide` directory you created
6. Click **Install**

After a few seconds, **company-style-guide** will appear in the list of Installed Powers.

#### Step 5: Test the Power

Open a new chat in Kiro and try the following prompts:

**Test 1 - Check activation**
```txt
Review this code according to our company style guide.
```

**Test 2 - Check naming conventions**
```txt
Name the variables and function for this user information lookup logic according to our company standards.
```

**Test 3 - Full review**
```txt
Please review this TypeScript function based on our internal coding standards.
```

Observe whether the agent follows the rules you defined in `POWER.md`.

#### Optional: Add more components

You can enhance the Power later with the following components:

| Component      | File / Directory | When to use                         |
|----------------|------------------|-------------------------------------|
| MCP tools      | `mcp.json`       | When the Power needs external tools |
| Skills         | `skills/`        | For complex, multi-step workflows   |
| Steering files | `steering/`      | For detailed workflow guidance      |
| Hooks          | `hooks/`         | For event-driven automation         |

Minimal `mcp.json` example, if needed later:

```json
{
	"mcpServers": {
		"example-server": {
			"command": "npx",
			"args": ["-y", "@modelcontextprotocol/server-example"]
		}
	}
}
```
