This is a prototype for the bida database.

It is written in Perl and uses the Berkeley key-value Database as
storage backend. The purpose of this implementation is to develop the
database API before caring for a more efficient implementation.
The database API is developed with the latter implementation in mind,
though.

## Installation

    $ sudo apt-get install libdbd-sqlite3-perl libjson-xs-perl
    $ cd bida-prototype/
    $ git submodule init; git submodule update

## Usage

    $ ./bida example_config.pl

then from another terminal:

    $ rlwrap telnet localhost 1234
    ["save", [["name","Jaeger"]], ["Christian", "Jaeger"]]

which outputs `["OID",1]`

    ["get", 1]

which outputs `["VALUE", ["Christian","Jaeger"]]`

    ["list", "name", "Jaeger"]

which outputs `["OIDS",[1]]`

    ["delete", [["name","Jaeger"]], 1]

which outputs `["OK"]`

    ["list", "name", "Jaeger"]

which outputs `["OIDS",[]]`

