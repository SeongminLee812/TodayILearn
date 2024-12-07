### Problem 1 Solution

To derive the gradients for the parameters and input in the multi-class classification problem with the given MLP structure, we start from the definitions and proceed step by step.

#### Gradients for the Loss Function
The loss function is defined as:
$$
L = -\sum_{i=1}^C y_i \log p_i,
$$
where \( p_i = \text{softmax}(a)_i \). The softmax function is:
$$
\text{softmax}(a)_i = \frac{\exp(a_i)}{\sum_{j=1}^C \exp(a_j)}.
$$

The gradient of \( L \) with respect to \( a \) is:
$$
\frac{\partial L}{\partial a} = \delta_1 = p - y \in \mathbb{R}^C,
$$
where \( \delta_1 \) is the difference between the predicted probabilities \( p \) and the one-hot encoded labels \( y \).

#### Gradients for Parameters \( V \) and \( b_2 \)
The weight matrix \( V \) and bias vector \( b_2 \) in the output layer contribute to the output \( a \):
$$
a = Vh + b_2.
$$
The gradient of \( L \) with respect to \( V \) is:
$$
\frac{\partial L}{\partial V} = \delta_1 h^\top \in \mathbb{R}^{C \times K},
$$
and with respect to \( b_2 \):
$$
\frac{\partial L}{\partial b_2} = \delta_1 \in \mathbb{R}^C.
$$

#### Gradients for the Hidden Layer
The hidden layer activations are given by:
$$
h = \text{ReLU}(z),
$$
where \( z = Wx + b_1 \). The gradient of \( L \) with respect to \( z \) is:
$$
\frac{\partial L}{\partial z} = \delta_2 = (V^\top \delta_1) \odot H(z) \in \mathbb{R}^K,
$$
where \( H(z) \) is the element-wise Heaviside step function:
$$
H(z)_i =
\begin{cases}
1 & \text{if } z_i > 0, \\
0 & \text{otherwise.}
\end{cases}
$$

#### Gradients for Parameters \( W \) and \( b_1 \)
The weight matrix \( W \) and bias vector \( b_1 \) in the hidden layer contribute to \( z \):
$$
z = Wx + b_1.
$$
The gradients are:
$$
\frac{\partial L}{\partial W} = \delta_2 x^\top \in \mathbb{R}^{K \times D},
$$
$$
\frac{\partial L}{\partial b_1} = \delta_2 \in \mathbb{R}^K.
$$

#### Gradient for Input \( x \)
The gradient of \( L \) with respect to \( x \) is:
$$
\frac{\partial L}{\partial x} = W^\top \delta_2 \in \mathbb{R}^D.
$$

### Summary
The derived gradients are:

1. $\frac{\partial L}{\partial V} = \delta_1 h^\top$
2. $\frac{\partial L}{\partial b_2} = \delta_1$ 
3. $\frac{\partial L}{\partial W} = \delta_2 x^\top$
4. $\frac{\partial L}{\partial b_1} = \delta_2$
5. $\frac{\partial L}{\partial x} = W^\top \delta_2$

These results are consistent with the backpropagation algorithm for MLPs.
