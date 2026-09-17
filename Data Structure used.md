# DSA Implementation Status

To keep the project practical, we distinguish between the DSA concepts that are **actually implemented in the system** and those that were evaluated during the design process but are not part of the final implementation.

The project does not need to implement every DSA topic. Each implemented structure has a specific responsibility within the construction resource-planning system. Structures that do not provide a meaningful advantage for the project's requirements have been excluded from the core implementation.

---

# 1. Actually Implemented

| Data Structure / Algorithm | Implementation | Purpose in the System                                                  | Why It Is Used                                                                                                   |
| -------------------------- | -------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **General Tree**           | Implemented    | Represents project hierarchy, phases, and sub-activities               | A construction project naturally has multiple levels, and each phase can contain more than two sub-activities.   |
| **Tree Traversal**         | Implemented    | Processes and displays the project hierarchy                           | Traversal allows the system to visit and process project phases and activities systematically.                   |
| **Graph**                  | Implemented    | Represents dependencies between construction activities                | Activities can have multiple dependencies, so a graph represents these relationships more naturally than a tree. |
| **Adjacency List**         | Implemented    | Stores the activity dependency graph                                   | The dependency graph is expected to be sparse, so storing only existing connections saves memory.                |
| **BFS**                    | Implemented    | Explores activity dependencies level by level                          | Useful for identifying activities at different dependency levels and exploring nearby dependencies.              |
| **DFS**                    | Implemented    | Traces downstream activities affected by a delay                       | Useful for following dependency chains and identifying activities that may be affected by a delayed activity.    |
| **Topological Sort**       | Implemented    | Generates an activity order that respects dependencies                 | Ensures that activities are considered only after their required predecessor activities.                         |
| **AVL Tree**               | Implemented    | Maintains ordered worker/resource records                              | Provides guaranteed O(log n) search, insertion, and deletion while keeping the tree balanced.                    |
| **Hash Table**             | Implemented    | Provides fast lookup of workers, materials, and equipment              | Resource IDs can be used as keys, providing O(1) average-case lookup.                                            |
| **Max Heap**               | Implemented    | Selects the highest-priority activity                                  | Provides quick access to the activity with the highest priority.                                                 |
| **Min Heap**               | Implemented    | Selects activities based on minimum criteria such as earliest deadline | Useful when scheduling requires selecting the smallest value, such as the earliest deadline.                     |
| **Queue**                  | Implemented    | Manages activities waiting for unavailable resources                   | FIFO processing provides a simple and fair way to manage activities waiting for shared resources.                |

---

# 2. Considered but Not Implemented

The following structures were considered during the design process but are not part of the final core implementation.

| Data Structure           | Status          | Why It Was Considered                                                      | Why It Is Not Used                                                                                                                        |
| ------------------------ | --------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **Binary Tree**          | Not implemented | Provides the basic structure behind several binary-tree-based DSA concepts | A construction project can have more than two sub-activities, so a Binary Tree is too restrictive for project hierarchy.                  |
| **BST**                  | Not implemented | Provides ordered searching based on resource IDs                           | A normal BST can become unbalanced and degrade to O(n). AVL provides the same ordered-search concept with guaranteed O(log n) operations. |
| **Adjacency Matrix**     | Not implemented | Provides O(1) direct edge checking                                         | It requires O(V²) space and can waste memory when most activities do not have direct dependencies.                                        |
| **Linked List**          | Not implemented | Useful for dynamically changing sequential records                         | The current system does not require enough frequent insertion/deletion operations to justify using it as a core structure.                |
| **Threaded Binary Tree** | Not implemented | Can improve certain binary-tree traversal operations                       | Its traversal benefits are not important enough for this system to justify the additional implementation complexity.                      |

---

# 3. Final DSA Architecture

The actual implementation can therefore be summarized as:

```text
                    SMART CONSTRUCTION
                    RESOURCE PLANNING
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ↓                ↓                ↓
     PROJECT DATA      ACTIVITY DATA    RESOURCE DATA
          │                │                │
          ↓                ↓                ↓
    GENERAL TREE         GRAPH         AVL / HASH TABLE
          │                │                │
          ↓                ↓                ↓
   TREE TRAVERSAL    ADJACENCY LIST    RESOURCE SEARCH
                           │
                           ↓
                    GRAPH ALGORITHMS
                           │
              ┌────────────┼────────────┐
              ↓            ↓            ↓
             BFS          DFS     TOPOLOGICAL SORT
              │            │            │
              ↓            ↓            ↓
         Exploration   Delay Impact   Activity Order
                       Analysis
                           │
                           ↓
                   PRIORITY MANAGEMENT
                           │
                    ┌──────┴──────┐
                    ↓             ↓
                 MAX HEAP      MIN HEAP
                    │             │
                 Priority      Deadline/
                 Activities    Minimum Criteria
                    │             │
                    └──────┬──────┘
                           ↓
                         QUEUE
                           │
                           ↓
                  Waiting Activities
                           │
                           ↓
                  RESOURCE ALLOCATION
```

---

# 4. Why These Structures Were Selected

The final implementation follows a **problem-driven approach**. Each construction requirement is mapped to a suitable data structure or algorithm.

