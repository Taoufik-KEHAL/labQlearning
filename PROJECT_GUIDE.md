# LabQlearning Project Guide

## 1) What this project is

`labQlearning` is a reinforcement learning (RL) lab based on the UC Berkeley Pacman projects.

It combines:
- a **Gridworld** environment (small MDPs for debugging and understanding RL behavior),
- a **Pacman** environment (larger, game-like RL setting),
- an **autograder** and test cases to validate implementations.

Main learning goals:
- understand **Value Iteration** (model-based RL / dynamic programming),
- implement **Q-Learning** (model-free RL),
- tune RL hyperparameters and analyze policy behavior.

## 2) Why this project exists

This project is designed to move from theory to practice:
- You apply Bellman equations and TD updates in real code.
- You compare planning (`ValueIterationAgent`) vs learning from interaction (`QLearningAgent`).
- You observe exploration/exploitation effects (`epsilon`, `alpha`, `gamma`) visually and via tests.

## 3) What is inside (file map)

### Core RL code (student work)
- `qlearningAgents.py`
  - `QLearningAgent`: tabular Q-learning implementation.
  - `PacmanQAgent`: Q-learning agent with Pacman defaults.
  - `ApproximateQAgent`: placeholder for feature-based approximation (currently not implemented in this repo).
- `valueIterationAgents.py`
  - `ValueIterationAgent` scaffold (currently mostly TODO in this repo).
- `analysis.py`
  - parameter answers for analysis questions (many still `None`).

### RL framework and interfaces
- `learningAgents.py`
  - abstract agent classes: `ValueEstimationAgent`, `ReinforcementAgent`.
  - episode lifecycle (`startEpisode`, `observeTransition`, `final`, etc.).
- `mdp.py`
  - generic MDP interface used by value iteration.
- `environment.py`
  - environment abstraction.
- `featureExtractors.py`
  - feature extractors used by approximate Q-learning.
- `util.py`
  - helper tools (`Counter`, random helpers, etc.).

### Environments and execution
- `gridworld.py`
  - Gridworld MDP, transition/noise logic, CLI options, episode runner.
- `pacman.py`
  - Pacman game engine, command-line parser, game loop.
- `game.py`
  - low-level game mechanics used by Pacman/Gridworld displays.
- `layouts/`
  - Pacman board definitions (`.lay` files).

### Evaluation and testing
- `autograder.py`
  - runs question-based tests.
- `reinforcementTestClasses.py`, `testClasses.py`, `testParser.py`, `grading.py`
  - test harness internals.
- `test_cases/`
  - public tests and expected solutions per question (`q1`..`q8`).
- `projectParams.py`
  - project metadata and default student files.

### Display/UI helpers
- `graphicsDisplay.py`, `graphicsGridworldDisplay.py`, `textDisplay.py`, `textGridworldDisplay.py`, `graphicsUtils.py`, etc.

## 4) How it works (conceptual flow)

### A) Value Iteration path (planning)
1. Build an MDP (`gridworld.py` + `mdp.py`).
2. `ValueIterationAgent` iterates over all states and updates `V(s)` using transition model + rewards.
3. Policy is derived by selecting action with max `Q(s,a)` from values.
4. Agent executes greedy actions from this computed policy.

In this repo, `valueIterationAgents.py` is still mostly scaffolded (`*** YOUR CODE HERE ***` + `raiseNotDefined()`), so full value iteration behavior depends on completing that file.

### B) Q-Learning path (interaction)
1. Agent starts with empty Q-table (`self.qvalues` in `qlearningAgents.py`).
2. At each step, chooses action with epsilon-greedy policy:
   - random action with probability `epsilon`,
   - best known action otherwise.
3. After transition `(s, a, r, s')`, updates:
   - `Q(s,a) <- Q(s,a) + alpha * (r + gamma * max_a' Q(s',a') - Q(s,a))`
4. Over episodes, Q-values converge toward good behavior.

In this repo, tabular `QLearningAgent` methods are implemented.

### C) Training vs testing behavior
`ReinforcementAgent` in `learningAgents.py` turns off learning/exploration after training:
- when `episodesSoFar >= numTraining`:
  - `epsilon = 0.0` (no exploration),
  - `alpha = 0.0` (no further updates).

## 5) Current implementation status in this repo

- Implemented:
  - core tabular Q-learning in `qlearningAgents.py` (`getQValue`, `computeValueFromQValues`, `computeActionFromQValues`, `getAction`, `update`).
- Not implemented / incomplete:
  - `ApproximateQAgent.getQValue` and `ApproximateQAgent.update`.
  - most of `ValueIterationAgent`.
  - many parameter answers in `analysis.py`.

## 6) How to run the project

Run commands from the `labQlearning` directory.

### A) Run Gridworld (text mode example)
```bash
python3 gridworld.py -t
```

Useful variants:
```bash
# Value iteration on BookGrid
python3 gridworld.py -a value -i 100 -g BookGrid -t

# Q-learning on BridgeGrid for 200 episodes
python3 gridworld.py -a q -k 200 -g BridgeGrid -e 0.3 -l 0.5 -t

# Manual control


```

### B) Run Pacman with Q-learning agent
```bash
python3 pacman.py -p PacmanQAgent -x 2000 -n 2010 -l smallGrid
```

Explanation:
- `-x 2000`: 2000 training games (quiet),
- `-n 2010`: total games, so last 10 are evaluation after training.

### C) Run autograder
```bash
# All questions
python3 autograder.py

# Single question
python3 autograder.py -q q6

# No graphics mode
python3 autograder.py --no-graphics
```

## 7) Typical workflow

1. Implement missing TODOs in student files:
   - `valueIterationAgents.py`
   - `qlearningAgents.py`
   - `analysis.py`
2. Validate quickly in Gridworld (fast debugging).
3. Test behavior in Pacman.
4. Run autograder until all targeted questions pass.

## 8) Key RL parameters you will tune

- `gamma` (discount): preference for long-term rewards.
- `alpha` (learning rate): how quickly Q-values adapt.
- `epsilon` (exploration): probability of random action.
- `noise` (Gridworld transitions): stochasticity of environment.
- `livingReward`: per-step reward shaping in Gridworld.

## 9) Notes

- This project includes Berkeley licensing/attribution headers; keep them intact.
- Display modules are optional for grading but useful for intuition.
- If GUI has issues in your environment, use text mode (`-t`) and `--no-graphics`.
