# Push your coverage metrics to Coveralls.io from Travis-ci

## Usage

Append the following `install` stanza of your `.travis.yml`:

```yml
install:
  ...
  - wget https://raw.githubusercontent.com/simonjbeaumont/ocaml-travis-coveralls/master/travis-coveralls.sh
```

and add the following to your the `script` stanza:

```yml
script: ... && bash travis-coveralls.sh
```

## Prerequisites

0. Your project is enabled on [Coveralls.io][1]; and
0. Your project is configured using [Oasis][2].

The dependency on Oasis allows this tool to link in [Bisect][3] (the OCaml tool
for collecting coverage metrics) in a predictable way.

## Configuration
You can configure the commands used to build and what commands are measured by
setting the following environment variables in your `.tavis.yml`:

* `$COV_CONF`: The command used to configure your project, maybe to enable any
  tests that are not built by default. This defaults to a no-op;
* `$COV_BUILD`: The command used to build your project. This defaults to
  `make`;
* `$COV_TEST`: The command under inspection of the coverage tools. Defaults to
  `make test`.

For example, if you need to enable tests with a configure script option you can
set the following:

```yml
env:
  global:
    ...
    - COV_CONF="./configure --enable-tests"
```

You can also exclude portions of code from coverage metric collection by adding
a `.coverage.excludes` file to your repo. For example if you wanted to exclude
the entire contents of some autogenerated files you could add the following to
a `.coverage.excludes`:

```
file "lib/ffi_generated.ml" [ regexp ".*" ];
file "lib/ffi_generated_types.ml" [ regexp ".*" ];
```

## Local usage

You can also use this script to see your coverage metrics locally. The script
detects that it is not running on Travis and instead outputs the information to
stdout and also generates a HTML report.

```sh
$ wget https://raw.githubusercontent.com/simonjbeaumont/ocaml-travis-coveralls/master/travis-coveralls.sh
$ bash travis-coveralls.sh
...
Ran: 27 tests in: 0.01 seconds.
OK
$TRAVIS not set; displaying results of bisect-report...
...[ snip ]...
File 'lib/request.ml':
 - 'binding' points: 77/124 (62.10%)
 - 'sequence' points: 2/2 (100.00%)
 - 'for' points: none
 - 'if/then' points: 3/6 (50.00%)
...[ snip ]...
Summary:
 - 'binding' points: 328/637 (51.49%)
 - 'sequence' points: 34/94 (36.17%)
 - 'for' points: 1/1 (100.00%)
 - 'if/then' points: 29/85 (34.12%)
 - 'try' points: 1/1 (100.00%)
 - 'while' points: none
 - 'match/function' points: 124/180 (68.89%)
 - 'class expression' points: none
 - 'class initializer' points: none
 - 'class method' points: none
 - 'class value' points: none
 - 'toplevel expression' points: 1/1 (100.00%)
 - 'lazy operator' points: 4/4 (100.00%)
 - total: 522/1003 (52.04%)
```

## Examples
For an example of using this tool and its features, check out the following
projects:

* [simonjbeaumont/ocaml-pci][4]
* [djs55/ocaml-9p][5]
* [xapi-project/message-switch][6]

[1]: https://coveralls.io
[2]: http://oasis.forge.ocamlcore.org/
[3]: http://bisect.x9c.fr/
[4]: https://github.com/simonjbeaumont/ocaml-pci
[5]: https://github.com/djs55/ocaml-9p
[6]: https://github.com/xapi-project/message-switch

