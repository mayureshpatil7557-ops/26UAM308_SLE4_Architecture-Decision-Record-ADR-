# SLE-4: Final Viva & Architecture Decision Record (ADR)



## About this repository

This repository contains my SLE-4 submission. It records one important design decision from my grid-based robot pathfinding project as an **Architecture Decision Record (ADR)** and explains the evidence behind it. SLE-4 connects my earlier work:

| SLE | What I did | Link |
| --- | --- | --- |
| SLE-1 | Built BFS and A* search code for grid pathfinding, with an AI contribution log | – |
| SLE-2 | Profiled BFS vs A* (timeit, node counting, py-spy flame graphs) | [SLE-2 repository](https://github.com/mayureshpatil7557-ops/26UAM308_SLE2) |
| SLE-3 | Designed the system using all four C4 levels | [SLE-3 repository](https://github.com/mayureshpatil7557-ops/SLE3_ArchitectureDesign-C4Models) |
| SLE-4 | Wrote an ADR justifying the default algorithm choice | This repository |

## Files

| File | Description |
| --- | --- |
| `SLE4_26UAM308.docx` | Full SLE-4 submission: ADR, SLE journey summary, AI contribution note, viva preparation notes |
| `README.md` | This file |

## The decision (summary)

**Title:** Use of BFS as the Default Search Strategy, with A* (Manhattan Distance) Kept as a Pluggable Alternative in the Search Engine

**Problem:** A robot must find a path from S to G on an 11 × 12 grid with obstacles. Moves are up, down, left and right, and every move costs the same.

**Decision:** The Search Engine uses **BFS by default**. A* with the Manhattan distance heuristic stays in the same container as an alternative, for use when the grid becomes larger or more open.

**Alternatives considered:**
- A* as the only algorithm
- DFS (theory only, not profiled)
- Greedy Best-First Search (theory only, not implemented)

## Evidence from SLE-2

Both algorithms were run on the same grid, start, goal and movement rules.

| Case | Path length | Nodes expanded (BFS / A*) | Avg time in ms (BFS / A*) | Faster |
| --- | --- | --- | --- | --- |
| Best (goal 0,1) | 1 | 3 / 2 | 0.0051 / 0.0066 | BFS |
| Average (goal 4,9) | 13 | 53 / 32 | 0.0799 / 0.0935 | BFS |
| Worst (goal 10,11) | 21 | 69 / 68 | 0.1044 / 0.2009 | BFS |

**Key points:**
- Both algorithms found the same path lengths, so A* gave no improvement in solution quality.
- A* expanded fewer nodes, but BFS was faster in every case, because A* has extra heuristic and priority-queue overhead.
- The SLE-3 design keeps the Manhattan heuristic in a separate module, so A* can be selected later without changing the other containers.

## Trade-offs

- BFS explores in all directions, so it may become slower than A* on much larger or more open grids.
- The results come from one 11 × 12 grid and three goals, so larger grids need to be re-profiled.
- BFS assumes equal move costs; weighted cells would need A* or Dijkstra.

## AI contribution

- **Tools used:** ChatGPT (SLE-1 to SLE-3) and Claude (to help draft and structure the ADR).
- **AI helped with:** organising the ADR format, presenting the SLE-2 numbers as evidence, and improving the wording.
- **I did myself:** implemented the algorithms, ran the experiments and recorded the results, designed the C4 model, reviewed the ADR against my own SLE-2 and SLE-3 work, and I will defend the decision in the viva.
