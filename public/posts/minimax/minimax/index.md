# The power of Iterative Deepening Minimax search with Alpha Beta Pruning :boom:

I did a mini project when taking CS2109s (Introduction to AI and ML), where I built an AI agent for a simple board game, Breakthrough, with iterative deepening minimax search algorithm. What it can achieve blew my mind!
<!--more-->

### The Board Game
Breakthrough!!
{{< image src="/breakthrough.jpg" caption="breakthrough (`image`)" >}}

{{< admonition type=note title="Breakthrough" open=true >}}
Breakthrough is an abstract strategy board game invented by Dan Troyka in 2000 and made available as a Zillions of Games file (ZRF). It won the 2001 8x8 Game Design Competition, even though the game was originally played on a 7x7 board, as it is trivially extensible to larger board sizes.   -- from WikiPedia
{{< /admonition >}}
 
##### Rules and Winning condition of the game
- each play moving ***one piece per turn***.
- a pawn may move one pace ***straight or diagonally forward***.
- a piece can move into a square containing opponent's pawn and capture it, if the square is ***diaganoally forward***
- winning condition 1: The first player to reacht the opponent's home row
- winning condition 2: the other player has no more pawns.


### Pseudo Code of My Algorithm
``` python
class PlayerAI:
    def evaluate_state(self, board):

    # generate all possible moves for black in a priority queue such that 
    # the top element is the one with the largest evaluation score
    def possible_moves(self, board):
        moves = PriorityQueue()
        return moves

    # represent a board as a string for smaller storage and faste comparison
    def stringify_board(self, board):
        res = ""
        for i in range(6):
            for j in range(6):
                res += board[i][j]
        return res

    def make_move(self, board):
        # minimax searching function that returns the best move found in a given number of steps
        start_time = time.time()
        end_time = start_time + 2.90

        depth = 0
        current_best_score = -math.inf
        current_best_move = None

        def minimax(board, depth, is_black, alpha, beta, visited):
            if is_game_over or depth == 0:
                return evaluate_state(board), None

            best_move = None
            best_score = -math.inf

            moves = self.possible_moves(board)
            while not moves.empty():
                current_move = moves.get()[1]
                if time.time() > end_time:
                    break
                # use memoization to keep track of the board being evaluated
                if new_repr not in visited:
                    current_score = -minimax(invert_board, depth - 1, 
		    			not(is_black), alpha, beta, visited)[0]

                if current_score > best_score:
                    best_score = current_score
                    best_move = current_move

                if is_black:
                    alpha = max(best_score, alpha)
                else:
                    beta = min(-best_score, beta)

                if alpha >= beta:
                    break

            return best_score, best_move

        # implement iterative deepening minimax search
        while time.time() < end_time:
            current_search = minimax(board, depth, True, -math.inf, math.inf, {})
            if current_search[0] > current_best_score:
                current_best_score = current_search[0]
                current_best_move = current_search[1]

            depth = depth + 1

        return current_best_move
````

### The General Approah I took
The overall algorithm is based on the minimax search algorithm with alpha-beta pruning. We will use a heuristic function, which shall be discussed in the latter section, to evaluate the desirability of each state of the board from the point of view of the black player. (i.e, the larger the score of a particular board, the more desirable the board is for the black player.) When it is the White player's turn to move, we invert the board to find the most desirable move for the White player and return the most negative score of that board since the score of the board is from the perspective of the black player. The algorithm also incorporates alpha-beta pruning where alpha only updates for the black player and beta is only updated for the white player to reduce the number of boards expand, thus increasing the level searched

### Some Constraints Given by the questions
Since the question states a time constraint of making each move within actual 3 seconds, regardless of the machine where the algorithm runs. It is therefore suitable to enhance the algorithm with iterative deepening, where the depth of the minimax search increases gradually within the given time. To achieve this, we first find the time before the algorithm's execution and calculate the end time to be incremented by 2.90. Second, when entering each iteration, namely when evaluating each possible next step, we compare the current time with the supposed end time. If it exceeds 2.90 seconds, we return the best move we have found so far. The 0.1 second left allows the algorithm to finish executing the remaining instructions. The 2.90 seconds limit works fine both on my laptop and desktop.

### Some Additional Features that I Included to Further Improve the performance
- to increase the pruning for the alpha-beta algorithm, moving ordering is also adopted. The possible moves of a certain board are stored in a priority queue sorted according to each board’s score in descending order. This means that the most promising move will be evaluated first, and pruning is more likely to happen. 
- Memoization is used in the evaluate_board function to further speed up the algorithm by storing the score of each board as a dictionary in the class PlayerAI(). This avoids calling the evaluate_board function repeatedly on the same board at the base cases. Such an approach is viable as the number of the possible state of the 8x8 breakthrough game is still relatively small and we are evaluating the board in various places (when generating possible moves, and when evaluating the base case). I also create a stringify_state function to produce a string representation of a board since the original list representation of the board is not hashable.

### The Heuristic Function!!! 
the heuristic of my evaluation_board function takes several factors into consideration. The most obvious case is that for the winning state, the score should be a large positive number if the black player wins the game and a large negative number otherwise. Also, if the white pawns are in the second last row, meaning that we are almost losing, such a state should be penalized significantly.
There four following basic factors are considered:
- The piece score in the evaluation function stores the amount by which the number of black pawns exceeds the number of white pawns.
- The position score in the evaluation function stores the amount by which the sum of distances of each black pawn from its baseline exceeds that of white pawns. 
- The offensive score in the evaluation function stores the farthest distance from the one black pawn to the baseline.
- The defensive score in the evaluation function stores the closest distance from the one white pawn to the black pawn's baseline.

The larger the more desirable for all four factors. However, unfortunately, the weightage of each factor is determined empirically. I have created several test players which only consider one of the aforementioned factors to play against each other. After much testing, I decided to assign more weightage to the defensive score as the defensive player performs the best among all the test players while assigning a much low weight for the offensive score as it renders the player more prone to move forward recklessly. A comprehensive player is also designed to consider all four above basic stats for further testing. The source code for all five players is attached below in the document.
Moreover, after actually playing the game myself, I have discovered various non-obvious features of the board that we may what to ponder in our evaluation function:
- The danger penalty in the evaluation function stores the number of white pawns that can attack our black pawns. (i.e., which can be captured by white pawns in the next turn).
- In addition, the protected bonus calculates whether an endangered black pawn is being protected by other black pawns, meaning that if the white pawns actually attack our black pawn, the protecting black pawn can attack the white pawn in revenge. 
The weight for the protected bonus is designed to the higher than the danger penalty. Therefore, if the pawn is endangered without being protected, we will penalize it heavily as we will lose the pawn without being able to revenge. On the other hand, if the pawn is actually endangered but protected at the same time, we will rather reward the state. With such a strategy, we can lure the opponent to eat our pawn such that we can immediately counterattack. This is especially effective when playing against players that prioritize attacking.
- The other feature that came to my attention is when there are empty columns. I found that if we have columns without black pawns, it becomes more difficult to defend in terms of blocking the opponent on the column and attacking the opponent on the neighbouring column. Therefore, I have created this empty column penalty, to penalize boards with empty columns. Furthermore, a more precarious situation is when there is a column with a white pawn but not a black pawn. We will double penalize that case.

### The Testing
After the implementation of the aforemented algorithm, which is able to search for around 5 levels on my machine within 3 seconds. I have also created a few other more naive players that only searches for 3 levels to test out my agent. 
Here are the heuristic functions I used to the naive player.
```python
# compare the number of pieces
def piece_evaluation(board):
        number_of_pieces = 0
        for i in range(6):
            for j in range(6):
                if(board[i][j] == 'B'):
                    number_of_pieces += 1
                if(board[i][j] == 'W'):
                    number_of_pieces -= 1
        return number_of_pieces
    
