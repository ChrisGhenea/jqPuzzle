Options
=======

`rows`
------

Specifies the number of rows of the puzzle.

Expects any integer value greater than or equal to 2.

Defaults to 4.

There is no upper boundary for the number of rows. However, the calculation whether a board is solvable fails for big boards. It is safe for boards with a maximum of approx. 86 pieces (rows*cols).


`cols`
------

Specifies the number of columns of the puzzle.

Expects any integer value greater than or equal to 2.

Defaults to 4.

There is no upper boundary for the number of columns. However, the calculation whether a board is solvable fails for big boards. It is safe for boards with a maximum of approx. 86 pieces (rows*cols).


`hole`
------

Specifies the number of the puzzle piece that is replaced with the hole.

Expects any integer value between 1 (top left) and `rows*cols` (bottom right).

Defaults to `rows*cols`.

The `hole` option is ignored when a `board` is specified.


`initialHole`
-------------

Specifies the initial position of the hole on a shuffled board.

Expects any integer value between 1 (top left) and `rows*cols` (bottom right).

Defaults to a random position.

The `intiialHole` option is ignored when a `board` is specified.


`board`
-------

Specifies the initial, shuffled board layout. Implicitly defines the `hole` and the `initialHole` position also.

Expects an array of integers denoting each puzzle piece from top left to bottom right (see below).

Defaults to a shuffled board.

There's no need to specify `rows` or `cols` when the board is square.

When `board` is specified, the following options are ignored:

* `hole`
* `initialHole`
* `solvable`
* `shuffle`

### Setup a board

Board setup is best described based on the solved board. A solved board is a board where
* all numbers from 1 to `rows*cols` are in sequential order and
* any one number is replaced with the special value `0` representing the hole

A shuffled board contains all the numbers of a solved board in random order. Keep in mind that only every other random board layout is solvable!

That way, one can define both the `hole` and the `initialHole` position where

* `hole` is the number that is replaced with `0`
* `initialHole` is the position of that `0` on the shuffled board

#### Examples

* A 2x2 board that is not yet solved (0 replaced for 4, numbers not in sequential order):

		board: [1,2,0,3]
<!-- separate code blocks -->
		+---+---+
		| 1 | 2 |
		+---+---+
		|   | 3 |
		+---+---+

* A 2x2 board that is already solved (0 replaced for 4, numbers in sequential order):

		board: [1,2,3,0]
<!-- separate code blocks -->
		+---+---+
		| 1 | 2 |
		+---+---+
		| 3 |   |
		+---+---+

* A 2x2 board that is already solved (0 replaced for 3, numbers in sequential order):

		board: [1,2,0,4]
<!-- separate code blocks -->
		+---+---+
		| 1 | 2 |
		+---+---+
		|   | 4 |
		+---+---+

* A 2x2 board that is not yet solved (0 replaced for 3, numbers not in sequential order):

		board: [1,2,4,0]
<!-- separate code blocks -->
		+---+---+
		| 1 | 2 |
		+---+---+
		| 4 |   |
		+---+---+


`solvable`
----------

Specifies whether a shuffled board must be solvable or not (as only every other random board layout is solvable).

Expects either `true` or `false`. Any other value is interpreted as 'random' meaning that a shuffled board might or might not be solvable.

Defaults to `true`.

The `solvable` option is ignored when a `board` is specified.


`shuffle`
---------

Specifies whether the board is immediately shuffled or not. If set to `false`, the game must be explicitly started by calling `shuffle()`.

Expects either `true` or `false`. Any integer value is interpreted as the number of random moves away from the solved board (meaning that the puzzle can be solved with at most the specified number of moves). 

Note that there is a difference between the values `0` and `false`. While `shuffle: 0` starts a game that is immediately solved, `shuffle: false` does not start the game before explicitly calling `shuffle()`.

Defaults to `true`.

The `shuffle` option is ignored when a `board` is specified.



Methods
=======

`getPosition(<arguments>)`
--------------------------

Returns the `position` object for any kind of position value.

A `position` object defines a single position on the board. A position is a slot that that can hold a piece.

