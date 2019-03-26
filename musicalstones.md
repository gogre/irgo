**Musical Stones**

irgo's user display is based on Lizzie, with a few tweaks.  

irgo draws a new colour map and shadow map on the board for every move in a game tree, before navigation, so a review can proceed smoothly without interruptions, except when new variations are added during a session, when maps are redrawn live.

The user can see how maps change from one move to the next by stepping down the game tree, and can compare alternatives by stepping across variations. 

Lizzie steps directly sideways, from the nth move in one variation to the nth move in the next.  irgo instead steps from the nth move in one variation to the 1st move of the next (or previous), because it is felt that is more logical for the user.

As irgo is primarily a review tool, users are expected to spend more time looking at variations than controlling Leela's pondering, especially as search time can be preset by limiting --playouts to as few as a few hundred -  for all but pros, leela's first few discovered moves are usually more than good enough to reveal amateurish mistakes.

irgo uses spacebar to shift from the end of one variation to the end of the next, cyclically, so the user can see the pros and cons of alternatives and cycle repeatedly through the set when so desired.

So irgo uses Insert (or some other keyboard button) to toggle pondering.

irgo's display of leela-generated move choices is a little different from Lizzie's, for the following reasons :-


**1. **                                      

People can remember lots of things,
but there is good evidence,
from George Miller's experiments, https://www.simplypsychology.org/short-term-memory.html
that our perceptions
can only cope with and remember,
a few different things
at the same time,

For this reason, 
irgo only shows leela's top 5 moves,


**2. **                                  

Playing Go is a bit like walking on a tightrope,
especially if you are following a joseki,
which has been worked out 
by many experts over many years,
and only produces a locally even result,
if you follow it to the letter,


One slip and you're off,

In many ways, Go is not so much a game,
of making good moves,
but one of of not making bad ones,

Lizzie and Sabaki both provide a graph of win%,
to help the user see where she slipped up,
but this takes up space on the display screen,
and distracts from the matter at hand:
the board, and the moves,

So irgo displays advantage changes
right on the board,
by colouring moves with their change in win%,
as well as their absolute values,


**3. **                                     

Leela zero is a probabilistic player,
so the more a move has been evaluated,
the more likely its value is to be accurate,

For this reason,
leela zero hones in on certain moves,
and studies their continations much more than others,
 
This is a rational way for a probabilistic player to behave,
for an advantage is an advantage, 
no matter how slight it may be,

leela zero's tendency to go for the safest win
instead of a bigger one,
has given some observers cause to question,
whether probabilistic players are weak at yose,

personally, i don't think they are,
since a win is a win,


**4. **                                          

i don't think it's either necessary or helpful to display
playout count (or node visit count) to a player,

who only wants to know
how good a move is,

but to be on the safe side,
"top" moves can be constrained to only those which have
a large number of node visits,


**5. **                                               

although people can do 
some arithmetic in their heads,
we more often talk about things 
in terms of their qualitative values,
such as good, bad and ugly,


this phenomenon may be related to Miller's theory,
of human short-term memory size limitations,
which affects how we perceive differences,



**6. **

so when it comes to conveying leela's findings to a player,
irgo does not display numbers, 
but just qualitative values,
so the player can see at a glance,
which move is better than which

move values are expressed by two rings
around a bulls-eye of the colour 
of the player on move,

the inner ring expresses absolute win%, 

and its fringe depicts delta win%, 
which is the difference between that move's win%, 
and the win% of that player after her previous move,

irgo's colour scheme follows
the aesthetic code that, 

yellow means bright and cheerful, 
green is good, 
brown is boring, 
and red means danger,

**7. **

classification colours are similar to
the use of A,B,C,D, etc
to grade student work,

except that the whole idea of grading sudents
is an exercise in futility,
because the entire system of
training students to jump through hoops,
is so flawed as to be both inutile to ermployers,
and detrimental to the students themselves

but grading players does make sense in Go,
where there is a single metric of perfomance,
and grading moves on their win probability,
is equally meaningful

**8. **

for example, a brown ring with a green fringe, 
means you are not out of the woods yet, 
but you are going the right way,

and a red ring,  with a green fringe, 
means all is not quite lost, 

but a brown ring, with a red fringe,
means you are headed for trouble,

and a red ring with a red fringe 
means you should give up the game.

**9. **  

irgo's  colours are like
the notes of a two-note musical chord, 

which can be played instead of 
the usual sound of a stone click,
to give you auditory feedback during your game, 
or when you review it, 

stone sounds for either or both players can be toggled

the base note of the chord is 
the player's win% for that move

and its treble note is its delta win% 

two-note chords  have semiotics consistent with 
the emotional significance of sounds:
higher notes are happier than lower ones

the musical scales of the base and treble intervals are chosen 
so that any combination of them sounds harmonious

**examples**
https://youtu.be/zNdK3as5k0A
****
