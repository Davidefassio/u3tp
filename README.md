# u3tp (Ultimate tic-tac-toe Protocol)

## Table of Contents

1. [Overview](#overview)
2. [Move Format](#move-format)
3. [Handshake](#handshake)
4. [Commands](#commands)
5. [License](#license)
6. [Author](#author)

## Overview

Define a set of text messages that can be sent to and from an engine using the stdint and stdout.

Each command can followed by a variable number of parameters. \
Each parameter has a default value defined by this standard. \
The parameters that don't have a default value are required to be set every time. \
The parameters after the command doesn't need to follow an established order.

The standard message will look like: \
`[command] -[param1 value1] -[param2 value2] [data]`

Every message must be termianted with `'\n'` (OS dependent).

If a message doesn't comply with this standard the behaviour is undefined (usually it is ignored).

The I/O must be handled instantly (or close enough), so a separate thread is reccomended or frequent polling.

## Move format

A 3x3 tic-tac-toe board can be represented like this:

1 2 3\
4 5 6\
7 8 9

The 9x9 board used in u3t is a 3x3 big board of 3x3 small boards, so the move format is as follows: `[big board][small board]`.

For example to play in the top right board the center move: `35`.


## Handshake

At the start of every connection the client will send to the engine the message: \
`u3tp`

The engine has a certain amount of time (implementation dependent: suggested 10 seconds) to respond with: \
`u3tpok`

If the engine fails to do so the engine is forcefully shut down. \
If the engine succesfully responds it waits for more commands.

## Commands

### Client to Engine

* `u3tp`: start handshake
* `help`: print usage and list of commands
    * if data is a known command print help about it
* `info`: print info about the engine
* `setoption`: set internal option
* `setconstraint`: set time and node constraints. One time constraint and one node constraint can be active at any time
    * `-time`
        * *default*: no change
        * `inf`
        * `move`
        * `total`
        * `custom`
    * `-node`
        * *default*: no change
        * `inf`
        * `count`
        * `speed`
* `clear`: clear internal memory (eg: hash tables, position tree, known evaluations ...)
* `position`: set a board position clearing internal memory
    * `-notation`
        * *default*: custom
        * `custom`
* `move`: make a move on the board
    * data is the move in the [format](#move-format)
* `go`: analyze the position respecting the constraints
* `ponder`: analyze the position without respecting the constraints
* `stop`: interrupt prematurely the search
* `debug`: print internal state
* `quit`: shut down the engine as quickly as possible

### Engine to Client

* `u3tpok`: respond to the handshake
* `option`: send the client the list of modifiable options
    * data is a list of options

## License

### MIT License

Copyright 2023 Davide Fassio

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Author

Davide Fassio ([@Davidefassio](https://github.com/Davidefassio))
