---
sort: 2 # Order in the sidebar
---

# Understanding Types

:leaves: **This tutorial covers:**
- Enumerated types
- Built-in types (Ints and Bools)
- Tuples
- Extended types

:seedling: **Before starting, you should be familiar with:**
- [The BoGL basics](./GettingStarted.md)

:deciduous_tree: **At the end, you should be able to:**
- Write a type definition
- Create an enumerated type
- Describe the set of possible values for a Bool
- Describe the set of possible values for an Int
- Create an extended type
- Create a tuple type

## Basic Types

Jack and Rosa want to play a board game. They decide that who gets to go first should be determined by the result of a coin toss.

In these first few tutorials, we will create a program using the BoGL language that will take the result of a coin toss and choose who gets to go first based on that result.

In order to do this, our program must be able to...

1. Capture the inputted value of the coin toss result.
2. Output whether Jack or Rosa gets to go first based off of the result value.

![program input and output diagram](../imgs/types-program-diagram.jpg)

We will start out by creating our game.

{% highlight haskell %}
game WhoGoesFirst
{% endhighlight %}

Next we will capture the coin toss result. For this we are going to create a type that allows us to describe the result of a coin toss.

![toss result variable type diagram](../imgs/types-tossresult-diagram.jpg)


There are currently no defined types for a coin toss result, so we will make one ourselves!
The pieces of information we need to create our own type are:

* The name we want to call our type
* The possible values of our type

To define a type, we must first use the keyword `type` in our program, followed by the desired name for our type. It is required by the language that the first letter of this name be capitalized. We will then define what the possible values are for this type using the `=` operator, followed then by a list of the possible values, seperated by commas, and contained within `{}` brackets. These values will also be given desired names (with their first letters capitalized). It will end up looking like this:


{% highlight haskell %}
type TossResult = {Heads, Tails}
{% endhighlight %}

![analysis of declaring a type](../imgs/types-code-analysis.jpg)

Think of a type as a classification. We, as the programmer, must specify what information is allowed to be classified as our type.
For this example we will also be needing a type for the first player (the eventual output of this program).
{% highlight haskell %}
type FirstPlayer = {Jack, Rosa}
{% endhighlight %}
We will return to this example in the [functions tutorial](./functions), where we will be able to utilize the TossResult type.

Below are a few more examples of user-defined types that do not pertain to coin results, just to nail down the concept of defining types!

{% highlight haskell %}
-- board game themed types
type CardSuit = {Diamond, Club, Heart, Spade}
type PlayerColor = {Blue, Red, Green, Yellow}
type DiceResult = {One, Two, Three, Four, Five, Six}
type ChessPiece = {Pawn, Bishop, Knight, Rook, Queen, King}
type ClueCharacter = {MissScarlett, ColonelMustard, MrsWhite, ReverendGreen, MrsPeacock, ProfessorPlum}

-- non board game themed types
type ClassicIceCreamFlavor = {Vanilla, Chocolate, Strawberry}
type WaterValveState = {Open, Closed}
type StateOfMatter = {Solid, Liquid, Gas, Plasma}
type ClothingArticle = {Shirt, Pants, Hat, Shoe, Jacket, Scarf, Glove}
{% endhighlight %}

