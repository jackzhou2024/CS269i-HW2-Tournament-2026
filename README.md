# Iterated File-Sharing Dilemma

This coding tournament is based on a CS 269i class project developed by the former CS269i student, Cary Huang (We really appreciate for Cary's wonderful work).



Nicky Case's "The Evolution of Trust" is also super fascinating, but it's not necessary to understand this project: https://ncase.me/trust/

---



## How It Works

When you run `code/dilemma_run.py`, it will automatically search for all Python strategy files in the `code/exampleStrats/` folder. Then, it simulates the **Iterated File-Sharing Dilemma** for every possible pair of strategies in a round-robin format — that is, it evaluates all $n \choose 2$ matchups.

After all simulations complete, the script calculates each strategy's average score and generates a **leaderboard**, saving the results to `results.txt`.

To **add your own strategy**, simply create a new `.py` file in `code/exampleStrats/` following the same format as the example strategies. When you run `dilemma_run.py`, your strategy will be included in the tournament automatically.

---

## Payoff Chart

|                        | **Player B cooperates** | **Player B defects** |
|------------------------|-------------------------|----------------------|
| **Player A cooperates** | A: +2, B: +2            | A: -1, B: +3         |
| **Player A defects**    | A: +3, B: -1            | A:  0, B:  0         |

In this code:  
`0 = 'D' = Defect`  
`1 = 'C' = Cooperate`



---

## Strategy Function Interface

Each strategy must define a function with the following signature:

```python
def strategy(history, avgPreviousScore, avgPreviousCoop, memory):
    ...
    return moveChoice, memory
```

- `history`: A 2×n NumPy array, where `n` is the number of turns in this matchup so far.  
  - `history[0]` contains **your moves**.
  - `history[1]` contains **your opponent’s moves**.
  
  Example:
  ```
  [[0 0 1]    # You: D D C
   [1 1 1]]   # Opponent: C C C
  ```


- `avgPreviousScore`: A float representing your opponent’s **average score per turn** averaged across all turns in all of their *previous* matchups.
- `avgPreviousCoop`: A float representing the **probability that your opponent cooperated**, averaged across all turns in all of their *previous* matchups
  - **Both values exclude the current matchup** (i.e., the one against your strategy).
  - If this is your opponent’s **first matchup**, both values will be set to `0.0`.

- `memory`: A flexible parameter to retain any information between turns.  
  If unused, you can simply return `None`. For example:
  - In `titForTat.py`, memory is unused.
  - In `grimTrigger.py`, memory can be a Boolean indicating if the opponent has defected before.
  - For complex strategies, you can store lists or custom objects as memory.

### Outputs:
- `moveChoice`: Your move (0 = Defect, 1 = Cooperate)
- `memory`: An object used to retain information between turns **within the current matchup**.  
  Note: You should **not store data across different matchups**. Your strategy must rely only on the current matchup’s `history` and `memory` along with `avgPreviousScore` and `avgPreviousCoop`.


---

## Simulation Details

- **Each matchup runs for 200 turns.**

---

## Your Task

You're expected to write a Python file named `strategy.py`, implementing the function `strategy(...)` as described above.

1. Place your file in the `exampleStrats/` folder.
2. Run the simulation:

```bash
cd code
python dilemma_run.py
```

Your strategy will automatically compete against all others in the folder.

---


## Tips for Running in a Clean Environment

To start from a clean Python enviornment, It’s recommended to use conda 

https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html

First, install conda

Second, create a clean python enviornment (say python 3.7) with conda

Third, install any deps and execute your script in that conda enviornment 

The major commands are as below. 

```
conda create --name [NAME]

conda activate [NAME]

conda install pip

pip install -r requirements.txt
```

After you have successfully create the environment, enter the code environment, and run dilemma_run.py

```
cd code 

python dilemma_run.py
```

Without writing any code, you should be able to run the competition for the existing strategies in the exampleStrats folder. Then, you write your own strategy.py and put the file into the folder, rerun dilemma_run.py to see whether you can beat these baselines.


## Note

All you need to do is to write a strategy.py file and add it to exampleStrats. No need to change any existing files.

When submitting to gradescope, you only submit strategy.py, no other files are needed.

 🚨 **Please do not write code that inspects or manipulates the game format, the objective, or your opponents' strategies.**

For any questions, feel free to make a post on edstem.
