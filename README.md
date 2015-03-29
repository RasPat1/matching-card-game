Webapp that lets you create a custom deck of spot it cards. A spot it card contains a set of images on it such that it has one and exactly one image in common with any other card in the deck.  THe user can choose how many images to upload and the app will make a deck of maximum size using those images and following those properties. The user cna then downlaod that deck.

Written as a practice grails app.






How to make set of cards?

n - images in deck
k - images per card
c - cards in deck

Form all possible cards select a subset of size c in which the following rule holds:
Each card has one and only one image in common with each other card.

C1 = A B C
C2 = A D E
C3 = B D F
C4 = C E F

C1 U C2 = A
C1 U C3 = B
C1 U C4 = C
C2 U C3 = D
C2 U C4 = E
C3 U C4 = F

previous example 
n = 5
k = 3
c = 4

Interesting part comes in when you try to pick the maximum size set (max C) with some specific n images and k slots per card.

Unknown what constraints do fixing any of n, k, and c put on the deck construction.  i.e. If you want to make a deck with 50 cards how many images do you need minimum? maximum? how does the image number vary with the number of spots.  Is ymmetry guaranted?  in other words will each image appear on the same number of cards, or will some images be favored over others?  Overall, what is the mathematical relationship betwen the parameters?


Take 1 -- Brute Force
Can we create a trivial deck without usign brute force in an acceptable amount of time?
How many possible cards are there?
Each image is either included or excluded and there are k images on a card
Model a card as an n-bit integer with k ones
That's (n+k)! / (n! * k!) solutions
n ~ 10-80
k ~ 2-15
reasonable for small n and k
n=10, k=5
53,130 combinations

n=80, k=10
5.7E12 combinations

We don't have to exhaustively search though.
Possible implementation

Seed with random card.  Add a random unchecked card and see if it fits the criteria.  Continue until a deck of size c is made or all posiibilites have been checked.
Is the solution space in the problem space dense or sparse?  It picks guys randomly but if we knew how probable picking a working card is we would reason about its performance.
THe checking process must do one comparison against each card in teh deck... So the difficulty increase with deck size however decks are expected to be small typically on the order of ~50 cards

Check:
xor each pair of cards and check if the population count (Hamming weight) is 1.

Number of pairs to check is nC2 so n!/(2!*(n-2)!) which is O(n^2)

Hammign weight can be calculated so speed depends on the number of 1's so [O(k)](http://blogs.msdn.com/b/jeuge/archive/2005/06/08/hakmem-bit-count.aspx). 

Hamming Weight = O(k)
Pairs = O(nCk).. [O(min(n^k,n^(n-k))](http://stackoverflow.com/questions/24643367/whats-time-complexity-of-this-algorithm-for-finding-all-combinations)

So checks are NP as well but we might be okay since decks are reasonlay small
CHecking forves us to use a lot of wrok.  Generating a random card seems possibly horrible depending on the density of good cards. Don't do this.


Seed with a card.  Construct a new card we know works since all possible cards are options if this does not exist we have failed to create a deck.  How to contrcut a new card.

We want some operation that takes in all the current cards and produces some card which works for all the curernt ones.  Can't reason about it much till I find out how this magical operation works.

11000
10100
10010
10001

If you go along this path and create this deck.  No other cards can be made.  It is possible to create a bad deck.  Can't jsut do it randomly.  Have to enforce symmetry.  This has poor symmmetry (image 1 occurs 4 times while images 2-5 occur once each)

There must be a method to construct a deck just like we did earlier C1-C4 kinda thing.  And also I have to get around to solving this problem mathematically...  Something to do with non-euclidean spaces?
