# Concurrent-Math-Engine: High-Performance Linear Algebra Framework

## Project Overview
This project is a high-concurrency engine designed to execute complex linear algebra operations, such as Matrix Addition, Multiplication, Transpose, and Negation, across a multi threaded architecture. The core focus of the system is to maximize CPU utilization by decomposing nested mathematical expressions into atomic tasks, which are then distributed across a specialized, fatigue-aware thread pool.
* **This project was developed as a joint academic effort at Ben-Gurion University in collaboration with a project partner.**
* **Final Version Showcase**: This repository contains the final, refactored version of the engine, optimized for performance and code clarity.

## Core Engineering Features

### 1. Custom Fatigue-Aware Thread Pool (TiredExecutor)
Instead of relying on standard Java executor libraries, I implemented a low-level thread pool from scratch to manage thread lifecycles manually:
* **Monitor-Based Synchronization**: The entire scheduling and worker-management logic is built using core Java monitor primitives, specifically synchronized blocks, wait(), and notifyAll().
* **Dynamic Fatigue Scheduling**: To ensure workload fairness, the executor tracks a "fatigue factor" for each worker thread. New tasks are dynamically assigned to the least-fatigued idle threads, preventing performance bottlenecks and ensuring an even distribution of work across the system.

### 2. Thread-Safe Shared Memory Architecture
The engine manages shared matrix resources using a tiered locking strategy designed to balance safety with high performance:
* **Fine-Grained Locking**: Each SharedVector (representing an individual row or column) is protected by a ReentrantReadWriteLock.
* **Concurrent Reads & Exclusive Writes**: This model allows multiple worker threads to read data simultaneously, while ensuring that write operations remain strictly atomic and thread-safe.
* **In-Place Computation**: The system is optimized for memory efficiency by updating results directly within the shared matrix structures, significantly reducing the overhead of object allocation during large scale operations.

### 3. Tree-Based Task Decomposition
The engine parses complex JSON based computations into a recursive tree of ComputationNode instances:
* **Iterative Resolution**: The engine identifies "resolvable" nodes those whose operands are already concrete matrices and prepares them for execution.
* **Scalable Task Execution**: Operations are decomposed into individual Runnable tasks, allowing the custom thread pool to execute massive matrix operations in parallel across all available cores.

## Project Structure
The project is organized according to the standard Maven directory structure, separating core engine logic from the comprehensive unit testing suite used to validate matrix dimensions and arithmetic correctness.
