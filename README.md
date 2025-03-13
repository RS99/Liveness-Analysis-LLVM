
# Liveness Analysis in LLVM

This project implements a **Liveness Analysis pass** using LLVM for a custom compiler. The pass calculates the `gen`, `kill`, `in`, and `out` sets for each basic block using a worklist algorithm.

## 🚀 Objective
- To understand the working of liveness analysis and implement it in the form of LLVM passes.
- Compute accurate liveness information for variables at each basic block using data-flow analysis.

---

## 🏆 Tasks Completed
- Established the environment for building the LLVM-based liveness analysis.
- Implemented the liveness analysis algorithm using a worklist approach.
- Debugged and optimized the calculation of `LIVEOUT`, `UEVAR`, and `VARKILL` sets.
- Automated testing using a custom `demo.sh` script.

---

## 🔥 Challenges Faced and Solutions
### Challenge 1: Setting the Build Environment
- Initially faced issues while setting up the build environment using files from a previous project.
- **Solution:** Created a fresh build folder using shell commands:
```bash
$ mkdir build
$ cd ./build
$ cmake -DLT_LLVM_INSTALL_DIR=/lib/llvm-17 ..
$ make
$ cd ..
```

### Challenge 2: Incorrect `LIVEOUT` Values in Test Cases
- Faced incorrect output due to improper handling of IN and OUT sets.
- **Solution:** Fixed the issue by carefully recalculating the sets based on the algorithm provided in the course material.

---

## 🧠 Algorithm Overview
1. **Initialization:** Initialize `gen`, `kill`, `in`, and `out` sets for each basic block.
2. **Worklist Initialization:** Initialize the worklist with the reverse post-order of basic blocks.
3. **Worklist Processing:**  
   - Remove a basic block from the front of the worklist.
   - Compute `gen` and `kill` sets based on store and load instructions.
   - Update the `OUT` set as the union of `IN` sets of successors.
   - Update the `IN` set using the equation:
     ```
     IN = gen ∪ (OUT - kill)
     ```
   - If `IN` changes, mark the analysis as changed and repeat.

4. **Convergence:** The analysis terminates once no `IN` set changes between iterations.

---

## 📌 Data Structures
- `Struct Info`: Contains `gen`, `kill`, `in`, and `out` sets.
- `std::map<BasicBlock *, Info> infos`: Maps each basic block to its info structure.
- `std::deque<BasicBlock *> worklist`: For worklist-based processing.
- `std::set<Value *> gen, kill, in, out`: Holds LLVM IR values.

---

## 📂 Code Snippets
### Creating the build environment:
```bash
$ mkdir build
$ cd ./build
$ cmake -DLT_LLVM_INSTALL_DIR=/lib/llvm-17 ..
$ make
```

### Running Liveness Analysis on Test Case:
```bash
$ clang -S -emit-llvm test_case1.c -o test_case1.bc
$ opt -load-pass-plugin ./build/lib/LivenessAnalysis.so -passes=liveness -disable-output test_case1.bc
```

---

## 🎯 Testing
1. Created a `demo.sh` file to automate testing:
```bash
#!/bin/bash

echo "Compiling..."
make

echo "Running test cases..."
for file in test_cases/*.c; do
    clang -S -emit-llvm $file -o ${file%.c}.bc
    opt -load-pass-plugin ./build/LivenessAnalysis.so -passes=liveness -disable-output ${file%.c}.bc
done
```

2. Successfully tested on multiple cases to generate accurate `LIVEOUT`, `UEVAR`, and `VARKILL` sets.

---

## 🚀 Learnings
- Learned how LLVM compilation works and how to set up an LLVM-based project.
- Understood the underlying structure of `gen`, `kill`, `in`, and `out` sets and how to compute them.

---

## 🔮 Next Steps
- Fine-tune the algorithm to handle complex control flows.
- Extend the analysis to support additional LLVM instructions.

---

## 🌟 Technologies Used
- **Programming:** C++  
- **Compiler Toolchain:** LLVM  
- **Build:** CMake  
- **Scripting:** Shell  

---

## ✅ Status: Complete
