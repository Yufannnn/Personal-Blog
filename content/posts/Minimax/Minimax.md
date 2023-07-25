---
weight: 1
title: "The Power of Iterative Deepening Minimax Search with Alpha-Beta Pruning 💥"
date: 2022-09-11
lastmod: 2022-09-11
draft: false
author: "Zhu Yufan"
description: "A Journey into AI: Implementing Iterative Deepening Minimax Search with Alpha-Beta Pruning for Breakthrough 🕶️"
featuredImage: "Minimax.png"
featuredImagePreview: "Minimax.png"

tags: ["computer science", "algorithm", "AI"]
categories: ["Academia"]

lightgallery: true
linkToMarkdown: false

toc:
  auto: false
---

In my pursuit of knowledge in Computer Science, I undertook a mini-project during the course CS2109s (Introduction to AI and ML). The project aimed to build an AI agent for a simple yet intriguing board game called Breakthrough, implementing the iterative deepening minimax search algorithm with alpha-beta pruning. The capabilities of this AI agent left me amazed and eager to share my journey with you! 🌐🔍
<!--more-->

### The Board Game: Breakthrough 🎲

![Breakthrough](/breakthrough.jpg)

Breakthrough is an abstract strategy board game that was invented by Dan Troyka in 2000 and made available as a Zillions of Games file (ZRF). It has gained popularity for its simplicity yet strategic depth, making it an intriguing challenge for players of all ages.

##### Rules and Winning Condition of the Game 🏆

Breakthrough is played on an 8x8 square board, although it was originally designed for a 7x7 board, allowing for easy scalability to larger sizes. Each player controls a set of pawns, represented by black and white pieces, and the goal is to outmaneuver the opponent to achieve victory.

The game starts with the pawns of both players positioned at opposite ends of the board. The rules are as follows:

1. **Piece Movement:** Players take turns moving one of their pawns in a forward direction. A pawn can move one square either vertically, horizontally, or diagonally, as long as the target square is unoccupied. However, diagonal movement can only be used for capturing an opponent's pawn.

2. **Capture:** If a player's pawn lands on a square diagonally adjacent to an opponent's pawn, it captures the opponent's pawn, removing it from the board. Capturing is an essential tactic for gaining an advantage in the game.

3. **Promotion:** When a player's pawn reaches the opponent's home row, it gets promoted to a "super pawn." The super pawn gains additional movement options, allowing it to move both forward and backward one square, adding more strategic possibilities.

##### Winning Conditions 🏆

Breakthrough offers two ways to claim victory:

1. **Reaching the Opponent's Home Row:** The first player to advance one of their pawns to the opponent's home row wins the game immediately. This approach emphasizes aggressive and rapid advancement.

2. **Eliminating the Opponent's Pawns:** Alternatively, a player can win by eliminating all of the opponent's pawns, leaving them with no legal moves. This approach emphasizes careful planning and tactical precision.

The game's simplicity belies its complexity, as players must carefully balance offense and defense while predicting their opponent's moves. Breakthrough provides an engaging and intellectually stimulating experience, making it a favorite among strategy game enthusiasts. Whether you are a seasoned player or a newcomer, Breakthrough promises a rewarding and enjoyable gaming session.

### The General Approach I Took 🚀

The foundation of my AI agent's remarkable performance in Breakthrough lies in the iterative deepening minimax search algorithm with alpha-beta pruning. This combination empowers the agent to intelligently explore the game tree and make strategic decisions based on the evaluation of each board state.

#### Minimax Search Algorithm 🌀

At the core of my AI agent's decision-making process is the minimax search algorithm, a fundamental technique used in two-player, zero-sum games like Breakthrough. The algorithm explores the possible moves and outcomes of the game tree to find the best move for the current player, assuming the opponent will make optimal moves. The objective is to maximize the agent's evaluation score while minimizing the opponent's score.

#### Alpha-Beta Pruning 🍃

To address the computational complexity of the minimax algorithm, especially in games with a large branching factor, I implemented alpha-beta pruning. This intelligent technique prunes or cuts off parts of the game tree that are proven to be irrelevant to the final decision. By reducing the number of nodes explored, alpha-beta pruning significantly improves the efficiency of the search, allowing the AI agent to explore deeper into the game tree within the specified time constraints.

#### The Heuristic Evaluation Function 🧠

