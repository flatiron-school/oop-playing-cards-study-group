# Topic 21: Object-Oriented Programming 

- onl01-dtsc-ft-022221
- 04/27/21

## Topics

- **OOP-Vocabulary**
- **Defining/Initializing Classes**
- **Inspecting classes:**
    - `help(obj)` vs `dir(obj)`
    
- **Deeper dive into Classes/Objects**
    - special methods/properties (`__repr__(),__str__(),__call__(),__version__(),__name__()`)
    - Methods: vs Bound Methods vs Static Methods 
    
- **Extended Activity: Building a Deck of PlayingCards**

# What does it mean to be 'Object-Oriented'?

> ### ___"Everything is an object."___
- some Python sensei


- Any function, method, class, variable are ALL objects. 
    - Built from a template Class
    - Can be stored in memory under any name 


```python
## Any function can be assigned to a new variable and will behave the same way
prove_it = print
prove_it
```




    <function print>




```python
prove_it("This is now equal to print.")
```

    This is now equal to print.



```python
help(prove_it)
```

    Help on built-in function print in module builtins:
    
    print(...)
        print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
        
        Prints the values to a stream, or to sys.stdout by default.
        Optional keyword arguments:
        file:  a file-like object (stream); defaults to the current sys.stdout.
        sep:   string inserted between values, default a space.
        end:   string appended after the last value, default a newline.
        flush: whether to forcibly flush the stream.
    



```python
prove_it.__name__
```




    'print'



# OOP VOCABULARY


#### VOCAB RELATED TO FUNCTIONS:

- **Function: A resuable, fleixbile block of code that runs a process**  

    - Parameters: inputs that are expected by python/that function
    
    - Argument: the actual value/variable passed into the function. 
        - Positional Argument:
        - Keyword/default Arguments:

- "Calling" a function: `( )`


```python
def my_func(posarg1, 
            posarg2, kwarg1='example',kwarg2='example2'):
    print('positional arguments:')
    print(f"    {posarg1}, {posarg2}")
    
    print("keyword arguments:")
    print(f"    kwarg1={kwarg1}")
    print(f"    kwarg2={kwarg2}")
```


```python
## What does my_func look like?
my_func
```




    <function __main__.my_func(posarg1, posarg2, kwarg1='example', kwarg2='example2')>




```python
## Run my_func with posarg1=1,posarg2=2
my_func(1,2)
```

    positional arguments:
        1, 2
    keyword arguments:
        kwarg1=example
        kwarg2=example2



```python
## Change kwarg1 to something else
my_func(1,2,kwarg1='something else')
```

    positional arguments:
        1, 2
    keyword arguments:
        kwarg1=something else
        kwarg2=example2


#### VOCAB RELATED TO CLASSES:

- "Object": 
- **Class:** 
- Instance: 
- Attribute:
- Method:
- Private Attributes/Methods: 
- Getters/Setters:


- Object: 

- "dunders" = double underscores __ 

# Defining and Initializing Classes


- Use `class NewClassName():` like you use `def function_name():` for functions.
    - the `()` are optional for classes. (used to inherit other classes, more on that later)

#### Naming Classes
    
- Convention for naming classes = `UpperCamelCase`
- Convention for naming function = `snake_case`


```python
## Bare minimum to define a class.
class PlayingCard:
    pass
```

## Attributes and Methods

- Attribute: a variable is stored inside a class/object

- Method: a function that is stored inside of and (usually) operates on the object/class

## What makes a PlayingCard?

***Card:***
- *Value / Name*:
    - 2, 3, 4, 5, 6, 7, 8, 9, 10, J, Q, K, A
- *Suit*:
    - Hearts (H), Spades(S), Clubs(C), Diamonds(D) 
- *Color*:
    - black or red


```python
## Make a new PlayingCard class with a value of 2 and a suit of spades, and color=black
class PlayingCard:
    value = 2
    suit = 'S'
    color = 'black'
```


```python
## Make an instance of a PlayingCard and display it
card = PlayingCard()
card
```




    <__main__.PlayingCard at 0x7ffd59ae4340>




```python
## Print out the vard's value and suit in one statement
print(f"{card.value} of {card.suit}")
```

    2 of S


> Improving our class: let's add a `.flip()` method that will print the value and suit. 



```python
## Copy previous version of class and add the .flip method.
class PlayingCard:
    value = 2
    suit = 'S'
    color = 'black'
    
    def flip():
        print(f"{value} of {suit}")
    
```


```python
## Insantiate a playing card and run .flip()    
card = PlayingCard()
card.flip()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-14-3e3653bddd7a> in <module>
          1 ## Insantiate a playing card and run .flip()
          2 card = PlayingCard()
    ----> 3 card.flip()
    

    TypeError: flip() takes 0 positional arguments but 1 was given


> Ruh roh! What happened?! What does the error message mean?

### Know thy `self`

- Because Methods are designed to operate on the `object_its.attached_to()`, Python automatically gives every method a copy of instance its attached to, which we call `self`
- We have to pass `self` as the first parameter for every method we make.
- Otherwise it will think that the first thing we give it is actually itself. This will cause an *existential crisis** and corresponding error.


```python
## Update our class by adding self where its needed.
class PlayingCard:
    value = 2
    suit = 'S'
    color = 'black'
    
    def flip(self):
        print(f"{self.value} of {self.suit}")
        
