<h1 align="center">Reservoir Computing for Chaotic Dynamics</h1>
<div align="center"><i>Reconstructing and forecasting complex dynamical systems via Takens’ Embedding and Echo State Networks.</i></div>

## Introduction

This project is a study and implementation repository based on the paper: *[Learning strange attractors with reservoir systems](https://arxiv.org/pdf/2108.05024)*.

The primary goal of this project was to:

1.  **Deeply understand** the mathematical foundations of Reservoir Computing (RC) and its embedding properties.

2.  **Reproduce** the proposed methodology for **one-step-ahead prediction** on chaotic systems.

<p align="center">
  <img src="images/Reservoir_Dynamics.gif" alt="dynamics" width="400">
</p>


## Theoretical Core

This implementation builds upon Theorem 4.5, which establishes that a Reservoir Computer functions as a linear embedding of the underlying dynamical system.

**Key Insight** Even with randomly initialized internal weights, the reservoir state $\mathbf{r}(t)$ synchronizes with the input system. This guarantees that the original chaotic attractor can be reconstructed solely by observing the reservoir dynamics.

**Conditions for Diffeomorphism** Given the Echo State Property (ESP), the synchronization map is a diffeomorphism provided that:

- **Dimension:** The reservoir size $N$ is sufficiently large (e.g., $N \ge 20$ for the Lorenz system).

- **Generic Coupling:** The weight matrices $\mathbf{A}$ and $\mathbf{C}$ are drawn from non-singular distributions.

- **Stark’s Extension:** The injective property remains generic even under non-linear activations (such as $\tanh$).

We have compiled detailed derivations, including the proof of the embedding theorem and the specific requirements for the reservoir dimension $N$, in our study notes:

👉 **[Read Full Mathematical Notes (HackMD)](https://hackmd.io/@SuperChang/rnn)**


## Implementation Details

We implemented the system using Python with the following key steps:

1.  **Data Generation**:
    * Generated time-series data by simulating the Lorenz-63 system via numerical integration (Runge-Kutta methods).

2.  **Reservoir Projection**:
    * Mapped input data into a high-dimensional reservoir space.
    * Utilized **Orthogonal Matrix Initialization** to control the Spectral Radius ($< 1$), ensuring the Echo State Property (ESP) is satisfied.

3.  **Training**:
    * Solved the readout weights using **Ridge Regression (L2 Regularization)** to mitigate overfitting and enhance robustness against noise.
    
4.  **Prediction & Evaluation**:
    * **One-Step-Ahead Prediction (Open-loop)**: 
      Achieved high-fidelity reconstruction on the validation dataset, maintaining a low Mean Squared Error (MSE) of approximately $10^{-3}$.

    * **Multi-Step Prediction (Closed-loop)**: 
      Fed the model's output back as input recursively. While the prediction remains accurate in the short term, the trajectory eventually diverges due to the system's inherent chaotic sensitivity (Butterfly Effect), with MSE rising to the single-digit range ($O(1)$) over longer horizons.


## Repository Structure

* `images/`: Contains all generated visualization results and figures.
* `src/`:
    * `anime.ipynb`: Visualizes data distribution within the Reservoir system using PCA and generates animations synchronized with the observed data.

    * `one_step.ipynb`: Evaluates model performance using one-step-ahead (open-loop) prediction.

    * `multi_step.ipynb`: Implements multi-step (closed-loop) recursive forecasting to validate long-term stability.


## References

[1] Grigoryeva, L., & Ortega, J.-P. (2021). "Learning strange attractors with reservoir systems." The Annals of Applied Probability, 31(1), 106-127.