
## What is this
A solver for [Semantle](https://semantle.com/) that works without knowing which embedding model the game uses. It only needs the relative ordering of similarity scores, not the scores themselves.

## References
- Inspired by the halfspace idea and [notebook](https://colab.research.google.com/drive/1zBht4wWCAsbUR5PYIQxy_8ZAWnhHoFK5?usp=sharing) by [Daniel Vitek](https://danielv.tech/)
- Follow-up to a different [solver](https://github.com/EthanJantz/semantle/) built with [Ethan Jantz](https://jantz.website/)

## How it works
Each ordered pair of guesses defines a halfspace constraint. If guess 1 ranked closer to the target than guess 2, then $t \cdot (g_1 - g_2) > 0$. With enough guesses, only the target satisfies all constraints.

The core solver (`CrossModelSolver`) builds these constraints using a different model than the game's. Since the models don't agree perfectly, hard filtering eliminates the target too early. Instead, each constraint contributes a sigmoid probability and candidates are scored by sum of log-probabilities. No word is ever fully eliminated, just made more or less likely to be sampled next. The target is found in 100-200 guesses.

There is also an exact-model solver (`HalfspaceSolver`) that uses the game's own embeddings with hard filtering, which converges in ~10 guesses.

## Running

```
uv run solver.py --cross   # cross-model probabilistic solver
uv run solver.py            # exact-model solver with hard filtering
```