```


```python
## Instantiate a playing card and test out .flip()
card = PlayingCard()
card.flip()
```

    2 of S


## Initialization 


- As we have seen, we create an instance by setting a `instance = ClassName()`
-  This uses the template `ClassName` to create an instance of the class ( which we named `instance`).
> - How do we change our PlayingCard class so that we can make other cards besides the 2 of spades?

### `__init__`

> - When an instance is `initialized`, we `call` it using `()`, which runs a default `__init__()` method.


```python
## Update our class by adding an __init__ that controls value, suit, color
class PlayingCard:

    def __init__(self, value, suit):
        """Creates a playing card instance with the specified value and suit
        Value should be 2-10, or J,Q,K,A
        Suit should be 'H','D','C','S'
        """
        self.value = value
        self.suit = suit
        
        self.color = 'black' if suit in ('S', 'C') else 'red'
    
    def flip(self):
        print(f"{self.value} of {self.suit}")
        
```


```python
## test out our updated playing card class by making  card1 = a 'J of Hearts'
card1 = PlayingCard('J','H')
card1.flip()
```

    J of H



```python
## test out our updated playing card class by making a 7 of Spades
card2 = PlayingCard(7,'S')
card2.flip()
```

    7 of S


### is card 1 greater than card 2?


```python
card1 > card2
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-20-9f3801cab5e7> in <module>
    ----> 1 card1 > card2
    

    TypeError: '>' not supported between instances of 'PlayingCard' and 'PlayingCard'


# Special Class Methods

#### Special Methods

It is common for a class to have magic methods. These are identifiable by the "dunder" (i.e. **d**ouble **under**score) prefixes and suffixes, such as `__init__()`. These methods will get called **automatically**, as we'll see below.

