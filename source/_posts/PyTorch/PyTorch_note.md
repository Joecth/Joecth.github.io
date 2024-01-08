

```python
torch.tensor([[0.1, 1.2], [2.2, 3.1], [4.9, 5.2]])

torch.tensor([0, 1])  # Type inference on data

torch.tensor([[0.11111, 0.222222, 0.3333333]],
             dtype=torch.float64,
             device=torch.device('cuda:0'))  # creates a double tensor on a CUDA device

torch.tensor(3.14159)  # Create a zero-dimensional (scalar) tensor

torch.tensor([])  # Create an empty tensor (of size (0,))
```







# Ref

- [Deep Learning with PyTorch: A 60 Minute Blitz — PyTorch Tutorials 2.0.1+cu117 documentation](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)
  - https://pytorch.org/docs/stable/generated/torch.tensor.html#torch.tensor