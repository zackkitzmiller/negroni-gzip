# gzip

Gzip middleware for [Negroni](https://github.com/codegangsta/negroni).

Mostly a copy of the Martini gzip module with small changes to make it function
under Negroni. Support for setting the compression level has also been added
and tests have been written. Test coverage is 100% according to `go cover`.

## Zack's Fork

This is a fork of the original package. This fork includes a faster implementation
of gzip

## Usage

~~~ go
package main

import (
    "fmt"
    "net/http"

    "github.com/codegangsta/negroni"
    "github.com/zackkitzmiller/negroni-gzip/gzip"
)

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
    	  fmt.Fprintf(w, "Welcome to the home page!")
    })

    n := negroni.Classic()
    n.Use(gzip.Gzip(gzip.DefaultCompression))
    n.UseHandler(mux)
    n.Run(":3000")
}
~~~

Make sure to include the Gzip middleware above any other middleware that alter
the response body.

## Tips

As noted above, any middleware that alters response body will need to be below
the Gzip middleware. If you wish to gzip static files served by the default
negroni Static middleware you will need to include `negroni.Static()` after
`gzip.Gzip()`.

~~~go
    n := negroni.New()
    n.Use(negroni.NewRecovery())
    n.Use(negroni.NewLogger())
    n.Use(gzip.Gzip(gzip.DefaultCompression))
    n.Use(negroni.NewStatic(http.Dir("public")))
~~~

## Authors
* [Jeremy Saenz](http://github.com/codegangsta)
* [Shane Logsdon](http://github.com/slogsdon)
* [David O'Rourke](https://github.com/phyber)

And many others. Check the commit log for a complete list.
