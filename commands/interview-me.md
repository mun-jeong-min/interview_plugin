# /interview-me

Generate technical interview questions from your recent code changes.

## Usage

```
/interview-me [file_path] [options]
```

## Arguments

- `file_path` (optional): Specific file to analyze. If omitted, uses `git diff HEAD~1`.

## Options

- `--staged`: Analyze only staged changes (`git diff --staged`)
- `--recent=<duration>`: Analyze files modified within duration (e.g., `30m`, `2h`, `1d`)
- `--count=<n>`: Number of questions to generate (default: 3, max: 7)
- `--focus=<categories>`: Filter by categories (comma-separated: `security,perf,concurrency`)
- `--lang=<ko|en>`: Question language (default: ko)
- `--show-answer`: Show model answer outlines
- `--save`: Save output to `.interview/YYYY-MM-DD-HHmm.md`

## Instructions

You are the interviewer agent defined in `agents/interviewer.md`. Follow these steps:

### Step 1: Collect Target Code

Based on the arguments provided:

1. **No arguments**: Run `git diff HEAD~1` to get the last commit's changes
2. **File path given**: Read the specified file
3. **`--staged` flag**: Run `git diff --staged`
4. **`--recent=Nm` flag**: Find files modified in the last N minutes using `find . -mmin -N -type f`

If no changes are found, inform the user and suggest alternatives.

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
- Model answer outline (only if `--show-answer`)

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
     `/interview-me --show-answer` 로 모범 답안 볼 수 있음.
─────────────────────────────────────────────────────────────────────
```

**Design rules:**
- Header uses round corners (╭╮╰╯)
- Question boxes use square corners (┌┐└┘)
- Box width: 60 characters fixed
- Question text: max 54 chars per line, auto-wrap
- Category emojis: 🔒 Security / ⚡ Concurrency / 🚀 Performance / 🏛 Design / 🧪 Testing / 👁 Observability / 🗄 Data
- Difficulty: ★☆☆ / ★★☆ / ★★★ (exactly this format)

### Step 5: Handle --show-answer

If `--show-answer` is specified, add an answer outline section inside each question box:

```
│                                                                    │
│  📝 Answer Outline:                                                │
│  • Key point 1                                                     │
│  • Key point 2                                                     │
│  • Key point 3                                                     │
│                                                                    │
```

### Step 6: Handle --save

If `--save` is specified:
1. Create `.interview/` directory if it doesn't exist
2. Save output to `.interview/YYYY-MM-DD-HHmm.md`
3. Confirm the save location to the user

### Language

- Default output language: Korean (ko)
- If `--lang=en`, output questions in English
- Technical terms can remain in English regardless of language setting
