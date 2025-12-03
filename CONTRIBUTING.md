# Contributing Guide

**Purpose:** Guidelines for contributing to this Arch Linux Modular Installation System repository.

**Thank you for your interest in contributing!** Your contributions help make this installation system better for everyone.

---

## How to Contribute

### Reporting Issues

If you find a problem:
1. Check if issue already exists
2. Create detailed issue report:
   - What happened
   - What you expected
   - Steps to reproduce
   - Your hardware/configuration

### Suggesting Improvements

Have an idea for improvement?
1. Check if similar suggestion exists
2. Create detailed proposal:
   - What you want to improve
   - Why it's useful
   - How it should work

### Contributing Code/Documentation

1. Fork the repository
2. Create a branch for your changes
3. Make your changes
4. Test thoroughly
5. Submit a pull request

---

## Contribution Guidelines

### Documentation Standards

- **Markdown format:** All documentation in Markdown
- **Consistency:** Follow existing style and structure
- **Clarity:** Write clearly and concisely
- **Links:** Include links to ArchWiki where relevant
- **Examples:** Provide practical examples

### Module Structure

When adding/editing modules, follow this structure:

```markdown
# Module: [Name]

**Purpose:** [Clear purpose statement]

**Prerequisites:**
- [List prerequisites]

**Time:** [Time estimate]

**ENVIRONMENT:** [Where to run - Live USB, chroot, after boot]

---

## Step 1: [Step name]
[Instructions]

## Step 2: [Step name]
[Instructions]

## Verification
[How to verify it works]

## Troubleshooting
[Common issues and solutions]

## Official Resources
[Links to ArchWiki, etc.]
```

### Phase Structure

When adding/editing phases:

```markdown
# Phase [XX]: [Name]

**Purpose:** [Clear purpose]

**Time Estimate:** [Time]
**Difficulty:** [⭐ Easy | ⭐⭐ Intermediate | ⭐⭐⭐ Advanced]

**Prerequisites:**
- [List prerequisites]

**Steps (execute in order):**
1. [Step with link]
2. [Step with link]

## Verification
[Verification steps]

## Navigation
**Previous:** [Link]
**Next:** [Link]
```

---

## Code of Conduct

- Be respectful and constructive
- Help others learn
- Follow Arch Linux principles
- Maintain quality standards

---

## Testing

Before submitting:
- ✅ Test all commands
- ✅ Verify all links work
- ✅ Check markdown formatting
- ✅ Ensure consistency with existing docs

---

## Pull Request Process

1. **Fork and branch:** Create feature branch
2. **Make changes:** Follow guidelines
3. **Test:** Verify everything works
4. **Document:** Update relevant docs
5. **Submit:** Create pull request with description

### PR Description Should Include:
- What changed
- Why it changed
- How to test
- Screenshots (if applicable)

---

## Areas Needing Contribution

### High Priority
- Additional hardware support
- More troubleshooting solutions
- Better examples
- Translation (future)

### Medium Priority
- Performance optimization tips
- Additional desktop environments
- More comparison tables
- Visual diagrams

### Low Priority
- Code examples
- Automation scripts
- Additional use cases

---

## Questions?

- Open an issue for discussion
- Check existing issues
- Review documentation

---

**Thank you for contributing!** 🎉

---

**Back to:** [Main Index](INDEX.md) | [Repository Root](README.md)
