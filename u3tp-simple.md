# u3tp-simple (Ultimate tic-tac-toe Protocol)

## Table of Contents

1. [Overview](#overview)
2. [Move Format](#move-format)
3. [Handshake](#handshake)
4. [Commands](#commands)
5. [License](#license)
6. [Author](#author)

## Overview

This protocol is a subset of u3tp.

Define a set of text messages that can be sent to and from an engine using the stdint and stdout.

Each command can followed by a variable number of parameters. \
Each parameter has a default value defined by this standard. \
The parameters that don't have a default value are required to be set every time. \
The parameters after the command doesn't need to follow an established order.

The standard message will look like: \
`[command] -[param1 value1] -[param2 value2] [data]`

Every message must be termianted with `'\n'` (OS dependent).

If a message doesn't comply with this standard the behaviour is undefined (usually it is ignored).

The I/O can be handled at the end of the execution of the current command.

## Move format

A 3x3 tic-tac-toe board can be represented like this:

1 2 3\
4 5 6\
7 8 9

The 9x9 board used in u3t is a 3x3 big board of 3x3 small boards, so the move format is as follows: `[big board][small board]`.

For example to play in the top right board the center move: `35`.


## Handshake

At the start of every connection the client will send to the engine the message: \
`u3tp-simple`

The engine has a certain amount of time (implementation dependent: suggested 10 seconds) to respond with: \
`u3tpok`

If the engine fails to do so the engine is forcefully shut down. \
If the engine succesfully responds it waits for more commands.

## Commands

### Client to Engine

* `u3tp-simple`: start handshake
* `setconstraint`: set time constraints
    * `-time`
        * `move`: data is in format `[number][time literal ms/s/m/h/d]`. eg: `1m` (1 minute per move)
        * `total`: data is in format `[total time (number)(literal)]+[increment]-[delay]`. eg: `45m+30s` (45 minute total and after each move a 30 seconds increment). Increment and delay can be omitted if they are 0. Main time must always be written.
* `position`: set a board position clearing internal memory
    * data is the position
* `move`: make a move on the board
    * data is the move in the [format](#move-format)
* `go`: analyze the position respecting the constraints
* `quit`: shut down the engine as quickly as possible

### Engine to Client

* `u3tp-simpleok`: respond to the handshake

## License

### MIT License

Copyright 2023 Davide Fassio

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Author

Davide Fassio ([@Davidefassio](https://github.com/Davidefassio))
