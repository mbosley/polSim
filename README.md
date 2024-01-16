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
- [ ] Generate roadmap
- [ ] Decide on agent architecture options
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

Below is an example YAML configuration for an individual agent in a synthetic population for `polSim`. This configuration file would represent one agent's profile, including their traits, information, and decision rule.

```yaml
# Agent Configuration
agent_id: 0001

# Traits of the agent
traits:
  demographic:
    age: 29
    gender: "female"
    race: "black"
    country: "Canada"
    education_level: "bachelors"
    current_occupation: "data_scientist"
    previous_occupations: ["research_assistant", "analyst"]
    income: 55000
    marital_status: "single"
    political_affiliation: "independent"

# Information that the agent possesses
information:

# Decision rule for the agent to follow during simulations
decision_rule:
  voting_behavior:
    propensity_to_vote: 0.85
    issues_considered: ["economy", "education", "healthcare"]
```

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
