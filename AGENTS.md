# Repository Guidelines

## Project structure

This repository stores reusable development materials. `README.md` describes the
available templates, and `common_make/` contains two GNU Make templates:

- `Makefile.bin` builds the `app_foo` executable from C++ sources in the project
  root and `src/`, with headers in `include/`.
- `Makefile.libs` builds `libs/libfoo.a` from C++ sources in `src/`, with headers
  in the project root and `include/`.

These files are templates for consumer projects. The repository currently has
no C++ sources, automated test suite, or CI configuration.

## Build and validation

Run the templates from a consumer project directory, adjusting the template path
as needed:

```sh
make -f /path/to/materials/common_make/Makefile.bin
make -f /path/to/materials/common_make/Makefile.libs
```

Both templates provide `all`, `clean`, and `distclean` targets. `clean` removes
objects and the build target; `distclean` also removes generated dependency
files. Compilation stores dependency files under `.deps/`.

For changes to a template, validate it in a temporary fixture outside the
repository with representative C++ sources and headers. Build the target, check
that header changes rebuild dependent objects, and exercise the cleanup targets.
The executable template requires a source defining `main`. The library template
requires source files under `src/`. A build from this repository alone is not a
meaningful validation of the executable template because it has no input sources.

## Editing conventions

- Keep Make recipes tab-indented and preserve GNU Make syntax.
- Follow the variable naming and formatting of the template being edited.
- Preserve source discovery, dependency generation, and cleanup behavior unless
  the requested change explicitly alters them.
- Keep changes focused and document changes to template usage in `README.md`.
- Do not commit generated binaries, objects, archives, or `.deps/` files.

## Change reporting

Describe which template changed, the resulting build behavior, and the validation
performed. If validation could not be run, state that explicitly. Do not claim
repository-wide tests exist or pass when only a temporary fixture was checked.