For more on these "magic methods", see [here](https://www.geeksforgeeks.org/dunder-magic-methods-python/).

## Using Special Methods to evaluate comparisons 

### `__gt__` & `__lt__`:



```python
## Add a __gt__ and __lt__ method to compare self to other
class PlayingCard:

    def __init__(self, value, suit):
        """Creates a playing card instance with the specified value and suit
        Value should be 2-10, or J,Q,K,A
        Suit should be 'H','D','C','S'
        """
        self.value = value
        self.suit = suit
        
        self.color = 'black' if suit in ('S', 'C') else 'red'
    
    def flip(self):
        print(f"{self.value} of {self.suit}")
        
        
    def __gt__(self,other):
        return self.value > other.value

    def __lt__(self,other):
        return self.value < other.value
```


```python
## Remake card 1 and card2 and test if card1>card2
card1 = PlayingCard('J','H')
card1.flip()

card2 = PlayingCard(7,'S')
card2.flip()
card1>card2
```

    J of H
    7 of S



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-22-bd54fc6d35d3> in <module>
          5 card2 = PlayingCard(7,'S')
          6 card2.flip()
    ----> 7 card1>card2
    

    <ipython-input-21-924a55780a00> in __gt__(self, other)
         17 
         18     def __gt__(self,other):
    ---> 19         return self.value > other.value
         20 
         21     def __lt__(self,other):


    TypeError: '>' not supported between instances of 'str' and 'int'


> - Ruh Roh! We can't compare a str and an int! Let's fix this with out `__init__ `



#### Adding our card's value

- Create a string-based `name` for the card
    - Update `.flip()` to use `name` instead of `value`
- Save the numeric `value` of the card for comparison
- 


```python
## Value dictionary for lookup 
value_dct = {
    '2': 2,'3': 3, '4': 4, '5': 5, '6': 6,
    '7': 7, '8': 8, '9': 9, '10': 10,
    'J': 11,'Q': 12, 'K': 13, 'A': 14
    }
value_dct
```




    {'2': 2,
     '3': 3,
     '4': 4,
     '5': 5,
     '6': 6,
     '7': 7,
     '8': 8,
     '9': 9,
     '10': 10,
     'J': 11,
     'Q': 12,
     'K': 13,
     'A': 14}




```python
## Create a string-based name for the card
# save the numeric value of the card for comparison

class PlayingCard:

    def __init__(self, name, suit):
        """Creates a playing card instance with the specified value and suit
        Value should be 2-10, or J,Q,K,A
        Suit should be 'H','D','C','S'
        
        """
        ## Save the name as a str, and save suit
        self.name = str(name)
        self.suit = suit

        
        ## Lookup the correct
        value_dct = {
            '2': 2,'3': 3, '4': 4, '5': 5, '6': 6,
            '7': 7, '8': 8, '9': 9, '10': 10,
            'J': 11,'Q': 12, 'K': 13, 'A': 14
            }
        
        self.value = value_dct[self.name]
        self.color = 'black' if suit in ('S', 'C') else 'red'
        
    
    def flip(self):
        print(f"{self.name} of {self.suit}")
        
        
    def __gt__(self,other):
        return self.value > other.value

    def __lt__(self,other):
        return self.value < other.value
```


```python
## Run this cell to test our value comparison from before
card1 = PlayingCard('J','H')
card1.flip()

card2 = PlayingCard(7,'S')
card2.flip()

card1>card2
```

    J of H
    7 of S





    True




```python
## What about card1<card2?
card1<card2
```




    False



> Success! This is great and all, but the PlayingCard isn't terribly fun-looking 


```python
## Display card1
card1
```




    <__main__.PlayingCard at 0x7ffd59b78d90>



## Using special methods to control the output of a class


```python
## Display card 1
display(card1)
card1
```


    <__main__.PlayingCard at 0x7ffd59b78d90>





    <__main__.PlayingCard at 0x7ffd59b78d90>



### `__repr__()`  &  `__str__()`

- Whenever an object is displayed, it runs the object's  `__repr__()` method. 
- Whenever an object is printed, it runs the  `__str__()` method.

- They are both designed to return string-representations of the object. 
    - But `__repr__()` focuses on minimizing ambiguity while `__str__()` focuses on readability. 
- However, if your class has no `__str__()` method, it will fall back on `__repr__()` (if it exists!). 
    - For more on this distinction, see [this post](https://dbader.org/blog/python-repr-vs-str).

### Add a `__repr__` and ` __str__` method to our PlayingCard

- have `__repr__` return 'An Instance of a PlayingCard'
- have `__str__` return "A Playing Card"


```python
### add a __repr__() method that returns "A Playing Card"
class PlayingCard:

    def __init__(self, name, suit):
        """Creates a playing card instance with the specified value and suit
        Value should be 2-10, or J,Q,K,A
        Suit should be 'H','D','C','S'
        
        """
        ## Save the name as a str, and save suit
        self.name = str(name)
        self.suit = suit

        
        ## Lookup the correct
        value_dct = {
            '2': 2,'3': 3, '4': 4, '5': 5, '6': 6,
            '7': 7, '8': 8, '9': 9, '10': 10,
            'J': 11,'Q': 12, 'K': 13, 'A': 14
            }
        
        self.value = value_dct[self.name]
        self.color = 'black' if suit in ('S', 'C') else 'red'
        
    
    def flip(self):
        print(f"{self.name} of {self.suit}")
        
        
    def __gt__(self,other):
        return self.value > other.value

    def __lt__(self,other):
        return self.value < other.value
    
        
    def __repr__(self):
        return "An Instance of a Playing Card"
    
    def __str__(self):
        return "A PlayingCard"
```


```python
## Test out our __repr__ & __str__
card1 = PlayingCard('J','H')
card1.flip()
print(card1)
card1
```

    J of H
    A PlayingCard





    An Instance of a Playing Card



### Making Things More Interesting

-  Instead of our text-based `__repr__` and `__str__`, let's be sneaky and use an image of card back for our `__repr__`.

<img src="card_back.png" width=100>


```python
from PIL import Image
img = Image.open("card_back.png")
img.resize((100,150))
```




    
<img src="card_back.png" width=100>
    



> - Add a `back` attribute that stores the image of the card. 
- Replace our `__repr__` with display the image
    - Go ahead and delete our `__str__` since we want it to behave the same as `__repr__` anyway


```python
### add a .back containing the stored image of the card.
class PlayingCard:

    def __init__(self, name, suit):
        """Creates a playing card instance with the specified value and suit
        Value should be 2-10, or J,Q,K,A
        Suit should be 'H','D','C','S'
        
        """
        ## Save the name as a str, and save suit
        self.name = str(name)
        self.suit = suit

        
        ## Lookup the correct
        value_dct = {
            '2': 2,'3': 3, '4': 4, '5': 5, '6': 6,
            '7': 7, '8': 8, '9': 9, '10': 10,
            'J': 11,'Q': 12, 'K': 13, 'A': 14
            }
        
        self.value = value_dct[self.name]
        
        self.color = 'black' if suit in ('S', 'C') else 'red'
        
        
        ## Save the card back
        self.back = Image.open("card_back.png").resize((100,150))
                                                       
        
    
    def flip(self):
        print(f"{self.name} of {self.suit}")
        
        
    def __gt__(self,other):
        return self.value > other.value

    def __lt__(self,other):
        return self.value < other.value
    
        
    def __repr__(self):
                                                       
        display(self.back)
        return ''
```


```python
## Test out our __repr__
card1 = PlayingCard('J','H')
card1
```


    
<img src="card_back.png" width=100>
    





    




```python
print(card1)
```


    
<img src="card_back.png" width=100>
    


    



```python
card1.flip()
```

    J of H


> Success!

### Making our class even MORE fun 

- Our `.flip()` method is kind of boring compared to our `__repr__`
- If only we had some way of using symbols in our text instead of just boring old font.... 🤔


```python

symbols = {'S':'♠️','C':'♣️','D':'♦️','H':'♥️'}
symbols
```




    {'S': '♠️', 'C': '♣️', 'D': '♦️', 'H': '♥️'}



> #### Use these emoji symbols plus the provided make_ascii_card to overhaul our class's .flip()


```python
def make_ascii_card(name, suit):
    """Ascii Card adapted from: https://codereview.stackexchange.com/questions/82103/ascii-fication-of-playing-cards
    """
    symbols = {'S':'♠️','C':'♣️',
               'D':'♦️','H':'♥️'}
    suit_symbol = symbols[suit]
    if name == '10':
        space=''
    else:
        space = ' '

    # add the individual card on a line by line basis
    _ascii=[]
    _ascii.append('┌─────────┐')
    _ascii.append(f'│{name}{space}       │')#.format(rank, space))  # use two {} one for char, one for space or char
    _ascii.append('│         │')
    _ascii.append('│         │')
    _ascii.append(f'│    {suit_symbol}   │') #.format(suit))
    _ascii.append('│         │')
    _ascii.append('│         │')
    _ascii.append(f'│       {space}{name}│')#.format(space, rank))
    _ascii.append('└─────────┘')
    
    return '\n'.join(_ascii)



print(make_ascii_card('A','D'))
```

    ┌─────────┐
    │A        │
    │         │
    │         │
    │    ♦️   │
    │         │
    │         │
    │        A│
    └─────────┘


> - Add a .face containing the stored ascii version of the card.
- Change `.flip()` to use the new face.



```python
### add a .face containing the stored ascii version of the card.
class PlayingCard:

    def __init__(self, name, suit):
        """Creates a playing card instance with the specified value and suit
        Value should be 2-10, or J,Q,K,A
        Suit should be 'H','D','C','S'
        
        """
        ## Save the name as a str, and save suit
        self.name = str(name)
        self.suit = suit

        
        ## Lookup the correct
        value_dct = {
            '2': 2,'3': 3, '4': 4, '5': 5, '6': 6,
            '7': 7, '8': 8, '9': 9, '10': 10,
            'J': 11,'Q': 12, 'K': 13, 'A': 14
            }
        
        self.value = value_dct[self.name]
        
        self.color = 'black' if suit in ('S', 'C') else 'red'
        
        
        ## Save the card back
        self.back = Image.open("card_back.png").resize((100,150))
        
        ## save the .face
        self.face = make_ascii_card(name,suit)
                                                       
        
    
    def flip(self):
        print(self.face)
#         print(f"{self.name} of {self.suit}")
        
        
    def __gt__(self,other):
        return self.value > other.value

    def __lt__(self,other):
        return self.value < other.value
    
        
    def __repr__(self):
                                                       
        display(self.back)
        return ''
```


```python

## Run this cell to test our value comparison from before
card1 = PlayingCard('J','H')
card1.flip()

card2 = PlayingCard(7,'S')
card2.flip()

card1>card2
```

    ┌─────────┐
    │J        │
    │         │
    │         │
    │    ♥️   │
    │         │
    │         │
    │        J│
    └─────────┘
    ┌─────────┐
    │7        │
    │         │
    │         │
    │    ♠️   │
    │         │
    │         │
    │        7│
    └─────────┘





    True



> Yay!!! We are HUGE nerds but yay!!!!

# NEXT: two options

1. Option 1: Make a `Deck` of `PlayingCards` to explore some additional special methods.

2. Option 2: Discuss Inheritance and make our own OneHotEncoder that is more convient than the default

## Option 1: Make a `Deck` to Explore some additional special methods. 

### Overview - Whats in a Deck?

***A Deck***
- "a collection of Cards"
    - Contains all standard 52 cards in a `.cards` attribute.
- Has a  `.shuffle()` method.
- Returns the number of cards in the deck as the `__repr__`

- Has a `__getitem__` method to make the deck subscriptable


```python
import numpy as np
class Deck:
    
    
    def __init__(self):
        self.cards = []
        
        cards_to_make = list(range(2,11))
        cards_to_make.extend(['J','Q','K','A'])
        
        suits_to_make = ['S','C','H','D']
        
        for suit in suits_to_make:
            for value in cards_to_make:
                self.cards.append( PlayingCard(value,suit))
        
        
    def shuffle(self):
        np.random.shuffle(self.cards)

    def __repr__(self):
        return f"There are {len(self.cards)} cards in the deck."


    def __getitem__(self,index):
        return self.cards[index]

```


```python
## Test out the Deck class by running the next cells
deck = Deck()
deck
```




    There are 52 cards in the deck.




```python
## Slice out the first .cards
deck.cards[0]
```


    
<img src="card_back.png" width=100>
    





    




```python
## Slice first card directly and flip
deck[0].flip()
```

    ┌─────────┐
    │2        │
    │         │
    │         │
    │    ♠️   │
    │         │
    │         │
    │        2│
    └─────────┘



```python
## shuffle and flip the first card again
deck.shuffle()
deck[0].flip()
```

    ┌─────────┐
    │4        │
    │         │
    │         │
    │    ♥️   │
    │         │
    │         │
    │        4│
    └─────────┘



```python
## Completed Deck Class
import numpy as np
class Deck:
    
    cards = []
    def __init__(self,shuffle=True):
        
        ## Make a complete deck
        cards_to_make = [str(i) for i in range(2,11)]
        cards_to_make.extend(['A','K','Q','J'])
        
        deck = []
        for value in cards_to_make:
            for suit in ['H','S','C','D']:
                deck.append(PlayingCard(value,suit))
        self.cards = deck

        if shuffle==True:
            self.shuffle()
    
    def shuffle(self):
        np.random.shuffle(self.cards)

    def __repr__(self):
        return f"There are {len(self.cards)} cards in the deck."

    def __getitem__(self, index):
        return self.cards[index]

```


```python

```

# Nice!

## To recap, we:

1. Built a Card object using `class`.
    - Used `__init__(self)` to set attributes and run processes when the object is created.
    - Created a method `flip(self)` which "flips the card over" (shows the name and suit).
    - Experimented with `__str__` and `__repr__`.

  
2. Built a Deck object that uses `Cards`!
    - Decks can `shuffle` and `shuffle_and_deal`.
    
---

There are some points which we missed for the sake of drawing up the example.

- We could clean up the Card functions for deciding what to do if a user tries to create a Card without a real `name` or `suit`.

- We also don't have a plan for what happens if the Deck uses `shuffle_and_deal` but doesn't have enough cards left!

___

## Option 2: Use inheritance to make our own OneHotEncoder

### Inheritance

- We can inherit all of the properties of another class when we write a new one to save ourselves time and effort. 


```python
# !pip install -U fsds
from fsds.imports import *
```

    fsds v0.4.5 loaded.



<style type="text/css">
#T_a32d7_row0_col0, #T_a32d7_row0_col2, #T_a32d7_row0_col3, #T_a32d7_row1_col0, #T_a32d7_row1_col2, #T_a32d7_row1_col3, #T_a32d7_row2_col0, #T_a32d7_row2_col2, #T_a32d7_row2_col3, #T_a32d7_row3_col0, #T_a32d7_row3_col2, #T_a32d7_row3_col3, #T_a32d7_row4_col0, #T_a32d7_row4_col2, #T_a32d7_row4_col3, #T_a32d7_row5_col0, #T_a32d7_row5_col2, #T_a32d7_row5_col3, #T_a32d7_row6_col0, #T_a32d7_row6_col2, #T_a32d7_row6_col3, #T_a32d7_row7_col0, #T_a32d7_row7_col2, #T_a32d7_row7_col3 {
  text-align: left;
}
#T_a32d7_row0_col1, #T_a32d7_row0_col4, #T_a32d7_row1_col1, #T_a32d7_row1_col4, #T_a32d7_row2_col1, #T_a32d7_row2_col4, #T_a32d7_row3_col1, #T_a32d7_row3_col4, #T_a32d7_row4_col1, #T_a32d7_row4_col4, #T_a32d7_row5_col1, #T_a32d7_row5_col4, #T_a32d7_row6_col1, #T_a32d7_row6_col4, #T_a32d7_row7_col1, #T_a32d7_row7_col4 {
  text-align: left;
  text-align: center;
}
</style>
<table id="T_a32d7_">
  <caption>Loaded Packages & Info</caption>
  <thead>
    <tr>
      <th class="col_heading level0 col0" >Package</th>
      <th class="col_heading level0 col1" >Handle</th>
      <th class="col_heading level0 col2" >Version</th>
      <th class="col_heading level0 col3" >Documentation</th>
      <th class="col_heading level0 col4" >Imported</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td id="T_a32d7_row0_col0" class="data row0 col0" >pandas</td>
      <td id="T_a32d7_row0_col1" class="data row0 col1" >pd</td>
      <td id="T_a32d7_row0_col2" class="data row0 col2" >1.3.0</td>
      <td id="T_a32d7_row0_col3" class="data row0 col3" ><a href="https://pandas.pydata.org/docs/">https://pandas.pydata.org/docs/</a></td>
      <td id="T_a32d7_row0_col4" class="data row0 col4" >Y</td>
    </tr>
    <tr>
      <td id="T_a32d7_row1_col0" class="data row1 col0" >fsds</td>
      <td id="T_a32d7_row1_col1" class="data row1 col1" >fs</td>
      <td id="T_a32d7_row1_col2" class="data row1 col2" >0.4.5</td>
      <td id="T_a32d7_row1_col3" class="data row1 col3" ><a href="https://fs-ds.readthedocs.io/en/latest/">https://fs-ds.readthedocs.io/en/latest/</a></td>
      <td id="T_a32d7_row1_col4" class="data row1 col4" >Y</td>
    </tr>
    <tr>
      <td id="T_a32d7_row2_col0" class="data row2 col0" >numpy</td>
      <td id="T_a32d7_row2_col1" class="data row2 col1" >np</td>
      <td id="T_a32d7_row2_col2" class="data row2 col2" >1.19.5</td>
      <td id="T_a32d7_row2_col3" class="data row2 col3" ><a href="https://numpy.org/doc/stable/reference/">https://numpy.org/doc/stable/reference/</a></td>
      <td id="T_a32d7_row2_col4" class="data row2 col4" >Y</td>
    </tr>
    <tr>
      <td id="T_a32d7_row3_col0" class="data row3 col0" >matplotlib</td>
      <td id="T_a32d7_row3_col1" class="data row3 col1" >mpl</td>
      <td id="T_a32d7_row3_col2" class="data row3 col2" >3.3.1</td>
      <td id="T_a32d7_row3_col3" class="data row3 col3" ><a href="https://matplotlib.org/stable/api/index.html">https://matplotlib.org/stable/api/index.html</a></td>
      <td id="T_a32d7_row3_col4" class="data row3 col4" >Y</td>
    </tr>
    <tr>
      <td id="T_a32d7_row4_col0" class="data row4 col0" >matplotlib.pyplot</td>
      <td id="T_a32d7_row4_col1" class="data row4 col1" >plt</td>
      <td id="T_a32d7_row4_col2" class="data row4 col2" ></td>
      <td id="T_a32d7_row4_col3" class="data row4 col3" ><a href="https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.html#module-matplotlib.pyplot">https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.html#module-matplotlib.pyplot</a></td>
      <td id="T_a32d7_row4_col4" class="data row4 col4" >Y</td>
    </tr>
    <tr>
      <td id="T_a32d7_row5_col0" class="data row5 col0" >seaborn</td>
      <td id="T_a32d7_row5_col1" class="data row5 col1" >sns</td>
      <td id="T_a32d7_row5_col2" class="data row5 col2" >0.11.0</td>
      <td id="T_a32d7_row5_col3" class="data row5 col3" ><a href="https://seaborn.pydata.org/api.html">https://seaborn.pydata.org/api.html</a></td>
      <td id="T_a32d7_row5_col4" class="data row5 col4" >Y</td>
    </tr>
    <tr>
      <td id="T_a32d7_row6_col0" class="data row6 col0" >IPython.display</td>
      <td id="T_a32d7_row6_col1" class="data row6 col1" >dp</td>
      <td id="T_a32d7_row6_col2" class="data row6 col2" ></td>
      <td id="T_a32d7_row6_col3" class="data row6 col3" ><a href="https://ipython.readthedocs.io/en/stable/api/generated/IPython.display.html">https://ipython.readthedocs.io/en/stable/api/generated/IPython.display.html</a></td>
      <td id="T_a32d7_row6_col4" class="data row6 col4" >Y</td>
    </tr>
    <tr>
      <td id="T_a32d7_row7_col0" class="data row7 col0" >sklearn</td>
      <td id="T_a32d7_row7_col1" class="data row7 col1" ></td>
      <td id="T_a32d7_row7_col2" class="data row7 col2" >0.23.2</td>
      <td id="T_a32d7_row7_col3" class="data row7 col3" ><a href=""></a></td>
      <td id="T_a32d7_row7_col4" class="data row7 col4" >N</td>
    </tr>
  </tbody>
