# OS-Jackfruit: Multi-Container Runtime

A lightweight Linux container runtime in C with a long-running supervisor and a kernel-space memory monitor.

## 1. Team Information
- **Name 1:** Vinaayak G Dasika, **SRN:** PES2UG24CS588
- **Name 2:** Vignesh Nunna, **SRN:** PES2UG24CS582

## 2. Build, Load, and Run Instructions

### Building the Project
```bash
cd src
make
```

### Loading the Kernel Module
```bash
sudo insmod src/monitor.ko
ls -l /dev/container_monitor
```

### Running the Supervisor
```bash
sudo ./src/engine supervisor ./rootfs-base
```

### Launching Containers (CLI)
*To be implemented in Task 2*

### Clean Up
```bash
sudo rmmod monitor
```

## 3. Demo with Screenshots
*To be added later*

## 4. Engineering Analysis
*To be added later*

## 5. Design Decisions and Tradeoffs
*To be added later*

## 6. Scheduler Experiment Results
*To be added later*
