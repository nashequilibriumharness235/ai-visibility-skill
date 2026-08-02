# ai-visibility

A Claude Skill that measures whether AI assistants recommend your brand — and
tells you what to fix when they don't.

ChatGPT, Claude, Gemini, Perplexity and Copilot don't rank pages, they
recommend brands. A buyer asks "which is the best X for Y?" and gets three to
five names. There is no second page. If you're not in the list, you don't
exist for that buyer.

This skill turns that into something measurable: run your buyers' real
questions past several assistants, score every answer, and get a ranked fix
list.

Free, self-hosted, no account, no database. Python standard library only.

## Install

**Claude Code / Claude Desktop — for all projects:**

```bash
git clone https://github.com/petar-nauka/ai-visibility-skill ~/.claude/skills/ai-visibility
```

**One project only:**

```bash
git clone https://github.com/petar-nauka/ai-visibility-skill .claude/skills/ai-visibility
```

Or just copy the folder there. Restart the session and ask something like
*"check whether ChatGPT recommends my site"* — the skill triggers on its own.

**claude.ai:** grab `ai-visibility.zip` from the
[latest release](https://github.com/petar-nauka/ai-visibility-skill/releases/latest)
and upload it under Settings → Capabilities → Skills.

**Perplexity Computer:** grab `ai-visibility-perplexity.zip` from the same
release and upload it at
[perplexity.ai/computer/skills](https://www.perplexity.ai/computer/skills) →
*Create skill* → *Upload a skill*.

The two archives are not interchangeable, and swapping them is the usual reason
an upload fails: Claude expects `SKILL.md` under a top-level folder, Perplexity
expects it at the archive root. To rebuild both from source:

```bash
python scripts/package.py
```

Requires Python 3.10+. Nothing to `pip install`.

## Quick start

```bash
cd ~/.claude/skills/ai-visibility/scripts

python aeo.py -w ~/my-audit init example.com --name "Example"
# fill in brand.json (aliases, keywords, competitors) and prompts.json

python aeo.py -w ~/my-audit run --models chatgpt gemini perplexity claude
# paste each assistant's answer into the generated files

python aeo.py -w ~/my-audit analyze
python aeo.py -w ~/my-audit report      # → report.html + summary.md
```

Or let Claude drive it — that's the point of the skill. It will interview you
for the brand config, help you write prompts your buyers would actually type,
read the results with you, and produce the fix list.

**[EXAMPLES.md](EXAMPLES.md) has seven ready-to-send prompts** (English and
Bulgarian) covering the full audit, a five-minute crawler check, finding out
what your buyers actually ask, diagnosing a specific loss, rewriting one page,
fixing a wrong description, and the monthly re-check. Start there if you'd
rather not touch the CLI at all.

To skip the pasting, set `OPENROUTER_API_KEY` and use `collect` instead of
`run`. One key covers every model; expect a few cents per prompt.

## What you get

**Measurement**

- Visibility % overall, per model, and split by purchase intent
- Share of voice against every competitor the models named
- Your average position in ranked lists (being 5th of 5 is not being mentioned)
- The sentence each model uses to describe you — your positioning as the model
  understands it, which you did not write
- Which pages the models cite, ranked — a concrete outreach target list
- Month-over-month trend, withheld when two runs aren't comparable

**Diagnosis**

- Every high-intent prompt you lost, and who won it instead
- Root-cause analysis: coverage, consistency, or specificity
- Mentions that are present but weak, buried, or off-message

**Fixes**

- Crawler audit across 19 AI bots (`GPTBot`, `ClaudeBot`, `PerplexityBot`,
  `OAI-SearchBot`, `Google-Extended`, …) with robots.txt recipes
- Per-page AEO scoring against seven criteria, with paste-ready rewrites
- JSON-LD generator (`Organization`, `FAQPage`, `Article`, `Product`)
- `llms.txt` generator from your sitemap
- Bing Webmaster setup — ChatGPT searches through Bing
- GA4 channel-group regex for measuring AI referral traffic

## Commands

| Command | Purpose |
|---|---|
| `init <domain>` | Create the workspace |
| `run [--models …]` | Create empty answer files to paste into |
| `collect [--models …]` | Fetch answers automatically via OpenRouter |
| `analyze` | Score every answer |
| `report` | Build `report.html` + `summary.md` |
| `tech [domain]` | Crawler access, llms.txt, sitemap, schema, AEO score |
| `page <url>` | Extract AEO signals from one page |
| `schema <spec.json>` | Generate JSON-LD |
| `llmstxt` | Generate `llms.txt` |
| `selftest` | Verify the detection logic (49 checks) |

## Language

The tooling is language-neutral — ask your buyers' questions in your buyers'
language. Bulgarian and English sentiment vocabularies ship by default; for
other languages extend `POSITIVE_WORDS` / `NEGATIVE_WORDS` in
`scripts/aeolib.py` and set `context_words` in `brand.json`.

## Layout

```
SKILL.md              the workflow Claude follows
EXAMPLES.md           seven prompts to copy and send (EN + BG)
scripts/aeo.py        CLI
scripts/aeolib.py     mention, competitor, sentiment, list-position, citation detection
scripts/aeoweb.py     crawler / robots.txt / schema / page-signal checks
scripts/aeoreport.py  metrics and report generation
scripts/package.py    build the install archives for each platform
references/           prompt research, diagnosis, content rubric, technical fixes, playbook
```

## Verify

```bash
python scripts/aeo.py selftest
```

49 checks against realistic AI answers in both Bulgarian and English —
markdown link lists, numbered headings with prose between items, inline
domains, robots.txt edge cases, tracking-parameter stripping.

## License

MIT.

---

## На български

Умение за Claude, което измерва дали AI асистентите (ChatGPT, Claude, Gemini,
Perplexity, Copilot) препоръчват твоя бранд — и какво да поправиш, ако не го
правят.

AI търсенето не класира страници, а **препоръчва брандове**. Купувачът пита
„кой е най-добрият X за Y?" и получава три до пет имена. Няма втора страница.
Ако те няма в списъка, за този купувач не съществуваш.

Инструментът пуска реалните въпроси на твоите клиенти през няколко асистента,
анализира отговорите и връща приоритизиран списък с поправки: къде губиш от
конкурент, защо, и какво точно да направиш. Безплатно, локално, без
регистрация и без база данни.

Инсталация: копирай папката в `~/.claude/skills/ai-visibility`, рестартирай
сесията и просто попитай „провери дали ChatGPT препоръчва сайта ми". Claude ще
те преведе през целия процес — на български.

За claude.ai качи `ai-visibility.zip`; за Perplexity Computer —
`ai-visibility-perplexity.zip`. И двата са в
[Releases](https://github.com/petar-nauka/ai-visibility-skill/releases/latest).
Архивите са различни (Claude иска `SKILL.md` в папка, Perplexity — в корена),
затова не ги разменяй.

Готови промпти за копиране (на български и английски) има в
**[EXAMPLES.md](EXAMPLES.md)** — пълен одит, петминутна проверка дали AI
ботовете четат сайта ти, откриване на реалните въпроси на клиентите ти,
диагноза защо конкурент печели, пренаписване на конкретна страница, поправяне на
грешно описание и месечна повторна проверка.
