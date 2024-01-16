# polSim: Population Simulation Platform for Social Science Research

## Overview
`polSim` is a platform designed for simulating synthetic populations in social science research. The platform leverages large language models to generate populations with user-defined attributes, facilitating a broad range of simulations from simple surveys to complex behavioral studies.

## Features
- **Modular Architecture**: System components are designed for easy updates and scalability.
- **Population Representation**: Agents are represented as individual `.txt` files within a structured directory.
- **Attribute Customization**: Users can define attribute categories, options, and sampling methods.
- **Population Size Flexibility**: Supports simulation of varying population sizes according to research needs.
- **Task Segregation**: Profiles are independent from tasks, allowing for versatile simulation workflows.
- **Configuration**: Both manual and GPT-assisted configuration methods are available.
- **Interface Options**: Includes a command line interface with plans for a future graphical user interface (GUI).
- **Open Source**: Hosted on GitHub for community collaboration.
- **Model Compatibility**: Compatible with OpenAI’s API, High-Performance Computing (HPC) models, or local models.

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

## Configuration
Configuration is managed via a global YAML file. This file specifies attribute categories, options, population sizes, and other simulation parameters.

## Usage
To start a simulation, use the following command:
```
python simulate.py --config path/to/config.yaml
```

Replace `path/to/config.yaml` with the path to your configuration file.

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

### Generating Population
To generate a population, run:
```
python generate_population.py
```
This will create a population of agents with attributes defined in your `config.yaml`.

## Contribution
Contributions are welcome. Please ensure that you submit a pull request with a clear description of the changes and the benefits they bring to the project.

## License
`polSim` is released under the MIT License. For more details, see the LICENSE file in the repository.

## Roadmap
The project roadmap is available in the repository's issues tracker. Contributors are encouraged to discuss future features and improvements there.

## Community and Support
For support, questions, or to join the conversation around `polSim`, please open an issue in the GitHub repository or join our mailing list at [mailing-list-link].

## Ethical Considerations
Users of `polSim` must adhere to ethical guidelines for social science research. Simulations should respect privacy, ensure data security, and avoid misuse of synthetic data.

## Troubleshooting
Common issues and troubleshooting steps will be documented in the `TROUBLESHOOTING.md` file. For unresolved issues, please open a ticket on the GitHub issue tracker.

## Contact
For collaboration inquiries or further assistance, please contact the maintainers at mcbosley@umich.edu