</table>




<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-latest.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-latest.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-latest.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




```python
## Getting the dataset ready (don't worry about this code for now)

df= fs.datasets.load_iowa_prisoners()
df.fillna('MISSING',inplace=True)
drop_cols= [col for col in df.columns if 'New' in col]
drop_cols.append('Days to Recidivism')
df.drop(columns=drop_cols,inplace=True)
df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fiscal Year Released</th>
      <th>Recidivism Reporting Year</th>
      <th>Race - Ethnicity</th>
      <th>Age At Release</th>
      <th>Convicting Offense Classification</th>
      <th>Convicting Offense Type</th>
      <th>Convicting Offense Subtype</th>
      <th>Release Type</th>
      <th>Main Supervising District</th>
      <th>Recidivism - Return to Prison</th>
      <th>Part of Target Population</th>
      <th>Recidivism Type</th>
      <th>Sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010</td>
      <td>2013</td>
      <td>Black - Non-Hispanic</td>
      <td>25-34</td>
      <td>C Felony</td>
      <td>Violent</td>
      <td>Robbery</td>
      <td>Parole</td>
      <td>7JD</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>New</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010</td>
      <td>2013</td>
      <td>White - Non-Hispanic</td>
      <td>25-34</td>
      <td>D Felony</td>
      <td>Property</td>
      <td>Theft</td>
      <td>Discharged – End of Sentence</td>
      <td>MISSING</td>
      <td>Yes</td>
      <td>No</td>
      <td>Tech</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010</td>
      <td>2013</td>
      <td>White - Non-Hispanic</td>
      <td>35-44</td>
      <td>B Felony</td>
      <td>Drug</td>
      <td>Trafficking</td>
      <td>Parole</td>
      <td>5JD</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Tech</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2010</td>
      <td>2013</td>
      <td>White - Non-Hispanic</td>
      <td>25-34</td>
      <td>B Felony</td>
      <td>Other</td>
      <td>Other Criminal</td>
      <td>Parole</td>
      <td>6JD</td>
      <td>No</td>
      <td>Yes</td>
      <td>No Recidivism</td>
      <td>Male</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2010</td>
      <td>2013</td>
      <td>Black - Non-Hispanic</td>
      <td>35-44</td>
      <td>D Felony</td>
      <td>Violent</td>
      <td>Assault</td>
      <td>Discharged – End of Sentence</td>
      <td>MISSING</td>
      <td>Yes</td>
      <td>No</td>
      <td>Tech</td>
      <td>Male</td>
    </tr>
  </tbody>
</table>
</div>




```python
## let's encode "ReleaseType" using onehotencoder
from sklearn.preprocessing import OneHotEncoder
cols_to_encode = ['Release Type']

