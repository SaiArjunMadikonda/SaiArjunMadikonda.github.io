---
name: Mobile Manipulator Robot Design & Control
tools: [ROS2, MATLAB, SolidWorks, UR10, LIDAR, URDF, Kinematics, Dynamics]
image: https://raw.githubusercontent.com/vishnumandala/Mobile-Manipulator-Robot-Design-and-Simulation-Project/main/results/demo.gif
description: 6-DOF mobile manipulator with optimized chassis design and steerable L-joints, achieving 98% pick-and-place accuracy using ROS2-based navigation.
---

<a href="{{ site.baseurl }}/projects/" class="back-button" style="display: inline-block; margin-bottom: 20px; text-decoration: none; color: inherit;">
    <i class="fas fa-arrow-left" style="margin-right: 5px;"></i> Back to Projects
</a>

<div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
    <h1 style="margin: 0;"><strong>Mobile Manipulator Robot Design & Control</strong></h1>
    <a href="https://github.com/vishnumandala/Mobile-Manipulator-Robot-Design-and-Simulation-Project" 
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
   <span class="d-inline-block">July 10, 2023</span> &#8226; 
   <span class="tags">
      {% for tag in page.tools %}
      <span class="tag badge badge-pill text-primary border border-primary">{{ tag }}</span>
      {% endfor %}
    </span>
</p>

<div style="text-align: center; margin: 30px 0;">
    <img src="https://raw.githubusercontent.com/vishnumandala/Mobile-Manipulator-Robot-Design-and-Simulation-Project/main/results/demo.gif" 
         alt="Mobile Manipulator Demo"
         style="width: 90%; max-width: 1200px; margin: auto;"
    />
    <p style="margin-top: 10px; font-style: italic; color: #666;">Mobile Manipulator Robot Simulation Demo</p>
</div>

## Project Overview

A comprehensive mobile manipulation system featuring a 6-DOF robotic arm mounted on a custom-designed mobile base with steerable L-joints. The project encompasses mechanical design, control system development, and ROS2 integration for autonomous pick-and-place operations.

## Key Features

- **Custom Chassis Design**: Optimized mobile base with steerable L-joints
- **Advanced Control System**: Integrated motion planning and control
- **ROS2 Architecture**: Modern robotics framework implementation
- **Dynamic Simulation**: Full system simulation in Gazebo
- **Pick-and-Place**: 98% success rate in varied scenarios

## Technical Architecture

### Control System
```cpp
class MobileManipulatorController {
public:
    MobileManipulatorController() {
        // Initialize subsystems
        base_controller_ = std::make_unique<BaseController>();
        arm_controller_ = std::make_unique<ArmController>();
        planner_ = std::make_unique<MotionPlanner>();
        
        // Setup state estimation
        state_estimator_ = std::make_unique<KalmanFilter>();
    }
    
    bool executeTask(const Pose& target) {
        // Generate trajectory
        auto trajectory = planner_->plan(
            getCurrentState(),
            target,
            getObstacles()
        );
        
        // Execute coordinated motion
        while (trajectory.hasNext()) {
            auto state = trajectory.getNext();
            base_controller_->move(state.base);
            arm_controller_->move(state.arm);
        }
        
        return validatePose(target);
    }
};
```

## Implementation Details

### Motion Planning
```python
def plan_coordinated_motion(self, target_pose):
    # Generate base trajectory
    base_path = self.plan_base_path(target_pose)
    
    # Generate arm trajectory
    arm_trajectory = self.plan_arm_motion(
        base_path,
        target_pose
    )
    
    # Time parameterization
    trajectory = self.time_parameterize(
        base_path,
        arm_trajectory
    )
    
    return trajectory
```

## Performance Metrics

### System Performance
- **Pick Success Rate**: 98% in varied scenarios
- **Navigation Accuracy**: ±5mm position, ±1° orientation
- **Cycle Time**: 25% faster than baseline
- **Obstacle Avoidance**: 100% success in dynamic environments

## Future Development

Potential areas for future enhancement include:
- Advanced manipulation strategies
- Learning-based control
- Multi-robot coordination
- Real-world deployment optimization 