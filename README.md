# u3tp (Ultimate tic-tac-toe Protocol)

## Table of Contents

1. [Overview](#overview)
2. [Handshake](#handshake)
3. [Commands](#commands)
4. [License](#license)
5. [Author](#author)

## Overview

Define a set of text messages that can be sent to and from an engine using the stdint and stdout.

Each command can followed by a variable number of parameters. \
Each parameter has a default value defined by this standard. \
The parameters that don't have a default value are required to be set every time. \
The parameters after the command doesn't need to follow an established order.

The standard message will look like: \
`[command] -[param1 value1] -[param2 value2] [data]`

Every message must be termianted with `'\n'` (OS dependent).

If a message doesn't comply with this standard or it's incopatible with the state of the engine it is ignored.

The I/O can be managed by the engine not in real time, but at the end of the current task.

## Handshake

At the start of every connection the client will send to the engine the message: \
`u3tp`

The engine has a certain amount of time (implementation dependent: suggested 10 seconds) to respond with: \
`u3tpok`

If the engine fails to do so the engine is forcefully shut down. \
If the engine succesfully responds it enter idle mode waiting for more commands.

## Commands

*TODO* write the parameters and their default values.

### Client to Engine

* `u3tp`: start handshake
* `help`: print usage and list of commands
* `info`: print info about the engine
* `setoption`: set internal option
* `setconstraint`: set time or node count constraints
* `position`: set a board position
* `move`: make a move on the board
* `go`: analyze the position respecting the constraints
* `debug`: print internal state
* `stop`: stop as quick as possible the engine

### Engine to Client

* `u3tpok`: respond to handshake

## License

GPL v3

## Author

Davide Fassio
