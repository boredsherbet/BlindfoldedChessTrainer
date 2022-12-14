# BlindfoldedChessTrainer
I built this for a "capstone-esque" project in IBCS HL my senior year of high school. It's a trainer for blindfolded chess. The program is functional, but optimization is necessary. See README for more info. 

So this project took about 2-3 weeks to get to the place it is now, but while the algorithms and such are functional, the puzzles they generate are still relatively simple, since they're randomly generated. There is, of course, simple ways to improve this issue, and I'm hoping to get some time to refurbish the project up to snuff. Its functional, but its not optimal is all I'm saying. Here's the description on the project as a whole: 

# Level 1
This level is relatively simple, it just has a board, a square name pops up and you click on the square. The purpose of this is to train you into visualizing the board as a whole, so that you'll be able to interpret particular instructions better in future levels, and so that you understand where your pieces are without a board. 

### How it's built
Dev Notes: at this point, I'm not 100% sure how this level was formatted (I haven't been able to upload this because I didn't want to get in trouble with IB, and it was made right before winter break so it's been awhile since I even looked at it, but here we go), but I believe it involved a bunch of buttons generated by JetBrains. 

# Level 2
A tad more challenging, but still definitely simple. Your piece is placed on the board (an imaginary board) and you're given its coordinates. Enter in the squares that your piece attacks. This will help you get familiar with the pieces in the actual game, eventually, that is. 

### How it's built
I don't remember, but I remember the board was a 2D array. This level is still pretty simple to understand probably, and I'll add further documentation eventually. 

# Level 3
You're given two squares, write down your moves to get between them. With pieces like the queen, this is somewhat useless, but its challenging when you throw in a knight. The goal is to find the shortest path, so tread carefully!

### How it's built
So this one was, to put it simply, a fair bit more confusing. We have a few objects-- a piece and a square. Each square is given an ID 0-63. Using these IDs, we create a graph of the square objects. This graph of square objects is created by checking what piece is active for the user (is it a knight, rook, bishop or queen?) and then using addition to connect the squares in the graph from which a piece can move to. You can see this in the IF statements that detail the amount added to a number that then signifies the ids that are connected to that square's index in the 2D array that represents the graph. For example, the square 0 (which I believe was a8 on the board), for a knight, is connected to the squares with IDs 10 and 17-- the squares c7 and b6 respectively. The graph (which probably should've just been statically stored) is then parsed via BFS till it hits the target square (randomly selected). We then utilize the elements saved in the array and eliminate them by parsing back from the target square, checking what's connected to that that's also been looked at by the BFS. This allows us to backtrack back to the target square (admittedly this algorithm is still a bit foggy from me-- I found it online and pasted it in, but it works like magic so I'm fine with it). This then provides us with an array of square ids representing the shortest path. And then, and only then, can the puzzle be given to the user (without determining whether the puzzle has a solution, the puzzle cannot be given to the user). When generating the puzzle, there's also a few checks to ensure that the piece can be moved to that square (particularly when concerning bishops, seeing as a light squared bishop would never be able to get to a dark square, but outside of that this is how it works)

# Level 4
This is where your opponent has pieces on the board, and you're required to navigate around them. This is the closest I could get to stimulating a game without a database of puzzles or litterally playing a game against an engine or a friend. 

### How it's built.
  Okay, so this is going to take more than a minute, and I hope you have an understanding of how Level 3 works before reading this, because this took 2 weeks to figure out, and I'm not talking two weeks where I work on this like a hobby, I'm talking two weeks where if I'm not asleep, I'm doing this (that might be a bit of an exaggeration, seeing as I still had school, ate food, did other stuff and built the GUI, but I spent a few hours with my teacher and asked about 5 professional developers with a combined about 40ish years of experience till it popped into my brain while I was on the toilet, not kidding). Okay, so here we go. Remember that we need to solve the puzzle before the puzzle is given to the user-- and also ensure we find the shortest path. 
  The algorithm for this level is relatively similar to level three in that finding the solution works nearly the same way. I'll refer to that solution method as the level3SolutionMethod just for brevity's sake. There's two issues that need to be resolved to get from the method in level 3 to the algorithmic solution for this level: 
1. On level 3, we had no opponent pieces, so all the squares were safe. Now, we have squares that are unsafe, at least until we take the piece that makes that square unsafe. If the target square can be any square, it could also be one of these unsafe squares. 
2. Some squares can NEVER be selected as target squares (I called these "**eliminated squares**"). They exist in what I call "piece cycles" (You'll see why in a minute). These squares exist because such issues happen as two of the opponent's rooks defending one another. In this case, our one piece can never take either of the opponents rooks, since taking one would mean the other one would take you down. We need to find a way to identify these squares as unsafe. 
3. We need to account for the fact that you're able to take pieces, and though this is similar to problem one, it creates some extra issues (such as recomputing the square graph over and over and over again).
  UPDATE COMING
