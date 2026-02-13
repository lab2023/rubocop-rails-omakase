# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is `rubocop-rails-omakase`, a Ruby gem that distributes curated RuboCop linting rules for Rails applications. It is a **configuration-only gem** — the value is entirely in the YAML config files, not Ruby code. The gem is included by default in Rails 7.2+ applications.

## Structure

- `rubocop.yml` — Core RuboCop configuration, distributed via the gem package (`s.files`)
- `.erb_lint.yml.template` — Reference template for ERB linting; **not included in the gem package**. Users copy this to their project root as `.erb_lint.yml`
- `rubocop-rails-omakase.gemspec` — Gem metadata, version (`s.version`), and dependencies
- `lib/rubocop-rails-omakase.rb` — Empty stub for bundler compatibility

## Key Style Decisions in rubocop.yml

- **Double quotes** for strings (`Style/StringLiterals`)
- **All metrics cops disabled** (AbcSize, ClassLength, MethodLength, BlockLength, LineLength, etc.)
- **Documentation cops disabled** (Style/Documentation, FrozenStringLiteralComment)
- **New cops auto-enabled** (`NewCops: enable`)
- **Plugins**: rubocop-rails, rubocop-performance, rubocop-minitest, rubocop-rake, rubocop-thread_safety

## Build

```bash
gem build rubocop-rails-omakase.gemspec
```

## How Consumers Use This Gem

RuboCop config is inherited via `inherit_gem` in the consumer's `.rubocop.yml`:
```yaml
inherit_gem:
  rubocop-rails-omakase: rubocop.yml
```

ERB Lint does **not** support `inherit_gem`. Consumers must copy `.erb_lint.yml.template` to their project as `.erb_lint.yml`. The template uses `inherit_from: .rubocop.yml` so gem rules are inherited indirectly through the consumer's local `.rubocop.yml`.

## Editing Guidelines

- Changes to `rubocop.yml` affect all downstream Rails applications using this gem.
- Only `rubocop.yml` is in `s.files` (shipped with the gem). The `.erb_lint.yml.template` lives in the repo as a reference only.
- When updating `.erb_lint.yml.template`, also update the matching example in `README.md` under the "ERB Lint" section.
