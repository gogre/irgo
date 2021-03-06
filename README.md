# irgo
AI-enhanced Go playing client/editor

![Image description](https://github.com/gogre/irgobot/blob/master/pyramidirgit.png)

irgo, inspired by the works of Yoshio Ishida and Michael Redmond, combines a (debugged) Swim colour-map [1]  with Influencie shadow map [2] and localised life-and-death analysis by leela-zero [3], to compute groups and Ishida-type graphics [4] of their territory and influence (and, coming soon, moyos; and, maybe later, more). 

1. https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3071677
2. https://github.com/featurecat/Influencie
3. https://github.com/leela-zero/leela-zero
4. https://senseis.xmp.net/?AllAboutThickness

A prototype of irgo is probably most easily programmed in a fork of Influencie embedded in a fork of Lizzie or Sabaki with GTP callouts to leela-zero.

Lizzie tweaks: https://github.com/gogre/irgo/blob/master/musicalstones.md

Programming irgo (or a part of it) would make a practical project for people interested in AI and visualisation.


**Case Study**

Black to play.  This problem, taken from a book written by a famous player, is aimed at a wide variety of skill levels. https://www.youtube.com/watch?v=fctgnSqUVQg 

![Image description](https://github.com/gogre/irgo/blob/master/finallyfinal18.png)

It presents a non-trivial challenge for irgo, to see whether its Ishida-type maps could help guide a reader towards the author's answer.  Like any whole-board problem, its solution requires the solver to grasp the overall picture, to formulate and instantiate a strategic plan, and to find a tactic that serves that plan.  Translated into Go lingua franca, that means seeing what is urgent, what is important, where are the weak spots, and how to find a multipurpose sente move that launches a large-scale attack and prepares the way for black to stabilise her own groups and gain influence as the attack unfolds.

Part 1 Step 1 of irgo's meaning-visualisation process is presented in https://www.youtube.com/watch?v=LcBoZ-qIlEg, where irgo first sketches the influence and territory that would result if white were to play first in the upper left corner, to start to estimate the value of white's territory there, and start to show what influence it would have on nearby black stones in the centre.  Part 1 Step 2 will examine what would happen if black were to play in that area first.

By this means, irgo obtains the value of a black move in the upper left.

Then irgo will do the same thing for all the other groups.  Then, ...to be continued.

==================================================================================================================
                                                               
***Irgo Algorithm***


**ontology**

board = {point}                           
point = [row, column, occupancy]                         
occupancy = [stone|influence]                              
influence = real *in range -1 to +1*                           
*-1..<0 => black; 0 = neutral; >0..1 => white*                           

stone = [colour, status]                                
status = [firstmove,dead|alive]                                    
*a dead stone not yet captured has residual aji*                                                                 
firstmove = black|white                                 
cluster = {point}                          
group ={cluster}                                

***Main Program***                             

**1. Draw colourmap**  
Propagate colour control  
Circumscribe clusters  
Identify and mark obviously dead stones    

**2. Redraw colour-map**  

**3. Compute cluster shadows and groups**  
foreach cluster do  
...pretendboard := board  
...Put pretend stones on colour-controlled points in pretendboard   
...Call Influencie with pretendboard to get cluster shadows                                                     
...foreach point do board.point.influence:= pretendboard.point.influence                                      
...Circumscribe group on board from pretendboard cluster+shadows  
...*a group is a set of clusters whose shadows are contiguous*

**4. Perform group life-and-death analysis**            
*whether group can live (with territory or seki), or opp can kill, etc*                    
 foreach group do                                 
 ...foreach colour do                            
......make boardcopy                            
......fillup rest of zboard with black stones                 
......*in video simulations i manually fillup with alternating colours*                                                                                                                      
......poke 2 eyes in rest of zboard; endofgame:= false                                                                                                                                 
......until endofgame do                            
.........for onmove = [colour, opp(colour)] do                                       
............move:= biggest(leela-zero(zboard, onmove))                                 
............if ko(lastmove) *simulates every ko move being answered outside the group being examined*                  
...............or self-atari(move) *prevent leela filling in her own eyes*                                                                               
............then move(zboard):= pass                                              
............else move(zboard):= move                                                                       
............lastmove:= move                                        
......foreach point in group do                                                            
.........if point.occupant = stone and not(zboard.point.occupant = same(stone))                                                                                                         
.........then board.point.stone:= <onmove,dead>                                                       
......group.territory.firstmove = <colour, count(board.group.internal empty points)>


**5.** Redraw colour map and shadows  

**6.** Draw graphics on board  


**Propagate colour control =**  
Until no new coloured points or links are discovered, repeat 1,2,3                    
...1. a newly-coloured point colours its links *a stone is a coloured point*                       
...2. an uncoloured empty point [edge point],                     
.........at least 3 [2] of whose links are same-coloured                    
.........and none opposite-coloured,                    
......is coloured                  
...3. an uncoloured link connecting 2 points,                     
.........each of which has at least one coloured link                     
.........and no opposite-colored links,                    
......is coloured                                    


**Circumscribe clusters =**                    
clusters.numberof := 0                                                             
foreach point in board do                                                                 
...if every(point.link) is same-colour               
...then point.colour := colour-controlled                                                  
...if every(point.link) is same-colour or neutral                                                        
...then foreach (point.link.otherpoint)                                                             
........do if member(point.link.otherpoint, cluster)                                                                  
...........then add(point, point.link.otherpoint.cluster)                                                    
...........if not(member(point, any cluster)                                                
...........then makenewcluster(point)                                                            
........if member(point, clusterA) and member(point, clusterB)                                                                       
........then unite(clusterA,clusterB)                                            
                                     

**Identify and mark obviously dead stones =**                                                          
...foreach cluster in clusters do                                                            
......identify(cluster.eyes)                                                                           
......if number(cluster.eyes) > 2                                                                      
.........or size(cluster.eyes) > 3                                                                
.........and shape(cluster.eye) not(in{dead-shapes})                                                                
......then cluster.status := alive                  
......elsif surrounded(cluster, enemies)                
............and foreach enemy in enemies (enemy.lad = alive)                      
......then cluster.status := dead                                                  

**identify(cluster.eyes) =**                                 
foreach point in cluster do                                          
...if colour-controlled(point) and                                        
......not border(point) or stone(point)                                            
...then append(point, cluster.eyes) 


**surrounded (cluster, enemies) =**          
not(forany point in border(cluster)                                                       
...path(friend(point)                                                     
...or path(openspace, point))                                                       


**makenewcluster(point) =**    
..clusters.numberof +:= 1;   
...let newcluster = ({point}, clusters.numberof)   
...paint(board.point, point.newcluster.number, point.colour(point)   


**Circumscribe group on board from pretendboard clusters+shadows =**                                          
iboard := board;                        
foreach cluster in board do                      
...foreach point in cluster do                       
......if point is coloured then iboard.point := pretendstone(colour)                       
...isboard:= Influencie (iboard)                              
...foreach point in board do point.colour:= isboard.point.colour;                            
                       

                        

