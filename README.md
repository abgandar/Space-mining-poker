# Space Mining Poker

## Introduction

This game models (in a rough way) the incentive/reward structure of asteroid mining and turns it into a game. The game theory 
aspects of the game are loosely modelled after poker.

The goal for players is to model the game based on the rules below, and code competitive strategies to return a profit over time. Periodically held automated tournaments against other players can be used to build up a database of previous game data with which to improve strategies further.

## Rules

The game is played with N players. Each player has M money ($) and T tech. The M attribute is public knowledge, the T attribute is private.
Each player starts with M=$1000 and T=0.

A game is made up of repeated rounds, over which each player tries to maximise their bankroll M. Each individual round of the game is made up of the following phases:

### 0) Discovery phase:
In this phase, the asteroid is found and analysed. The game assigns a fixed, positive reward of $P0 (from some distribution DP) and an unknown, positive reward $Pu (same distribution) to the asteroid. These represent a guaranteed return from mining the asteroid, which can already be determined from observation at this time, and an additional potential gain that is not yet known and only discovered when actually mining the asteroid.
The reward $P0 is made available as public knowledge to all players, while $Pu remains secret.

### 1) Business phase:
In this phase, each player has to invest $10 in order to develop their technology. In return, they receive a random amount of tech (drawn from some distribution DT), representing the return of their investment in research and development during this round. This models investments made into developing launch vehicles, mining equipment, and processing know-how as well as securing the required regulatory approval prior to launch.

Players who cannot afford to pay go bankrupt and are removed from the game.

### 2) Auction phase:
In this phase, an auction is held where players can compete in a space race for the best new tech developed by the brightest talent on Earth. This is when players start making crucial decisions.

The rules of the auction are as follows:
1. The game fixes an unknown, positive amount of tech (which will be drawn from DT).
2. Each player submits a blind bid for the new tech (may be $0) and additionally may announce to prepare for launch (see below).
3. The highest bidder is awarded the tech (adding it to their running total T) and their bid is subtracted from their bankroll M. If they do not have sufficient funds, the bidder goes bankrupt and is removed from the game, and no tech is awarded.
In the case of a tie, the bid is awarded to all tied bidders (each buying the tech at full price). The tech awarded, the winning player(s) and the winning bid are all announced at the point of the award.

Repeated auctions are held until one or more players announce that they will launch. Once this happens, the game moves on to the mission phase.

### 3) Mission phase:
This phase corresponds to the launch of a mission to mine the asteroid.
Once a player announces a launch, the race is on. Each player has the option to either compete in the launch or to sit it out. A player who sits out the launch preserves their tech total T for the next round and takes no further part in this round.
A player who joins the launch has a chance of mining the asteroid.
Launching players spend all their tech on the launch.
A winner is chosen for the round at random, where the probability of a win for each player is

p(player n wins) = Tn / (sum(Ti) + F).

F plays the role of allowing the possibility of no winner where the asteroid is not successfully mined. F is an unknown, but modelled to reduce over repeated rounds to simulate growing experience.

The payoff to the winning player for mining the asteroid is

payoff = P0 + Pu + Pt.

Pt is an extraction efficiency reward which depends on the average tech spent by all players over past 10 rounds.

After mining is completed, the payoff, and winning player are made public information. This concludes the round and the game continues with another round starting at discovery phase.


## How to play
At the moment, it is required to modify the player names and strategies in the `players_rc.py` file.
Network players use their network name/IP address and port as a string instead of a strategy, and will need
to separately run `strategy_server.py` with the port to use (or an offset to the base port 49000) as the
command line argument on their machine. Network players can set their strategy by editing
`strategy_server.py` accordingly.


## Possible extensions:
- Borrowing money at interest (payable per turn).
- Limit number of bidding rounds but reveal more information about asteroid in each round (similar to flop & river in poker).
