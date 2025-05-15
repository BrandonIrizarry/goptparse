# Traditional long option parser for Go

Package optparse parses command line arguments very similarly to GNU
`getopt_long()`. It supports long options and optional arguments, but
does not permute arguments. It is intended as a replacement for Go's
flag package.

    go get github.com/BrandonIrizarry/goptparse

Like the traditional `getopt()`, it doesn't automatically parse option
arguments, instead delivering them as strings. Nor does it automatically
generate a usage message.

## `goptparse`

This particular fork of
[optparse-go](https://github.com/skeeto/optparse-go) extends the
original by automatically adding `--help` and `-h` based on a `Help`
field included in each user-configured `Option` (see example below.) A
built-in help facility is a feature I missed from the original `flag`
package. In general, I feel programs should be oriented around
documentation. Emacs is a star example of this paradigm in action.

There is one limitation though: both `--help` and `-h` flags are now
reserved by the application; defining your own `--help` or `-h` is
illegal.

## Example usage

This is more or less modeled after the `optparse-go` upstream example,
but with some relevant modifications.

```go
package main

import (
	"fmt"
	"log"
	"os"
	"strconv"

	"github.com/BrandonIrizarry/goptparse"
)

func main() {
	options := []goptparse.Option{
		{Long: "amend", Short: 'a', Kind: goptparse.KindNone, Help: "Amend something"},
		{Long: "brief", Short: 'b', Kind: goptparse.KindNone, Help: "Give a brief summary"},
		{Long: "color", Short: 'c', Kind: goptparse.KindOptional, Help: "Colorize output"},
		{Long: "delay", Short: 'd', Kind: goptparse.KindRequired, Help: "Add an ARG millisecond delay"},
		{Long: "erase", Short: 'e', Kind: goptparse.KindNone, Help: "Erase it"},
	}

	var amend bool
	var brief bool
	var color string
	var delay int
	var erase int

	// If -h or --help were given, the help message will print at
	// this step, and the program will exit.
	results, rest, err := goptparse.Parse(options, os.Args)

	if err != nil {
		log.Fatal(err)
	}

	// Note that we don't need to handle "help" separately.
	for _, result := range results {
		switch result.Long {
		case "amend":
			amend = true
		case "brief":
			brief = true
		case "color":
			color = result.Optarg
		case "delay":
			delay, err = strconv.Atoi(result.Optarg)

			if err != nil {
				log.Fatal(err)
			}
		case "erase":
			erase++
		}

	}

	fmt.Println("amend", amend)
	fmt.Println("brief", brief)
	fmt.Println("color", color)
	fmt.Println("delay", delay)
	fmt.Println("erase", erase)
	fmt.Println(rest)
}
```
## Licensing

I decided to simply keep the original upstream `UNLICENSE` document.
