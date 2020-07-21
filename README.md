# Deepobs for TCML Cluster

Interface for Deeobs on TCML-Cluster

## Getting Started

Copy Folder into your Cluster /home/usr_name folder. Or copy files into folder of your choice.

### Prerequisites

TCML-CLuster Account.

### Running the Interface

### 1.) General Approach

First you will need to generate a Configuration ```.txt``` file. Let's say we call it ```my_configurations.txt```
It has to contain the following:

```
Testprobmlem Name - See Deepobs Documentation for available Testproblems
Optimizer class Name - Name of the optimizer Class
Optimizer Path - Path to where the optimizer is stored
Optimizer module - Module description in Python import style
Set of Hyperparameters for the Optimizer - Hyperparamters which should be tested (every Combination will get tested)
Additional Paramters for Deeobs Trainingphase - See Deepobs Documentation for Details
Sbatch Parameters - Sbatch Parameters used for the configurations
```

### 2.) General info about Entries

Entries of the ```my_configurations.txt``` should generally be structured like this:

```
key: value
```
#### On Hyperparameter values

Given Multiple Hyperparameters the tool will run every possible combination of the given Hyperparamters (cartesian product).
Be aware, that you should look up the correct types for the ```value``` as the wrong types will cause a runtime error.
Every ```value``` will be combined with every other ```value``` which is a Hyperparameter. Resulting in every possible combination of given Hyperparameter values to be run.

### 3.) Values and how they should look like

Every singelton value can be specified as in:

```some key: x``` 

Or in case of Hyperparameter values may also be specified like:

```some key: [x]``` 

x being of any correct type with respect to the key. 

Non singelton Float, Integer or Boolean values may be specified in the following ways:

#### 1. List- or Array-like

```[x, y, z]``` 

#### 2. Range Notation (Float or Integer only):

```(x, y, z)``` 

representing:

```(lower, upper, increment)``` 

So for example ```(0.1, 0.5, 0.1)``` would result in the Hyperparameters ```[0.1, 0.2, 0.3, 0.4, 0.5]``` to be run in every 
possible combination with the other Hyperparameters.

### 4.) Examples and Optimizers

#### 4.1.) Using a Pytorch build-in Optimizer

Let's take a look at an example ```my_configurations.txt``` file using pytorch build-in Optimizer ```SGD```
You may use Optimizers coming with pytorch by default like this:

```
Testproblem: mnist_mlp
Optimizer: SGD
lr: 0.01
momentum: [0.99, 0.79]
nesterov: False
num_epochs: 1
batch_size: 200
sbatch_job_name: some_experiment_name
sbatch_nnodes: 1
sbatch_ntasks: 1
sbatch_cpus_per_task: 5
sbatch_gres: gpu:1080ti:1
sbatch_partition: test
sbatch_time: 15:00
```

where ```lr: [0.01]``` would also be accepted.

```
Testproblem: mnist_mlp
Optimizer: SGD
lr: [0.01, 0.02]
momentum: [0.99, 0.79]
nesterov: False
num_epochs: 1
batch_size: 200
sbatch_job_name: some_experiment_name
sbatch_nnodes: 1
sbatch_ntasks: 1
sbatch_cpus_per_task: 5
sbatch_gres: gpu:1080ti:1
sbatch_partition: test
sbatch_time: 15:00
```

In this case we have not specified an outputfolder name. Therefore a default ouput folder will be created in the current working directory.
(See example below on how that works)

#### 4.2.) Using custom (user written) Optimizer

We could also specify a user written Optimizer which would have to be a subclass of the pytorch optimizer class (See Deeobs Documentation for further Info)
In this case you would have to specify the ```path``` to wherever the Optimizer is located, as well as the ```optimizer class name``` and 
the ```optimizer module```. 
The ```optimizer module``` has to be specified in the way one would specify a Python package import. (See example)

Then the For example your ```my_configurations.txt``` could look like this:

```
Testproblem: mnist_mlp
Optimizer: name_of_optimzer_class
Optimizer Path: /home/usr_name/user_optimizer/user_optimizer_file.py
Optimizer Module: user_optimizer.user_optimizer_file.name_of_optimzer_class
lr: (0.01, 0.05, 0.01)
momentum: [0.99, 0.79]
nesterov: False
num_epochs: 1
batch_size: 200
sbatch_job_name: The_jobs_name
sbatch_nnodes: 1
sbatch_ntasks: 1
sbatch_cpus_per_task: 5
sbatch_gres: gpu:1080ti:1
sbatch_partition: test
sbatch_time: 15:00
output: user_specified_outputfolder

```

## Running the Configurations

After you set up the ```my_configurations.txt``` file as described above simply navigate to the copied folder containing the files and type 

``` python3 testobs path_to_configurations_file/my_configurations.txt ```

Of course you do not have to specify the path if the ```my_configurations.txt``` file is located in the same folder.


## For Infos on Deepobs see:

* [Deepobs Documentation](https://deepobs.readthedocs.io/en/v1.2.0-beta0/) - The DNN Optimizer Benchmark suite


## Acknowledgments

* Hat tip to anyone whose code was used
* etc
