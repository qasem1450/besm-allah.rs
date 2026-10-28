# NexusRing 🦀⚡

A high-performance, lock-free Single-Producer Single-Consumer (SPSC) Ring Buffer written in **Rust**, engineered for ultra-low latency inter-thread communication and high-frequency data pipelines.

---

## 🏗️ Architectural Overview

Traditional multi-threaded data sharing relies on locks (`Mutex`, `RwLock`), which introduce kernel-level context switches, OS scheduling overhead, and unpredictable latency spikes (microseconds). 

**NexusRing** achieves **zero-lock, nanosecond-level throughput** by utilizing:
- **Hardware-Level Atomics**: Managed via `AtomicUsize` with strict memory ordering (`Acquire` / `Release`) to prevent instruction reordering without locking the bus.
- **Interior Mutability & Safety**: Uses `UnsafeCell` encapsulated safely within a bounded `Box<[UnsafeCell<Option<T>>; CAP]>` array.
- **Zero Allocations on Hot Path**: Fixed-size circular buffer allocated entirely on initialization using Rust's modern `std::array::from_fn`.

---

## 🚀 Performance & Benchmark

The engine simulates high-frequency data ingestion and low-latency packet processing across concurrent threads:
- **Capacity**: 1,024 slots (Power of 2 alignment).
- **Workload**: 10,000 high-frequency payload transmissions.
- **Synchronization**: Zero locks, pure `spin_loop()` optimizations.

---

## 🛠️ Core Implementation

```rust
pub struct SpscRingBuffer<T, CAP: const usize> {
    buffer: Box<[UnsafeCell<Option<T>>; CAP]>,
    head: AtomicUsize, // Read index (Consumer)
    tail: AtomicUsize, // Write index (Producer)
}git clone [https://github.com/qasem1450/nexus-ring.git](https://github.com/qasem1450/nexus-ring.git)
cd nexus-ring
cargo run --release
# besm-allah.rs
