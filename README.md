# Rubik's Cube Solver

## Files

### rubiks.db

#### Table first_cross

This table contains all the first cross edges configuration (190,080
possibilities) and the shortest combination of moves to (HTM metrics) to solve
it. When many combinations with the same number of moves exist, the one with the
smallest amount of moves in QTM metrics is preferred.

It means that a solution with one half turn and two quarter turns will always be
preferred to a solution with two half turn and one quarter. It also means that a
solution with three half turns will always be preferred to a solution with four
quarter turns.

If more than one solution exists with the same number of moves for both HTM and
QTM metrics, then we choose one of the solution arbitrarily.

##### id column

This column represents the edges configuration for the edges of the first cross.

###### For the top cross

For a given edge, we have two colors. The top color, and the band color. From
this, we can get the configuration represented as a number :

0. If the top color is on the top face, and the band color is on the back face
1. If the top color is on the back face, and the band color is on the top face
2. If the top color is on the top face, and the band color is on the left face
3. If the top color is on the left face, and the band color is on the top face
4. If the top color is on the top face, and the band color is on the right face
5. If the top color is on the right face, and the band color is on the top face
6. If the top color is on the top face, and the band color is on the front face
7. If the top color is on the front face, and the band color is on the top face
8. If the top color is in the left face, and the band color is on the back face
9. If the top color is on the back face, and the band color is on the left face
10. If the top color is on the left face, and the band color is on the front face
11. If the top color is on the front face, and the band color is on the left face
12. If the top color is on the left face, and the band color is on the down face
13. If the top color is on the down face, and the band color is on the left face
14. If the top color is on the front face, and the band color is on the right face
15. If the top color is on the right face, and the band color is on the front face
16. If the top color is on the front face, and the band color is on the down face
17. If the top color is on the down face, and the band color is on the front face
18. If the top color is on the right face, and the band color is on the back face
19. If the top color is on the back face, and the band color is on the right face
20. If the top color is on the right face, and the band color is on the down face
21. If the top color is on the down face, and the band color is on the right face
22. If the top color is on the down face, and the band color is on the back face
23. If the top color is on the back face, and the band color is on the down face

Now, for each edge, you can calculate a configuration. The id is :

```
  [configOfTopLeftEdge]*24^3
+ [configOfTopFrontEdge]*24^2
+ [configOfTopRightEdge]*24
+ [configOfTopBackEdge]
```

Let's see it with two examples.

First, the solved cube :
```
configOfTopLeftEdge = 2
configOfTopFrontEdge = 6
configOfTopRightEdge = 4
configOfTopBackEdge = 0

Now, the configuration is :
2*24^3 + 6*24^2 + 4*24 + 0 = 31200
```

First, the cube after the following moves: `L2-R2-F2-B2-U2-D2` a.k.a. the chessboard cube :

```
configOfTopLeftEdge = 21
configOfTopFrontEdge = 22
configOfTopRightEdge = 13
configOfTopBackEdge = 17

Now, the configuration is :
21*24^3 + 22*24^2 + 13*24 + 17 = 303305
```

###### For the bottom cross

For a given edge, we have two colors. The down color, and the band color. From
this, we can get the configuration represented as a number :

0. If the down color is on the down face, and the band color is on the front face
1. If the down color is on the front face, and the band color is on the down face
2. If the down color is on the down face, and the band color is on the left face
3. If the down color is on the left face, and the band color is on the down face
4. If the down color is on the down face, and the band color is on the right face
5. If the down color is on the right face, and the band color is on the down face
6. If the down color is on the down face, and the band color is on the back face
7. If the down color is on the back face, and the band color is on the down face
8. If the down color is in the left face, and the band color is on the front face
9. If the down color is on the front face, and the band color is on the left face
10. If the down color is on the left face, and the band color is on the back face
11. If the down color is on the back face, and the band color is on the left face
12. If the down color is on the left face, and the band color is on the top face
13. If the down color is on the top face, and the band color is on the left face
14. If the down color is on the back face, and the band color is on the right face
15. If the down color is on the right face, and the band color is on the back face
16. If the down color is on the back face, and the band color is on the top face
17. If the down color is on the top face, and the band color is on the back face
18. If the down color is on the right face, and the band color is on the front face
19. If the down color is on the front face, and the band color is on the right face
20. If the down color is on the right face, and the band color is on the top face
21. If the down color is on the top face, and the band color is on the right face
22. If the down color is on the top face, and the band color is on the front face
23. If the down color is on the front face, and the band color is on the top face

