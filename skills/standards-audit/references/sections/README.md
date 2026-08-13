# Sections

One file per category in [`../standards.md`](../standards.md). `standards.md` says which standards
exist; these files say what each one means at each of the four levels in
[`../levels.md`](../levels.md).

## Conventions

- File name is the category name, lower-cased and hyphenated.
- `# <Category>` matches the category heading in `standards.md` exactly.
- `## <Standard>` matches the standard name in `standards.md` exactly — the same join key the facet
  files in [`../stacks/`](../stacks/) use, so names must be globally unique.
- Content is stack-neutral. Concrete tooling belongs in the facet files.
- **Applies when** marks a precondition; if it doesn't hold the control is not applicable, and an
  audit must cite the failed precondition.
- **Requires** marks a prerequisite control; implementing this one first is worse than not
  implementing it, so an audit reports it as *blocked* rather than *missing*.
- *As level N* means nothing further is required. **Binary** means the control is on or off.
- Effort is per level: **S** hours, **M** a day or two, **L** a week or more.
