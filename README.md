![polSim Banner](polsim.png)


# polSim: Population Simulation Platform for Social Science Research

## Overview
`polSim` is a platform designed for simulating synthetic populations in social science research. The platform leverages large language models to generate populations with user-defined attributes, facilitating a broad range of simulations from simple surveys to complex behavioral studies and elections.

## Features
- **Modular Architecture**:
   - The simulation of populations is a separate process from experiments run on the population.
   - Compatible with models from OpenAI's API, as well as locally run models.
- **Population Representation**:
  - Agents are represented as individual `.yaml` files within a structured directory.
  - Users can define attribute categories, options, and sampling methods.
- **Configuration**: Both manual and GPT-assisted configuration methods are available.
- **Interface Options**: Includes a command line interface with plans for a future graphical user interface (GUI).

## Timeline
- [x] Generate roadmap
- [x] Decide on agent architecture options
- [ ] End-to-end proof of concept with OpenAI models
- [ ] Build model-agnostic simulation infrastructure
- [ ] Write a survey experiment script
- [ ] Write an election simulation script
- [ ] Publish beta version

## Installation
Clone the repository and navigate to the project directory:
```
git clone [repository-url]
cd polSim
```
Install the required dependencies:
```
pip install -r requirements.txt
```

### Usage
To generate a population, run:
```
python generate_population.py path/to/your/simulation-config.yaml
```
This will create a population of agents with attributes defined in your `simulation-config.yaml`.thon 

To run an experiment, run:
```
python run_experiment.py path/to/your/experiment-config.yaml path/to/your/simulation-config.yaml
```


## Configuration
Configuration is managed via a series of YAML files. These files specifies attribute categories, options, population sizes, and other simulation parameters. 

### Simulations
Each simulation has its own config file kept in the `configs/sims/` directory. `simulation-config-example.yaml` provides an example configuration for the simulation of a synthetic population.

### Experiments
Each experiment has its own config file kept in the configs/experiments/` directory. There should be an experiment config corresponding to the python script that implements each experiment in the `src/experiments` directory. `configs/experiments/experiment-config.yaml` and `src/experiments/example-experiment.py` provide an example configuration and script for an experiment, respectively. 

Each experiment should specify a _sample_ and a _schedule_. Schedules can be arbitrarily simple or complicated: for example, an experiment can specfy some sample of the specified synthetic population to query (survey simulation), implement an RCT to determine the effect of some treatment (survey experiment simulation) or implement a multi-step election simulation where agents are polled about an upcoming election and iteratively update their knowledge about the likely outcomes before casting their vote (election simulation). 

## Synthetic Populations
Synthetic populations are made up of LLM agents, with the profile of each agent being encoded as a discrete YAML file. All agents are made up of three key building blocks: traits (including demographic information, preferences/tastes, etc.), information (including potentially the history of the simulation so far), and a decision rule. Any of these blocks can be left empty. 

### Example Agent

All agents are made up a set of fundamental building blocks: (1) /core traits/ central to the agent's identity; (2) rules of behavior that the agent must ascribe to; (3) /memory/ of previous events; (4) a description of the /task/ at hand that the agent must perform; (5) the /thought/ that the agent makes when reasoning about which option to choose; (6) the /choice/ that the agent makes about the task that they are provided, given all preceding blocks; and (7) the /observation/ of the outcome of making the previous choice.

All agent actions and outcomes occur through one of the following function calls: `assign_task`, `receive_information`, `generate_thought`, `execute_task`, `observe_outcome`,  `update_memory`.
In `assign_task`, an agent is provided a task by either by the simulation or by another agent.
Similarly, in `receive_information`, an agent is provided information by either the simulation or another agent.
In `generate_thought`, a call to a LLM is made to generate a thought about the action at hand using its traits, rules, memory, task description, and thought.
In `execute_task`, a call to an LLM is made using the same blocks as the previous step plus thought, and the agent decides how to act.
In `observe_outcome`, an agent observes the outcome of the task, and the consequences of the action.
in `update_memory`, an agent updates its memory of the simulation, where each memory is a tuple of the form `(task, thought, choice, outcome)`.

We can think about instantiating an agent as a class in python.
```python
class Agent:
    def __init__(self, traits, rules, memory, task, thought, choice):
        self.traits = traits
        self.rules = rules
        self.memory = memory
        self.task = task
        self.thought = thought
        self.choice = choice
      
    def assign_task(self, task):
        self.task = task
        
    def receive_information(self, information):
        self.information = information
        
    def generate_thought(self):
        self.thought = LLM.generate_thought(self.traits, self.rules, self.memory, self.task, self.thought)
        
    def execute_task(self):
        self.choice = LLM.execute_task(self.traits, self.rules, self.memory, self.task, self.thought, self.choice)
        
    def observe_outcome(self, outcome):
        self.outcome = outcome
        
    def update_memory(self):
        self.memory.append((self.task, self.thought, self.choice, self.outcome))
```


In the simplest simulations, where one or more agents are asked to give a response to e.g. a survey, each agent will consider its core traits, the rules of behavior, the description of the task, and generate a thought about how they should respond to the task. Then, using the thought, the agent will decide how to act. Because there is no history of interaction, the memory block will be empty in this scenario. 

### Defining Attributes
Edit the `config.yaml` to specify the attributes of the population you want to simulate. The configuration structure is as follows:
```
attributes:
  age:
    type: range
    min: 18
    max: 65
  occupation:
    type: categorical
    options:
      - Student
      - Employed
      - Unemployed
      - Retired
```

## License
`polSim` is released under the MIT License. For more details, see the LICENSE file in the repository.

## Troubleshooting
Common issues and troubleshooting steps will be documented in the `TROUBLESHOOTING.md` file. For unresolved issues, please open a ticket on the GitHub issue tracker.

## Contact
For collaboration inquiries or further assistance, please contact mcbosley@umich.edu](mcbosley@umich.edu)
