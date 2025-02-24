---
name: RC-NFQ Algorithm for Autonomous Navigation
tools: [PyTorch, Keras, CNN, DQN, Path Planning, Reinforcement Learning]
image: https://raw.githubusercontent.com/vishnumandala/Development-and-Evaluation-of-RC-NFQ-Algorithm-for-Autonomous-Navigation/main/results/demo.gif
description: Enhanced RC-NFQ (Regularized Convolutional Neural Fitted Q-Iteration) leveraging CNNs with dropout regularization for improved autonomous navigation.
---

<a href="{{ site.baseurl }}/projects/" class="back-button" style="display: inline-block; margin-bottom: 20px; text-decoration: none; color: inherit;">
    <i class="fas fa-arrow-left" style="margin-right: 5px;"></i> Back to Projects
</a>

<div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
    <h1 style="margin: 0;"><strong>RC-NFQ Algorithm for Autonomous Navigation</strong></h1>
    <a href="https://github.com/vishnumandala/Development-and-Evaluation-of-RC-NFQ-Algorithm-for-Autonomous-Navigation"
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
   <span class="d-inline-block">August 15, 2023</span> &#8226; 
   <span class="tags">
      {% for tag in page.tools %}
      <span class="tag badge badge-pill text-primary border border-primary">{{ tag }}</span>
      {% endfor %}
    </span>
</p>

<div style="text-align: center; margin: 30px 0;">
    <img src="https://raw.githubusercontent.com/vishnumandala/Development-and-Evaluation-of-RC-NFQ-Algorithm-for-Autonomous-Navigation/main/results/demo.gif" 
         alt="RC-NFQ Algorithm Navigation Demo"
         style="width: 90%; max-width: 1200px; margin: auto;"
    />
    <p style="margin-top: 10px; font-style: italic; color: #666;">RC-NFQ Algorithm Navigation Demo</p>
</div>

## Project Overview

An enhanced implementation of the RC-NFQ (Regularized Convolutional Neural Fitted Q-Iteration) algorithm that leverages Convolutional Neural Networks with dropout regularization for improved autonomous navigation. The project focuses on developing a more efficient and robust approach to autonomous navigation in dynamic environments.

## Key Features

- **Enhanced RC-NFQ Implementation**: Developed an improved version with CNN and dropout regularization
- **Reduced Collision Rate**: Achieved 15% reduction in collision rates compared to baseline
- **Improved Training Efficiency**: Reduced training latency by 30%
- **Optimized Performance**: Outperformed traditional NFQ/DQN baselines in navigation tasks
- **Streamlined Pipeline**: Implemented efficient training pipeline in HighwayEnv environment

## Technical Architecture

### Neural Network Design
```python
class RC_NFQ(nn.Module):
    def __init__(self, input_channels=4, n_actions=5):
        super(RC_NFQ, self).__init__()
        # Convolutional layers
        self.conv1 = nn.Conv2d(input_channels, 32, kernel_size=8, stride=4)
        self.conv2 = nn.Conv2d(32, 64, kernel_size=4, stride=2)
        self.conv3 = nn.Conv2d(64, 64, kernel_size=3, stride=1)
        
        # Dropout for regularization
        self.dropout = nn.Dropout(0.5)
        
        # Fully connected layers
        self.fc1 = nn.Linear(self._get_conv_output(), 512)
        self.fc2 = nn.Linear(512, n_actions)
        
        # Activation functions
        self.relu = nn.ReLU()
```

### Training Pipeline
```python
def train_step(self, state, action, reward, next_state, done):
    # Convert tensors
    state = torch.FloatTensor(state).to(self.device)
    next_state = torch.FloatTensor(next_state).to(self.device)
    action = torch.LongTensor(action).to(self.device)
    reward = torch.FloatTensor(reward).to(self.device)
    done = torch.FloatTensor(done).to(self.device)
    
    # Current Q Value
    current_Q = self.model(state).gather(1, action.unsqueeze(1))
    
    # Next Q Value
    next_Q = self.target_model(next_state).detach().max(1)[0]
    target_Q = reward + (self.gamma * next_Q * (1 - done))
    
    # Compute loss and optimize
    loss = self.criterion(current_Q.squeeze(), target_Q)
    self.optimizer.zero_grad()
    loss.backward()
    self.optimizer.step()
```

## Implementation Details

### Environment Setup
```python
env = gym.make('highway-v0')
env.configure({
    'observation': {
        'type': 'Kinematics',
        'vehicles_count': 15,
        'features': ['presence', 'x', 'y', 'vx', 'vy'],
        'absolute': False
    }
})
```

### Reward Function
```python
def compute_reward(self, action, obs):
    """Custom reward function for better training convergence"""
    # Base reward for reaching goal
    reward = self.get_goal_reward()
    
    # Penalty for collisions
    if self.check_collision(obs):
        reward -= 50
        
    # Reward for maintaining safe distance
    reward += self.get_safety_reward(obs)
    
    # Reward for smooth actions
    reward += self.get_smoothness_reward(action)
    
    return reward
```

## Performance Metrics

### Training Results
- **Collision Rate**: Reduced from 18% to 3%
- **Training Time**: 30% reduction (from 8 hours to 5.6 hours)
- **Success Rate**: 92% in dynamic environments
- **Memory Usage**: 40% reduction through efficient state representation

## Future Development

Potential areas for future enhancement include:
- Integration with real-world robotic systems
- Extension to multi-agent scenarios
- Implementation of advanced reward mechanisms
- Adaptation to different environmental conditions 