The `position` object contains the following properties:

* `index`: 
The one-dimensional index of the position as an integer value between 1 (top left) and `rows*cols` (bottom right).
* `row`: 
The row of the position as an integer value between 1 and `rows`.
* `column`:
The row of the position as an integer value between 1 and `columns`.

`getPosition()` accepts the following arguments:

* `getPosition(<index>)` - one-dimensional index

		puzzle.getPosition(4)

* `getPosition(<row>, <col>)` - row and col as separate arguments

		puzzle.getPosition(2, 1)

* `getPosition([<row>, <col>])` - row and col as array

		puzzle.getPosition([2, 1])

* `getPosition({row: <row>, col: <col>})` - row and col as object

		puzzle.getPosition({row: 2, col: 1})

All examples would return the following `position` object on a 3x3 board:

		{ index: 4, row: 2, col: 1 }


`getPiece(<arguments>)`
-----------------------

Returns the `piece` object of the piece that is currently located at the position specified by any kind of position value.

A `piece` object defines a single piece on the board. A piece is a numbered element that has a position.

The `piece` object contains the following properties:

* `number`: 
The number of the piece as an integer value between 1 and `rows*cols`.
* `position`: 
The position of the piece as a `position` object (see `getPosition()`).

`getPiece()` accepts the same arguments as `getPosition()`:

		puzzle.getPiece(4)
		puzzle.getPiece(2, 1)
		puzzle.getPiece([2, 1])
		puzzle.getPiece({row: 2, col: 1})

All examples would e.g. return the following `piece` object on a 3x3 board:

		{
			number: 8,
			position: { index: 4, row: 2, col: 1 }
		}


`getPieceByNumber(<number>)`
----------------------------

Returns the `piece` object of the piece with the specified number.

See `getPiece()` for a description of the `piece` object.

`getPieceByNumber()` accepts an integer value between 1 and `rows*cols`.

		puzzle.getPieceByNumber(8)

This would e.g. return the following `piece` object on a 3x3 board:

		{
			number: 8,
			position: { index: 4, row: 2, col: 1 }
		}


`getPieceByDirection(<direction>)`
----------------------------------

Returns the `piece` object of the piece that can be moved in the specified direction.
Returns `false` if no piece can be moved in this direction.

See `getPiece()` for a description of the `piece` object.

`getPieceByDirection()` accepts the following direction keywords: `left`, `right`, `up`, `down`

		puzzle.getPieceByDirection('up')

This would e.g. return the following `piece` object on a 3x3 board:

		{
			number: 8,
			position: { index: 4, row: 2, col: 1 }
		}


`canMove(<arguments>)`
----------------------

Checks if the piece can be moved that is currently located at the position specified by any kind of position value.

Returns a `move` object if the piece can be moved.
Returns `false` if the piece cannot be moved.

A `move` object defines a single (potential) move on the board. A move changes the position of a piece by swapping it with the hole.

The `move` object contains the following properties:

* `number`: 
The number of the piece to be moved as an integer value between 1 and `rows*cols`.
* `from`: 
The position of the piece to be moved as a `position` object (see `getPosition()`).
* `to`: 
The position to which the piece is to be moved as a `position` object (see `getPosition()`).
* `direction`:
The direction in which the piece is to be moved. One of `left`, `right`, `up`, `down`.

`canMove()` accepts the same arguments as `getPosition()`:

		puzzle.canMove(4)
		puzzle.canMove(2, 1)
		puzzle.canMove([2, 1])
		puzzle.canMove({row: 2, col: 1})

All examples would e.g. return the following `move` object on a 3x3 board:

		{
			number: 8,
			from: { index: 4, row: 2, col: 1 },
			to: { index: 1, row: 1, col: 1 },
			direction: 'up'
		}



`canMoveByNumber(<number>)`
---------------------------

Checks if the piece with the specified number can be moved.

Returns a `move` object if the piece can be moved.
Returns `false` if the piece cannot be moved.

See `canMove()` for a description of the `move` object.

`canMoveByNumber()` accepts an integer value between 1 and `rows*cols`.

		puzzle.canMoveByNumber(8)

