# ACG Rules Spec v1

This document is the authoritative gameplay rules specification for ACG Version 1. It is intended to be specific enough to implement directly in code.

## 1. Match Structure

1. A match supports 2 to 6 players.
2. Supported formats may include 1v1, free-for-all, and teams.
3. Each player starts with:
   - 25 LP
   - a shuffled legal deck
   - 0 energy unless a setup rule or card effect says otherwise
4. Each player draws 5 cards at game start.
5. A player loses immediately when their LP reaches 0 or less.
6. A player also loses immediately if they must draw from an empty deck, unless a card they control explicitly prevents or replaces that loss.
7. The game ends when only one player remains, or one team remains in team play.

## 2. Turn Order and Turn Structure

1. Turn order proceeds clockwise from the starting player.
2. A full turn consists of these phases in order:
   - Start Phase
   - Draw Phase
   - Main Phase
   - Attack Phase
   - End Phase
3. After End Phase fully resolves, turn passes to the next active player.

## 3. Start Phase

1. Resolve all effects that say:
   - at start of turn
   - at the beginning of turn
   - during your turn start
2. Continuous effects already in play remain active unless removed.
3. No normal card draw occurs during this phase.

## 4. Draw Phase

1. The active player draws 1 card.
2. If a card effect modifies draw count or skips draw, apply that rule here.
3. If the player is required to draw and their deck is empty, that player loses immediately unless a controlled effect says otherwise.

## 5. Main Phase

1. The active player may play as many legal cards as they can afford and legally resolve.
2. To play a card, all of the following must be true:
   - timing is legal
   - energy cost can be fully paid
   - required target exists if the card requires a target
   - required slot or zone destination is available if the card enters play
   - all card-specific conditions are satisfied
3. Unit cards enter the unit field if a unit slot is available.
4. Continuous Action cards enter the action field if an action slot is available.
5. Non-continuous Action cards resolve, then move to void.
6. Energy cards resolve as Energy cards and do not remain in play.
7. Energy cards can never be continuous.
8. Only Action cards may use the infinity marker to indicate continuous behavior.
9. Lightning cards may also be playable outside Main Phase if a legal Lightning timing window exists.

## 6. Attack Phase

1. Only the active player may declare attacks.
2. By default, units may attack only during their controller's Attack Phase.
3. A unit summoned this turn cannot attack unless it has SuperSpeed.
4. The attacking player chooses which eligible units attack.
5. Each attacking unit chooses one legal target.
6. A legal target may be:
   - a player, if card rules allow that player to be attacked
   - a unit with the target attribute
7. Units without the target attribute cannot be attacked directly unless a specific card effect overrides this rule.
8. If a unit says it can attack any player, the controlling player chooses any legal player target.

## 7. Blocking

1. When a player or their eligible targetable unit is attacked, the defending player may declare blockers if able.
2. Blocking is optional unless a card says must block if able.
3. A blocking unit must be in play and otherwise eligible to block.
4. Multiple blockers may block the same attacker.
5. A single blocker cannot block multiple attackers unless a card explicitly allows it.
6. If no blocker is declared, the attack remains unblocked.

## 8. Combat Resolution

1. Combat resolves after blockers are declared.
2. If an attack is unblocked and targets a player, that player loses LP equal to the attacker's current ATKP unless modified by effects.
3. If combat occurs between units, each involved unit deals damage according to current ATKP and active effects.
4. If multiple blockers are assigned to one attacker, all combat damage is resolved during the same combat event.
5. A unit is destroyed if its HP is reduced to 0 or less.
6. Destroyed units are moved to void after combat resolution completes, unless a card effect replaces that movement.
7. If multiple units are destroyed in the same combat event, they are all destroyed in that event before post-combat triggers resolve.

## 9. End Phase

1. Resolve all effects that say:
   - at end of turn
   - until end of turn
   - during end phase
2. Temporary effects with duration until end of turn expire here.
3. Lightning windows remain open during End Phase before cleanup.
4. After all End Phase effects finish, turn passes to the next player.

## 10. Energy Rules

1. Each player has an energy pool.
2. Energy carries over from turn to turn.
3. Default maximum energy is 10 unless a card explicitly changes that limit.
4. If a player would gain energy above 10, excess energy is lost unless a card says otherwise.
5. Energy may be gained, spent, removed, or modified by card effects.
6. A card's energy cost must be paid in full before that card resolves.
7. If a card has multiple legal play modes, the chosen mode determines whether energy is gained, spent, or ignored.

## 11. Field and Zone Rules

Each player has these zones:
- deck
- hand
- unit field
- action field
- void

Zone limits and legality:
1. Unit field maximum is 6 units.
2. Action field maximum is 6 action cards.
3. If a player has 6 continuous action cards in play, they cannot play more Action cards that require action field placement.
4. If a player has 6 units and 6 action cards in play, that player cannot play additional cards that require field placement.
5. A player with a full board may still:
   - play Energy cards
   - activate legal card effects from hand

## 12. Void Rules

1. Destroyed units go to void unless replaced by a card effect.
2. Resolved non-continuous Action cards go to void.
3. Discarded cards go to void unless a card effect says otherwise.
4. Effects may move cards from void to deck or other legal zones if card text allows it.

## 13. Deck Construction Rules

1. Legal deck size is minimum 40 cards and maximum 50 cards.
2. Default copy limit is 1 per card.
3. A card may override its own copy limit in card text or schema.
4. Cards from all animated shows may be mixed freely in the same deck.
5. Deck legality should be validated before match start.

## 14. Continuous Effects

1. Continuous effects only exist on Action cards unless a future core rule changes this.
2. A continuous effect remains active while its source card remains legally in play.
3. If a continuous Action card leaves play, its continuous effect ends immediately unless the card says otherwise.

## 15. Lightning Timing Windows

Lightning cards may be played only during these legal timing windows:
1. after a card is played or an effect resolves
2. before attacks are declared in the Attack Phase
3. after blockers are declared
4. during End Phase before cleanup

If multiple Lightning plays occur in sequence, each resolved card or effect may create another legal post-resolution Lightning window.

## 16. Trigger Ordering

1. When multiple triggers become ready at the same timing point, they are queued together.
2. Simultaneous triggers resolve in reverse play order, newest to oldest.
3. If a future card explicitly overrides trigger order, that card text takes precedence.

## 17. Established Named Rule Terms

### SuperSpeed
A unit with SuperSpeed may attack on the same turn it is summoned.

## 18. Rule Priority

If rules conflict, use this priority order:
1. Explicit card text
2. This Rules Spec v1
3. Default engine behavior
