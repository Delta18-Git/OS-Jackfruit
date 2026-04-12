# CPU Scheduling Experiments

This document describes the scheduling experiments we run with the mini-container runtime to observe Linux scheduler behavior.

## Experiment A: Nice Value Comparison

### Objective
Demonstrate that nice values affect CPU time allocation between competing processes.

### Setup
- Container 1: `high_prio` - CPU-bound workload with high priority (nice=-20)
- Container 2: `low_prio` - CPU-bound workload with low priority (nice=19)

### Commands
```bash
./engine run high_prio rootfs-alpha "/bin/cpu_hog 15" --nice -20
./engine run low_prio rootfs-beta "/bin/cpu_hog 15" --nice 19
```

### Expected Behavior
- The CFS (Completely Fair Scheduler) allocates more CPU time to the higher-priority (lower nice) container
- The `high_prio` container should complete its 15-second workload before `low_prio`
- Monitor with `ps` to see both containers competing; note the RSS column and completion order

### Observed Behavior
1. Which container completes first
2. The nice column in `ps` output matches the containers (-20 vs 19)
3. Elapsed time difference between the two containers 

---

## Experiment B: CPU-bound vs I/O-bound (Same Nice)

### Objective
Demonstrate scheduler responsiveness when mixing CPU-bound and I/O-bound workloads.

### Setup
- Container 1: `cpu_work` - CPU-bound workload at nice=0
- Container 2: `io_work` - I/O-bound workload (frequent sleep/yield) at nice=0

### Commands
```bash
./engine run cpu_work rootfs-alpha "/bin/cpu_hog 20" --nice 0
./engine run io_work rootfs-beta "/bin/io_pulse 20 200" --nice 0
```

### Expected Behavior
- Both containers have the same nominal priority (nice=0)
- The I/O-bound container (`io_pulse`) frequently yields the CPU when sleeping
- The CPU-bound container (`cpu_hog`) should steadily accumulate CPU time
- I/O container appears more "responsive" in logs (frequent progress messages)

### Observed Behavior
1. I/O container completes all iterations while CPU container is still running
2. CPU container shows steady progress (accumulator increasing)
3. RSS of CPU container may grow as it keeps computing

---

## Running Experiments

Use the provided script to run both experiments:

```bash
cd src
./run_experiments.sh
```
