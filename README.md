# Markov Switching Multi-fractal model
BT variation of MSM model is implemented in Go.

## Model
De-meaned returns are modelled as 

$$
\begin{align*}
r_t &= \sigma_t z_t \\
\sigma_t &= \sigma_0 \sqrt{M_1 M_2 ... M_k}\\
z_t &\sim \mathcal{N}(0, 1)
\end{align*}
$$

$k$ is the model dimension and $M_i$ are 2-state Markov Chains.

Refer to Calvet and Fisher (2001)[^1]

[^1]: Calvet, L.; Fisher, A. (2001). "Forecasting multifractal volatility" [(PDF)](https://archive.nyu.edu/bitstream/2451/26894/2/wpa99017.pdf). Journal of Econometrics. 105: 27–58.

## BT Variation
The original model parametrises the transition probabilities of Markov chains $M_i$ with just two parameters. So the model is parsimonious and parameters do not grow with k. In BT variation, each chain $M_i$ is characterised by a probability parameter $g_i$ i.e. transition matrix is given by

$$
\begin{bmatrix}
1-g_i & g_i \\
g_i & 1-g_i
\end{bmatrix}.
$$

This allows $k-2$ more degrees of freedom.

## Go Implementation
The model is implemented as unconstrained optimisation with suitable parameter transformation. There are a total of $k+2$ parameters, $m_0$ the binomial pdf paramter $(1< m_0 <2)$, $\sigma_0 > 0$ the unconditional volatility and $k$ probabilities. $m_0$ is mapped into $(-\infty, \infty)$ with inverse sigmoid, $\sigma_0$ with log and probabilities with inverse sigmoid.

## Usage
```
msm -k 4 -n 200 -w 240 -i input.csv -o results.csv
```

Command line flags:

```
 -i string
    	Name of csv file containing return data
  -k int
    	MSM model dimension, +ve integer, caution: k > 10 can take minutes to run
  -n int
    	Number of simulations for computing mean and std dev of vol estimate, minimum 100 (default 200)
  -o string
    	Name of output file (default "results.csv")
  -w int
    	Time window for vol prediction in number of bars, minimum 30 (default 30)
```

The output file is a csv containing model parameters, negative log likelihood, model dimension, mean of predicted vol and std dev of predicted vol. Std error of mean can be inferred as std dev / sqrt(n).