encoder = OneHotEncoder(sparse=False).fit(df[cols_to_encode])

data_ohe = encoder.transform(df[cols_to_encode])
data_ohe
```




    array([[0., 0., 0., ..., 0., 0., 0.],
           [0., 1., 0., ..., 0., 0., 0.],
           [0., 0., 0., ..., 0., 0., 0.],
           ...,
           [0., 0., 0., ..., 0., 0., 0.],
           [0., 0., 0., ..., 1., 0., 0.],
           [0., 0., 0., ..., 0., 0., 0.]])




```python
## Turn data_ohe into a dataframe:
data_ohe = pd.DataFrame(data_ohe,
                        columns = encoder.get_feature_names(cols_to_encode))
data_ohe
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Release Type_Discharged - Expiration of Sentence</th>
      <th>Release Type_Discharged – End of Sentence</th>
      <th>Release Type_Interstate Compact Parole</th>
      <th>Release Type_MISSING</th>
      <th>Release Type_Parole</th>
      <th>Release Type_Parole Granted</th>
      <th>Release Type_Paroled to Detainer - INS</th>
      <th>Release Type_Paroled to Detainer - Iowa</th>
      <th>Release Type_Paroled to Detainer - Out of State</th>
      <th>Release Type_Paroled to Detainer - U.S. Marshall</th>
      <th>Release Type_Paroled w/Immediate Discharge</th>
      <th>Release Type_Released to Special Sentence</th>
      <th>Release Type_Special Sentence</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>26015</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26016</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26017</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26018</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26019</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>26020 rows × 13 columns</p>
