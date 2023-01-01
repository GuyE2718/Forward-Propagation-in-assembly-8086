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
