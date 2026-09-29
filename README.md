# AP9EV excercise 1

Project for the Evolutionary Computation Techniques class. First assignment.

Author: [Ondřej Hruboš](https://github.com/hruboson), [Github repository](https://github.com/hruboson/AP9EVex1)

This script implements a basic genetic algorithm for working with binary representation of individuals. The implementation is tested on the one max and leading ones problems.

The general idea was to create an object-oriented implementation where each individual/candidate is an instance of class `Candidate` which are then part of a `Population`. Roulette and rank selection inside of populations were implemented and can be toggled using the `SELECTION` constant.

## Implementation

Each run is a generational genetic algorithm with a budget of `EVALS_PER_DIM * dimension`
fitness evaluations per run, so the horizontal axis of the plots is comparable across
dimensions. The starting population is generated randomly and fully evaluated.

The main loop builds each new generation as:

1. **Elitism**: the top `round(ELITISM_RATIO * pop_size)` candidates are copied over
   unchanged (and *not* re-evaluated).
2. **Selection**: two parents are drawn with either *roulette* (fitness-proportional,
   with `+1` so that a zero-fitness individual still has a chance) or *rank* (linear
   weights `1..N` on the fitness-sorted population). The same individual is re-drawn
   up to 10 times to avoid self-fertilisation.
3. **One-point crossover**: a random cut point in `1..d-1` is used to produce *two*
   complementary children from the two parents.
4. **Mutation**: each bit is flipped independently with probability `MUTATION_PROBABILITY`.

`RUNS_NO` independent runs are performed for (problem, dimension) pair with a fixed
`SEED`. The plotted graph is the mean over runs of the fitness after each
evaluation, and the table below the plots summarises the final best fitness of every run
(best, worst, mean, median, standard deviation).

# Results

 The graph was generated for configuration:

```python
RUNS_NO = 10
MUTATION_PROBABILITY = 0.0075
POPULATION_SIZE = 50
ELITISM_RATIO = 0.1
SELECTION = "roulette" # "rank", "roulette"
EVALS_PER_DIM = 100
DIMENSIONS = (10, 30, 100)

SEED = 42
```

![Genetic algorithm results](./convergence_results.png)


# Usage

You can tweak the parameters at the top of the script. All parameters are `CAPITALIZED`.

```python
RUNS_NO: int
MUTATION_PROBABILITY: float <0;1>
POPULATION_SIZE: int
ELITISM_RATIO: float <0;1>
SELECTION: str "roulette" | "rank"
EVALS_PER_DIM: int
DIMENSIONS: touple

SEED: int
```

## Run

- Either use the prepared [`nix`](./shell.nix) shell file:
    - Install [`nix`](https://nixos.org/download/) on your system (currently only for Linux/MacOS)
    - Enter the shell: `nix-shell ./shell.nix`
    - Run the Python script: `python ./main.py`
- Use your own installation of **Python 3.14** (older versions will most likely not work due to the usage of type hints!)
    - Create a virtual environment with the attached [`requirements.txt`](./requirements.txt): 
        - Windows: `python -m venv .venv && .venv\\Scripts\\activate && pip install -r requirements.txt`
        - Linux/MacOS: `python -m venv .venv && . .venv/bin/activate && pip install -r requirements.txt`
    - Run the Python script: `python ./main.py`
    - Leave virtual environment: `deactivate`