To evaluate the desirability of each board state, I carefully designed a heuristic evaluation function. This function takes into account various factors that influence the value of a particular board state. By assessing the characteristics of the board, the heuristic function aims to approximate the true value of the board state, enabling the AI agent to make informed and strategic decisions.

#### Factors Considered in the Heuristic Function 📊

To create a comprehensive evaluation, the heuristic function considers multiple factors:

1. **Piece Score:** This factor quantifies the difference in the number of black pawns and white pawns on the board. A higher piece score indicates an advantageous position for the black player, as it signifies a greater number of pieces compared to the opponent.

2. **Position Score:** The position score represents the difference in the sum of distances of each black pawn from its baseline and the sum of distances of white pawns from their baseline. A higher position score implies a better positional advantage for the black player, positioning its pawns closer to achieving the winning condition.

3. **Offensive Score:** This score reflects the farthest distance from any black pawn to the opponent's baseline. A higher offensive score indicates that black pawns are positioned closer to the opponent's home row, bringing them closer to victory.

4. **Defensive Score:** Conversely, the defensive score represents the closest distance from any white pawn to the black pawn's baseline. A higher defensive score suggests a stronger defensive position for the black player, safeguarding its pawns from imminent threats.

5. **Danger Penalty:** The heuristic function penalizes board states where white pawns can attack black pawns in the next turn, as such a situation endangers the black pawns and might lead to potential losses.

6. **Protected Bonus:** On the other hand, if an endangered black pawn is protected by other black pawns, the heuristic function rewards the state. This strategic move lures the opponent into capturing a pawn that can be immediately counterattacked, turning a potentially adverse situation into an advantageous one.

7. **Empty Column Penalty:** The heuristic function penalizes boards with empty columns, as they can create difficulties in defense and counterattacks.

8. **Empty White Column Penalty:** Additionally, the heuristic function doubles the penalization for columns with a white pawn but no black pawn, as this creates a vulnerable position for the black player, inviting potential threats.

### Balancing the Weightage of Heuristic Factors ⚖️

The effectiveness of the heuristic evaluation function relies on the careful balance of weightage for each factor. I conducted rigorous testing and experimented with different weightage values to find an optimal configuration that produced superior performance. This iterative process allowed my AI agent to fine-tune its evaluation function and demonstrate strategic prowess in the game of Breakthrough.

### Discovering Non-obvious Features 🕵️‍♂️

As I played the game myself, I discovered several non-obvious features of the board that significantly influence the evaluation function and overall gameplay:

- **Danger Penalty:** Calculating the number of white pawns that can attack black pawns in the next turn helps identify imminent threats. A higher danger penalty penalizes the state more heavily, indicating a precarious situation where the black pawns are at risk of being captured.

- **Protected Bonus:** The protected bonus becomes instrumental in determining whether an endangered black pawn is being protected by other black pawns. If a black pawn is endangered but protected, we reward the state instead, as it offers the opportunity for a counterattack. This strategic maneuver can be especially effective against opponents that prioritize attacking.

- **Empty Column Penalty:** The presence of empty columns on the board complicates defensive and offensive tactics, making it difficult to block opponents on a particular column or launch attacks on neighboring columns. Penalizing boards with empty columns discourages the player from leaving columns without black pawns, promoting better strategic play.

### Dealing with Time Constraints ⏰

Given the project's strict time constraint of 3 seconds for each move, regardless of the machine's processing power, I implemented iterative deepening. This approach gradually increases the search depth within the specified time frame, striking a balance between search depth and computational efficiency. By stopping the search and returning the best move found when nearing the 3-second limit, the AI agent adheres to the time constraint without compromising its decision-making quality.

### Additional Features to Enhance Performance 🔍

To further optimize the alpha-beta algorithm's pruning, I introduced moving ordering. This technique prioritizes the possible moves of a board by storing them in a priority queue, sorted according to each board's score in descending order. By evaluating the most promising move first, the algorithm is more likely to achieve pruning and significantly improves overall efficiency.

Additionally, to speed up the evaluation process, I implemented memoization in the `evaluate_board` function. This technique stores the score of each board as a dictionary in the `PlayerAI()` class, avoiding repeated calls to the `evaluate_board` function for identical boards at the base cases. With the manageable number of possible states in the 8x8 Breakthrough game, this approach dramatically reduces computation time and optimizes the AI agent's performance.


### Pseudo Code of My Algorithm 📝

