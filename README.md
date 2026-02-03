# nanoStream

**nanoStream** is an ultra‑low‑latency event pipeline for Python, powered by a C++ lock‑free engine and pybind11.  
It’s designed for real‑time bots, data ingestion systems, and HFT‑style event processing.

---

## 🚀 Features

- ⚡ Lock‑free ring buffer (SPSC or MPMC)
- 🧠 C++ filtering engine with optional SIMD acceleration
- 🕒 Microsecond‑accurate batching and pacing
- 🔁 GIL‑free ingestion and dispatch
- 🔌 Seamless Python integration via pybind11
- 🧱 Built with C++20 and CMake

---

## 📦 Installation

```bash
pip install .
```

---

## 🧪 Quick Start

```python
from nanostream import Pipeline

def on_event(event):
    print("Received:", event)

p = Pipeline(buffer_size=4096)
p.start(on_event)
```

---

## 🧱 Architecture

```mermaid
flowchart LR
    subgraph C++ Engine
        B[C++ Ingest Thread]
        C[Lock-Free Ring Buffer]
        D[Filtering & Dedup Engine]
        E[Batching / Rate Limiter]
    end

    subgraph Python Layer
        F[Python Callback → Discord]
    end

    A[Reddit API] --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

```mermaid
sequenceDiagram
    participant R as Reddit API
    participant I as C++ Ingest Thread
    participant Q as Lock-Free Ring Buffer
    participant F as Filtering Engine
    participant B as Batching Engine
    participant P as Python Callback

    R ->> I: Fetch new posts
    I ->> Q: Push events
    Q ->> F: Pop event
    F ->> B: Filtered event
    B ->> P: Dispatch to Python
    P ->> P: Send to Discord
```
