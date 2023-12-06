# Code Idea Description: Virtual Casino in Python

## Project Overview:

##### Hello! For the semester project, previously I had planned to write code for 3 Casino games : BlackJack, Roulette, Slots Machine and I also had decided to use GUI in the project. In due course of developing this game I have come to realization that the expected length of code for this project is met with just one game, therefore I have decided to code only for a BlackJack Game. I am using 'pygame' library for majority of rendering as I find it convenient.

## Code Overview:

### Blackjack:

#### Game Description: Blackjack is a card game where the player's goal is to beat the dealer by getting a hand value as close to 21 as possible without exceeding it.

#### Coding Process:

* Create a deck of 52 cards and shufle them.
* Implement functions for dealing cards to the player and the dealer.
* Calculate the value of the player's and dealer's hands.
* Implement player decisions (hit or stand) and dealer's strategy.
* Determine the winner and adjust the player's balance accordingly.

### Deck Creation

```
import random
from constants import *

class Deck:
    def __init__(self):
        self.cards = []
        self.build()

    def build(self):
        for value in RANKS:
            for suit in SUITS:
                self.cards.append((value, suit))
  
    def shuffle(self):
        random.shuffle(self.cards)
        

    def deal(self):
        if len(self.cards) > 1:
            return self.cards.pop()
            
class Hand(Deck):
    def __init__(self):
        self.cards = []
        self.card_img = []
        self.value = 0 

    def add_card(self, card):
        self.cards.append(card)

    def calc_hand(self):
        first_card_index = [a_card[0] for a_card in self.cards]
        non_aces = [c for c in first_card_index if c != 'A']
        aces = [c for c in first_card_index if c == 'A']

        for card in non_aces:
            if card in 'JQK':
                self.value += 10
            else:
                self.value += int(card)

        for card in aces:
            if self.value <= 10:
                self.value += 11
            else:
                self.value += 1


    def display_cards(self):
        for card in self.cards:
            cards = "".join((card[0], card[1]))
            if cards not in self.card_img:
                self.card_img.append(cards)
```


## Features I would like to Implement:

* Implement a virtual wallet or balance for the player to track their money.
* Allow the player to place bets and adjust the balance after each game.
* Display the player's statistics and results for each game.


