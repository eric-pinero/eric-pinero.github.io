# Atomic Warriors
[Live Game](https://eric-pinero.github.io)

### Architecture and Technologies
* JavaScript for game logic
* Canvas for rendering
* Webpack to bundle script and compile JS as needed
* HTML5 and CSS3 for styling and presentation


### Background and Overview
My goal in creating this project is to create modern slice of my favorite parts of 80s and 90s arcade games. The game has a straightforward appearance and controls, making it easy to pick up. It is polished enough that there is a wide range of options created by the few controls. There are two characters, each controlled by a player. Each character can walk forwards, backwards, and can jump. Additionally, each player has a shield that can be held in three positions, top, middle, and low. A player wins by hitting the other player with their shield in an area not protected by a shield, or by jumping on their head. If a held shield contacts the other player's shield, are sent in opposite directions, at a speed corresponding to the speed of the collision. The first player to win three rounds wins the game.

![](./assets/a-war-full.gif)

### Functionality and MVP Features
* Character can move, jump, and move their shield based on keyboard inputs
+ Left A/J Right D/L, Raise Shield W/I, Lower Shield S/K, Jump F/H

```javascript
    move(command){

        switch (command) {
            case "left":
                    this.dx -= 3;
                break;
            case "right":
                    this.dx += 3;
                break;
            case "jump":
                if (this.warriorPos[1] === 461)
                this.dy -= 20;
                break;
            case "up":
                if (this.shieldStance === "low"){
                    this.shieldStance = "middle";
                } else if(this.shieldStance === "middle") {
                    this.shieldStance = "top";
                }
                break;
            case "down":
                if (this.shieldStance === "middle"){
                    this.shieldStance = "low";
                } else if(this.shieldStance === "top") {
                    this.shieldStance = "middle";
                }
                break;
            default:
                break;
        }
    }
```

* Character movements are limited appropriately by the environment (cannot jump on air, fall through floor, walk through walls, etc.)
* A character is eliminated if their body contacts the other character's shield or if the other     character lands on their head
* The game correctly identifies when a player is eliminated, ending the round, attributing the point and starting a new round

```javascript
    collideWith(otherObject, collisionType) {
        if (!otherObject.destroyed && !this.destroyed){
            switch (true) {
                case collisionType === "shieldClash":
                    this.dx = -this.dx;
                    this.dy = -this.dy;
                    break;
                case collisionType === "hitwithShield":
                    this.destroyWarrior();
                    otherObject.winner = true;
                    break;
                case collisionType === "warriorClash" && this.dy > 0:
                    otherObject.destroyWarrior();
                    this.winner = true;
                    break;
                case collisionType === "warriorClash":
                    this.dx = 0;
                    break;
                default:
                    break;
            }
        }
    }

    destroyWarrior(){
        this.destroyed = true;
    }
}    
```

* The game tracks each player's wins, ending the match when one player hits three wins
* Distinct color for the each character and their shield
* Menu allows players to pause/play game, restart match, and control sound
+ Spacebar pauses/plays and M mute/unmutes sound

### Implementation
* 6/18/19 - Skeleton of character movement is completed
* 6/19/19 - Characters are able to eliminate one another and second player is added
* 6/20/19 - and the system for winning a match is fully implemented
* 6/21/19 - Stage design complete and round and match system finalized

### Future Goals
* Scrolling backstory loop while game is waiting for match to start
* More stages
* Choice of character color
* Shields can be thrown and regenerate appropriately
* AI added
