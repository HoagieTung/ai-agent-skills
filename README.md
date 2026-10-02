# AI Agent Skills

This repository contains agent skills created and maintained by Hogan Tong. Each skill packages practical instructions, reference material, and supporting resources that help an AI agent handle a particular kind of task consistently.

## Repository structure

Each skill lives in its own directory:

```text
<skill-name>/
└── SKILL.md
```

A skill may also include supporting files, such as `references/`, `scripts/`, or `assets/`, when needed. The `SKILL.md` file describes what the skill does and when an agent should use it, then provides the relevant instructions.

## Available skills

- `basket-reverse-engineering` - Infer the themes, selection logic, and thesis behind a stock basket.
- `browser-login` - Set up and use an authenticated browser session safely.
- `design-taste-frontend` - Design and improve distinctive, considered frontend interfaces.
- `diet-log` - Track food intake, exercise, and progress toward a weight goal.
- `nuwa-skill` - Research a person's ideas and turn their methods into a reusable perspective skill.
- `patina` - Identify and revise AI-sounding writing while preserving meaning.
- `political-scientist-analyst` - Analyze political events through political science and international relations frameworks.
- `powerpoint` - Create, edit, render, and quality-check PowerPoint presentations.
- `quant-literature-research` - Find and assess research on quantitative finance topics.
- `research-writing` - Research a topic and produce a source-grounded written synthesis.
- `shuorenhua` - Review or humanize Chinese and English writing while preserving meaning.
- `thematic-basket-construction` - Research and construct investable thematic stock baskets.
- `weekly-market-recap` - Prepare a concise weekly market recap across major regions and asset classes.

## Format

Skills use the `SKILL.md` convention: YAML frontmatter with a `name` and `description`, followed by Markdown instructions. Supporting material can be placed alongside it in the skill directory. See the [Agent Skills specification](https://agentskills.io/specification).

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
