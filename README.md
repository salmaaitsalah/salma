do# RL4JAM Project

## Build the container :  
To run an experiment, you need to build the docker container.  
Place you in this directory : ```RL4Jam/ns3gym``` and use this command : ```docker-compose up --build```.

#salma: before you launch it in interactive mode, you should start the container using this command : ```docker start ``` plus the container ID
Then, launch it in iteractive mode : ```docker exec -it ns3-gym /bin/bash```



The next instructions are inside the container
## Simulate one experiment :
To simulate one experiment, open two interactive instances of the ns3-gym container : one for Ns3Gym and one for the experiment.  
- Launch Ns3Gym :  
# Salma: it's ns3-gym not ns3Gym
Place you in this directory :  ```/ns3gym/``` and use this command : ```./waf --run jam```

#Salma: if you had a problem to connect python with Ns3 you should create agents to connect to the python part using this command : ``` ./waf --run "jam --agentNum=8 --openGymPort=5556" ```

The Ns3Gym simulation need 8 connections by default, so 8 nodes have to be generated during the experiment
- Launch the experiment :  
# salma:it's ns3-gym  not ns3Gym
Place you in this directory : ```/ns3gym/scratch/jam/``` and use this command :   
```
usage: main.py [-h] name_algo nb_node epsilon C L mode title_report pretrain_mode
positional arguments:
  name_algo      The name of the algorithm used
  nb_node        number of node used
  epsilon        error value used in the evaluation
  C              number of round used in the computation of certain QOS Mode
  L              delay between the QoS computation and the used value (in round)
  mode           QoS Computation mode
  title_report   title of the outputed report
  pretrain_mode  using the pretraining mode  
```   
For example :  
``` python main.py linucb 8 0.007 20 1 QOSAvgExtremumDiv Test True``` will run an experiment using the "linucb" algorithm, 8 nodes are generated, we have an epsilon, a C and a L value of respectively 0.007, 20 and 1, the QOS computation mode is QOSAvgExtremumDiv, the name of the generated report will be Test and the pretrain phase is activated.  

### List of the available Qos Computation mode :
- QOS  
- QOSNormalisationSub
- QOSNormalisationDiv
- QOSAvg
- QOSExtremumAvg
- QOSMedian  
- QOSAvgMedianSub
- QOSAvgMedianDiv
- QOSAvgExtremumSub
- QOSAvgExtremumDiv

At the end of the experiment, a PDF Report is generated and can be found here : ```ns3gym/scratch/jam/report/```  

## Run multiple experiments :
To run multiple experiments consecutively, you only need to run one interactive instance of the ns3-gym container.  
Open the file ```ns3gym/scratch/jam/run.py``` and add item to the ```run_conf.json``` json file using this template :  
```json
{
  'Mode' : {
    'Epsilon' : [Min, Max, Step],
    'C' : [Min, Max, Step],
    'L' : [Min, Max, Step]
  }
}
```  
#salma: the template has already been added, but you should make sure of that if it's not you should open the file using the command: vim ...., save it and then you can use the commande python run.Py --conf

For example : an item defined like
```
{
  'QOSAvgMedianDiv' : {
    'Epsilon' : [0.03, 0.091, 0.03],
    'C' : [5, 21, 5],
    'L' : [5, 50, 15]
  },
  ...
}
```
means that the program launch an experiment using the linucb algorithm and the QOSAvgMedianDiv computation mode for every epsilon value between 0.03 and 0.091 with a step of 0.03 , for every C value between 5 and 21 with a step of 5 and for every L value between 5 and 50 with a step of 15. The pretrain phase is activated by default.

#salma: it's ns3-gym not ns3Gym
Then, place you in this directory : ```ns3gym/scratch/jam/``` and use this command : ```python run.Py --conf```

#salma: after using this command if you find the problem of port incompatibility, make sure that The port set from 5556 in NS-3 to connect the corresponding port set in Python code(run.Py) in NS-3, the command is “./waf —run "jam --agentNum=8 --openGymPort=5556"


It is also possible to use run.py to launch an instance of _ns3gym_ and the _agent_ like this : ```python run.Py -n 8 -e 0.1 -c 10 -l 10 -m QOSAvgMedianDiv -t 'QOSAvgMedianDiv' -p```. <br>
As the same as in the main.py script :
- _-n X_ : stand for the number of node.
- _-e X_ : represent the epsilon value.
- _-c X_ : is the _C_ value.
- _-l X_ : will be the _L_ value.
- _-m X_ : will represent the QoS mode.
- _-t X_ : is the pdf title.
- _-p_ : if specified, the script will do a pre-training.

At the end of the process, you can check the best results (global and for each mode) in the csv files available in this
# salma:it's ns3-gym not ns3Gym
 directory : ```ns3gym/scratch/jam/best_report/```.  
You can find the report corresponding to the best experiments by reading the ```best.csv``` file and find the report in this
#salma: it's ns3-gym  not ns3Gym
 directory : ```ns3gym/scratch/jam/report```
