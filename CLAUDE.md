<!--
CLAUDE.md — modelo para economia de tokens (Claude Code)

Comentários HTML em bloco como este são removidos antes de o arquivo entrar no
contexto: não custam tokens. Use-os para notas de manutenção.

Como usar:
1. Copie este arquivo para a raiz do projeto (ou para ~/.claude/CLAUDE.md, se
   quiser aplicá-lo a todos os projetos; nesse caso, remova a seção "Project").
2. Preencha a seção "Project" só com o que o Claude não descobre lendo o código.
   Apague as linhas que não se aplicam. Meta oficial: menos de 200 linhas; aqui,
   quanto menos, melhor.
3. Instruções longas e específicas (migrações, deploy, revisão de PR) vão para
   skills (.claude/skills/<nome>/SKILL.md) ou para regras com `paths:` em
   .claude/rules/, que só carregam quando são usadas.
4. Edições neste arquivo só valem após /clear, /compact ou reinício da sessão.

Hábitos que economizam mais do que qualquer instrução (são ações suas):
- /clear entre tarefas não relacionadas; após duas correções sem sucesso,
  /clear e um prompt melhor.
- Escolha modelo e /effort no início da sessão: trocar no meio invalida o cache.
  Sonnet no dia a dia; Opus/Fable para arquitetura e raciocínio difícil; esforço
  low/medium para tarefas simples.
- /compact <foco> em pausas naturais; /rewind (Esc Esc) para abandonar um caminho
  errado (reaproveita o cache); /btw para perguntas que não devem ficar no contexto.
- /context e /usage para ver o que está consumindo tokens; desative MCPs sem uso
  (/mcp) e prefira CLIs (gh, aws, gcloud).
- Prompts específicos (arquivo, função, sintoma, critério de "pronto") evitam
  varreduras amplas.
- Limites determinísticos de subagentes: CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS e
  CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH.

Manutenção: para cada linha, pergunte "removê-la faria o Claude errar?". Se não,
corte. Se uma regra é ignorada, o arquivo provavelmente está longo demais. Regras
que precisam valer 100% das vezes viram hooks, não texto.

Fontes (documentação oficial da Anthropic):
- https://code.claude.com/docs/en/costs
- https://code.claude.com/docs/en/best-practices
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/prompt-caching
- https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
- https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5
-->

# Project

<!-- Preencha. Exemplos; apague o que não se aplica. -->
- Build: `<command>`
- Test one file: `<command> path/to/test` (prefer this over the full suite)
- Lint/typecheck: `<command>`
- Conventions that differ from language defaults: `<...>`
- Gotchas / non-obvious behavior: `<...>`

# Scope

- Deliver what was asked, at the scope intended. No unrequested features, refactors, renames, or cleanup of surrounding code.
- Don't add comments, docstrings, or type annotations to code you didn't change. Don't add error handling for cases that can't happen; validate only at system boundaries.
- No helpers or abstractions for one-time operations, and no design for hypothetical future needs.
- Make routine judgment calls yourself. Ask only when different readings of the request would lead to materially different work.
- Plan first only for multi-file or uncertain changes. If the diff fits in one sentence, just do it.
- Delete temporary scripts and files you created before finishing.

# Context and tools

- Never speculate about code you haven't opened; read it first. Read only what the task needs.
- Locate with Grep/Glob before reading. For large files, read the relevant line range, not the whole file.
- Don't re-read a file that is already in context or that you just edited.
- Run independent tool calls in parallel in a single turn.
- Keep command output small: run the narrowest test, use quiet flags, and pipe long output through `tail`, `head`, or `grep`.
- Prefer CLI tools (`gh`, cloud CLIs) over MCP servers when both can do the job.
- Use a subagent only for large, independent work (wide multi-file investigation) or to keep verbose output (logs, long test runs) out of the main context. Don't delegate what you can finish in a few tool calls, and don't use subagents to double-check your own work. One subagent is better than several.

# Verification

- Verify once with the narrowest relevant check (single test, typecheck of changed code), then stop. Don't repeat checks that already passed.
- Report evidence (command and result), not claims.

# Output

- Be concise. Lead with the outcome; no preamble, no recap of what the user said, no closing summary of unchanged facts.
- Don't narrate routine steps between tool calls. Speak up only for an important finding or a change of direction.
- Reference code as `path:line`; don't paste back code you just wrote or files the user can open.
- Correct an earlier statement only when the error would change the user's code or decisions.
- Written documents: cover the substance without filler sections or redundant summaries.
- Reply in the user's language.

# Compact instructions

When compacting, keep: the task goal, files modified, commands to build/test, decisions made and why, and open TODOs. Drop exploration output, file dumps, and resolved errors.
