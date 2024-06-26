---
  layout: post
  title: "Eternity II Puzzle Solver"
  categories: projects
  alias: experiments/eternity-ii-puzzle-solver/
  tags: code
---

[Click to read about my new better solver!](/projects/eternity-2-solver/)

Wikipedia article about the puzzle: [wikipedia.org/wiki/Eternity_II_puzzle](http://en.wikipedia.org/wiki/Eternity_II_puzzle)

Not to discourage you but please consider:

 * There is no prize money left to win.
 * The problem is NP-complete.
 * There is no built in save / resume, if your computer reboots, you have to start all over again.

The built in pattern will place the pieces from the middle piece in a spiral pattern towards the edges. I have solved different "areas" of the puzzle which works but there is no guarantee that it can actually solve the entire thing - and if you change the pattern in any way, the solution might not work. I have double checked all the pieces with another person so they should be correct but again, no guarantee. Whenever the algorithm successfully places a new piece (or finds a new solution with atleast equally many pieces as the record) it will output the solution to the terminal and to a file. Be ware that the code is pretty bad and not optimized at all.

Wed Feb 01 22:42:26 CET 2012 - New Record or record variation!

```text
[ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0]
[ 0(0] [189(2] [180(3] [107(0] [ 94(0] [204(0] [160(0] [ 80(2] [246(1] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0]
[ 0(0] [147(1] [161(1] [127(2] [ 78(2] [202(2] [158(2] [250(0] [222(3] [146(0] [119(2] [154(0] [133(2] [184(3] [ 0(0] [ 0(0]
[ 0(0] [223(3] [230(1] [162(0] [ 79(3] [ 97(1] [175(0] [240(3] [208(3] [138(2] [109(0] [152(2] [187(2] [210(2] [ 0(0] [ 0(0]
[ 0(0] [225(2] [232(1] [143(2] [171(1] [233(2] [150(0] [229(1] [153(0] [176(2] [110(2] [185(1] [128(3] [115(1] [ 0(0] [ 0(0]
[ 0(0] [190(0] [169(3] [122(3] [129(1] [234(2] [137(2] [164(1] [145(2] [188(1] [192(1] [243(3] [211(0] [249(1] [ 0(0] [ 0(0]
[ 0(0] [ 66(3] [ 77(1] [239(1] [209(2] [181(3] [131(3] [103(0] [200(3] [206(1] [255(2] [247(2] [212(0] [254(0] [ 0(0] [ 0(0]
[ 0(0] [144(2] [174(1] [125(3] [126(1] [221(3] [219(1] [ 67(3] [ 95(1] [194(0] [220(3] [118(3] [ 63(3] [ 88(1] [ 0(0] [ 0(0]
[ 0(0] [256(0] [ 98(0] [179(0] [173(3] [ 89(3] [ 83(1] [139(2] [165(1] [148(3] [ 69(0] [ 92(0] [108(2] [182(1] [ 0(0] [ 0(0]
[ 0(0] [251(3] [ 73(2] [172(2] [244(1] [199(3] [114(3] [106(1] [213(1] [ 75(3] [ 62(1] [ 85(2] [215(0] [134(0] [ 0(0] [ 0(0]
[ 0(0] [218(0] [101(3] [132(1] [253(1] [130(0] [228(2] [170(3] [178(1] [227(2] [155(3] [142(1] [191(0] [135(2] [ 0(0] [ 0(0]
[ 0(0] [214(0] [136(2] [197(2] [124(0] [105(2] [151(1] [235(4] [ 90(3] [ 84(1] [157(1] [104(3] [111(1] [252(2] [ 0(0] [ 0(0]
[ 0(0] [216(2] [102(3] [117(1] [120(2] [248(1] [196(4] [ 81(4] [ 68(4] [168(1] [226(1] [123(2] [ 74(3] [ 93(1] [ 0(0] [ 0(0]
[ 0(0] [100(1] [121(2] [238(3] [183(3] [166(4] [198(2] [ 87(2] [ 71(2] [149(4] [237(3] [ 76(4] [163(3] [156(1] [ 0(0] [ 0(0]
[ 0(0] [201(0] [186(2] [116(4] [ 72(4] [140(1] [141(4] [205(4] [ 70(3] [ 64(1] [ 82(1] [ 61(2] [ 99(1] [203(4] [ 0(0] [ 0(0]
[ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0] [ 0(0]
Pieces: 176.
```

The output shows the piece number and rotation so [189(2)] is piece 189 rotated 2 steps to the right.

[About the puzzle on Wikipedia](http://en.wikipedia.org/wiki/Eternity_II_puzzle).

[Code on my GitHub page](https://github.com/AntonFagerberg/Eternity-II-Puzzle-Solver).
