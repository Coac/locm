# Coac - Legends of Code and Magic 
My bot for [Strategy Card Game AI Competition](https://github.com/acatai/Strategy-Card-Game-AI-Competition) CEC 2019

## Building
```$xslt
mkdir build
cd build
cmake ..
make
```

## Configuration
If running on a slow or already CPU loaded computer, the bot may timeout. 
You can reduce `TIME_LIMIT`.

## Strategy

### Search
My AI bot is based on a minimax algorithm with alpha-beta pruning at a fixed depth.
Since we can do multiple actions per player turn, the minimax algorithm is repeated until there are no more available actions.

For one turn, we compute the best actions using minimax. 
Then among these, we keep only the first action. 
The action is executed in the simulation environment.
We repeat the minimax algorithm.

For the opponent simulation, only attack players and attack monsters are considered.

The bot actions should be determinist if it has time to compute the multiple minimaxes. 
In most of the case, due to the small search depth, the bot gets enough time. 
It uses a `TIME_LIMIT` in case of a very very wide tree (very rare) to securely outputs actions before time out.

### Draft
To choose what card to pick between the three proposed, my bot will pick the one with the highest rank.
The bot uses 2 fixed ranking of [cards](https://jakubkowalski.tech/Projects/LOCM/cardlist.html) available in the game. 
These ranking will be chosen at the start based on the player position if the bot plays first or not.


### Evaluation function

The evaluation of the state of the game uses the following features: 

    card.defense
    card.attack
    card.ward
    card.lethal
    card.guard
    card.passed
    player.cardsToDraw
    player.hp

Each feature has an associated weight on it that was first hand-tuned. A genetic algorithm has then been used
to search for the best weights.

