# Week 2

[TOC]

## Intelligent Agent

### Agent and environments

- Agent: An `agent` is anything that  can be viewed as:
  - perceiving its `environment` through sensors and acting upon that environment through actuators.
  - An agent program runs in cycles of : `(1) perceive, (2) think and (3) act`.
- Agent = Architecture + Program
- Human Agent: 
  - Sensors: eyes, ears, and other organs
  - Actuators: hands, legs, mouth, and other body parts.
- Robotic Agent:
  - Sensors: Cameras and infrared range finders
  - Actuators: Various motors

> PEAS for a self-driving car:
>
> P: Performance: Safety, time, legal drive comfort
>
> E: Environment: Roads, other cars, pedestrians, road signs
>
> A: Actuators: Steering, accelerator, brake, signal, horn
>
> S: Sensors: Camera, sonar, GPS, Speedometer, odometer, accelerometer, engine sensors, keyboard

### Environment types

- **Fully observable (vs. partially observable):** An agent's sensors give it access to the complete state of the environment at each point in time
- **Deterministic (vs. stochastic):** The next state of the environment is completely determined by the current state and the action executed by the agent. (If the environment is deterministic except for the actions of other agents, then the environment is strategic)
- **Episodic (vs. sequential):** The agent's experience is divided into atomic "episodes" (each episode consists of the agent perceiving and then performing a single action), and the choice of action in each episode depends only on the episode itself.
- **Static (vs. dynamic): ** The environment is unchanged while an agent is deliberating. (The environment is semi-dynamic if the environment itself does not change with the passage of time but the agent's performance score does.)
- **Discrete (vs. continuous): ** A limited number of distinct, clearly defined percepts and actions. E.g., checkers is an example of a discrete environment, while self-driving car evolves in a continuous one.
- **Single agent (vs. multi-agent): ** An agent operating by itself in an environment.
- **Known (vs. Unknown): ** The designer of the agent may or may not have knowledge about the environment makeup. If the environment is unknown the agent will need to know how it works in order to decide.