```python
# Pseudo Code of the Iterative Deepening Minimax Search Algorithm with Alpha-Beta Pruning

class PlayerAI:
    def evaluate_state(self, board):
        # Implement the heuristic evaluation function to assess the desirability of each board state.
        # The evaluation function should consider factors like piece score, position score, offensive score, and defensive score.

    def possible_moves(self, board):
        # Generate all possible moves for the black player in a priority queue where the top element has the largest evaluation score.
        moves = PriorityQueue()
        return moves

    def stringify_board(self, board):
        # Represent a board as a string for efficient storage and faster comparison.
        res = ""
        for i in range(6):
            for j in range(6):
                res += board[i][j]
        return res

    def make_move(self, board):
        # Minimax searching function that returns the best move found within a given number of steps.
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
                # Use memoization to keep track of the boards being evaluated.
                new_repr = self.stringify_board(board)
                if new_repr not in visited:
                    current_score = -minimax(invert_board, depth - 1, not is_black, alpha, beta, visited)[0]

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

        # Implement iterative deepening minimax search.
        while time.time() < end_time:
            current_search = minimax(board, depth, True, -math.inf, math.inf, {})
            if current_search[0] > current_best_score:
                current_best_score = current_search[0]
                current_best_move = current_search[1]

            depth = depth + 1

        return current_best_move
```

Certainly! Here's the enhanced version with emojis for each point:

#### Enhancements Left Unexplored 🚧

While the current implementation already demonstrates formidable performance, there are still some enhancements I did not have the time to explore fully:

Absolutely! Let's delve deeper into each unexplored enhancement:

- **Zobrist Hashing 🧩:** Zobrist hashing is a clever technique used in board game AI to efficiently represent and compare board states. It involves assigning a random number (a hash value) to each possible piece and position on the board. By XORing these hash values together, we can create a unique hash for each board state. Zobrist hashing allows us to quickly compare and identify previously visited board states, reducing redundant evaluations and improving search efficiency. Implementing this technique could have further optimized the algorithm's performance, particularly in scenarios with a high number of board states.

- **Transposition Tables 🔢:** Transposition tables are a valuable addition to any advanced board game AI. They store previously computed board evaluations, along with their associated depth and best move information. When encountering a previously visited board state during the search, the AI can retrieve the stored evaluation and use it to update the current evaluation score. This saves time by avoiding redundant evaluations and allows the AI agent to explore deeper levels in the game tree.

- **Opening Book 📚:** Creating an opening book involves precomputing and storing a collection of well-known and well-analyzed opening moves. During the early stages of the game, the AI agent can refer to this book to make moves without extensive search, as these moves have been strategically analyzed beforehand. Utilizing an opening book can significantly speed up the early moves, providing the AI with a solid foundation for the rest of the game.

