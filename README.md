# Traveling Salesman Genetic Solver

University project that applies a genetic algorithm to the Traveling Salesman Problem (TSP). The implementation explores different mutation/recombination rates, replication and crossover schemas, and can emit plot-ready data for parameter sweeps.

## Project Layout

- `src/task3/`: Core implementation.
- `src/test/`: JUnit tests.
- `maps/`: Sample TSP city maps (`*.64`).
- `plots_data/`: Example outputs for plotting.
- `scripts/`: Gnuplot helpers for visualizing results.

## Build

There is no build tool checked in. Compile with `javac` and include Log4j 1.x on the classpath.

```sh
mkdir -p out
javac -cp "lib/log4j-1.2.17.jar" -d out src/task3/*.java src/test/*.java
```

## Run

The entry point is `task3.Main`. It expects exactly 10 CLI arguments.

```sh
java -cp "out;lib/log4j-1.2.17.jar" task3.Main \
  --pm=0.02 --pc=0.5 --genecount=200 --genelen=200 --maxgen=1000 \
  --runs=10 --protect=best --initrate=5 --crossover_scheme=1 --replication_scheme=1
```

Notes:
- The code currently reads `file1.64` from the working directory (`TextProcessor.readFileTSP("file1.64")`).
  - Either run from `maps/`, copy a map file to the working directory, or update the path in `src/task3/Main.java`.
- Output data is written to `plot<timestamp>.txt` in the current working directory.

## Algorithm Overview

- **Genome**: a permutation of city indices (`DNA`).
- **Fitness**: path length computed from pairwise city distances.
- **Selection/Replication**: configurable schemas (`replicationSchema`).
- **Crossover**: configurable schemas (`crossOverSchema`), including a greedy crossover.
- **Mutation**: swap mutation with rate `pm`.
- **Protection**: optional preservation of the best gene (`--protect=best`).
- **Parallel runs**: `Constants.THREADS_NUM` controls the thread pool size.

## Plotting

Use the scripts in `scripts/` with Gnuplot. Example:

```sh
gnuplot scripts/plot_script.plt
```

## Tests

JUnit tests live in `src/test`. If you want to run them, add JUnit to the classpath:

```sh
java -cp "out;lib/log4j-1.2.17.jar;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore test.MainTest
```
