Project->elevator simulation

# Elevator Simulator: Design Document

## Task 1: Problem Statement

### Q1. What is being simulated?
**My answer:** An elevator that receives requests from a CSV file and calculates metrics depending on the scheduling algorithm chosen.

**Corrected answer:** A building with one elevator (more later) serving passenger requests that are read from a CSV file. The elevator picks its next stop using a scheduling algorithm, and the program records how well that algorithm performed.

**Reasoning:** The algorithm is a setting chosen at run time. It is not part of the data.

### Q2. What is being compared, and why?
**My answer:** Different scheduling algorithms, compared by waiting time, turnaround time, etc., to see which is better for which scenario.

**Corrected answer:** Different elevator scheduling algorithms (FCFS, Priority, SSTF, SCAN, LOOK, C-SCAN) are run on the SAME workload and compared. This is the same idea as comparing CPU scheduling algorithms in OS.

**Reasoning:** Using the same input for every algorithm is what makes the comparison fair.

### Q3. What is measured?
**My answer:**
- Passenger side: waiting time, and reaching the destination fast after boarding.
- Owner side: the elevator should serve the maximum requests so that very little free time is left.

**Corrected answer:**
- **Wait time** = pickup tick - request tick
- **Turnaround time (TAT)** = drop-off tick - request tick
- **Maximum wait time** = the worst wait of any passenger (detects unfairness and starvation)
- **Total distance travelled** = floors moved (energy and cost for the owner)
- **Idle time** = ticks with nothing to do
- **Throughput** = requests completed per unit of time

**Reasoning:** "Serve maximum requests, less free time" must become measurable numbers (throughput, idle time). Maximum wait matters because an average can hide one passenger who waited far too long.

### Q4. Why not real clock time (sleep)? What is used instead?
**My answer:** (not answered)

**Corrected answer:** Use a **tick counter**, a plain integer that counts simulated time (tick 0, 1, 2, ...). One tick = one time unit, for example one floor of movement.

**Reasoning:**
- Real time is slow: 1,000 requests at 1 second per floor would take hours, but a counter finishes in milliseconds.
- Real time is not repeatable: timing varies between runs, so results would differ.
- A tick simulation is **deterministic** (same input gives same output), so tests can check exact numbers.

### Q5. What does one input row contain?
**My answer:** The lift's initial position, the request's source floor and destination floor, and the scheduling algorithm.

**Corrected answer:** `request_id, time, source_floor, destination_floor, priority`

**Reasoning:**
- `time` is needed so the simulation knows WHEN the request appears.
- `request_id` is needed to track each request in the stats.
- The scheduling algorithm is a **run setting** (command-line argument). If it were in the data, the same workload could not be run under different algorithms.
- The elevator's starting floor happens once per run, not once per request. It is a config value (default: floor 0).
- Rule: **data describes the world; settings describe how the experiment is run.**

## Task 2: Classes (to be completed)

### Homework questions (write my own answers here)
1. What is the difference between a thing that HOLDS DATA and a thing that MAKES DECISIONS? One example of each from the elevator:
   - My answer:
2. If the elevator has 3 requests waiting, what information does the decision maker need to choose the next stop?
   - My answer: