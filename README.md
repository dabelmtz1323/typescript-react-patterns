# typescript-react-patterns

> Production-grade Agent Skill for TypeScript, React, and Next.js.

A structured markdown reference that helps AI coding agents generate better code, catch bugs, review PRs, and make architecture decisions for frontend projects.

**Not an npm package. Not a tutorial.** A field manual for AI agents helping frontend engineers.

## Before / After

### Without this skill

```
User: "Type this API call"
Agent: const user: User = await res.json()  // ❌ no runtime validation
```

### With this skill

```
User: "Type this API call"
Agent:
  const userSchema = z.object({ id: z.string(), name: z.string(), email: z.string().email() })
  type User = z.infer<typeof userSchema>
  const user = userSchema.parse(await res.json())  // ✅ runtime + type safety
```

### Without this skill

```
User: "Add loading state to this component"
Agent: const [isLoading, setIsLoading] = useState(false)
       const [error, setError] = useState(null)
       const [data, setData] = useState(null)  // ❌ boolean flags, nullable fields
```

### With this skill

```
User: "Add loading state to this component"
Agent: // Uses discriminated union — impossible states are unrepresentable
  type State<T> =
    | { status: 'idle' }
    | { status: 'loading' }
    | { status: 'success'; data: T }
    | { status: 'error'; error: Error }
```

## Install

```bash
# Global (all projects)
git clone https://github.com/leejpsd/typescript-react-patterns.git ~/.claude/skills/typescript-react-patterns

# Project-only
git clone https://github.com/leejpsd/typescript-react-patterns.git .claude/skills/typescript-react-patterns
```

Compatible with Claude Code, Cursor, Codex, Gemini CLI, and any agent reading `SKILL.md`.

## Structure

```
typescript-react-patterns/
├── SKILL.md                                ← Hub: agent rules, decision guide, checklists
├── rules/                                  ← Pattern references (loaded on demand)
│   ├── typescript-core.md                     Narrowing, unions, generics, utility types, as const, satisfies
│   ├── react-typescript-patterns.md           Props, children, events, hooks, context, forwardRef
│   ├── nextjs-typescript.md                   App Router, params, Server Actions, RSC, Edge, useOptimistic
│   ├── component-patterns.md                  Discriminated Props, compound components, modal/dialog, polymorphic
│   ├── data-fetching-and-api-types.md         Fetch, Zod, TanStack Query, Result<T,E>, pagination, error handling
│   ├── forms-and-validation.md                Form state, Zod, react-hook-form, Server Actions, multi-step forms
│   ├── state-management.md                    Local vs context vs Zustand (+ middleware) vs TanStack Query vs URL
│   ├── performance-and-accessibility.md       Memoization, effects, semantic HTML, ARIA, focus management
│   ├── debugging-checklists.md                Quick diagnosis router, serialization, null access
│   ├── code-review-rules.md                   Risk vs preference, architecture smells, comment templates
│   └── anti-patterns.md                       12 common mistakes with root causes and fixes
├── playbooks/                              ← Step-by-step debugging guides
│   ├── type-error-debugging.md                Systematic type error resolution
│   ├── hydration-issues.md                    SSR/CSR mismatch diagnosis flowchart
│   └── effect-dependency-bugs.md              Loops, stale closures, cleanups, debounce example
├── README.md
└── LICENSE
```

## What Makes This Different

| Feature | Typical skill | This skill |
|---------|-------------|-----------|
| Pattern guidance | ✅ | ✅ |
| Agent behavior rules | ❌ | ✅ What to check first, what NOT to assume |
| Decision guide | ❌ | ✅ Situation → recommended pattern → file |
| Debugging playbooks | ❌ | ✅ Type errors, hydration, effects, serialization |
| Code review heuristics | ❌ | ✅ Risk vs preference, comment templates |
| Rule classification | ❌ | ✅ [HARD RULE] / [DEFAULT] / [SITUATIONAL] |
| Generation + review checklists | ❌ | ✅ Separate checklists in SKILL.md |

## Each File Contains

Consistent structure across all modules:

- **Scope** + **Consult when** + **See also** (cross-references)
- **Key rules** labeled as [HARD RULE], [DEFAULT], or [SITUATIONAL]
- **When to use / When NOT to use**
- **Examples** from real frontend scenarios (forms, APIs, lists, modals)
- **Counterexamples** showing the wrong approach
- **Tradeoffs** — what you gain and what you lose
- **Common bug patterns** — specific failure modes
- **Review checklist** — copy-pasteable for PRs

## Contributing

PRs welcome. Priority areas:

- Testing patterns (Vitest, Testing Library)
- Internationalization typing
- More debugging playbooks (React Query cache, Zustand devtools)
- Accessibility deep dive (ARIA patterns, focus management)
- Edge/middleware typing patterns

### How to contribute a new rule

Each file should follow the template in any existing `rules/*.md` file:
1. Scope + Consult when + See also
2. Patterns labeled [HARD RULE] / [DEFAULT] / [SITUATIONAL]
3. Real-world examples (not toy math examples)
4. Common bug patterns
5. Review checklist

## License

MIT
