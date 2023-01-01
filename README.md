# Forward-Propagation-in-assembly-8086


This repository contains an implementation of forward propagation in assembly 8086 for an artificial neural network trained to recognize handwritten digits. The network was trained in python on the MNIST dataset, a widely-used benchmark for machine learning tasks involving handwritten digits.

This is my final project for my 11th grade computer science class, completed in 2021. The project is an implementation of forward propagation in assembly 8086. It took several months to develop and can be run on emu8086, an emulator of the Intel 8086 microprocessor architecture. This project is intended for educational purposes and as a reference for those interested in low-level implementation of neural network algorithms.


Project Structure
------
I began this project by implementing the neural network from scratch in Python, without using any machine learning libraries. The structure of the network is as follows:

![model](https://user-images.githubusercontent.com/121691820/210157185-71c1885c-7f05-430a-a6b1-e29c6bfbf749.png)

### The network has 3 layers:
* input layer has 784 neurons, one for each pixel in the 28x28 MNIST images.
* hidden layer of size 10.
* output layer has 10 neurons, corresponding to the possible predictions of 0-9.

The Python script - network.py in this project builds and trains a neural network on the MNIST dataset. After training is completed, the values of all the optimized weights and biases are written to a text file for future use in running the network in assembly. 

```python
np.set_printoptions(precision=1, suppress=True)
np.set_printoptions(threshold=sys.maxsize)
weights1 = open('w1.txt', 'w')
weights1.write(str(W1))
weights1.close()

biases1 = open('b1.txt', 'w')
biases1.write(str(b1))
biases1.close()

weights2 = open('w2.txt', 'w')
weights2.write(str(W2))
weights2.close()

biases2 = open('b2.txt', 'w')
biases2.write(str(b2))
biases2.close()
```

Implementing backpropagation in 8086 assembly is a bit out of scope for this project, but it could be a fun challenge to tackle in the future. I'm definitely up for a challenge, and implementing gradient descent in 8086 assembly sounds like it could be pretty interesting. It would definitely be a unique and difficult task, but who knows, maybe it could turn out to be really cool.

The Assembly Code
------
In my CS class, we used the emu8086 emulator to work with 8086 assembly. I really enjoy using the emulator, but I ran into a problem when I started my project. The neural network I was building used floating point numbers for weights, biases, and neurons, but natively, the 8086 processor does not support floating point arithmetic. After some research, I discovered the FPU (Floating Point Unit), a specialized processor that can perform arithmetic operations on floating point numbers. The FPU is part of the 8087 coprocessor, an optional add-on chip that can be used with the 8086 to provide hardware support for floating point operations. This allowed me to work with floats and perform calculations on them in my assembly code.

I ran into a big problem when testing my code in the emulator - **none of the FPU instructions were working.** My whole project relied on the FPU, so this was a big issue. I did some research and found a Russian forum with some helpful information about the FPU in emu8086. Using Google Translate, I was able to understand what they were saying and found the following information: (https://www.cyberforum.ru/assembler-math/thread2148889.html)

![fpuhelpRU](https://user-images.githubusercontent.com/121691820/210167887-2ce1e9cc-5f4a-4fc8-acd0-74e77089febd.png)

![fpuhelpEN](https://user-images.githubusercontent.com/121691820/210167931-b8f4ac68-c24c-4200-b475-d82bcdd0e12a.png)

It seems that emu8086 was developed by a small team or an individual, and they accidentally deleted the FPU unit from the 8086 assembly directory, rendering it lost forever. Fortunately, they discovered that if you create your project and check the "Use Flat Assembler syntax" box at the start, you can access the FPU unit that was not deleted for FASM. This solution allowed me to finally start using the FPU in my project and move past the issue that had been holding me back.

Code Structure 
------
In the data segment, I define all of the variables that I will need in memory. I import the weights and biases as matrixes into the project as double word arrays of a specific size. I also create some arrays for the neurons, each consisting of 10 segments in the array. These arrays and variables will be used throughout my project to store and manipulate data.

The steps of forward propagation can be represented in mathematical notation as follows:

![forwordmath](https://user-images.githubusercontent.com/121691820/210168279-e4f9ebae-b91f-4ea3-8eeb-2b5fcdaacb1c.png)

The first step involves taking the dot product of the input image and the weights of the first layer, summing the result, and adding the biases of the first layer to the output. This process generates the output for the first layer, which can then be passed on to the next layer as input. This process is implemented as two procedures in the code, named **Dot_X_W1** and **Add_Z1_B1**. These procedures handle the dot product and bias addition, respectively, and are used to compute the output for the first layer of the neural network.

The second step in forward propagation involves applying the ReLU nonlinear activation function to the output of the first layer, also known as the first layer's neurons (Z1). This step is necessary to improve the network's ability to classify data by introducing nonlinearity to the values. The ReLU function is a simple operation - it takes a number as input and returns the number if it is greater than zero, or zero if it is less than or equal to zero. I chose to use the ReLU function over other options such as sigmoid or tanh in order to avoid the vanishing gradient problem. The procedure that implements the ReLU function is called **ReLU** and can be represented mathematically as follows:

![relu](https://user-images.githubusercontent.com/121691820/210168792-fb5b00ee-0802-4fdf-81f4-073266efa8e6.png)

The third step in forward propagation is similar to the first step, but with a few differences. This time, we are computing the output for the second (final) layer of the neural network, so each neuron in the output layer (Z2) has 10 weights instead of 784 like in the first step. The procedures that handle this calculation are named **Dot_Z1_W2** and **Add_Z2_B2** and are slightly different from the ones used in the first step, but follow a similar structure. These procedures compute the dot product of the output from the first layer and the weights of the second layer, sum the result, and add the biases of the second layer to obtain the final output of the neural network.

To make a prediction based on the information provided by the final layer of the neural network, we typically use the softmax function. Softmax is a function that takes a set of values and converts them into a probability distribution, with each value being transformed into a corresponding probability between 0 and 1, such that the probabilities sum to 1.
Due to time constraints, I was unable to implement the softmax function for this project. Instead, I decided to simply take the index of the largest number in the output layer as the network's prediction. This index represents the class that the network is most confident matches the input data. The procedure that performs this operation is called "Find_Max".


That concludes my journey of implementing a forward propagation algorithm in 8086 assembly. I learned a lot about assembly language and gained a deeper understanding of the algorithm by implementing it in such a low-level language. Working with assembly was a rewarding experience, and I believe that implementing the network at this level has given me a more solid understanding of how it works. If you have any questions about this project, please feel free to contact me at Guye750@gmail.com. Thank you for reading.
