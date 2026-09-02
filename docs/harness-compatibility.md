# Harness Compatibility / Harness 兼容性

`simplify-codebase` is one portable Agent Skills package. Its installation directory, `SKILL.md` frontmatter `name`, and invocation name are all `simplify-codebase`. This alignment is required by some harnesses and avoids platform-specific aliases.

`simplify-codebase` 是一个可移植的单一 Agent Skills 包。安装目录、`SKILL.md` 的 `name` 与调用名称统一为 `simplify-codebase`；部分 harness 要求三者一致，这一约定可避免平台专属别名。

## Portable contract / 可移植契约

- `SKILL.md` begins with only the portable `name` and `description` frontmatter fields.
- Detailed guidance is loaded through repository-relative Markdown links.
- Codex display metadata lives in `agents/openai.yaml`; other harnesses may ignore that optional file safely.
- Do not add vendor-only fields to `SKILL.md`. Put vendor-specific behavior in a separate adapter only when it is necessary and does not change the shared workflow.

Run this release check from the repository root:

```bash
python scripts/verify_harness_contract.py
```

After installation, add `--verify-directory-name` to confirm that the installed directory also matches `simplify-codebase`.

安装后可添加 `--verify-directory-name`，确认实际安装目录同样命名为 `simplify-codebase`。

## Supported discovery locations / 支持的发现目录

| Harness | User scope | Project scope | Verify |
| --- | --- | --- | --- |
| Codex | `~/.codex/skills/simplify-codebase/` | repository-specific Codex skill directory when configured | Start a new task and invoke `$simplify-codebase`. |
| Claude Code | `~/.claude/skills/simplify-codebase/` | `.claude/skills/simplify-codebase/` | Invoke `/simplify-codebase` or ask a matching request. |
| Cursor | `~/.agents/skills/simplify-codebase/` or `~/.cursor/skills/simplify-codebase/` | `.agents/skills/simplify-codebase/` or `.cursor/skills/simplify-codebase/` | Use `/simplify-codebase` or verify it appears in Customize → Skills. |
| GitHub Copilot | `~/.copilot/skills/simplify-codebase/` or `~/.agents/skills/simplify-codebase/` | `.github/skills/simplify-codebase/`, `.claude/skills/simplify-codebase/`, or `.agents/skills/simplify-codebase/` | Invoke `/simplify-codebase` or confirm it is listed by the Copilot client. |
| Cline | `~/.cline/skills/simplify-codebase/` | `.cline/skills/simplify-codebase/`, `.clinerules/skills/simplify-codebase/`, or `.claude/skills/simplify-codebase/` | Enable Skills, then invoke a matching request or confirm it appears in the Skills panel. |
| Gemini CLI | `~/.gemini/skills/simplify-codebase/` | `.gemini/skills/simplify-codebase/` | Run `/skills list`, then invoke or approve activation. |
| OpenCode | `~/.agents/skills/simplify-codebase/` or `~/.config/opencode/skills/simplify-codebase/` | `.agents/skills/simplify-codebase/` or `.opencode/skills/simplify-codebase/` | Confirm the `skill` tool advertises `simplify-codebase` in a new session. |

Use one location at the intended precedence level. Do not install a second copy with the same name unless an override is intentional.

选择目标优先级中的一个目录安装即可。除非明确需要覆盖，不要安装同名的第二份副本。

## Sources / 依据

- [Claude Code Skills](https://code.claude.com/docs/en/skills)
- [Cursor Agent Skills](https://prod.cursor.com/docs/skills)
- [GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Cline Skills](https://docs.cline.bot/customization/skills)
- [Gemini CLI Agent Skills](https://geminicli.com/docs/cli/tutorials/skills-getting-started/)
- [OpenCode Agent Skills](https://opencode.ai/docs/skills)
