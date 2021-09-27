# Hybrid quantum-classical auto-encoder

This is a Keras-Pennylane implementation of the hybrid quantum-classical auto-encoder suggested in "Continuous-variable quantum neural networks" 
Physics Review Research 1, 033063 (2019). https://arxiv.org/pdf/1806.06871v1.pdf

The architecture of the hybrid network is 


<img width="500" alt="auto_encoder" src="https://user-images.githubusercontent.com/22792633/134831949-8b4dc389-01b8-45a5-9977-d64cb0a04267.png">

The data flow of the code is:
- A dataset of 3-dimensional vectors is created.
- Encoder: Keras sequential layer of depth 6 and 5 neurons is created.
- The encoded vector is 2-dimensional.
- Encode each vector as a quantum state by using each element of the vector as parameters of Displacement gate.
- Initialize weights (parameters of quantum gates). The gates used are:
  - Squeezer
  - Rotation gate
  - Displacement gate
  - Kerr gate
- Set the cufoff dimension to 3, since we want the output vectors to be 3-dimensional for Prob() measurement.
- Decoder: a quantum circuit of 25 layers as specified in the paper.
- Using Pennylane's Tensorflow plug-in, convert the quantum circuit into a Keras layer.
- Create a hybrid model by concatenating the classical and quantum layers.
- Use Keras's built-in loss functions and optimizers for training (I used MSE for loss function and Adam for optimizer).

Note: The loss function is different from the one proposed in the paper. With Pennylane, the functionality of extracting state vectors is not supported yet, 
hence I had to resort to a simpler loss function.

From my experiment, SGD (stochastic gradient descent didn't perform as well as Adam. Experiments with different learning rates are highly encouraged.
