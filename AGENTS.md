# Common agent instructions

## Scope and instruction precedence

Apply language-specific guidance only to that language. Follow explicit user requirements and applicable project instructions within their stated project, repository, or paths. Use the most specific applicable scope for local requirements and conventions.

For style choices left open, prioritise consistency within the repository, then across the wider project when it spans multiple repositories, then the defaults in this file. Follow checked-in configuration and the established convention nearest the code being changed. Isolated defects or incidental inconsistencies do not establish a convention.

Apply the permission requirements below independently of style conventions and documented workflows. A command listed in project guidance does not itself authorise execution. If instructions conflict and prevent a correct decision, identify the conflict and seek clarification.

## Task scope and permissions

- Work within the user's requested scope. Read-only inspection and ordinary file edits in the current workspace are permitted unless the task restricts them. This permission does not cover the restricted operations below.
- Read applicable instructions and task-relevant documentation, code, and checked-in configuration before making changes.
- Read-only searches, file reads, dependency queries, `git status`, `git diff`, `git log`, `git show`, and branch queries are permitted. Other state-changing operations require approval unless specifically permitted here or already authorised by the user. Apply these permissions to the operation whether performed through shell commands, editing tools, APIs, or other interfaces.
- Run tests, linters, formatters, auto-fixers, and pre-commit hooks only when explicitly requested or approved. When proposing a run, state the exact command and obtain approval before executing it.
- Obtain explicit approval for the specific command or equivalent tool operation before changing dependencies or Git state. This includes installing, upgrading, or removing packages, and staging, committing, switching branches, creating or deleting branches, resetting, restoring, merging, rebasing, stashing, cleaning, pulling, or pushing.
- An explicit user request to execute a specific command or tool operation counts as approval for that execution. Reuse approval already given within its stated scope. A general request to fix, build, or finish work does not implicitly authorise restricted operations.
- Clean up caches, virtual environments, build outputs, or similar artefacts only when explicitly requested.
- Preserve existing work. During a review, assess the current working state as-is, including staged and unstaged changes. Discuss staging distinctions only when relevant to the code's logic or the requested task.
- Treat this common `AGENTS.md` as user-owned and frozen. Do not modify, replace, reformat, or append tool-managed content to it unless the user explicitly requests the change. Put tool-specific guidance in separate, tool-specific files.
- Read files containing secrets only when necessary for the authorised task. Do not introduce secrets into tracked files or expose them in tool output, logs, documentation, or responses. Use placeholders in examples.
- Keep changes focused on the task. Do not invent requirements or problems, or expand the scope based on unstated assumptions. Seek clarification when missing information materially affects the requested outcome.
- Keep responses direct, concise, and proportional to the task. In final responses, state the outcome, relevant decisions, meaningful validation performed, and any material unresolved limitations. Distinguish checks that passed from checks that were not run. Link relevant files where useful. Avoid filler, repetition, unsupported claims, and hypothetical problems unrelated to the task.

## Design and implementation

Use the Zen of Python as practical design guidance:

- Make behaviour, assumptions, contracts, and dependencies explicit.
- Prefer simple, understandable solutions. Readability matters more than brevity or cleverness.
- Handle errors meaningfully. Make intentional suppression explicit and justified.
- Preserve consistent rules while allowing reasoned exceptions when practicality warrants them.

Choose descriptive domain names and clear operations. Use named intermediate results when they clarify a calculation or transformation. Keep functions and modules focused on recognisable responsibilities. Introduce abstractions when they solve a concrete problem.

Reuse supported extension points. Preserve public contracts unless the task intentionally changes them, and account for affected callers and documentation when it does.

Keep resource ownership and lifetime explicit through context managers or reliable clean-up. Keep optional integrations and their dependencies optional. Follow the repository's build, dependency, and packaging model.

Prefer British English in new names and prose. Preserve established repository spelling, external API vocabulary, and compatibility-sensitive identifiers.

## Python structure, naming, and imports

- Use `snake_case` for functions, variables, and modules; `PascalCase` for classes; and `UPPER_SNAKE_CASE` for constants. Prefer meaningful names even when longer. Retain useful, established domain abbreviations.
- Give ordinary helpers descriptive names without a single leading underscore. Framework hooks, special methods, private model attributes, and cached backing properties may retain their required or established naming.
- Prefer module-qualified standard-library and third-party usage, such as `pathlib.Path`, `pandas.DataFrame`, and `pydantic.Field`. Avoid habitual shorthand aliases such as `pd` and `np`. Direct imports of selected types and framework classes, and deliberate aliases for re-exports or entry points, remain appropriate.
- Use relative imports within packages and intended public imports for consumers. Preserve local imports that keep optional integrations optional.
- For new Python packages, prefer a `src` layout. When tests are introduced, organise them separately around the package's responsibilities. Follow the existing layout when extending a repository.
- Define `__all__` for modules exposing a supported import surface, including deliberate package re-exports. It may be omitted in entry points and application-only modules.
- Prefer `LOGGER` for a module logger when existing repository naming leaves a choice.

## Python typing and runtime validation

