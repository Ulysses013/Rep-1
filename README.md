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

### Constatnts Creation

```
import pygame as pygame

display_width = 900
display_height = 700

background_color = (34,139,34)
grey = (220,220,220)
black = (0,0,0)
green = (0, 200, 0)
red = (255,0,0)
light_slat = (119,136,153)
dark_slat = (47, 79, 79)
dark_red = (255, 0, 0)
pygame.init()
font = pygame.font.SysFont("Arial", 20)
textfont = pygame.font.SysFont('Comic Sans MS', 35)
game_end = pygame.font.SysFont('dejavusans', 100)
blackjack = pygame.font.SysFont('roboto', 70)


SUITS = ['C', 'S', 'H', 'D']
RANKS = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']

CARD_SIZE = (72, 96)
CARD_CENTER = (36, 48)
CARD_BACK_SIZE = (72, 96)
CARD_BACK_CENTER = (36, 48)
```

### Hand Calculations

```
from blackjack_deck import *

deck = Deck()
deck.shuffle()

player = Hand()
dealer = Hand()

for i in range(2):
    player.add_card(deck.deal())
    dealer.add_card(deck.deal())

print(player.cards)
print(dealer.cards)


player.add_card(deck.deal())

print(player.cards)

player.calc_hand()
dealer.calc_hand()
```

### Initialization

```
import pygame as pygame
from blackjack_deck import *
from constants import *
import sys
import time
pygame.init()

clock = pygame.time.Clock()

gameDisplay = pygame.display.set_mode((display_width, display_height))

pygame.display.set_caption('BlackJack')
gameDisplay.fill(background_color)
pygame.draw.rect(gameDisplay, grey, pygame.Rect(0, 0, 250, 700))

###text object render
def text_objects(text, font):
    textSurface = font.render(text, True, black)
    return textSurface, textSurface.get_rect()

def end_text_objects(text, font, color):
    textSurface = font.render(text, True, color)
    return textSurface, textSurface.get_rect()

play_blackjack = Play()

running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        button("Deal", 30, 100, 150, 50, light_slat, dark_slat, play_blackjack.deal)
        button("Hit", 30, 200, 150, 50, light_slat, dark_slat, play_blackjack.hit)
        button("Stand", 30, 300, 150, 50, light_slat, dark_slat, play_blackjack.stand)
        button("EXIT", 30, 500, 150, 50, light_slat, dark_red, play_blackjack.exit)
    
    pygame.display.flip()
```


## Features I would like to Implement:

* Implement a virtual wallet or balance for the player to track their money.
* Allow the player to place bets and adjust the balance after each game.
* Display the player's statistics and results for each game.

## Requirements Engineering

### Here are the Requirements (Also [click here](https://trello.com/invite/b/GzDiRVdF/ATTI030e1756a1fa6bd0dd8d32468b8bacbd9AB3D320/virtual-casino) for my Trello Board) :

#### Requirement 1: Game Initialization
- **Description:** The game should initialize with a standard deck of cards and allow players to start a new round.

#### Requirement 2: Player Actions
- **Description:** Players should be able to make actions such as placing bets, hitting, and standing during their turn.

#### Requirement 3: Dealer Logic
- **Description:** Implement the logic for the dealer's turn, including hitting until a certain point and revealing cards at the end.

#### Requirement 4: Score Calculation
- **Description:** The game should accurately calculate and display the scores for both players and the dealer.

#### Requirement 5: Game Outcome
- **Description:** Determine and display the outcome of each round, including wins, losses, or ties.

#### Requirement 6: User Interface
- **Description:** Design a simple console-based user interface for a smooth gaming experience.

#### Requirement 7: Game State Persistence
- **Description:** Allow players to save and load game states for a continuous gaming experience.

#### Requirement 8: Error Handling
- **Description:** Implement error handling for invalid inputs and edge cases during the game.

## Build Management:

### For my Blackjack game I am using 'setuptools' for packaging and distribution, and I am planning to use 'pytest' for testing, and integrating with a CICD service like Travis CI.

### Setuptools for packaging

#### 'setup.py'

```
from setuptools import setup, find_packages

setup(
    name='blackjack_game',
    version='0.1',
    packages=find_packages(),
    install_requires=[
        # requirementsTBD
    ],
)
```