</div>



### Make our own Encoder


```python
class OurOneHotEncoder(OneHotEncoder):
    pass
```


```python
help(OurOneHotEncoder)
```

    Help on class OurOneHotEncoder in module __main__:
    
    class OurOneHotEncoder(sklearn.preprocessing._encoders.OneHotEncoder)
     |  OurOneHotEncoder(*, categories='auto', drop=None, sparse=True, dtype=<class 'numpy.float64'>, handle_unknown='error')
     |  
     |  Encode categorical features as a one-hot numeric array.
     |  
     |  The input to this transformer should be an array-like of integers or
     |  strings, denoting the values taken on by categorical (discrete) features.
     |  The features are encoded using a one-hot (aka 'one-of-K' or 'dummy')
     |  encoding scheme. This creates a binary column for each category and
     |  returns a sparse matrix or dense array (depending on the ``sparse``
     |  parameter)
     |  
     |  By default, the encoder derives the categories based on the unique values
     |  in each feature. Alternatively, you can also specify the `categories`
     |  manually.
     |  
     |  This encoding is needed for feeding categorical data to many scikit-learn
     |  estimators, notably linear models and SVMs with the standard kernels.
     |  
     |  Note: a one-hot encoding of y labels should use a LabelBinarizer
     |  instead.
     |  
     |  Read more in the :ref:`User Guide <preprocessing_categorical_features>`.
     |  
     |  .. versionchanged:: 0.20
     |  
     |  Parameters
     |  ----------
     |  categories : 'auto' or a list of array-like, default='auto'
     |      Categories (unique values) per feature:
     |  
     |      - 'auto' : Determine categories automatically from the training data.
     |      - list : ``categories[i]`` holds the categories expected in the ith
     |        column. The passed categories should not mix strings and numeric
     |        values within a single feature, and should be sorted in case of
     |        numeric values.
     |  
     |      The used categories can be found in the ``categories_`` attribute.
     |  
     |      .. versionadded:: 0.20
     |  
     |  drop : {'first', 'if_binary'} or a array-like of shape (n_features,),             default=None
     |      Specifies a methodology to use to drop one of the categories per
     |      feature. This is useful in situations where perfectly collinear
     |      features cause problems, such as when feeding the resulting data
     |      into a neural network or an unregularized regression.
     |  
     |      However, dropping one category breaks the symmetry of the original
     |      representation and can therefore induce a bias in downstream models,
     |      for instance for penalized linear classification or regression models.
     |  
     |      - None : retain all features (the default).
     |      - 'first' : drop the first category in each feature. If only one
     |        category is present, the feature will be dropped entirely.
     |      - 'if_binary' : drop the first category in each feature with two
     |        categories. Features with 1 or more than 2 categories are
     |        left intact.
     |      - array : ``drop[i]`` is the category in feature ``X[:, i]`` that
     |        should be dropped.
     |  
     |  sparse : bool, default=True
     |      Will return sparse matrix if set True else will return an array.
     |  
     |  dtype : number type, default=np.float
     |      Desired dtype of output.
     |  
     |  handle_unknown : {'error', 'ignore'}, default='error'
     |      Whether to raise an error or ignore if an unknown categorical feature
     |      is present during transform (default is to raise). When this parameter
     |      is set to 'ignore' and an unknown category is encountered during
     |      transform, the resulting one-hot encoded columns for this feature
     |      will be all zeros. In the inverse transform, an unknown category
     |      will be denoted as None.
     |  
     |  Attributes
     |  ----------
     |  categories_ : list of arrays
     |      The categories of each feature determined during fitting
     |      (in order of the features in X and corresponding with the output
     |      of ``transform``). This includes the category specified in ``drop``
     |      (if any).
     |  
     |  drop_idx_ : array of shape (n_features,)
     |      - ``drop_idx_[i]`` is the index in ``categories_[i]`` of the category
     |        to be dropped for each feature.
     |      - ``drop_idx_[i] = None`` if no category is to be dropped from the
     |        feature with index ``i``, e.g. when `drop='if_binary'` and the
     |        feature isn't binary.
     |      - ``drop_idx_ = None`` if all the transformed features will be
     |        retained.
     |  
     |  See Also
     |  --------
     |  sklearn.preprocessing.OrdinalEncoder : Performs an ordinal (integer)
     |    encoding of the categorical features.
     |  sklearn.feature_extraction.DictVectorizer : Performs a one-hot encoding of
     |    dictionary items (also handles string-valued features).
     |  sklearn.feature_extraction.FeatureHasher : Performs an approximate one-hot
     |    encoding of dictionary items or strings.
     |  sklearn.preprocessing.LabelBinarizer : Binarizes labels in a one-vs-all
     |    fashion.
     |  sklearn.preprocessing.MultiLabelBinarizer : Transforms between iterable of
     |    iterables and a multilabel format, e.g. a (samples x classes) binary
     |    matrix indicating the presence of a class label.
     |  
     |  Examples
     |  --------
     |  Given a dataset with two features, we let the encoder find the unique
     |  values per feature and transform the data to a binary one-hot encoding.
     |  
     |  >>> from sklearn.preprocessing import OneHotEncoder
     |  
     |  One can discard categories not seen during `fit`:
     |  
     |  >>> enc = OneHotEncoder(handle_unknown='ignore')
     |  >>> X = [['Male', 1], ['Female', 3], ['Female', 2]]
     |  >>> enc.fit(X)
     |  OneHotEncoder(handle_unknown='ignore')
     |  >>> enc.categories_
     |  [array(['Female', 'Male'], dtype=object), array([1, 2, 3], dtype=object)]
     |  >>> enc.transform([['Female', 1], ['Male', 4]]).toarray()
     |  array([[1., 0., 1., 0., 0.],
     |         [0., 1., 0., 0., 0.]])
     |  >>> enc.inverse_transform([[0, 1, 1, 0, 0], [0, 0, 0, 1, 0]])
     |  array([['Male', 1],
     |         [None, 2]], dtype=object)
     |  >>> enc.get_feature_names(['gender', 'group'])
     |  array(['gender_Female', 'gender_Male', 'group_1', 'group_2', 'group_3'],
     |    dtype=object)
     |  
     |  One can always drop the first column for each feature:
     |  
     |  >>> drop_enc = OneHotEncoder(drop='first').fit(X)
     |  >>> drop_enc.categories_
     |  [array(['Female', 'Male'], dtype=object), array([1, 2, 3], dtype=object)]
     |  >>> drop_enc.transform([['Female', 1], ['Male', 2]]).toarray()
     |  array([[0., 0., 0.],
     |         [1., 1., 0.]])
     |  
     |  Or drop a column for feature only having 2 categories:
     |  
     |  >>> drop_binary_enc = OneHotEncoder(drop='if_binary').fit(X)
     |  >>> drop_binary_enc.transform([['Female', 1], ['Male', 2]]).toarray()
     |  array([[0., 1., 0., 0.],
     |         [1., 0., 1., 0.]])
     |  
     |  Method resolution order:
     |      OurOneHotEncoder
     |      sklearn.preprocessing._encoders.OneHotEncoder
     |      sklearn.preprocessing._encoders._BaseEncoder
     |      sklearn.base.TransformerMixin
     |      sklearn.base.BaseEstimator
     |      builtins.object
     |  
     |  Methods inherited from sklearn.preprocessing._encoders.OneHotEncoder:
     |  
     |  __init__(self, *, categories='auto', drop=None, sparse=True, dtype=<class 'numpy.float64'>, handle_unknown='error')
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  fit(self, X, y=None)
     |      Fit OneHotEncoder to X.
     |      
     |      Parameters
     |      ----------
     |      X : array-like, shape [n_samples, n_features]
     |          The data to determine the categories of each feature.
     |      
     |      y : None
     |          Ignored. This parameter exists only for compatibility with
     |          :class:`sklearn.pipeline.Pipeline`.
     |      
     |      Returns
     |      -------
     |      self
     |  
     |  fit_transform(self, X, y=None)
     |      Fit OneHotEncoder to X, then transform X.
     |      
     |      Equivalent to fit(X).transform(X) but more convenient.
     |      
     |      Parameters
     |      ----------
     |      X : array-like, shape [n_samples, n_features]
     |          The data to encode.
     |      
     |      y : None
     |          Ignored. This parameter exists only for compatibility with
     |          :class:`sklearn.pipeline.Pipeline`.
     |      
     |      Returns
     |      -------
     |      X_out : sparse matrix if sparse=True else a 2-d array
     |          Transformed input.
     |  
     |  get_feature_names(self, input_features=None)
     |      Return feature names for output features.
     |      
     |      Parameters
     |      ----------
     |      input_features : list of str of shape (n_features,)
     |          String names for input features if available. By default,
     |          "x0", "x1", ... "xn_features" is used.
     |      
     |      Returns
     |      -------
     |      output_feature_names : ndarray of shape (n_output_features,)
     |          Array of feature names.
     |  
     |  inverse_transform(self, X)
     |      Convert the data back to the original representation.
     |      
     |      In case unknown categories are encountered (all zeros in the
     |      one-hot encoding), ``None`` is used to represent this category.
     |      
     |      Parameters
     |      ----------
     |      X : array-like or sparse matrix, shape [n_samples, n_encoded_features]
     |          The transformed data.
     |      
     |      Returns
     |      -------
     |      X_tr : array-like, shape [n_samples, n_features]
     |          Inverse transformed array.
     |  
     |  transform(self, X)
     |      Transform X using one-hot encoding.
     |      
     |      Parameters
     |      ----------
     |      X : array-like, shape [n_samples, n_features]
     |          The data to encode.
     |      
     |      Returns
     |      -------
     |      X_out : sparse matrix if sparse=True else a 2-d array
     |          Transformed input.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors inherited from sklearn.base.TransformerMixin:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
     |  
     |  ----------------------------------------------------------------------
     |  Methods inherited from sklearn.base.BaseEstimator:
     |  
     |  __getstate__(self)
     |  
     |  __repr__(self, N_CHAR_MAX=700)
     |      Return repr(self).
     |  
     |  __setstate__(self, state)
     |  
     |  get_params(self, deep=True)
     |      Get parameters for this estimator.
     |      
     |      Parameters
     |      ----------
     |      deep : bool, default=True
     |          If True, will return the parameters for this estimator and
     |          contained subobjects that are estimators.
     |      
     |      Returns
     |      -------
     |      params : mapping of string to any
     |          Parameter names mapped to their values.
     |  
     |  set_params(self, **params)
     |      Set the parameters of this estimator.
     |      
     |      The method works on simple estimators as well as on nested objects
     |      (such as pipelines). The latter have parameters of the form
     |      ``<component>__<parameter>`` so that it's possible to update each
     |      component of a nested object.
     |      
     |      Parameters
     |      ----------
     |      **params : dict
     |          Estimator parameters.
     |      
     |      Returns
     |      -------
     |      self : object
     |          Estimator instance.
    


