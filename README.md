This is a prototype for the bida database.

It is written in Perl and uses the Sqlite3 database as
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

which outputs `["OIDS",[]]`.

Invalid inputs should give an explanation:

    ["save", "foo"]

outputs `["ERROR","'save' needs 2 arguments: indexnames_and_keys, value at ./bida line 207, <GEN1> line 2.\n"]`

The indexnames_and_keys is an array of arrays that start with the name
of the index, then the values in that index where the object should be
added.

`delete` needs the indexes, too, otherwise the object id is still in
the index even though the object is deleted.


To see the current state of the database, it can be dumped using:

    $ sqlite3 .tmp-data/db.sqlite
    sqlite> .dump

To run the tests (currently simply the above):

    $ ./test

## Protocol

The protocol is using JSON for both directions.

Every command and respons is using exactly one line. This means that
the JSON may not include newlines other than escaped as "\n" within
strings. This works:

    ["save", [], "Foo\nBar"]

This does not work, since the message is broken into two messages:

    ["save", [], "Foo
    Bar"]

