# Materiala (mkdocs-material + marginalia)

![logo](images/logo-black.png) Experience to leverage
[marginalia](https://github.com/gdeer81/marginalia) parser and
[mkdocs-material](https://squidfunk.github.io/mkdocs-material/) to create
documentation.

Example: https://davidpham87.github.io/materiala/

## Rationale

I **really** liked [calva.io](https://calva.io) website for documentation [I
still use emacs+cider] , and I saw it was based `mkdocs-material`. Since then,
I wanted to use it for my own documentation. We already have marginalia, codox
and cljdoc, so this library does not add a lot, but I still wanted to have a
nice UI for personal project and as a nice project. My only experience before
with auto-documentation/literal programming was org-file, marginalia and codox.


## Quick start

``` bash
pip3 install mkdocs-material mkdocs-awesome-pages-plugin

mkdir doc # save the output here
# parse your src folder and generate the files into ./doc
clojure -Sdeps '{:deps {materiala {:git/url "https://github.com/davidpham87/materiala/" :sha "54b466714e8cf0b62c169895ea5b6df44fa3558f"}}}' -m materiala.core src

mkdocs build # release, use `mkdocs serve` for dev
cd doc && python3 -m http.server
firefox localhost:8000 # or your most favorite browser (I use google-chrome)
```

## Installation

Add the following dependency to your `deps.edn`

``` clojure
materiala {:git/url "https://github.com/davidpham87/materiala/"
           :sha "54b466714e8cf0b62c169895ea5b6df44fa3558f"}
```

Change for the latest *sha* code, if required. Note the *group-id* and
*artifact-id* of the package might change once I get enough money to buy a
domain.

## Status

Status: **alpha**, although I will follow
[spec-ulation](https://www.youtube.com/watch?v=oyLBGkS5ICk) to give my best to
avoid breaking changes. I want to be a nice person, so I will make sure to not
break your code. The biggest issue is the library leverages on marginalia's
parser.

## Technical Solution

Marginalia API was *easier* to leverage (and marginalia had a nice website), so
the library is build on its parser. Our writer acts in two steps:

1. For code output from marginalia, the available information is extended (such
   as valid call forms from function).
2. Render the data into markdown consumed by mkdocs.

As alternative, codox (on which neanderthal and the uncomplicate ecosystem
built their documentation) was also considered for usage and extension by
coding a specific writer. However, most of the function codox writer are
private and I did not want to depend on private api.

Another alternative would have been to study how mkdocs and its extensions
converts the markdown into `html` and targets that output directly to keep the
data centric view of Clojure. The drawback of this solution is time, and we
totally loss the leverage on mkdocs. Moreover, it is doubtful that the user
will require to parse the markdown output again.

## Is it a good idea?

Well... Smashing strings together and having a DSL for representing data
definitively goes against Clojure's philosophy and main lessons.

But for this particular problem, I just wanted to have some user friendly and
searchable docs for my coworkers/friends and thanks to Lisp homoiconicity and
Clojure simplicity, parsing is not *too* hard. For big open source libraries
published on Clojars, I would still look at [cljdoc](https://cljdoc.org/).

## Extension

See [here](extension) for an example of extension.

## Commande line args

``` bash
clojure -m materiala.core -h
  -d, --dir DIR          ./doc  Directory into which the documentation will be written
  -f, --file FILE               File into which the documentation will be written
  -n, --name NAME               Project name - if not given will be taken from project.clj
  -v, --version VERSION         Project version - if not given will be taken from project.clj
  -D, --desc DESC               Project description - if not given will be taken from project.clj
  -a, --deps DEPS               Project dependencies in the form <group1>:<artifact1>:<version1>;<group2>...
                                 If not given will be taken from project.clj
  -m, --multi                   Generate each namespace documentation as a separate file
  -l, --leiningen               Generate the documentation for a Leiningen project file.
  -e, --exclude EXCLUDE         Exclude source file(s) from the document generation process <file1>;<file2>...
                                 If not given will be taken from project.clj
  -h, --help                    Show this help
```

I mainly copied `marginalia` options and adapted it to the newest version of
[clojure.tools.cli](https://github.com/clojure/tools.cli).

## Dependencies

You will have to install the following python dependencies:

``` bash
pip3 install mkdocs-material mkdocs-awesome-pages-plugin
```

## Example mkdocs.yml

Here is an example of `mkdocs.yml` file that could be used for a github
repository.


``` yaml
site_name: Materiala
repo_url: https://github.com/davidpham87/materiala
edit_uri: edit/master/doc
repo_name: github
docs_dir: doc
site_dir: docs

theme:
  name: material
  logo: images/logo.png
  palette:
    primary: red

markdown_extensions:
  - admonition
  - codehilite:
      guess_lang: false
  - toc:
      permalink: true
  - pymdownx.arithmatex
  - pymdownx.superfences
  - pymdownx.inlinehilite
  - pymdownx.details

extra_javascript: # Math
  - https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-MML-AM_CHTML

plugins:
  - search # necessary for search to work
  - awesome-pages # automatic navigation
```

See [mkdocs-material](https://squidfunk.github.io/mkdocs-material/) and
[mkdocs](https://www.mkdocs.org/) for more detail.

## Examples
- [Materiala](https://davidpham87.github.io/materiala/), obviously.

## See also

- [marginalia](https://github.com/gdeer81/marginalia)
- [mkdocs-material](https://squidfunk.github.io/mkdocs-material/)
- [codox](https://github.com/weavejester/codox)
- [cljdoc](https://cljdoc.org/)


## What's up with logo

Well, I was too lazy to find a proper one, so I took the latex mathematical
integral sign, which (*hopefully*) is still open source.

## Development

Run a repl (add your cider deps as well)

``` bash
clojure -Adev
```

Run the test (todo: create some specs and generate with `test.check`)

``` bash
clojure -Atest
```

## TODO

- Create an option to copy codox/cljcdoc link to source code instead of showing
  the raw code directly.

## License

Copyright (C) 2020 David Pham

Distributed under the Eclipse Public License, the same as Clojure.