| Construction Requirement                   | Implemented Solution | Reason for Selection                                                                 |
| ------------------------------------------ | -------------------- | ------------------------------------------------------------------------------------ |
| Organize project phases and sub-activities | **General Tree**     | Supports multiple children and naturally represents project hierarchy.               |
| Process project hierarchy                  | **Tree Traversal**   | Provides systematic processing of all project nodes.                                 |
| Represent activity dependencies            | **Graph**            | Supports multiple and complex relationships between activities.                      |
| Store sparse dependencies                  | **Adjacency List**   | Uses space according to actual relationships rather than all possible relationships. |
| Explore dependency levels                  | **BFS**              | Processes activities level by level.                                                 |
| Trace activities affected by delays        | **DFS**              | Follows dependency chains deeply.                                                    |
| Generate a valid dependency order          | **Topological Sort** | Produces an order that respects predecessor relationships.                           |
| Maintain ordered resource records          | **AVL Tree**         | Provides balanced and predictable O(log n) operations.                               |
| Find a resource quickly by ID              | **Hash Table**       | Provides O(1) average-case lookup.                                                   |
| Select the most urgent activity            | **Max Heap**         | Provides direct access to the highest-priority activity.                             |
| Select the earliest/minimum criterion      | **Min Heap**         | Provides direct access to the smallest scheduling value.                             |
| Handle activities waiting for resources    | **Queue**            | FIFO processing is suitable for managing waiting activities.                         |

This ensures that each implemented structure has a clear role instead of being included only to demonstrate a DSA concept.

---

# 5. Detailed Use and Non-Use Rationale

## General Tree — Used

### Why we use it

The project has a natural hierarchy:

```text
Project
 ├── Foundation
 │    ├── Excavation
 │    ├── PCC
 │    └── Footing
 ├── Structure
 │    ├── Columns
 │    ├── Beams
 │    └── Slabs
 └── Finishing
```

A General Tree allows a project phase to contain any number of activities.

### Why not Binary Tree?

A Binary Tree allows a maximum of two children per node. Using it would unnecessarily restrict the project structure.

---

## AVL Tree — Used

### Why we use it

Workers and other resources may need to be maintained in an ordered structure based on IDs.

AVL automatically balances itself, providing predictable:

```text
Search  → O(log n)
Insert  → O(log n)
Delete  → O(log n)
```

### Why not BST?

A normal BST can become unbalanced:

```text
10
  \
   20
     \
      30
```

Its operations can then degrade to O(n). AVL avoids this problem through automatic balancing.

---

## Hash Table — Used

### Why we use it

Many operations simply require finding a resource using its unique ID.

For example:

```text
Worker ID: 1056
       ↓
 Hash Table
       ↓
Worker Record
```

A Hash Table provides O(1) average-case lookup.

### Why not use only AVL?

AVL is useful when ordered records are required, while Hash Table is better for direct ID-based lookup. Therefore, both structures can have different responsibilities.

---

## Graph — Used

### Why we use it

Construction activities have dependencies:

```text
Excavation → Foundation → Columns → Beams → Slab
```

A directed graph represents these relationships naturally.

### Why not use Tree?

A Tree represents hierarchy well, but dependencies may involve multiple predecessors and more complex relationships.

Therefore:

```text
General Tree → Project hierarchy
Graph       → Activity dependencies
```

---

## Adjacency List — Used

### Why we use it

Most activities will have only a limited number of dependencies. An Adjacency List stores only the relationships that actually exist.

```text
Foundation → Columns, Walls
Columns    → Beams
Walls      → Plastering
```

### Why not Adjacency Matrix?

An Adjacency Matrix requires O(V²) space. For a large project, many entries may contain no relationship.

The Adjacency List requires approximately:

```text
O(V + E)
```

space, making it more appropriate for a sparse dependency graph.

---

## BFS — Used

### Why we use it

BFS explores the dependency graph level by level.

This can help the system understand how activities are distributed across dependency levels.

### Why not use only DFS?

DFS and BFS solve different exploration problems. BFS is useful for level-based exploration, while DFS is better for following dependency chains.

---

## DFS — Used

### Why we use it

DFS can follow a dependency chain deeply.

For example:

```text
Foundation
    ↓
Columns
    ↓
Beams
    ↓
Slab
    ↓
Finishing
```

If Foundation is delayed, DFS can help trace the downstream activities that may be affected.

### Why not use only BFS?

BFS is better for level-by-level exploration. DFS is more suitable when the goal is to follow dependency paths deeply.

---

## Topological Sort — Used

### Why we use it

Some activities cannot begin until their prerequisites are completed.

Topological Sort produces an order that respects these dependencies.

```text
Excavation
→ Foundation
→ Columns
→ Beams
→ Slab
```

### Why not simply use input order?

The order in which activities are entered does not guarantee that their dependencies are satisfied. Topological Sort derives the order from the dependency graph.

---

## Max Heap — Used

### Why we use it

When several activities are available, the system may need to select the most urgent one.

```text
Foundation → 90
Columns    → 80
Beams      → 70
```

A Max Heap provides quick access to the highest-priority activity.