- Annotate function parameters and return values, including `None` returns. Follow the repository's method-annotation conventions. Prefer `typing.Self` where appropriate and supported. Include useful annotations in tests and fixtures.
- Use concrete types, built-in generic collections, unions, enums, constrained types, and explicit models where they describe the domain. Allow `Any`, plain dictionaries, and similar flexibility when greater precision adds complexity without practical value.
- Match typing syntax to the supported Python versions and runtime consumers of annotations. Add postponed annotations or quoted forward references for a concrete compatibility need, following the existing configuration.
- Prefer Pydantic models and field constraints for runtime data contracts. Use validators to express relationships between fields.
- Prefer `@pydantic.validate_call(validate_return=True)` wherever compatible unless there is a reason to omit it. This preference also applies to internal helpers. Respect framework integration, explain non-obvious exceptions, and use suitable alternative validation where needed.
- Decide unexpected-field handling per model. Prefer `extra="forbid"` where practical, without imposing it on every model or requiring coercion-free strictness everywhere.
- Use positional-only primary operands and keyword-only configuration options where they clarify library APIs. Preserve existing signatures and framework calling requirements.

## Documentation

Keep documentation accurate, close to the contract, and concise enough to remain useful. Update it when behaviour or supported usage changes. Explain constraints, reasons, and non-obvious decisions. Let descriptive code carry the obvious mechanics.

In Markdown and plain-text documents, keep each prose paragraph or list-item paragraph on one source line and let the editor or renderer wrap it visually. Do not insert line breaks merely to meet a character count. Preserve structural and meaningful line breaks, including those in headings, lists, tables, code blocks, and other line-oriented content.

For Python source code:

- Document modules, public functions, and public classes. Document internal helpers where their behaviour needs explanation, following repository conventions.
- Use NumPy-style docstrings with a short, direct summary and the sections the contract needs. Describe parameters, defaults, returns, raised exceptions, and examples where useful. Avoid empty sections and repetitive filler.
- Use model field descriptions and examples where schemas feed API or tool documentation.
- Keep docstring examples illustrative by default. When tests are appropriate, verify behaviour through separate pytest tests. Retain an existing doctest setup where the repository uses one.

Use specific lint-rule suppressions and explain why they are needed. Explain intentional error suppression. Keep temporary TODO/FIXME comments actionable, including what needs to change and why the current approach exists.

When choosing tooling for new Python documentation, prefer Sphinx for detailed API references and MkDocs for predominantly Markdown guides. Follow an existing documentation system when one is already established.

## Testing

Time test investment to project maturity and behavioural stability. Add meaningful tests when the project or relevant behaviour is stable enough. Learning, exploration, and rapidly changing implementations need not start with tests merely to satisfy a rule. Respect existing suites and their established role when changing mature behaviour.

Test real logic directly where practical. Use mocks selectively to isolate external systems, expensive operations, failure modes, or a boundary's responsibilities. Test API handlers and service integration when they contribute behaviour such as validation, authorisation, response mapping, lifecycle handling, or error propagation. Avoid duplicating checks across layers or testing a third-party implementation merely because the application calls it.

For Python:

- Prefer ordinary pytest tests with descriptive names, parameterisation for related cases, and fixtures for genuinely shared setup. Follow the repository's test organisation.
- Prefer a short docstring for each test function. A sufficiently descriptive test name can stand alone. Full, detailed NumPy-style sections are not required for every unit test.
- Cover meaningful successful, negative, boundary, and invalid-input cases. Assert exceptions, warnings, logs, or schemas when they form part of the observable contract.
- Use real temporary files, dataframes, and local operations when practical. Make live external-service prerequisites explicit.
- Use Hypothesis when property-based testing adds clear value.

## Error handling and logging

- Represent domain failures with meaningful errors using the language's established mechanisms. Preserve causes when translating exceptions, for example with `raise ... from error` in Python.
- Catch specific exceptions when their recovery or classification is known. Use broad catches at boundaries with a clear responsibility, such as logging an unexpected failure, releasing resources, recording a failed operation, or translating an error. Propagate failures as appropriate to the contract.
- Keep unexpected failures diagnosable. Documented predicate results, optional results, and expected control flow may handle exceptions without logging or re-raising every catch. Explain intentional suppression or fallback.
- Follow existing logging interfaces and application ownership. In Python libraries, use module loggers and package-level `NullHandler` support. Let the consuming application own handler configuration. Services should use their central logging setup and existing structured-logging conventions.
- Include useful operation and correlation context, outcomes, and duration where relevant. Keep routine noise low. Use timezone-aware UTC for operational timestamps where applicable, while preserving domain-specific time semantics.
- Protect secrets and sensitive values in logs, validation errors, and exception messages. Prefer relevant, safe context over indiscriminate payload dumps.
- Follow the repository's message-formatting convention. Python logging f-strings are acceptable where used. Use structured fields when the logger supports them. Event taxonomies, sinks, and log levels belong to project context.

## Formatting and quality tools

Respect checked-in configuration, including `pyproject.toml`, `.pre-commit-config.yaml`, and tool-specific files. Follow existing hook stages, quality gates, supported runtimes, and dependency constraints.

For Python source code, follow the repository's established formatting and linting tools. Where the project leaves tool choice open, prefer Black for formatting and Pylint for linting. Keep isort compatible with Black. Where no convention exists, prefer 99 columns, four-space indentation, and double quotes. Follow configured trailing-comma behaviour. Tool execution still requires the approval specified above.

Retain the established toolchain and its separate formatting, linting, semantic, security, and typing checks. Mypy/Pyright are optional. Retain existing setups, but introduce them only for a concrete requirement. Coverage thresholds and tool versions are repository choices.
