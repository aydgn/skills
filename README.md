# Skills Collection

A personal collection of high-quality agent skills, designed for own use and useful for the public. These skills extend the capabilities of AI coding agents by providing specialized instruction sets for common development tasks.

## Skills Included

- **[Prompt Boost](./prompt-boost/SKILL.md)**: Iteratively refine and organize complex task prompts to ensure AI agents have all necessary context and clear objectives.
- **[Simplify](./simplify/SKILL.md)**: A systematic three-pass review (Reuse, Quality, Efficiency) to clean up code changes, remove redundancy, and optimize performance.

## Installation

These skills can be installed using the [skills CLI](https://github.com/vercel-labs/skills).

### Quick Install (All Skills)

To install all skills in this repository to all detected agents globally:

```bash
npx skills add aydgn/skills --all -g
```

### Install Specific Skills

To install a specific skill (e.g., `simplify`) globally:

```bash
npx skills add aydgn/skills --skill simplify -g
```

### Local Development

If you've cloned this repository locally, you can install from the local path:

```bash
npx skills add . --all -g
```

## Supported Agents

These skills are compatible with a wide range of agents, including:

- **Claude Code**
- **Cursor**
- **GitHub Copilot**
- **Gemini CLI**
- **Windsurf**
- ...and [many others](https://github.com/vercel-labs/skills#supported-agents).

## License

MIT
