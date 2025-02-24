---
name: Improved Bi-directional RRT* for Robot Path Planning
tools: [ROS, Turtlebot3, Gazebo, A*, APF, Dynamic Window, Sensor Fusion]
image: https://raw.githubusercontent.com/vishnumandala/Improved-Bi-directional-RRT-Algorithm-for-Robot-Path-Planning-/main/results/demo.gif
description: Enhanced Bi-Directional RRT* with Artificial Potential Field for efficient path planning in complex, dynamic environments.
---

<a href="{{ site.baseurl }}/projects/" class="back-button" style="display: inline-block; margin-bottom: 20px; text-decoration: none; color: inherit;">
    <i class="fas fa-arrow-left" style="margin-right: 5px;"></i> Back to Projects
</a>

<div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
    <h1 style="margin: 0;"><strong>Improved Bi-directional RRT* for Robot Path Planning</strong></h1>
    <a href="https://github.com/vishnumandala/Improved-Bi-directional-RRT-Algorithm-for-Robot-Path-Planning-" 
        class="github-link"
       style="text-decoration: none; background-color: #f5f5f5; padding: 10px 15px; border-radius: 8px; transition: all 0.3s ease;">
        <i class="fab fa-github fa-2x" style="color: #333333; transition: color 0.3s ease;"></i>
        <style>
            a:hover {
                background-color: #333333 !important;
            }
            .github-link:hover i {
                color: #ffffff !important;
            }
            .back-button:hover {
                background-color: transparent !important;
            }
        </style>
    </a>
</div>

<p class="post-metadata text-muted">
   <span class="d-inline-block">August 10, 2023</span> &#8226; 
   <span class="tags">
      {% for tag in page.tools %}
      <span class="tag badge badge-pill text-primary border border-primary">{{ tag }}</span>
      {% endfor %}
    </span>
</p>

<div style="text-align: center; margin: 30px 0;">
    <img src="https://raw.githubusercontent.com/vishnumandala/Improved-Bi-directional-RRT-Algorithm-for-Robot-Path-Planning-/main/results/demo.gif" 
         alt="RRT* Path Planning Demo"
         style="width: 90%; max-width: 1200px; margin: auto;"
    />
    <p style="margin-top: 10px; font-style: italic; color: #666;">RRT* Path Planning Algorithm Demo</p>
</div>

## Project Overview

An enhanced implementation of the Bi-directional RRT* algorithm that combines Artificial Potential Fields for efficient path planning in complex, dynamic environments. The project focuses on improving path optimization and computational efficiency in challenging robotic navigation scenarios.

## Key Features

- **Enhanced RRT* Implementation**: Developed a bi-directional variant with improved convergence
- **Artificial Potential Field Integration**: Added dynamic obstacle avoidance capabilities
- **Real-time Performance**: Achieved 40% faster path computation than standard RRT*
- **Dynamic Replanning**: Implemented continuous path adaptation for moving obstacles
- **ROS Integration**: Seamless integration with ROS navigation stack and Turtlebot3

## Technical Architecture

### Path Planning Algorithm
```python
def bi_directional_rrt_star(start, goal, obstacles):
    tree_start = RRTTree(start)
    tree_goal = RRTTree(goal)
    
    for iteration in range(max_iterations):
        # Grow trees
        point_new = sample_free_space()
        node_start = extend_tree(tree_start, point_new)
        node_goal = extend_tree(tree_goal, point_new)
        
        # Check for connection
        if node_start and node_goal:
            if check_path(node_start, node_goal):
                path = get_path(tree_start, tree_goal, node_start, node_goal)
                return optimize_path(path)
    
    return None

def optimize_path(path):
    # Apply artificial potential field
    for point in path:
        force = compute_potential_field(point, obstacles)
        point = adjust_position(point, force)
    return path
```

## Implementation Details

### ROS Integration
```python
class PathPlannerNode:
    def __init__(self):
        rospy.init_node('rrt_star_planner')
        self.map_sub = rospy.Subscriber('/map', OccupancyGrid, self.map_callback)
        self.path_pub = rospy.Publisher('/path', Path, queue_size=10)
        
    def plan_path(self, start, goal):
        path = bi_directional_rrt_star(start, goal, self.obstacles)
        if path:
            self.publish_path(path)
```

## Performance Metrics

### Experimental Results
- **Path Computation Time**: 40% reduction compared to standard RRT*
- **Path Length**: 15% shorter paths on average
- **Success Rate**: 95% in cluttered environments
- **Replanning Speed**: Under 100ms for dynamic obstacles

## Future Development

Potential areas for future enhancement include:
- Multi-robot coordination capabilities
- Learning-based sampling strategies
- Real-time trajectory optimization
- Integration with semantic mapping 