# compare the number of possible moves
def mobility_evaluation(board):
    invert_state = copy.deepcopy(board)
    utils.invert_board(invert_state)
    return len(possible_moves(board)) - len(possible_moves(invert_state))

# compare the total number of distance from the base line
def greedy_evaluation(board):
    total_position_score = 0
    for i in range(6):
        for j in range(6):
            if(board[i][j] == 'B'):
                total_position_score += i
            elif(board[i][j] == 'W' and i ):
                total_position_score -= (5 - i)

    return total_position_score

def reckless_evaluation(board):
    score = 0
    for i in range(6):
        for j in range(6):
            if board[i][j] == "B":
                score = max(score, i)
    
    return score

def defensive_evaluation(board):
    score = 6
    for i in range(6):
        for j in range(6):
            if board[i][j] == "W":
                score = min(score, i)
    
    return score
```
My AI agent is able to defeat all of them around 20 steps, it really surprised me that minimax search can be so powerful if we are able to search for several more layers

### Some Features that I Didn't have Time to Implement
- Zobrist Hashing

### Referenece
<https://www.codeproject.com/Articles/37024/Simple-AI-for-the-Game-of-Breakthrough>.
I have gained much high-level inspiration from this article, especially for my evaluation function but I have implemented all the algorithms on my own. :wink:

