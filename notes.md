## argument to `Tensor.backward()`
The `Tensor.backward()` function computes the gradient starting from somewhere in the graph. For example,
```
y.backward()
```
means to compute all the gradients, by starting from `y`. If `y` is a scalar, then this is a typical back propagation process, nothing special. If `y` is a tensor, then high-dimensional tensors will evolve in the process, which we do not want. Therefore, the function requires that since `y` is not a scalar output, then the gradient of a certain scalar output `Loss` with respect to `y` must be provided, say `dLoss / dy`, then if there is a variable `x` which is a tensor, the gradient of `Loss` with respect to `x` is computed by the chain rule:
```
dLoss / dy = (dLoss / dy) * (dy / dx)
```  
In other words, without having a scalar output already defined somewhere (in your head or wheverever possible), you are _not_ allowed to directly compute the jacobian tensor `dy / dx`. You can, but not allowed by PyTorch. You have to have defined somehow a scalar output, still, let's say `Loss`, and you will provide `g = dLoss / dy` to `y.backward()` as below:
```
y.backward(g)
```
Mathematically, it makes all the sense. If you know how to compute matrix-form back progapation, the above design should not be surprising to you. 

There is a post about it too: https://zhuanlan.zhihu.com/p/29923090

