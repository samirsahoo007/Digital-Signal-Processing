# Digital Signal Processing: Discrete HMM
- **Discrete Hidden Markov Model Implementation in C++**
- Implemented based on the course DSP offered by NTU: [HMM.pdf](https://drive.google.com/file/d/1mDkCqMQ3DoAsyoTG0nysyx3h-6ehDcmi/view?usp=sharing)


## Environment
* **< g++ [gcc version 8.2.0 (GCC)] >** (Tested)
* **< g++ [gcc version 7.3.0 (GCC)] >** (Tested)
* **< g++ [gcc version 4.2.1 (GCC)] >** (Tested)
* **< Python 3.6+ >**                   (Optional - for plot.py)
* **< Matplotlib 2.2.3 >**              (Optional - for plot.py)
 
## Algorithm

### Training - [Baum Welch algorithm](https://en.wikipedia.org/wiki/Baum–Welch_algorithm)
* **Initialization**: 
	- Set **θ = ( A , B , π )** with initial conditions (model_init.txt)
* **Forward Procedure**: 
	- Compute **α<sub>i</sub>(t) = P( O<sub>1</sub>=o<sub>1</sub>, ..., O<sub>t</sub>=o<sub>t</sub>, q<sub>t</sub>=i | θ )** recursively, the probability of seeing the observation sequence **o<sub>1</sub>, o<sub>2</sub>, ..., o<sub>t</sub>** and being in state **i** at time **t**.
* **Backward Procedure**: 
	- Compute **β<sub>i</sub>(t) = P( O<sub>t+1</sub>=o<sub>t+1</sub>, ..., O<sub>T</sub>=o<sub>T</sub> | q<sub>t</sub>=i, θ )** recursively, the probability of the ending partial observation sequence **o<sub>t+1</sub>, o<sub>t+2</sub>, ..., o<sub>T</sub>** given starting state **i** at time **t**.
* **Accumulation Procedure**:
	- Calculate the temporary variables, according to Bayes' theorem.
	- Gamma: the probability of being in state **i** at time **t** given the observed sequence **O** and the parameters **θ**.
	- Epsilon: the probability of being in state **i** and **j** at times **t** and **t+1** respectively given the observed sequence **O** and parameters **θ**.
* **Update Procedure**:
	- Parameters of the hidden Markov model θ can now be updated: **( A , B , π ) = ( A<sup>\*</sup> , B<sup>\*</sup> , π<sup>\*</sup> )**

### Testing - [Viterbi Algorithm](https://en.wikipedia.org/wiki/Viterbi_algorithm)
* A dynamic programming algorithm for finding the most likely sequence of hidden states, that results in a sequence of observed events.
* Given a hidden Markov model (HMM) with state space **Q**, initial probabilities **π<sub>i</sub>** of being in state **i** and transition probabilities **a<sub>(i,j)</sub>** of transitioning from state **i** to state **j**. Say we observe outputs **o<sub>1</sub>, ..., o<sub>T</sub>**. The most likely state sequence **q<sub>1</sub>, ..., q<sub>T</sub>** that produces the observations is given by the Viterbi relations.
* This algorithm generates a path **Q = ( q<sub>1</sub>, q<sub>2</sub>, ..., q<sub>T</sub> )**, which is a sequence of states **q<sub>t</sub> ∈ Q = { q<sub>1</sub>, q<sub>2</sub>, ..., q<sub>K</sub> }** that generate the observations **O = ( o<sub>1</sub>, o<sub>2</sub>, ..., o<sub>T</sub> )** with **o<sub>n</sub> ∈ O = { o<sub>1</sub>, o<sub>2</sub>, ..., o<sub>N</sub> }**, N being the count of observations.


## File Description
```
.
├── src/
|   ├── Makefile                g++ compiler make file
|   ├── hmm.h                   HMM implementation
|   ├── hmm.cpp                 HMM implementation
|   ├── test_hmm.cpp            Testing algorithm implementation
|   ├── train_hmm.cpp           Training algorithm implementation
|   ├── test                    Unix executable binary code for test_hmm.cpp  (See the next "Usage" section for more details)
|   ├── train                   Unix executable binary code for train_hmm.cpp (See the next "Usage" section for more details)
|   ├── plot.py                 Draws the training plot
|   ├── model_01~05.txt         Trained models
|   └── modellist.txt           Model name list
├── data/
|   ├── model_init.txt          Initial model for training
|   ├── seq_model_01~05.txt     Training data (observation sequences)
|   ├── testing_data1.txt       Testing data (observation sequences)
|   ├── testing_answer.txt      Real answer for "testing_data1.txt"
|   ├── testing_result.txt      Model generated answer for "testing_data1.txt"
|   └── testing_data2.txt       Testing data without answer
├──problem_description.pdf      Work Spec
└── Readme.md                   This File
```


## Usage

### Train models separately, then test
```
└── src/
    ├── make clean
    ├── make
    ├── ./train #iteration ../data/model_init.txt ../data/seq_model_01.txt model_01.txt
    ├── ./train #iteration ../data/model_init.txt ../data/seq_model_02.txt model_02.txt
    ├── ./train #iteration ../data/model_init.txt ../data/seq_model_03.txt model_03.txt
    ├── ./train #iteration ../data/model_init.txt ../data/seq_model_04.txt model_04.txt
    ├── ./train #iteration ../data/model_init.txt ../data/seq_model_05.txt model_05.txt
    └── ./test modellist.txt ../data/testing_data1.txt ../data/testing_result1.txt
- #iteration is positive integer, which is the iteration of the Baum-Welch algorithm.
```

### Train all models at once, then test
```
└── src/
    ├── make clean
    ├── make
    ├── ./train 250 ../data/model_init.txt ../data/ modellist.txt all
    └── ./test modellist.txt ../data/testing_data1.txt ../data/testing_result1.txt
- #iteration is positive integer, which is the iteration of the Baum-Welch algorithm.
```

### Train all models at once, along with the calculation of the HMM's test score in every iteration **(Suggest Usage)**
```
└── src/
    ├── make clean
    ├── make
    ├── ./train 250 ../data/model_init.txt ../data/ modellist.txt all test
    └── ./test modellist.txt ../data/testing_data1.txt ../data/testing_result1.txt
- #iteration is positive integer, which is the iteration of the Baum-Welch algorithm.
```

### Draw Training Plot
```
└── src/
    └── python3 plot.py
```


## Experinment: Iteration v.s. Accuracy Plot
* **Result**: Maximum accuracy **0.8708** achieved at the 2560-th iteration
* **Training Plot**:
  ![](https://github.com/andi611/DSP_HiddenMarkovModel/blob/master/data/acc.png)
