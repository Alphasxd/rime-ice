# Repository Guidelines

## Project Structure & Module Organization
This repository is a Rime configuration set. Per the official Rime docs, Rime is the input method engine/framework, while Squirrel (macOS) and Weasel (Windows) are separate frontends. Input behavior is defined by root schema files such as `rime_ice.schema.yaml`, `double_pinyin.schema.yaml`, `melt_eng.schema.yaml`, and `radical_pinyin.schema.yaml`. Shared defaults live in `default.yaml`. On macOS, frontend appearance and app-specific behavior are controlled by `squirrel.yaml`; `weasel.yaml` contains the Windows frontend configuration and does not affect local Squirrel behavior. Core dictionaries are in `cn_dicts/`, `en_dicts/`, and `opencc/`, while Lua translators and filters live in `lua/`. `custom_phrase.txt` stores custom phrase mappings. The `others/` directory is auxiliary material and does not participate in local Squirrel runtime behavior.

## Build, Test, and Development Commands
There is no standalone build step for local development. The normal loop is: edit config files, redeploy Squirrel, then verify behavior in real text fields. Useful commands:

- `rg --files` to scan available schemas, dictionaries, and Lua modules.
- `rg "lua_(translator|filter)|translator|filter" *.yaml lua/` to trace how a Lua module is wired into a schema.
- `git diff -- squirrel.yaml lua/ cn_dicts/ en_dicts/ opencc/` to review only runtime-relevant changes.

## Coding Style & Naming Conventions
Keep files UTF-8 and preserve existing Chinese comments and section headers. YAML uses two-space indentation and lower_snake_case keys. Lua files in this repo use tabs and simple module exports such as `local M = {}` followed by `return M`. Dictionary content after `# +_+` must remain tab-delimited as `词<TAB>编码或拼音<TAB>权重`; replacing tabs with spaces will break parsing and sorting.

## Testing Guidelines
This repo does not include an automated test suite. Verify changes by redeploying Squirrel and testing the affected path directly: candidate ordering, mixed Chinese-English input, custom phrases, translators, filters, and frontend style changes in `squirrel.yaml`. For bug fixes, record the schema name, macOS version, Squirrel version, and exact reproduction steps.

## Commit & Pull Request Guidelines
Match the existing commit style: `dict:`, `fix:`, `feat:`, `doc:`, and `build(ci):`. Keep subjects short and issue-oriented, for example `dict: 增改词汇 (#1489)` or `fix: uuid random seed #1473`. PRs should state which schema or dictionary changed, whether `squirrel.yaml` behavior changed on macOS, how the change was verified, and link the related issue when applicable.