- **Advanced Heuristic Features 🏛️:** While the current heuristic evaluation function is comprehensive and effective, there are numerous other strategic elements to explore. Incorporating additional features, such as considering pawn mobility (the number of possible moves for each pawn), central control (occupying the center of the board), or pawn promotion incentives (encouraging pawns to advance towards the opponent's home row), could lead to more nuanced and sophisticated gameplay. Fine-tuning the weightage of these features would be an exciting area to investigate.

- **Monte Carlo Tree Search (MCTS) 🌳🎲:** MCTS is a powerful algorithm for games with large branching factors. Combining MCTS with the current minimax search could provide a balanced approach to exploration and exploitation. MCTS involves performing random simulations from the current board state to gather statistics on moves' success rates. This information can guide the AI's decision-making process, allowing it to make more informed choices when exploring the game tree.

- **Parallelization 🔄:** Parallelizing the search process using multi-threading or distributed computing can dramatically speed up the AI agent's search speed. By dividing the search among multiple threads or machines, the AI can explore different parts of the game tree simultaneously, significantly increasing the number of nodes evaluated within the time constraints.

- **Machine Learning Techniques 🧠📊:** Exploring machine learning techniques, such as deep reinforcement learning or neural networks, can open up new possibilities for the AI agent's decision-making process. By learning from large datasets of gameplay data, the AI could adapt and improve its strategy over time, becoming a more formidable opponent.

- **Dynamic Evaluation Function 📈:** Rather than using a fixed evaluation function, a dynamic evaluation function could adjust its weightage or features based on the game state. For example, the function could prioritize certain features in the opening stage, and then shift its focus to different features in the middle or endgame. This adaptability could lead to more flexible and responsive gameplay.

- **Optimized Data Structures 💾:** Optimizing the data structures used in the algorithm, such as using bitboards instead of 2D arrays to represent the board state, could further improve memory efficiency and computational speed. Bitboards are a compact and efficient representation that allows bitwise operations, reducing memory overhead and speeding up evaluations.

While these enhancements were not fully explored within the project's constraints, they represent promising avenues for future development. The current implementation already showcases the power of the iterative deepening minimax search algorithm with alpha-beta pruning and a thoughtfully crafted heuristic evaluation function. With these enhancements left unexplored, there is ample room for continued innovation and advancement in the realm of Breakthrough AI. 🚧🚀

### The Testing: Unraveling the Capabilities 🧪🎮

Following the implementation of the iterative deepening minimax search algorithm with alpha-beta pruning, capable of searching up to approximately 5 levels within the 3-second time constraint, I devised a series of test scenarios to evaluate my AI agent's prowess.

### The Naive Players 👶

To assess my AI agent's performance against less sophisticated opponents, I created a few more naive players with only 3 levels of search depth. Here are the heuristic functions for these naive players:

```python
# Compare the number of pieces
def piece_evaluation(board):
    number_of_pieces = 0
    for i in range(6):
        for j in range(6):
            if board[i][j] == 'B':
                number_of_pieces += 1
            if board[i][j] == 'W':
                number_of_pieces -= 1
    return number_of_pieces

# Compare the number of possible moves
def mobility_evaluation(board):
    invert_state = copy.deepcopy(board)
    utils.invert_board(invert_state)
    return len(possible_moves(board)) - len(possible_moves(invert_state))

# Compare the total number of distances from the baseline
def greedy_evaluation(board):
    total_position_score = 0
    for i in range(6):
        for j in range(6):
            if board[i][j] == 'B':
                total_position_score += i
            elif board[i][j] == 'W':
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

# Heuristic Function: Random Evaluation
# This player makes random moves without any strategic evaluation.
def random_evaluation(board):
    return random.randint(-10, 10)

# Heuristic Function: Mobility Evaluation
# This player prioritizes maximizing its own mobility, irrespective of the opponent's moves.
def mobility_evaluation_v2(board):
    return len(possible_moves(board))

# Heuristic Function: Defensive Recklessness
# This player aggressively moves forward but also considers its distance from the opponent's baseline.
def defensive_recklessness_evaluation(board):
    score = 0
    for i in range(6):
        for j in range(6):
            if board[i][j] == "B":
                score = max(score, i)
            elif board[i][j] == "W":
                score = max(score, 5 - i)
    return score

# Heuristic Function: Proximity to Victory
# This player evaluates positions based on their proximity to winning conditions.
def proximity_to_victory_evaluation(board):
    winning_score = 100
    losing_score = -100
    black_win, white_win = check_victory(board)

    if black_win:
        return winning_score
    elif white_win:
        return losing_score
    else:
        return 0
```

By incorporating these additional naive players, my AI agent will now compete against various strategies with different evaluation criteria. This comprehensive testing will help me gain a deeper understanding of its performance and areas for potential improvement. Let the gaming begin! 🚀🎮

#### Astounding Results 🌟

My AI agent demonstrated its mettle by defeating all naive players within approximately 20 steps! The effectiveness and power of minimax search, coupled with an insightful heuristic function, exceeded my expectations. The ability to search several more layers in such a short time showcased the true potential of the iterative deepening minimax algorithm.

### Conclusion and Reference 🏁📚

While I have accomplished remarkable progress in building an AI agent for Breakthrough, there are still avenues for further exploration, such as implementing Zobrist hashing to optimize the algorithm's efficiency.

The development of the heuristic function drew inspiration from a valuable

 resource: <https://www.codeproject.com/Articles/37024/Simple-AI-for-the-Game-of-Breakthrough>. While high-level insights were gleaned from this article, all algorithms and implementations were crafted independently.

Join me in my journey of AI exploration as I delve deeper into the world of algorithms, optimization techniques, and the exciting realm of heuristic evaluation! 🚀🤖

Stay tuned for more AI adventures! 🌟🎲

If you have any suggestions, questions, or insights to share, feel free to reach out and embark on this fascinating AI journey together! 🤝😊

---

