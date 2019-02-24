# irgobot
AI-enhanced Go playing client/editor

![Image description](https://github.com/gogre/irgobot/blob/master/pyramidirgit.png)

irgobot, inspired by the works of Yoshio Ishida and Michael Redmond, combines a (debugged) Swim colour-map [1]  with Influencie shadow map [2] and localised life-and-death analysis by leela-zero [3], to compute groups and Ishida-type graphics [4] of their territory and influence (and, coming soon, moyos; and, maybe later, more). 

1. https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3071677
2. https://github.com/featurecat/Influencie
3. https://github.com/leela-zero/leela-zero
4. https://senseis.xmp.net/?AllAboutThickness

A prototype of irgobot is probably most easily programmed in a fork of Influencie embedded in a fork of Lizzie or Sabaki with GTP callouts to leela-zero.

Programming irgobot (or a part of it) would make a practical project for people interested in AI and visualisation.


**Case Study**

Black to play.  This problem, taken from a book written by a famous player, is aimed at a wide variety of skill levels.  Like any whole-board problem, its solution requires the solver to grasp the overall picture, to formulate and instantiate a strategic plan, and to find a tactic that serves that plan.  Translated into Go lingua franca, that means seeing what is urgent, what is important, where are the weak spots, and how to find a multipurpose sente move that launches a large-scale attack and prepares the way for black to stabilise her own groups and gain influence as the attack unfolds.

![Image description](https://github.com/gogre/irgobot/blob/master/finallyfinal18.png)

The problem (slightly bastardised because i had unwittingly added an extra black stone at F12) attracted a variety of responses from readers of Life in 19x19, ranging in strength from mid-kyu to mid-dan. https://www.lifein19x19.com/viewtopic.php?p=241486#p241486

All responses had their merits, but none quite found the author's own answer, which brilliantly illustrates several key principles of Go technique.

So it presents a non-trivial challenge for irgobot, to see whether its Ishida-type maps would be helpful in guiding a reader towards the author's answer.

Part 1 Step 1 of irgobot's meaning-visualisation process is presented in https://www.youtube.com/watch?v=LcBoZ-qIlEg, where irgobot first sketches the influence and territory that would result if white were to play first in the upper left corner, to start to estimate the value of white's territory there, and start to show what influence it would have on nearby black stones in the centre.  Part 1 Step 2 will examine what would happen if black were to play in that area first.

By this means, irgobot obtains the value of a black move in the upper left.

Then irgobot will do the same thing for all the other groups.  Then, ...to be continued.


**Algorithm**

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
...Circumscribe group on board from pretendboard cluster+shadows  
...*a group is a set of clusters whose shadows are contiguous*

**4. Perform group life-and-death analysis**            
*whether black can live (with territory or seki), white can kill, etc*                    
 foreach group do                               
...make boardcopy                            
...fillup rest of zboard with black stones                 
*easy for a machine; in video simulations i manually fillup with alternating black and white to save time*                                
...poke 2 eyes in rest of zboard; endofgame:= false                                                                      
...for onmove = {black, white} do                                                                              
......until endofgame do                                                                                       
.........call leela-zero(zboard, onmove) => bestmoves,endofgame                
.........if null(intersection(bestmoves, group)) then pass(zboard)                     
.........else makebestmove in group on zboard                             
......foreach point in group do                               
.........if point.occupant = enemystone and not(zboard.point.occupant = enemystone)                                                        
.........then board.point.stone:= <onmove,dead>            


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
                       

                        

