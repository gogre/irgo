# GoJaca
AI-enhanced Go client/editor/player interface

GoJaca design [0] combines a revised version [1a] of SWIM commonsense colour-map construction [1; 1b] with Influencie [2] shadow map generation and localised life-and-death analysis by Laizy [3], to compute and display board overlays which show Ishida-type graphics [4] of group strength, territory, influence and moyos. 

A GoJaca prototype is probably most readily programmed in a fork of Influencie embedded in a fork of Lizzie or Sabaki (to reuse their editing and GTP interface code), with GTP callouts to Laizy.

Programming and/or redesigning GoJaca - or parts of it - would make a doable computer science student project, and/or an ongoing hobby for people interested in AI and Go.

[0] tba
[1] https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3071677
[1a] https://www.lifein19x19.com/viewtopic.php?p=241243#p241243
[1b] https://github.com/pnprog/gomap
[2] https://github.com/featurecat/Influencie
[3] https://github.com/leela-zero/leela-zero
[4] https://senseis.xmp.net/?AllAboutThickness
