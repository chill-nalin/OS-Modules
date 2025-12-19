# OS Modules

A collection of modular Operating Systems concepts implemented in C/C++, including schedulers, loaders, shells, and multithreading primitives. This repository demonstrates core OS mechanisms in a clean, educational, and extensible format.

---

## 🚀 Overview

This repository contains multiple standalone OS modules designed to mirror how real operating systems handle tasks like process scheduling, command execution, program loading, and thread management.

Each module is self-contained, well-structured, and suitable for learning, teaching, or using as reference implementations.

---

## 📂 Repository Structure

```
OS-Modules/
├── Multithreader/
├── Scheduler/
├── Simple-Loader/
├── Smart-Loader/
├── Simple-Shell/
├── Makefile
└── README.md
```

### **1. Multithreader**

Implements basic user-level thread creation, context switching, and cooperative scheduling. Useful for understanding how lightweight threading is implemented.

### **2. Scheduler**

Contains different scheduling strategies (FCFS, Round Robin, etc.) implemented for simulated processes.

### **3. Simple-Loader**

Minimal program loader that simulates how ELF/binary loading works in operating systems.

### **4. Smart-Loader**

An extended version of the loader with additional optimizations or richer parsing logic.

### **5. Simple-Shell**

A basic UNIX-like shell supporting:

* Command parsing
* Fork/exec model
* Error handling
* Minimal built-in commands

---

## 🧠 Key Concepts Covered

* Process scheduling strategies
* Thread management and context switching
* Program loading and memory preparation
* Shell design and command execution model
* Input parsing and IPC fundamentals

---

## 🛠️ Build & Run

### **Build All Modules**

```bash
make
```

### **Clean compiled files**

```bash
make clean
```

### **Running individual modules**

Each module contains its own `README.md` or usage notes internally. Typical usage:

```bash
cd Scheduler
./scheduler
```

---

## 📘 Requirements

* GCC or Clang
* Make
* Linux/macOS recommended for POSIX compatibility

---

## 🤝 Contributing

Contributions are welcome! You can:

* Improve documentation
* Add new scheduling algorithms
* Add new shell features (pipes, redirection, background jobs)
* Add new modules (File system simulator, VM manager, etc.)

Fork the project, create a branch, and open a pull request.

---

## 📄 License

This project is licensed under the MIT License. You are free to use, modify, and distribute it with attribution.

---

## 📬 Contact
**Nalin Gupta** | CSE + Applied Math | IIIT Delhi  
📧 nalin24362@iiitd.ac.in | 💼 [LinkedIn](www.linkedin.com/in/-nalin-gupta)