Now, for each edge, you can calculate a configuration. The id is :

```
  [configOfDownLeftEdge]*24^3
+ [configOfDownBackEdge]*24^2
+ [configOfDownRightEdge]*24
+ [configOfDownBackEdge]
```

Let's see it with one example, the "dot in the middle", which is the cube after
the following moves: `U-D'-R-L'-F-B'-U-D'`

```
configOfDownLeftEdge = 10
configOfDownBackEdge = 3
configOfDownRightEdge = 8
configOfDownFrontEdge = 12

Now, the configuration is :
10*24^3 + 3*24^2 + 8*24 + 12 = 140172
```

##### Moves column

This column represents the moves to make in order to solve the first cross.

###### For the top cross

Each move is encoded with the following representation:

1. `L'`
2. `L`
3. `L2`
4. `R'`
5. `R`
6. `R2`
7. `F'`
8. `F`
9. `F2`
10. `B'`
11. `B`
12. `B2`
13. `U'`
14. `U`
15. `U2`
16. `D'`
17. `D`
18. `D2`

Now, let's see how to use the move columns, with the two configurations we saw as example above :

For the solved cube:
`SELECT moves FROM first_cross WHERE id = 31200`

We can see that the value is 0. So we stop here, the optimal solution to move
the top cross is 0 move.

Let's see with the "chessboard" configuration:
`SELECT moves FROM first_cross WHERE id = 303305`

We can see that the value is 435612.

* 435612%19 = 18. So the first move is `D2`. We continue with 435612/19 = 22926
* 22926%19 = 12. So the second move is `B2`. We continue with 22926/19 = 1206
* 1206%19 = 9. So the third move is `F2`. We continue with 1206/19 = 63
* 63%19 = 6. So the fourth move is `R2`. We continue with 63/19 = 3
* 3%19 = 3. So the fifth move is `L2`. As 3/19 = 0, we stop here.

You can try it, after those five moves, the top cross is solved (Of course, the cube is not). This example is interesting for two reasons :

* This solution provides only one solution with the minimum number of moves. We don't
guarantee that this solution is the only one. And in fact it is not. In that case, `R2 - L2 - F2 - B2 - U2` is also a five moves valid solution
* We only care of the edges. So we don't guarantee that the solution provided is the
one that will make you closer to solving the cube.

###### For the bottom cross

Each move is encoded with the following representation:

1. `L'`
2. `L`
3. `L2`
4. `R'`
5. `R`
6. `R2`
7. `B'`
8. `B`
9. `B2`
10. `F'`
11. `F`
12. `F2`
13. `D'`
14. `D`
15. `D2`
16. `U'`
17. `U`
18. `U2`

Now, let's see how to use the move columns, with the configuration we saw as
example above :

For the solved cube:
`SELECT moves FROM first_cross WHERE id = 140172`

We can see that the value is 11020654.

* 11020654%19 = 8. So the first move is `B`. We continue with 11020654/19 = 580034
* 580034%19 = 2. So the second move is `L`. We continue with 580034/19 = 30528
* 30528%19 = 14. So the third move is `D`. We continue with 30528/19 = 1606
* 1606%19 = 10. So the fourth move is `F'`. We continue with 1606/19 = 84
* 84%19 = 8. So the fifth move is `B`. We continue with 84/19 = 4
* 4%19 = 4. So the sixth move is `R'`. As 4/19 = 0, we stop here.

You can try, after those six moves, the bottom cross is solved, and you are
guaranteed that there is no better solution
