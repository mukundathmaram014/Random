## Optimizing data flow in a Tensor Processing Unit


This analysis and optimization was inspired by the [tinyTPU](https://www.tinytpu.com/). The example being used here is using this tensor processing unit to solve the XOR problem. In this spceific example, a 2->2->1 layer multi perceptron is used to solve the problem. In order to make predictions with our nueral network, we need to do matrix multiplications multiplying our inputs (X) by the weights in the network. We call this the "forward pass". Aditionally, in order to update our weights based on the accuracy of our current predictions, we need a loss function that tells us how far away our predictions are from the real values. To adjust our weights, we then need to calculate the partial derivative of the loss function with respect to each individual weight to know which direction to adjust the weights. For this specific problem, we are using an activation function of ReLu. The formula during the forward pass is just:

```
Z = X ⋅ W + b
H = ReLu(Z)
```

During the backward pass, its a bit more complicated as we need to take indermediate partial derivatives and do computations with these to get our partial derivatives of loss with respect to each individual weight, but the things we need to calculate to get there is shown in the diagram below:

<p align="center">
  <img src="../images/two_approaches_to_backpropogation.jpg" alt="Backpropogation" width="600">
</p>

<p align="center"><sub>calculating derivatives for backward pass</sub></p>

Now in the above diagram if you regard the green as the first step and yellow as they second step. There are two approaches we can take. The first approach, which is shown on top, is first calculating all our dl/dz's and then calculating each respective dl/dw. The second approach is calculating an individual dl/dw right after calculating its respective dl/dz. I wanted to investigate which is more efficient to implement in hardware.

### Legend

<p align="center">
  <img src="../images/flow_legend.png" alt="legend" width="600">
</p>

<p align="center"><sub>legend for forward and backward pass</sub></p>

## Flow during Forward Pass

The forward pass is unchanged between both approaches so the flow to do the matrix multiplications remain the same. This flow is shown in the diagram below.

<p align="center">
  <img src="../images/forward_pass.png" alt="legend" width="600">
</p>

<p align="center"><sub>Forward pass</sub></p>



## Flow during Backward Pass for Approach 1

The flow for the backward pass with approach 1 where we calculate all the dl/dz's and then calculating each respective dl/dw is shown below:


<p align="center">
  <img src="../images/backward_pass.png" alt="legend" width="600">
</p>

<p align="center"><sub>Backward pass Approach 1</sub></p>

Some notes about this:

- In the current flow, the Unified Buffer(UB) or memory is called 6 times (2 times in forward pass, 4 times in backward pass (one of these is just to supply Y to loss module). We also note that H[2] is not stored in memory and is instead caches because it is only used once right after. It might be possible to cache other data instead of going to memory so lets check how many times everything is used.


<p align="center">
  <img src="../images/UB_count_table.png" alt="legend" width="600">
</p>

<p align="center"><sub>Unified Buffer calculated data count table</sub></p>



