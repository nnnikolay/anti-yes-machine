# Anti-Yes-Machine Workflow for Claude Code

A development workflow that encourages critical thinking and prevents superficial code approval. This workflow transforms Claude from a "yes-machine" that might overlook issues into a critical reviewer that actively seeks improvements.

## Philosophy

Most AI assistants tend to be overly agreeable, often approving code that "looks fine" without deep analysis. This workflow implements an "anti-yes-machine" approach through:

- **Proactive questioning** before implementation
- **Critical analysis** of existing code
- **Alternative thinking** to challenge assumptions
- **Adversarial testing** to find breaking points

## Installation

1. Clone this repository and run the install script from the repo root:
   ```bash
   git clone https://github.com/yourusername/anti-yes-machine.git
   cd anti-yes-machine
   ./install.sh /path/to/your/project
   ```

   Or install in current directory:
   ```bash
   ./install.sh
   ```

2. If you have an existing `CLAUDE.md`, add these references to it:
   ```markdown
   ## Project Guidelines
   
   Please load and follow these guidelines:
   - Core Rules: @.claude/guidelines/core-rules.md
   - Detailed Guidelines: @.claude/guidelines/detailed-guidelines.md
   - AI Agent Playbook: @.claude/guidelines/ai-agent-playbook.md
   ```

   If you don't have a `CLAUDE.md`, create one using the template above.

3. Verify installation:
   ```bash
   claude /help
   ```
   You should see the custom commands listed as "(project)" commands.

## Project Structure

After installation, your project will have:

```
your-project/
├── .claude/
│   ├── commands/          # Anti-yes-machine commands
│   │   ├── plan.md       # Pre-implementation analysis
│   │   ├── check.md      # Critical code review
│   │   ├── rethink.md    # Challenge assumptions
│   │   └── break.md      # Adversarial testing
│   └── guidelines/        # Frontend architecture rules
│       ├── core-rules.md
│       ├── detailed-guidelines.md
│       └── ai-agent-playbook.md
└── CLAUDE.md             # Loads guidelines at session start
```

## Available Commands

### `/plan [task description]`
**Purpose**: Analyze and clarify before coding

Use this BEFORE implementing any feature. Claude will:
- Identify reusable components
- Determine correct feature location
- Clarify ambiguous requirements
- Consider edge cases upfront
- Assess impact on existing code

**Example**:
```bash
/plan implement user notification system with email and in-app alerts
```

### `/check [files/feature]`
**Purpose**: Critical code review

Use this AFTER implementation. Claude will actively search for:
- Architecture violations
- Security vulnerabilities
- Missing error handling
- Performance issues
- Accessibility problems

**Example**:
```bash
/check features/notifications/
```

### `/rethink [problem/feature]`
**Purpose**: Challenge assumptions and explore alternatives

Use this when STUCK or before major decisions. Triggers extended thinking to:
- Question if you're solving the right problem
- Explore radically different approaches
- Identify hidden trade-offs
- Consider build vs. buy decisions
- Find simpler solutions

**Example**:
```bash
/rethink our approach to real-time notifications
```

### `/break [component/feature]`
**Purpose**: Generate adversarial tests

Use this BEFORE deployment. Claude will create tests for:
- Edge cases and invalid inputs
- Race conditions
- Security exploits
- Performance limits
- Integration failures

**Example**:
```bash
/break NotificationService
```

## Typical Workflow

### 1. Starting a New Feature
```bash
> /plan add shopping cart persistence across sessions

Claude: Before implementing, I need to clarify:
- Should we use localStorage, IndexedDB, or server-side storage?
- How long should cart items persist?
- What happens when prices change while items are in cart?
[...]
```

### 2. After Initial Implementation
```bash
> /check features/cart/

Claude: Critical issues found:
❌ No encryption for sensitive cart data in localStorage
❌ Race condition when updating quantities rapidly
⚠️ Cart totals recalculated on every render
[...]
```

### 3. When Facing Challenges
```bash
> /rethink cart state management getting complex with discounts

Claude: [Extended thinking activated]
Let me thoroughly analyze this problem...
Current approach uses local state with complex calculations...
Alternative approaches:
1. State machine pattern for cart states
2. Move calculations to backend
3. Use a cart service library like Saleor
[...]
```

### 4. Before Shipping
```bash
> /break CartCheckout component

Claude: Generated breaking test cases:
- Test: Submit with expired session token
- Test: Concurrent checkout attempts  
- Test: Unicode in promo codes
- Test: Negative quantities via API
[...]
```

## Best Practices

1. **Use `/plan` first** - Prevent problems before they occur
2. **Be specific with `/check`** - Target specific files or features
3. **Use `/rethink` for big decisions** - It activates extended thinking
4. **Run `/break` before deploys** - Catch issues before users do

## What's Included

### Frontend Guidelines
- **Core Rules**: Essential architecture patterns (feature-based structure, TypeScript standards)
- **Detailed Guidelines**: Comprehensive rules with examples for all aspects of frontend development
- **AI Agent Playbook**: Templates and patterns for common implementation scenarios

### Commands
Each command references the guidelines automatically, ensuring consistent critical analysis aligned with frontend best practices.

## Why This Works

1. **Cognitive Modes** - Each command triggers a different analytical mindset
2. **Proactive Prevention** - `/plan` catches issues before they're written  
3. **Deep Analysis** - `/rethink` uses extended thinking for complex problems
4. **Adversarial Testing** - `/break` thinks like an attacker
5. **Continuous Improvement** - Regular `/check` maintains quality

## Troubleshooting

**Commands not showing in `/help`**
- Ensure `.claude/commands/` exists in your project root
- Check that `.md` files have proper frontmatter
- Commands should show with "(project)" suffix

**Guidelines not being followed**
- Verify CLAUDE.md exists and references the guideline files
- Check that file paths in CLAUDE.md use the `@` prefix
- Ensure `.claude/guidelines/` contains all three guideline files

**Installation conflicts**
- The installer will warn about existing `.claude` directory
- Existing files are preserved unless you confirm overwrite

## Customization

To adapt for your project:

1. **Modify guidelines** in `.claude/guidelines/` to match your standards
2. **Edit commands** in `.claude/commands/` to change prompts
3. **Update CLAUDE.md** to include project-specific context

Each command supports:
- Custom frontmatter for metadata
- `$ARGUMENTS` placeholder for dynamic input
- References to project files with `@filename`

## License

MIT - See LICENSE file

Remember: The goal is not to make development harder, but to catch issues early when they're cheap to fix. This workflow helps Claude be a critical thinking partner, not just an agreeable assistant.