This would e.g. return the following `move` object on a 3x3 board:

		{
			number: 8,
			from: { index: 4, row: 2, col: 1 },
			to: { index: 1, row: 1, col: 1 },
			direction: 'up'
		}



`canMoveByDirection(<direction>)`
---------------------------------

Checks if a piece can be moved in the specified direction.

Returns a `move` object if a piece can be moved in this direction.
Returns `false` if no piece can be moved in this direction.

See `canMove()` for a description of the `move` object.

`canMoveByDirection()` accepts the following direction keywords: `left`, `right`, `up`, `down`

		puzzle.canMoveByDirection('up')

This would e.g. return the following `move` object on a 3x3 board:

		{
			number: 8,
			from: { index: 4, row: 2, col: 1 },
			to: { index: 1, row: 1, col: 1 },
			direction: 'up'
		}


`move(<arguments>)`
-------------------

Moves the piece that is currently located at the position specified by any kind of position value.

Returns a `move` object if the piece can be moved.
Returns `false` if the piece cannot be moved.

A `move` object defines a single move on the board. A move changes the position of a piece by swapping it with the hole.

See `canMove()` for a description of the `move` object. In addition, the `move` object returned by this method contains the following property:

* `timestamp`: 
The point in time when the move was performed as a JavaScript `Date` object.

`move()` accepts the same arguments as `getPosition()`:

		puzzle.move(4)
		puzzle.move(2, 1)
		puzzle.move([2, 1])
		puzzle.move({row: 2, col: 1})

All examples would e.g. return the following `move` object on a 3x3 board:

		{
			number: 8,
			from: { index: 4, row: 2, col: 1 },
			to: { index: 1, row: 1, col: 1 },
			direction: 'up',
			timestamp: <date>
		}


`moveByNumber(<number>)`
------------------------

Moves the piece with the specified number.

Returns a `move` object if the piece can be moved.
Returns `false` if the piece cannot be moved.

See `move()` for a description of the `move` object.

`moveByNumber()` accepts an integer value between 1 and `rows*cols`.

		puzzle.moveByNumber(8)

This would e.g. return the following `move` object on a 3x3 board:

		{
			number: 8,
			from: { index: 4, row: 2, col: 1 },
			to: { index: 1, row: 1, col: 1 },
			direction: 'up',
			timestamp: <date>
		}


`moveByDirection(<direction>)`
------------------------------

Moves a piece in the specified direction.

Returns a `move` object if a piece can be moved in this direction.
Returns `false` if no piece can be moved in this direction.

See `move()` for a description of the `move` object.

`moveByDirection()` accepts the following direction keywords: `left`, `right`, `up`, `down`

		puzzle.moveByDirection('up')

This would e.g. return the following `move` object on a 3x3 board:

		{
			number: 8,
			from: { index: 4, row: 2, col: 1 },
			to: { index: 1, row: 1, col: 1 },
			direction: 'up',
			timestamp: <date>
		}



Internals
=========

Options
-------

The options hash contains properties that do not change once a game is initialized:

* `this.options.rows`
* `this.options.cols`
* `this.options.board`
* `this.options.hole`
* `this.options.initialHole`

Methods
-------

* `shuffle()`
* `restart()`
* `isSolvable()`
* `isSolved()`

Internal Properties
-------------------

These properties are pseudo-private and start with an underscore. They may change during the game and represent the current game state.

* `this._boardSize`  
Number of pieces of the board.
* `this._initialBoard`  
Copy of a specified or shuffled board to be able to reset.
* `this._sortedBoard`  
  The board that contains all numbers form `0` (hole) to `rows*cols` in sequential order with any one number missing (the one that is replaced by the hole).
* `this._solvedBoard`  
  The board in its solved state. Contains all numbers form `1` to `rows*cols` in sequential order with any one number replaced with the special value `0` representing the hole.
* `this._initialHole`  
  The initial position of the hole in the shuffled board. Either taken from the options or set during shuffle.
* `this._board`  
  The current board. Changes with each move.
* `this._hole`  
  The current hole position. Changes with each move.
