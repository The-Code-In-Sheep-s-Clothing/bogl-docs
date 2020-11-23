---
sort: 10 # Order in the sidebar
# permalink: /tutorials/boards
---

# Boards

## Background: What is a BoGL Board?
BoGL wouldn't be a board game language if there weren't a way to create a board!
*Board* is the name that *arrays* are given in BoGL.
An [*array*](https://en.wikipedia.org/wiki/Array_data_structure) in computer science is a [*data structure*](https://en.wikipedia.org/wiki/Data_structure), which is a term used to describe an organization of stored data.
Arrays are one of the simpler data structures to understand because they are often used to describe organizations that resemble lists (with one-dimensional arrays) or grids (with two-dimensional arrays).
For example, in the game of [Chess](https://en.wikipedia.org/wiki/Chess) the pieces are organized on a grid of squares (the board), and in the game of [Candy Land](https://en.wikipedia.org/wiki/Candy_Land) the path that the players move upon can be described as a list of squares.

It is important to note that the *board* term is specific to BoGL. If you were to directly translate a program that uses a board in BoGL to a different language, you would likely implement the board as a two-dimensional array.

<br/>
## Defining the Board Type

To create a board value, or board state, we must first create a type for our board.
BoGL only allows for one board type definition in a program, and it must be defined before any value and function definitions.
To define our board type, we must first write the keyword `type` followed by `Board` followed by an `=`.
After the `=` we write the keyword `Array` followed by parenthesis `()`.
Within the parenthesis are where the board dimensions are specified with two integer values seperated by a comma.
These integer values correspond to the width and length of our board respectively.
Following the parenthesis containing the board dimensions we write the keyword `of` followed by the desired
type for each square on our board. Below are a few examples of board type definitions.


Shown below is how we might define a 3 by 3 [Tic-tac-toe](https://en.wikipedia.org/wiki/Tic-tac-toe) board.
{% highlight haskell %}
type TicTacToeSquare = {O, X, Unmarked} -- Board square type

type Board = Array(3, 3) of TicTacToeSquare -- Board definition
{% endhighlight %}

Shown below is how we might define a 7 by 6 [Connect Four](https://en.wikipedia.org/wiki/Connect_Four) board.
{% highlight haskell %}
type ConnectFourSquare = {Red, Yellow, Empty} -- Board square type

type Board = Array(7, 6) of ConnectFourSquare -- Board definition
{% endhighlight %}

<br/>
## Creating a Board State

Using our defined board, we can create a board state (or value representing the board).
Doing this is similiar to how we would create a value normally.
We start by creating a name for our board state (must be lowercase) followed by a `:`, followed by the word `Board`.
After this we may then define *board equations*, which are the expressions that specify which parts of the board should be defined as which values.

To create a board equation we must first write the name we chose for the board state, immediately followed by an `!` followed by parenthesis that contain the position values for which part of the board is being defined.
Following the parenthesis is an expression that should evaluate to the same type that the board is made up of.
The board equation(s) must define a value for each part of the board.
If you leave a part of the board undefined, you will get an error.

Below is the code for one of the simplest boards we can make, which is a 1 by 1 board that holds a Bool. 
{% highlight haskell %}
type Board = Array(1, 1) of Bool -- Board definition

-- Board state
boardState : Board
boardState!(1,1) = True -- Board equation defining the value for position (1, 1)
{% endhighlight %}

Below is the code for a 1 by 2 board that holds Bool values. 
{% highlight haskell %}
type Board = Array(1, 1) of Bool -- Board definition

-- Board state
boardState : Board
boardState!(1,1) = True  -- Board equation defining the value for position (1, 1)
boardState!(1,2) = False -- Board equation defining the value for position (1, 2)
{% endhighlight %}

### Generalizing a Board Equation

If we wanted to initialize a Tic-tac-toe board with `Unmarked` values, we could do something like this:
{% highlight haskell %}
-- Initial board state
initialTTTBoard : Board
initialTTTBoard!(1,1) = Unmarked
initialTTTBoard!(1,2) = Unmarked
initialTTTBoard!(1,3) = Unmarked
initialTTTBoard!(2,1) = Unmarked
initialTTTBoard!(2,2) = Unmarked
initialTTTBoard!(2,3) = Unmarked
initialTTTBoard!(3,1) = Unmarked
initialTTTBoard!(3,2) = Unmarked
initialTTTBoard!(3,3) = Unmarked
{% endhighlight %}

In the above value definition we specify the initial value (Unmarked) for each part of our board. Since a Tic-tac-toe board is 3 by 3, there are 9 total board positions we need to define values for (hence the 9 board equations).
This is many lines of code.
In this instance, since we are making each position on the board the same initial value, there is no real reason we should have to write 9 equations.

We can write this same initialization by generalizing a board equation to refer to all position values rather than to a specific one.
To create a board equation that refers to all board positions, we will write a board equation that has non-integer (lowercase) names instead of integers for the position values. This will tell the equation to refer to all board positions rather than just one. The example code shown below does the same thing as the code shown above (but in less lines)!

{% highlight haskell %}
-- Initial board state
initialTTTBoard : Board
initialTTTBoard!(x,y) = Unmarked -- Board equation defining the values for all positions on the board 
{% endhighlight %}


<br/>
## Boards as Function Parameters
It can be useful to pass our board into functions and access certain positions of it. A board can be specified as a parameter by giving `Board` as one of the function's parameter types.

The function below takes a board as a parameter and returns the same board.
{% highlight haskell %}
boardFunc : Board -> Board
boardFunc(b) = b
{% endhighlight %}

We can retrieve the values of the specific positions of a board that has been passed to a function as an argument by typing the argument's name followed by an `!`, followed by an `(Int,Int)` tuple value (which represents the position of the board we are accessing).

The function below takes a board and returns the top left value of the board.
{% highlight haskell %}
boardFunc : Board -> TicTacToeSquare
boardFunc(b) = b!(1,1)
{% endhighlight %}

:dart: **Excercise:**  
Modify the `getBoardValue` function below so that it returns the value located at position (**posX**, **posY**) on the board (**posX** and **posY** are the names given to the arguments of the function).

{% include exercise_module_template.html
content = "game BoardExcercise

type TicTacToeSquare = {O, X, Unmarked} -- Board square type

type Board = Array(3, 3) of TicTacToeSquare -- Board definition

exampleBoard : Board
exampleBoard!(x,y) = Unmarked
exampleBoard!(1,1) = O
exampleBoard!(2,2) = X
exampleBoard!(3,3) = X

getBoardValue : (Board, Int, Int) -> TicTacToeSquare
getBoardValue(b, posX, posY) = b!(1,1)
"

checks="getBoardValue(exampleBoard, 1, 2)
getBoardValue(exampleBoard, 2, 2)
getBoardValue(exampleBoard, 3, 3)"

expects="Unmarked
X
X"
%}

<br/>
## Built-in Board Functions
BoGL has a few built-in functions for some common board procedures. Each of these functions has a parameter of type *Content*. The *Content* type is set to be a synonym of the type you define your board to be made up of. That is, if you define your board like this:
{% highlight haskell %}
type Board = Array(3, 3) of TicTacToeSquare -- Board definition
{% endhighlight %}
Then the *Content* type definition behind the scenes will look like:
{% highlight haskell %}
type Content = TicTacToeSquare
{% endhighlight %}

So make sure that the value you give as an argument to the *Content* type parameter is of the same type that you defined your board to be made up of.

### place
The `place` function allows you to place something on a board.

The function takes 3 arguments: A *Content* value which is what is going to be placed on the board, the board the *Content* will be placed on, and a tuple of integers which represents the location of where on the board the *Content* value will be placed.
The return value of a `Place` function call is the board value that was passed to the function updated with the `Content` placed at the specified position. 


:dart: **Excercise:**
1. Call the value `initialTTTBoard` in the interpreter below. What is the return value?
2. Enter the function call `place(X, initialTTTBoard, (2,2))` into the interpreter below. What is the return value?
{% include code_module_template.html
content = "game PlaceExample
type TicTacToeSquare = {O, X, U} -- Board square type (U stands for Unmarked)

type Board = Array(3, 3) of TicTacToeSquare -- Board definition

-- Initial board state for a Tic-tac-toe board
initialTTTBoard : Board
initialTTTBoard!(x,y) = U
"
%}

### inARow
The `inARow` function will tell you if there is a row of values on a board.

The function takes 3 arguments: An integer which is the length of the row we are looking for, a *Content* value which is the value we are looking to see if there is a row of, and the board we are looking for a row in. The return value of an `inARow` function call is a Bool which will be **True** if the specified row exists and **False** if it does not. 

:dart: **Excercise:**  
1. Call the value `noRowBoard` in the interpreter below? What gets returned?
2. Enter the function call `inARow(3, X, noRowBoard)` into the interpreter below. What gets returned?
3. Call the value `rowBoard` in the interpreter below. What gets returned?
4. Enter the function call `inARow(3, X, rowBoard)` in the interpreter below? What gets returned?
{% include code_module_template.html
content = "game PlaceExample
type TicTacToeSquare = {O, X, U} -- Board square type (U stands for Unmarked)

type Board = Array(3, 3) of TicTacToeSquare -- Board definition

-- Tic-tac-toe board without any rows of three
noRowBoard : Board
noRowBoard!(x,y) = O
noRowBoard!(1,1) = X
noRowBoard!(2,2) = X
noRowBoard!(3,2) = X
noRowBoard!(2,3) = X

-- Tic-tac-toe board with a row of three X values
rowBoard : Board
rowBoard!(x,y) = O
rowBoard!(1,1) = X
rowBoard!(2,2) = X
rowBoard!(3,3) = X
"
%}

### countBoard
The `countBoard` function will count the occurances of a value on a board.

The function takes 2 arguments: A *Content* value which is what we are counting the occurances of, and the board we are counting the occurances in. The return value of `countBoard` is the occurances of the specified value on the board.

:dart: **Excercise:**  
1. Call the value `tttBoard` in the interpreter below. What is the return value?
2. Enter the function call `countBoard(U, tttBoard)` into the interpreter below. What gets returned?
3. Enter the function call `countBoard(X, tttBoard)` into the interpreter below. What gets returned?
4. Enter the function call `countBoard(O, tttBoard)` into the interpreter below. What gets returned?

{% include code_module_template.html
content = "game PlaceExample
type TicTacToeSquare = {O, X, U} -- Board square type (U stands for Unmarked)

type Board = Array(3, 3) of TicTacToeSquare -- Board definition

-- Tic-tac-toe board
tttBoard : Board
tttBoard!(x,y) = U
tttBoard!(1,1) = X
tttBoard!(2,2) = O
tttBoard!(3,2) = X
tttBoard!(2,3) = O
tttBoard!(3,3) = X
"
%}