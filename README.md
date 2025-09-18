# Real-Time Scheduling Simulator (EDF / RM / Strict LST)

##  Project Overview
This project implements three widely studied **real-time scheduling algorithms**, namely:
- **EDF (Earliest Deadline First)**
- **RM (Rate Monotonic)**
- **Strict LST (Least Slack Time)**

These algorithms are commonly used in **real-time systems** to ensure that multiple tasks can be scheduled and executed within their **periodic constraints (periods)** and **deadlines**.  
The simulator models task arrivals, queue management, scheduling decisions, execution, and potential **missed deadlines**.

---

##  System Workflow

### Step 1: Load Task File
Each input file defines multiple periodic tasks with parameters such as:
- **Phase time** – initial release time  
- **Period** – task recurrence cycle  
- **Relative deadline** – deadline relative to release  
- **Execution time** – required execution time  
- **Task ID** – task identifier  

These tasks are stored in a `Mytask` structure.

---

### Step 2: Build Ready Queue
- A **deque** (double-ended queue, linked-list structure) is used to model the ready queue.  
- The ready queue stores tasks that are ready to execute, ordered by **priority** according to the chosen scheduling algorithm.

---

### Step 3: Scheduling Algorithms

#### 1. **EDF (Earliest Deadline First)**
- Priority rule: Task with the **earliest absolute deadline** is scheduled first.  
- Workflow:
  1. Perform schedulability test (utilization check).  
  2. Remove any jobs in the ready queue that already **missed their deadline**.  
  3. Admit new tasks that arrive at the current time.  
  4. Sort the ready queue by **earliest deadline**.  
  5. Execute the highest-priority task for one time unit and log results.  
  6. Update system clock.  
  7. At the end, report total **missed deadlines / total jobs**.  

---

#### 2. **RM (Rate Monotonic)**
- Priority rule: **Shorter period = higher priority**.  
- Workflow is similar to EDF but tasks are ordered based on **period length**.

---

#### 3. **Strict LST (Least Slack Time)**
- Priority rule:  
  $$
  Slack = \text{Absolute Deadline} - \text{Remaining Execution Time}
  $$
  Smaller slack → higher priority.  
- Workflow is similar to EDF but tasks are ordered based on **slack time**.

---

##  Algorithm Comparison

| Algorithm   | Priority Rule          | Advantages                         | Disadvantages                        |
|-------------|------------------------|-------------------------------------|---------------------------------------|
| **EDF**     | Earliest absolute deadline | Maximizes CPU utilization efficiency | Higher computation cost, dynamic reordering required |
| **RM**      | Shorter period first   | Simple, static priority assignment  | Not always optimal, may miss deadlines |
| **Strict LST** | Least slack time    | Accurately reflects real-time urgency | High complexity, requires frequent slack updates |

---

##  Execution Results
- The system outputs scheduling logs for each time unit.  
- Final results include:
  - **Total number of tasks**  
  - **Number of missed deadlines**  
  - **System feasibility** (whether all tasks meet their deadlines under given conditions)  

---

│  ├─ scheduler_rm.py      # RM scheduling simulation
│  ├─ scheduler_lst.py     # Strict LST scheduling simulation
│  └─ utils.py             # Utilities (feasibility checks, logging)
└─ output/                 # Simulation results (logs / statistics)
