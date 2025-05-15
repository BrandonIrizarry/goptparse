# Traditional long option parser for Go

Package optparse parses command line arguments very similarly to GNU
`getopt_long()`. It supports long options and optional arguments, but
does not permute arguments. It is intended as a replacement for Go's
flag package.

    go get github.com/BrandonIrizarry/goptparse

Like the traditional `getopt()`, it doesn't automatically parse option
arguments, instead delivering them as strings. Nor does it automatically
generate a usage message.

This particular fork of
[https://github.com/skeeto/optparse-go](https://github.com/skeeto/optparse-go)
extends the original by automatically adding `--help` and `-h` based
on a `Help` field included in each user-configured `Option` (see
example below.) This is a feature that I missed from the original
`flag` package.

There are two downsides to this though:

1. Both `--help` and `-h` flags are now reserved by the application;
   defining your own is an error.
2. The function `DisplayHelp` still needs to be automatically invoked
   if `--help` or `h` are given, or else if a non-existent flag is
   given. The latter condition makes it easy to include `DisplayHelp`
   as part of the `default` clause in the `switch` statement used to
   unmarshal the arguments into the application. The example given
   below should make this clearer.

## Example usage

Currently under construction.
