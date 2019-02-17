# irgobot
AI-enhanced Go playing client/editor

![Image description](https://github.com/gogre/irgobot/blob/master/pyramidirgit.png)

irgobot, inspired by the works of Yoshio Ishida and Michael Redmond, combines a (debugged) Swim colour-map [1]  with Influencie shadow map [2] and localised life-and-death analysis by leela-zero [3], to compute Ishida-type graphics [4] of group strength, territory, influence and moyos. 

1. https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3071677
2. https://github.com/featurecat/Influencie
3. https://github.com/leela-zero/leela-zero
4. https://senseis.xmp.net/?AllAboutThickness

A prototype of irgobot is probably most easily programmed in a fork of Influencie embedded in a fork of Lizzie or Sabaki with GTP callouts to leela-zero.

Programming irgobot (or a part of it) would make a doable computer science project for people interested in AI and visualisation.

**Examples of Use**

1. [Visualising Influence and Territory](https://youtu.be/pwxiBqykHGc)
2. Inducing Shape Flaws (tba)
3. tba
4. 

#Algorithm

**1. Draw colourmap**  
Propagate colour control  
Circumscribe clusters and eyes  
Identify and mark obviously dead stones    

**2. Redraw colour-map**  

**3. Compute shadows**  
Foreach cluster do  
...Make board copy  
...Put pretend stones on colour-controlled points in board copy  
...call Influencie with board copy to get cluster shadows  

**4. Perform local life-and-death analysis**  
 Foreach cluster do  
...Circumscribe cluster shadow  
...Make boardcopy  
...Fillup restof(boardcopy) with live groups with no legal moves  
...call leela-zero  
...Identify and mark locally dead stones in cluster   

**5.** Redraw colour map and shadows  

**6.** Draw graphics on board  


**Propagate colour control =**  
Until no new coloured points or links are discovered, repeat123                    
...1. a newly-coloured point colours its links;                       
...2. an uncoloured empty point [edge point],                     
.........at least 3 [2] of whose links are same-coloured                    
.........and none opposite-coloured,                    
......is coloured;                  
...3. an uncoloured link connecting 2 uncoloured points,                     
.........each of which has at least one coloured link                     
.........and no opposite-colored links,                    
......is coloured;                 
if a link becomes coloured by both colours,                       
...its colour is neutralised.                     


**Circumscribe clusters and eyes =**                    
clusters.numberof := 0;                 
for point in b do                       
...if all-links(point) are same-colour or neutral then                    
......for each coloured-link(point) do                 
......if member(otherpoint(link, point), cluster)                  
.........# ie the point at the other end of the link                   
......then add(point, cluster)                 
......else makenewcluster(point)  

**Identify and mark obviously dead stones =**                
...foreach cluster in clusters do               
......identify(cluster.eyes);                
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

**Compute and circumscribe cluster shadows =**                        
iboard := board;                        
foreach cluster in board do                      
...foreach point in cluster do                       
......if point.coloured then iboard.point := pretendstone (colour);                       
...isboard:= Influencie (iboard);                              
...foreach point in board do point.colour:= isboard.point.shadow;                            
...foreach cluster in board do circumscribe (cluster and its shadow)                         

**Perform obvious life-and-death analysis =**                            
foreach cluster in board do                         
...foreach point in cluster and its shadow do                         
......zboard.point:= board.point;                      
......fillup rest of zboard with black stones;                         
...poke 2 eyes in rest of zboard;                       
...until endofgame do                                              
......if null(intersection(leela-zero(zboard), cluster and its shadow)) then pass(zboard)                    
......else makebestmove (zboard)                    
...foreach point in cluster do                       
......if board.point.occupant = enemystone and not(zboard.point.occupant = enemystone)                          
......then board.point.occupant.cluster.status:= dead
