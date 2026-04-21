# /interview-me

Generate technical interview questions from your recent code changes.

## Usage

```
/interview-me [options]
```

## Options

- `--count=<n>`: Number of questions to generate (default: 3, max: 7)
- `--lang=<ko|en>`: Question language (default: ko)

## Instructions

You are the interviewer agent defined in `agents/interviewer.md`. Follow these steps:

### Step 1: Collect Target Code

Run `git diff HEAD~1` to get the last commit's changes.

If no changes are found, inform the user and suggest committing some code first.

### Step 2: Analyze the Code

Read the code carefully and identify:
- Security concerns (auth, input validation, secrets, injection)
- Concurrency issues (race conditions, transactions, locks)
- Performance problems (N+1, missing indexes, memory leaks)
- Design smells (tight coupling, god classes, missing abstractions)
- Testing gaps (untested paths, hard-to-mock dependencies)
- Observability needs (logging, metrics, error handling)
- Data issues (schema problems, consistency, migrations)

### Step 3: Generate Questions

Create questions following these rules:

**Quality criteria:**
- Reference specific code locations (file:line)
- Ask about concrete implementation choices, not abstract theory
- Include failure scenarios and edge cases
- Probe trade-offs and alternatives

**Avoid:**
- "What does this code do?" explanations
- Textbook definitions
- Questions about code that doesn't exist

**Format each question with:**
- Category emoji and name
- Difficulty stars (★☆☆ / ★★☆ / ★★★)
- Clear question text (max 54 chars per line)
- Code reference (📍 file:line)
- Hint keywords (💡 Hint: ...)

### Step 4: Output Design

Use this exact visual format:

```
╭──────────────────────────────────────────────────────────────╮
│  🎤  Interview Me  ·  {count} questions                      │
│  Target: {target_description}                                │
╰──────────────────────────────────────────────────────────────╯

┌─ Q1 ─────────────────────────────── {emoji} {category} · {stars} ─┐
│                                                                    │
│  {question_line_1}                                                 │
│  {question_line_2}                                                 │
│  {question_line_3}                                                 │
│                                                                    │
│  📍 {file}:{line}                                                  │
│  💡 Hint: {keyword1}, {keyword2}, {keyword3}                       │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘

... more questions ...

─────────────────────────────────────────────────────────────────────
 💬  답변 준비되면 그냥 이어서 말하면 돼.
─────────────────────────────────────────────────────────────────────
```

**Design rules:**
- Header uses round corners (╭╮╰╯)
- Question boxes use square corners (┌┐└┘)
- Box width: 60 characters fixed
- Question text: max 54 chars per line, auto-wrap
- Category emojis: 🔒 Security / ⚡ Concurrency / 🚀 Performance / 🏛 Design / 🧪 Testing / 👁 Observability / 🗄 Data
- Difficulty: ★☆☆ / ★★☆ / ★★★ (exactly this format)

### Language

- Default output language: Korean (ko)
- If `--lang=en`, output questions in English
- Technical terms can remain in English regardless of language setting
