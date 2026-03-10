# Environment File Safety - Aria Agent

## 🔒 Critical Safety Measures

Aria Agent is designed to **NEVER modify** your project's existing `.env` file. All configuration is isolated to the `Aria-Agent/` directory.

## ✅ What Aria Agent DOES

### Reads From:
- `Aria-Agent/.env` - Aria Agent's own configuration file
- Contains: Azure credentials, QA settings, automation flags

### Creates:
- `Aria-Agent/.env` - Only if copied from `Aria-Agent/.env.example` by user
- MCP config files: `.claude/`, `.cursor/`, `.windsurf/` (AI platform configs)

### Modifies:
- Nothing in your project root!
- Only creates files within `Aria-Agent/` directory

## ❌ What Aria Agent NEVER DOES

- ❌ Creates `.env` in your project root
- ❌ Modifies `.env` in your project root
- ❌ Reads `.env` from your project root
- ❌ Touches any existing project configuration

## 📂 File Structure

```
your-project/
├── .env                          # Your project's env (NEVER touched by Aria)
├── Aria-Agent/
│   ├── .env                      # Aria's env (only this is read)
│   ├── .env.example              # Template for Aria's env
│   ├── prompts/                  # Agent instructions
│   ├── output/                   # Aria's output directory
│   └── memory/                   # Aria's memory storage
└── [your project files]
```

## 🔐 Setup Instructions

When setting up Aria Agent:

```bash
# CORRECT - Copy to Aria-Agent directory
cp Aria-Agent/.env.example Aria-Agent/.env

# WRONG - Never copy to project root
cp Aria-Agent/.env.example .env  # ❌ DON'T DO THIS
```

## 🛡️ Protection Mechanisms

1. **Explicit Path References**: All prompts reference `Aria-Agent/.env` explicitly
2. **Git Protection**: `Aria-Agent/.env` is in `.gitignore`
3. **Clear Documentation**: SETUP-PROMPT.md emphasizes correct location
4. **Agent Instructions**: QA agent has explicit safety rules

## ✅ Verification

To verify Aria Agent is not interfering with your project:

```bash
# Check if .env was modified in your project root
git status .env

# Should show: "nothing to commit" or your own changes
# If it shows uncommitted changes from Aria, that's a bug!
```

## 📋 QA Agent Safety Rules

The QA agent follows these strict rules:

1. **READ FROM**: `Aria-Agent/.env` only
2. **NEVER CREATE**: `.env` in user's working repository
3. **NEVER MODIFY**: `.env` in user's working repository
4. **MISSING FILE**: If `Aria-Agent/.env` doesn't exist, skip QA phase with clear message

## 🎯 Summary

Aria Agent is designed to be a **good citizen** in your project:

- ✅ Isolated configuration in `Aria-Agent/` directory
- ✅ Never touches your project's `.env`
- ✅ Clear separation of concerns
- ✅ Safe to use in any project without risk of configuration conflicts

## 🔍 Troubleshooting

**If you suspect Aria Agent touched your .env:**

1. Check git history: `git log -- .env`
2. Check git status: `git status .env`
3. Verify Aria's config: `cat Aria-Agent/.env`
4. Report issue if you see unexpected changes

**Your `.env` file should remain under your control at all times!**