# pylnlst
> CLI tool to automate the creation of symbolic links in a destination directory.

## Why pylnlst?

Many times I find myself creating multiple symbolic links manually using [Nautilus](https://gitlab.gnome.org/GNOME/nautilus) (or similar) and feeling that the process could be automated. When tried to approach such automation using bash I've failed miserably because it's very difficult to deal with file names containing spaces, brackets, or exclamation marks because escaping such characters is required.

This tool has been created to address those problems.

The idea is:
+ User creates a file with a list of absolute or relative paths, one per file/folder.
+ User invokes the tool with two arguments: the location of the file list, and the target (base) location where symbolic links will be created.
+ The tool reads the entries from the file, one by one, and creates a symbolic link in the specified target location, pointing to the original file.
+ If a file with the same name already exists, a suffix is added so that there is no name clash. A warning is displayed.

The usage is as follows:

```bash
pylnlst --file-list fILELIST --dst DST_DIR
```

## Dev notes

Most of the script functionality relies on the [Typer](https://github.com/fastapi/typer) framework. Project management relies on [uv](https://github.com/astral-sh/uv), and linting/formatting is done with [Ruff](https://github.com/astral-sh/ruff).

There are currently no tests.

To check that the packaged script works as expected type:

```bash
# you must be in the directory that contains pylnlst package, not
# within pylnlst
$ uv run --directory pylnlst pylnlst --version
```
The application can be built with `uv build` and published with `uv publish`.

To publish to TestPyPI:

```bash
$ rm -rf dist
$ uv build
$ uv publish
 --publish-url https://test.pypi.org/legacy/ --token <test-pypi-token-with-sufficient-scope>
```

| NOTE: |
| :---- |
| Some of the tokens are defined on a per-project basis. Those won't work for new projects. |

You will be able to test that it works by simply doing:

```bash
$ mkdir pylnlst-test
$ cd pylnlst-test
$ python -m venv .venv --upgrade-deps
$ source .venv/bin/activate
(.venv) $ pip install pylnlst==0.1.1 --extra-index-url https://test.pypi.org/simple/
(.venv) $ pylnlst --version
pylnlst 0.1.1
```


To publish to PyPI:

```bash
$ rm -rf dist
$ uv build
$ uv publish --token <pypi-token>
```