### Add a method to transform the data as a dataframe.


```python

def transform_as_df(self,X,orig_features):
    data_ohe = self.transform(X)
    df_ohe = pd.DataFrame(data_ohe,
                        columns = encoder.get_feature_names(orig_features))
    return df_ohe
    
OurOneHotEncoder.transform_as_df = transform_as_df

```


```python
cols_to_encode = ['Release Type']

encoder = OurOneHotEncoder(sparse=False).fit(df[cols_to_encode])
data_ohe = encoder.transform_as_df(df[cols_to_encode],cols_to_encode)
data_ohe
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Release Type_Discharged - Expiration of Sentence</th>
      <th>Release Type_Discharged – End of Sentence</th>
      <th>Release Type_Interstate Compact Parole</th>
      <th>Release Type_MISSING</th>
      <th>Release Type_Parole</th>
      <th>Release Type_Parole Granted</th>
      <th>Release Type_Paroled to Detainer - INS</th>
      <th>Release Type_Paroled to Detainer - Iowa</th>
      <th>Release Type_Paroled to Detainer - Out of State</th>
      <th>Release Type_Paroled to Detainer - U.S. Marshall</th>
      <th>Release Type_Paroled w/Immediate Discharge</th>
      <th>Release Type_Released to Special Sentence</th>
      <th>Release Type_Special Sentence</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>26015</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26016</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26017</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26018</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>26019</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>26020 rows × 13 columns</p>
</div>




```python

```
