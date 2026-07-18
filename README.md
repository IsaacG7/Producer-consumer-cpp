# Producer / Consumer Simulation

A multithreaded C++ simulation of the classic producer-consumer problem, written for an Operating Systems course. Producer threads generate random numbers and insert them into a shared circular buffer, while consumer threads remove and analyze them — flagging any prime numbers detected. Thread synchronization is handled using POSIX mutexes and semaphores.

---

## How It Works

- **Producers** sleep for a random interval, then insert a random number (0–100) into the shared buffer
- **Consumers** sleep for a random interval, then remove a number from the buffer and check if it is prime
- A **mutex** protects the buffer from concurrent access
- Two **semaphores** (`full` and `empty`) prevent producers from inserting into a full buffer and consumers from removing from an empty one
- The simulation runs for a specified duration, then all threads are joined and statistics are printed

---

## Requirements

- Linux or WSL (Windows Subsystem for Linux) — pthreads is not natively supported on Windows
- g++ compiler
- pthread library (usually pre-installed on Linux)

---

## Compilation

```bash
g++ producer_consumer.cpp -lpthread -o producer_consumer
```

---

## Usage

```bash
./producer_consumer <simulation_length> <max_thread_sleep> <num_producers> <num_consumers> <display_buffer>
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `simulation_length` | int | How long the simulation runs in seconds |
| `max_thread_sleep` | int | Maximum time in seconds a thread can sleep between operations |
| `num_producers` | int | Number of producer threads to spawn |
| `num_consumers` | int | Number of consumer threads to spawn |
| `display_buffer` | string | `yes` to print buffer state after each operation, `no` to hide it |

### Example

```bash
./producer_consumer 15 3 2 4 yes
```

Runs a 15-second simulation with a max thread sleep of 3 seconds, 2 producers, 4 consumers, and buffer snapshots enabled.

---

## Example Output

With `display_buffer` set to `yes`, each production and consumption prints the current buffer state with a write (`W`) or read (`R`) pointer indicating the active slot:

```
Producer: 140234567 produced: 47
(buffers occupied: 1)
buffers: 47    -1    -1    -1    -1
         ____  ____  ____  ____  ____
          W

consumer: 140234123 consumed 47
(buffers occupied: 0)
buffers: -1    -1    -1    -1    -1
         ____  ____  ____  ____  ____
          R
```

Empty buffer slots are shown as `-1`. Prime numbers are flagged inline:

```
consumer: 140234123 consumed 47 ***** PRIME *****
```
