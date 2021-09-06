# Biquadris

## Introduction:

Biquadris is a two-player latinized version of the game Tetris. It consists of two boards, where each player takes turns to manipulate a block in their respective game boards.
Each player aims to fill one or several rows at a time i.e. to not to leave any spaces. Once an entire row is filled, it disappears from that board. The game ends for the player when its board can’t accommodate any further blocks. Scores depend on the level chosen and numbers of rows eliminated.

## UML:
![ ](https://github.com/anoushkagupta876/biquadris/files/7117185/uml-final.pdf)

## Overview:

The entire program is run by a Controller class called Game.
Game class owns two instances of Board Class :
   - player1
   - player2 </br>
   Board class has a composition relation with Game class.
   
Board class is the Player. Each player has an instance of it’s Game class: theGame

Game has an aggregation relation with the Board class.

Board stores Score, Level, Current Block, Next Block and Special Effects and the Grid for the player.

Cell class is the most fundamental element in our Board. It stores the type.
Grid stores a vector of vectors of Cell. It is also the class responsible for calculating scores and updating highscore as it is directly connected to the number of rows cleared. Whenever a block is added, the cell set’s its type to that blocktype. To check if a row should be cleared, the grid scans every row of cells and if all the cells have a type then it sets the type of the cell to null ( . ) and moves all the down the rows above to fill the gaps. 

A block is simply a tetromino which appears at the top left of each board. 
A block has a configuration where all it’s coordinates are listed (this changes with the left,right etc commands until it’s dropped). These coordinates are then mapped to the grid’s rows and columns and the type of those cells are set to the type of the block. AbstractBlock implements the manipulation of the configuration of these blocks - linear and rotational movements. Rotation of a block is implemented by rotating the block about the vertical axis and then shifting it back to original left origin.

AbstractLevel is responsible for implementations of different levels and deciding the type of each block and whether it has certain features like heavy. 

## Design :

While designing the project, we used several techniques to solve the various design challenges
- Template Method: 
The Template Method in our code answers the question: Which type of block?
It is used by AbstractLevel class to return the type of block that should be created according to the respective level. It is then used by AbstractBlock class to create that type of block with its specific coordinate configuration.

- Factory Pattern:
We have an abstract superclass AbstractLevel which helps us decide the type of the Block. We handle the scripted commands in Level0 and norandom in Level 3 and Level 4 with the help of factory pattern. The different level concrete classes implement their logic. They handle randomness and no-randomness by manipulating the variable blockStream in our superclass.

- Polymorphism:
Most of the AbstractLevel and AbstractBlock classes have inheritance and perform polymorphism with virtual and override functions.

- Compilation Dependencies:
Forward declarations are made for Board and Game to avoid cycles.

- Low Coupling and High Cohesion:
Our code is truly modular i.e. each class is responsible for one specific task. Therefore, cohesion is high. For example, AbstractLevel decides Block type, whereas AbstractBlock focuses on creation and manipulation of that block. The Board class acts as a meditator to pass on information between them.
Our code used minimum header files and no friend classes. Hence, there are minimum module dependencies. Hence, the code has low coupling.

## Resilience to Change:

Our code can be very easily manipulated.

- Addition of Players: Since our Controller class reuses the same code for the player. The game can easily be extended to more than 2 players with addition of new Board instances and updating the function switchplayers().

- Addition of commands: Given our Board class owns an instance of the AbstractBlock class. And all the concrete block classes have a IS-A AbstractBlock relation. An additional command just needs to be added to a member function in AbstractBlock class.(Example: rotate)
Similarly, is true for AbstractLevel class. All the concrete Level classes have an IS-A relation. Therefore, AbstractLevel functions can easily be implemented on them. Therefore, addition of a member function in AbstractLevel class is enough. They can individually be added for each class with the help of polymorphism. 

- Addition of Levels: Given our AbstractLevel is a virtual abstract class, implementation of  each level is defined in its own subclass. Therefore, we can always add more subclasses to increase the number of levels independent of the other concrete level classes.

- Addition of Type of Blocks: Given our AbstractBlock is a virtual abstract class, each type of block is implemented in a different subclass. And these subclasses are independent of each other. Therefore, we can always add more subclasses to increase the number of types of Blocks without disturbing the previous implementation.

## Lessons Developing Software in Teams

One of the most important lessons about developing in teams is communication. Since, there could be innumerable code dependencies in the beginning of the process. It is very important to keep every member updated about any structural changes. 

Our team implemented the basic code together. We also carefully discussed how each function is supposed to perform.Therefore, leaving the implementation and maths to the programmer. 
Furthermore, informing about progress is also a key element. 
In addition to that, proper documentation and good comments can go long to understand each other's code and the logic behind it.

Lastly, the best part about working in teams is that you could always ask for help. Therefore, asking for help, raising questions and discussing solutions is a must.
