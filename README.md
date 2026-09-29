# komp_fmt

The KFlat formatter. Installed, it is `komp fmt`:

```console
$ komp tool install komp_fmt
$ komp fmt                  # every .kf file under the current directory
$ komp fmt src/main.kf lib/ # the files and directories named
$ komp fmt --check          # name the files it would change; write nothing
```

A directory stands for every `.kf` file under it, leaving out `target/` and
hidden directories. `--check` exits 1 when a file is not formatted, which is
the gate for CI.

## What it changes

Only whitespace. Line breaks stay where the author put them; the formatter
decides the indentation and the space between tokens:

- a line is indented four spaces past the line that opened the innermost
  bracket around it, and four more when it continues an expression, after a
  trailing operator or before a leading `.`, `?.`, `?:`, `&&`, `||` or `|>`;
- a line that starts by closing a bracket lines up with the line that
  opened it;
- a run of spaces between tokens becomes one, and trailing whitespace goes;
- at most one blank line is kept in a row, and none at the start or end of a
  block or of the file.

String and char literals, the code in their `${ }` slots, and comments are
kept byte for byte.

A result that does not hold the same words, broken into the same lines, as
its source is thrown away and the file is left as it is, with a message; so
is a file with a string left open.

## Building it

With kflat installed; the `kflat` pin in `kf.toml` names the releases it
builds with:

```console
$ komp test .
$ komp build .
$ target/kflat/komp_fmt --check .
```
