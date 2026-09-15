# Smart Construction Project Resource Planning System

# 1. Problem Statement
- Construction projects involve multiple interdependent activities that must be completed in a proper sequence.
- Resources such as labour, materials, equipment, time, and budget are limited and need proper allocation.
- Poor resource allocation can cause resource conflicts, idle workers/equipment, increased costs, and project delays.
- Improper material planning can result in material shortages, over-ordering, and wastage.
- Changes or delays in one construction activity can affect several dependent activities.
- Manual planning becomes difficult as the number of activities, resources, and dependencies increases.
- There is a need for a simple and smart computational system that can organize activities, track resources, detect conflicts, and assist in scheduling.
- The proposed system will use DSA concepts such as Trees and Graphs to represent the project structure and activity dependencies and apply suitable algorithms for resource planning.


# 2. Problem Context
- Construction projects involve multiple activities that depend on one another, such as excavation → foundation → columns → beams → finishing.
- Each activity requires different resources, including labour, materials, equipment, time, and budget.
- Resources are often limited, so assigning the same resource to multiple activities can create conflicts and delays.
- Poor planning can lead to idle labour/equipment, material shortages, over-ordering, wastage, increased costs, and schedule overruns.
- A delay in one activity can affect several dependent activities, making it difficult to manually determine the overall impact.
- As the size and complexity of construction projects increase, manual or experience-based planning becomes more difficult to manage efficiently.
- Existing research is increasingly using digital technologies, optimization, simulation, and AI to address these challenges, but many approaches are complex and may be difficult to implement in simpler project environments.
- The literature also indicates a gap between research-based resource-management methods and their practical implementation at construction sites.
Therefore, there is an opportunity to develop a simple, structured, and interpretable system that combines activity planning with resource management.
- In the proposed system, Tree data structures can organize the construction project into a hierarchical structure, while Graph data structures can represent dependencies between activities.
- DSA algorithms can then be applied to determine activity order, identify dependencies, detect resource conflicts, prioritize activities, and support resource allocation.

# 3. Problem understanding
- Construction projects involve many activities, limited resources, and complex dependencies, making manual resource planning difficult.
- Poor allocation of labour, materials, and equipment can lead to delays, idle resources, wastage, and increased project costs.
- Existing research has already proposed solutions using optimization, simulation, BIM, AI, and machine learning. Therefore, our project is not claiming to invent construction resource planning for the first time.
- However, many existing approaches are complex, computationally expensive, data-dependent, or designed for large-scale professional applications.
- There is a gap for a simpler, transparent, and educational resource-planning system that demonstrates how fundamental DSA can be applied to a real construction problem.
- Our project aims to bridge this gap by combining project structure, activity dependencies, resource availability, and task prioritization in one system.
- Instead of restricting ourselves to Tree and Graph, we can select different data structures according to the problem, such as Graphs for dependencies, Trees for project hierarchy, Priority Queues for task prioritization, Hash Tables for resource lookup, and Queues for pending activities.
- The system can provide explainable decisions, such as why an activity is prioritized, why a resource conflict occurs, and which activities may be affected by a delay.

# 4. Objective
- To organize construction activities and their dependencies efficiently.
- To manage and allocate limited labour, materials, and equipment.
- To detect resource conflicts, shortages, and scheduling issues.
- To prioritize activities and determine an efficient execution order.
- To analyze how delays in one activity affect dependent activities.
- To apply suitable data structures and algorithms such as Trees, Graphs, Priority Queues, Hash Tables, and Queues.
- To develop a simple, efficient, and interpretable construction resource-planning system.