The examples seen above are what we call [enumeration types](https://en.wikipedia.org/wiki/Enumerated_type).
There are also two other types built into BoGL: integers and booleans.

{% highlight haskell %}
Int -- Integer type
Bool -- Boolean type
{% endhighlight %}
Numbers are useful! The possible values of the type `Int` are all integer values (..., `-2`, `-1`, `0`, `1`, `2`, `3`, ...).

Sometimes we just need a binary value. The possible values of the type `Bool` are `True` and `False`.

One more important thing to note about types is that they can be synonyms to other types (see examples below).

{% highlight haskell %}
type Score = Int -- Score is a synonym of Int, so its possible values are the same as Int.
type ShirtColor = PlayerColor -- ShirtColor has the same possible values that PlayerColor (an earlier defined type) has.
{% endhighlight %}

<br/>
## Tuples
Sometimes it can be useful to have a type that will consist of value pairs. Think of a grid coordinate that has an X and Y value.
A tuple type can be created in the same way as a normal type, except instead of putting values surrounded by curly brackets `{}` after the equals sign `=`, you can put types surrounded by parenthesis `()`.
{% highlight haskell %}
type Coordinate = (Int, Int) -- Tuple type for describing an XY coordinate.
{% endhighlight %}

Another example of a tuple is a standard card from a 52 card deck. Each standard card has two values: Suit and Rank. If we wanted to make a standard card type we could write out the 52 values that a card could possibly have, _or_ we could declare three types: Rank, Suit, and then Card which consists of a Rank and Suit pair.
{% highlight haskell %}
type Rank = {Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten, Jack, Queen, King, Ace}
type Suit = {Spade, Heart, Diamond, Club}

type Card = (Rank, Suit) -- Tuple type for describing a standard card.
{% endhighlight %}

Tuple types do not have to be only two values (despite how it sounds). Two is just the minimum amount of values a tuple can consist of.
{% highlight haskell %}
type Card = (Rank, Suit) -- Tuple consisting of two types
type Hand = (Card, Card, Card, Card, Card) -- Tuple type that represents a five card hand. Consists of five Card types.

type Score = Int -- Normal non-tuple type
type Player = (PlayerColor, Hand, Score) -- Tuple type that represents a player in a card game. Consists of three different types.
{% endhighlight %}

<br/>
## Extended Types
Something useful that we can do with types is extend them. An extended type is created by taking an existing type and adding more possible values to it.
To create an extended type we must first define the name of the new type (just like creating a normal type), and then set it equal to the type we are extending, followed by an ampersand `&` and a new set of possible values surrounded by curly brackets `{}`. Below is an example of creating a new extended type (IceCreamFlavor) based off of an existing one (ClassicIceCreamFlavor).
{% highlight haskell %}
type ClassicIceCreamFlavor = {Vanilla, Chocolate, Strawberry} -- Regular enumeration type
{% endhighlight %}
![ClassicIceCreamFlavor](../imgs/types-ClassicIceCreamFlavor.jpg)
{% highlight haskell %}
type IceCreamFlavor = ClassicIceCreamFlavor & {Mint, BirthdayCake, BubbleGum, Coffee} -- Extended type
{% endhighlight %}
![IceCreamFlavor](../imgs/types-IceCreamFlavor.jpg)
Since IceCreamFlavor was created by extending ClassicIceCreamFlavor, its possible values consist of those that were in ClassicIceCreamFlavor (Vanilla, Chocolate, Strawberry) along with the new values that were defined (Mint, BirthdayCake, BubbleGum, Coffee).
It would also be possible to create a type for the new values first, and then use that type to create the extended IceCreamFlavor type.
{% highlight haskell %}
type ClassicIceCreamFlavor = {Vanilla, Chocolate, Strawberry} -- Regular enumeration type
type NonClassicIceCreamFlavor = {Mint, BirthdayCake, BubbleGum, Coffee} -- Regular enumeration type
type IceCreamFlavor = ClassicIceCreamFlavor & NonClassicIceCreamFlavor -- Extended type
{% endhighlight %}

<br/>
:hammer_and_wrench: **Example: Canasta Card Game**  
Not all card games use Jokers, but some (like [Canasta](https://en.wikipedia.org/wiki/Canasta)) do. We can create a new type that allows for normal cards plus Jokers by extending a previously defined Card type.

{% highlight haskell %}
type CanastaCard = Card & {Joker} -- Extended type
{% endhighlight %}

<br/>
:hammer_and_wrench: **Example: A Hand of Cards**  
The Hand type shown below represents five cards held in a hand. Lets assume the game being played has a five card limit.
{% highlight haskell %}
type Hand = (Card, Card, Card, Card, Card) -- Tuple type that represents a five card hand. Consists of five Card types.
{% endhighlight %}
What if we wanted the ability to represent not having a card? This could be because a card that was in our hand got played, taken by someone, or discarded. To represent the absence of cards in our hand, we could do something like this:

{% highlight haskell %}
type MaybeCard = Card & {Nothing}
type Hand = (MaybeCard, MaybeCard, MaybeCard, MaybeCard, MaybeCard) -- Type representing a hand of cards. Consists of five potential cards.
{% endhighlight %}




<br/>
## Examples of Types
Here are some examples of defining types in BoGL:
{% highlight haskell %}
-- Normal enumeration types
type TossResult = {Heads, Tails}
type CardSuit = {Diamond, Club, Heart, Spade}
type PlayerColor = {Blue, Red, Green, Yellow}
type DiceResult = {One, Two, Three, Four, Five, Six}
type ChessPiece = {Pawn, Bishop, Knight, Rook, Queen, King}
type ClueCharacter = {MissScarlett, ColonelMustard, MrsWhite, ReverendGreen, MrsPeacock, ProfessorPlum}
type Rank = {Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten, Jack, Queen, King, Ace}
type Suit = {Spade, Heart, Diamond, Club}
type ClassicIceCreamFlavor = {Vanilla, Chocolate, Strawberry}
type WaterValveState = {Open, Closed}
type StateOfMatter = {Solid, Liquid, Gas, Plasma}
type ClothingArticle = {Shirt, Pants, Hat, Shoe, Jacket, Scarf, Glove}
type QuestionAnswer = {Yes} -- You are allowed to create a type that has only one possible value.

-- Types based off of the built in types
type TrueFalseQuestionAnswer = Bool -- Possible values are True and False
type Score = Int -- Possible values are any integer

-- Tuple types
type Coordinate = (Int, Int)
type Card = (Rank, Suit)
type Hand = (Card, Card, Card, Card, Card)
type Player = (PlayerColor, Hand, Score)

-- Extended types
type IceCreamFlavor = ClassicIceCreamFlavor & {Mint, BirthdayCake, BubbleGum, Coffee} -- Extended type
type CanastaCard = Card & {Joker} -- Extended type
type GamePiece = ChessPiece & ClueCharacter -- Type extended with another type.
{% endhighlight %}

Here are some of the things you are <span style="color:red">**not allowed**</span> to do with types in BoGL:
{% highlight haskell %}
type pastaSauce = {Alfredo, Marinara, Pesto} -- A type name must start with a capital letter.
type PastaSauce = {alfredo, marinara, pesto} -- Value names must start with capital letters.
type EmptyType = {} -- Every type needs at least one possible value.
type OneValueTuple = (Int) -- Tuples need two or more possible values.
{% endhighlight %}

Next, we'll show you how BoGL can use types to define values!