### Why not a normal list?

Finding the highest-priority activity repeatedly in an unsorted list may require scanning multiple elements. A Max Heap is designed specifically for priority-based selection.

---

## Min Heap — Used

### Why we use it

Some scheduling decisions require the smallest value:

* Earliest deadline
* Shortest duration
* Lowest cost
* Earliest availability

A Min Heap provides quick access to the minimum value.

### Why not only Max Heap?

Max Heap handles the highest value, while Min Heap handles the lowest value. Both are useful for different scheduling criteria.

---

## Queue — Used

### Why we use it

When a required resource is unavailable, activities may need to wait.

```text
Resource unavailable

Columns → Beams → Roof
```

A Queue processes these activities using FIFO order.

### Why not Stack?

A Stack uses LIFO ordering, which would prioritize the most recently added activity rather than the activity that has been waiting longest.

---

## Binary Tree — Not Used

### Why not use it?

The project's hierarchy is not limited to two children per node.

### Why it is still relevant

Binary Tree concepts are fundamental to:

* BST
* AVL Tree
* Binary Heap

Therefore, it is part of the conceptual study but not the core implementation.

---

## BST — Not Used

### Why not use it?

A normal BST does not guarantee balance. Its performance can degrade to O(n).

Since the project requires predictable resource operations, AVL is a better choice.

---

## Adjacency Matrix — Not Used

### Why not use it?

The project is expected to have a relatively sparse dependency graph.

An Adjacency Matrix stores every possible pair of activities, including pairs with no dependency.

The Adjacency List is therefore more space-efficient for our use case.

---

## Linked List — Not Used

### Why not use it?

The current project does not have a major requirement for frequent insertion and deletion in sequential records.

Arrays or other existing structures can handle the required attendance/history information more simply.

A Linked List can be reconsidered later if the requirements change.

---

## Threaded Binary Tree — Not Used

### Why not use it?

Its main advantage is improving certain binary-tree traversal operations. However, traversal is not a major performance bottleneck in this project.

Using it would add implementation complexity without solving an important construction-planning requirement.

---

# 6. Operation Complexity of Implemented Structures

| Implemented Structure / Algorithm | Main Operation   |   Complexity |
| --------------------------------- | ---------------- | -----------: |
| **General Tree**                  | Traversal        |         O(n) |
| **General Tree**                  | Search           |         O(n) |
| **AVL Tree**                      | Search           |     O(log n) |
| **AVL Tree**                      | Insert           |     O(log n) |
| **AVL Tree**                      | Delete           |     O(log n) |
| **Hash Table**                    | Search           | O(1) average |
| **Hash Table**                    | Insert           | O(1) average |
| **Hash Table**                    | Delete           | O(1) average |
| **Max Heap**                      | Get Maximum      |         O(1) |
| **Max Heap**                      | Insert           |     O(log n) |
| **Max Heap**                      | Delete Maximum   |     O(log n) |
| **Min Heap**                      | Get Minimum      |         O(1) |
| **Min Heap**                      | Insert           |     O(log n) |
| **Min Heap**                      | Delete Minimum   |     O(log n) |
| **Queue**                         | Enqueue          |         O(1) |
| **Queue**                         | Dequeue          |         O(1) |
| **Queue**                         | Peek             |         O(1) |
| **Adjacency List**                | Add Edge         |         O(1) |
| **BFS**                           | Graph Traversal  |     O(V + E) |
| **DFS**                           | Graph Traversal  |     O(V + E) |
| **Topological Sort**              | Generate Order   |     O(V + E) |
| **Tree Traversal**                | Pre/In/Postorder |         O(n) |

Where:

* `n` = number of elements or nodes
* `V` = number of activities (vertices)
* `E` = number of dependencies (edges)

For the Hash Table, **O(1)** represents the expected average-case complexity. In the worst case, significant hash collisions can result in O(n).

---

# 7. Final Implementation Scope

### Implemented in the Project

**Trees**

* General Tree
* AVL Tree
* Tree Traversal

**Graphs**

* Graph
* Adjacency List
* BFS
* DFS
* Topological Sort

**Priority Management**

* Binary Heap
* Max Heap
* Min Heap

**Resource Management**

* Hash Table
* Queue

### Not Implemented in the Core System

* Binary Tree
* BST
* Adjacency Matrix
* Linked List
* Threaded Binary Tree

The final project focuses on a smaller set of well-justified DSA structures rather than attempting to use every structure covered in the syllabus.

> **Important:** A structure being studied or considered does not mean that it is implemented. Only structures marked as **Implemented** should be represented in the project's actual code and demonstrations.

---

# 8. Overall Design Principle

The DSA selection follows a **problem-first approach**:

```text
Real Construction Problem
          ↓
Required Operation
          ↓
Possible Data Structures
          ↓
Compare Advantages and Limitations
          ↓
Select the Most Suitable Structure
          ↓
Implement
          ↓
Analyse Complexity
```

This approach keeps the project practical and demonstrates how DSA can be applied to a real-world problem rather than using data structures only to satisfy syllabus requirements.

The objective is simple:

> **Use the right data structure for the right problem.**
