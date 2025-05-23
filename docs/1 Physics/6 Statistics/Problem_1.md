# Problem 1

[Simulation](Simulation.html)



### Markdown Document: Exploring the Central Limit Theorem through Simulations

#### Objective
The Central Limit Theorem (CLT) states that the distribution of the sample mean $\bar{X}$ of a random sample of size $n$ from a population with mean $\mu$ and variance $\sigma^2$ approaches a normal distribution as $n$ increases, regardless of the population’s distribution. This project aims to:
1. Simulate sampling distributions for different population distributions.

2. Visualize the convergence of sample means to a normal distribution.

3. Explore the influence of population shape and variance on convergence.

4. Discuss real-world applications of the CLT.

#### Methodology
1. **Population Distributions**:
   - **Uniform**: Continuous on $[0, 10]$.
   - **Exponential**: Right-skewed with rate parameter $\lambda = 1$.
   - **Binomial**: Discrete with $n=10$, $p=0.5$.
   - Generate a population of size $N = 100,000$ for each.

2. **Sampling and Visualization**:
   - Draw random samples of sizes $n = 5, 10, 30, 50$.
   - Compute sample means for $M = 10,000$ samples.
   - Plot histograms of sample means with a kernel density estimate (KDE) and overlay the theoretical normal distribution.
3. **Parameter Exploration**:
   - Analyze how population skewness and variance affect convergence.
   - Verify the sampling distribution’s variance.
4. **Practical Applications**:
   - Estimating population parameters.
   - Quality control in manufacturing.
   - Financial modeling.



### Key Formulas

#### Population Parameters
For each distribution, the mean $\mu$ and variance $\sigma^2$ are:
- **Uniform** ($a = 0$, $b = 10$):
  $$
  \mu = \frac{a + b}{2} = \frac{0 + 10}{2} = 5
  $$
  $$
  \sigma^2 = \frac{(b - a)^2}{12} = \frac{(10 - 0)^2}{12} = \frac{100}{12} \approx 8.333
  $$
- **Exponential** ($\lambda = 1$):
  $$
  \mu = \frac{1}{\lambda} = \frac{1}{1} = 1
  $$
  $$
  \sigma^2 = \frac{1}{\lambda^2} = \frac{1}{1^2} = 1
  $$
- **Binomial** ($n = 10$, $p = 0.5$):
  $$
  \mu = n \cdot p = 10 \cdot 0.5 = 5
  $$
  $$
  \sigma^2 = n \cdot p \cdot (1 - p) = 10 \cdot 0.5 \cdot 0.5 = 2.5
  $$

#### Sample Mean
For a sample of size $n$, the sample mean is:
$$
\bar{X} = \frac{1}{n} \sum_{i=1}^n X_i
$$

#### Central Limit Theorem
For a population with mean $\mu$ and variance $\sigma^2$, the CLT states that for large $n$, the sampling distribution of $\bar{X}$ is approximately:
$$
\bar{X} \sim \mathcal{N}\left(\mu, \frac{\sigma^2}{n}\right)
$$
- **Mean of the sampling distribution**:
  $$
  \mathbb{E}(\bar{X}) = \mu
  $$
- **Variance of the sampling distribution**:
  $$
  \text{Var}(\bar{X}) = \frac{\sigma^2}{n}
  $$
- **Normal density function** (for plotting):
  $$
  f(x) = \frac{1}{\sqrt{2\pi \cdot \frac{\sigma^2}{n}}} \exp\left(-\frac{(x - \mu)^2}{2 \cdot \frac{\sigma^2}{n}}\right)
  $$



## Python Implementation
The following Python script simulates the sampling distributions, visualizes them with histograms, and compares empirical and theoretical variances.

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Set random seed for reproducibility
np.random.seed(42)

# Parameters
population_size = 100_000  # Population size (N)
sample_sizes = [5, 10, 30, 50]  # Sample sizes (n)
num_samples = 10_000  # Number of samples (M)

# Define populations with theoretical parameters
populations = {
    'Uniform': {
        'data': np.random.uniform(0, 10, population_size),
        'mean': 5,  # (0 + 10) / 2
        'variance': 100 / 12  # (10 - 0)^2 / 12
    },
    'Exponential': {
        'data': np.random.exponential(scale=1, size=population_size),
        'mean': 1,  # 1 / lambda, lambda = 1
        'variance': 1  # 1 / lambda^2
    },
    'Binomial': {
        'data': np.random.binomial(n=10, p=0.5, size=population_size),
        'mean': 5,  # n * p = 10 * 0.5
        'variance': 2.5  # n * p * (1 - p)
    }
}

# Function to simulate sampling and compute means
def simulate_sampling(population, sample_size, num_samples):
    """Generate sampling distribution of the mean."""
    sample_means = [np.mean(np.random.choice(population, size=sample_size)) for _ in range(num_samples)]
    return np.array(sample_means)

# Function to plot sampling distribution
def plot_sampling_distribution(sample_means, mu, sigma, sample_size, dist_name):
    """Plot histogram of sample means with theoretical normal curve."""
    plt.figure(figsize=(5, 4))
    sns.histplot(sample_means, bins=50, kde=True, stat='density', label='Sample Means')
    # Theoretical normal curve
    x = np.linspace(mu - 4*sigma, mu + 4*sigma, 100)
    plt.plot(x, 1/(sigma * np.sqrt(2*np.pi)) * np.exp(-(x-mu)**2/(2*sigma**2)), 
             'r--', label=f'Normal(μ={mu}, σ²={sigma**2:.3f})')
    plt.title(f'{dist_name}, n={sample_size}')
    plt.xlabel('Sample Mean')
    plt.ylabel('Density')
    plt.legend()
    plt.tight_layout()

# Simulate, visualize, and analyze variances
for dist_name, dist_info in populations.items():
    population = dist_info['data']
    mu = dist_info['mean']
    pop_variance = dist_info['variance']
    
    # Plot sampling distributions
    plt.figure(figsize=(15, 4))
    for i, sample_size in enumerate(sample_sizes, 1):
        sample_means = simulate_sampling(population, sample_size, num_samples)
        sigma = np.sqrt(pop_variance / sample_size)
        plt.subplot(1, len(sample_sizes), i)
        plot_sampling_distribution(sample_means, mu, sigma, sample_size, dist_name)
    plt.suptitle(f'Sampling Distributions for {dist_name} Population')
    plt.show()

    # Variance comparison
    print(f"\n{dist_name} Distribution (μ={mu}, σ²={pop_variance:.3f}):")
    for sample_size in sample_sizes:
        sample_means = simulate_sampling(population, sample_size, num_samples)
        empirical_variance = np.var(sample_means, ddof=1)
        theoretical_variance = pop_variance / sample_size
        print(f"Sample Size {sample_size}: Empirical Var = {empirical_variance:.4f}, "
              f"Theoretical Var = {theoretical_variance:.4f}")
```



#### Expected Results
- **Visualizations**:
  - For each distribution (Uniform, Exponential, Binomial), four histograms (for $n = 5, 10, 30, 50$) display the sampling distribution of $\bar{X}$.
  - A red dashed line shows the theoretical normal distribution $\mathcal{N}(\mu, \sigma^2/n)$.
  - **Uniform**: Rapid convergence to normality by $n=10$ due to symmetry.
  - **Exponential**: Skewed at $n=5$, approximates normality by $n=30$.
  - **Binomial**: Near-normal by $n=30$, reflecting moderate symmetry.
- **Variance Analysis**:
  - Empirical variances of sample means closely match the theoretical $\frac{\sigma^2}{n}$.
  - Example (Uniform, $\sigma^2 = 8.333$):
    $$
    n=5: \text{Var}(\bar{X}) = \frac{8.333}{5} \approx 1.667
    $$
    $$
    n=50: \text{Var}(\bar{X}) = \frac{8.333}{50} \approx 0.167
    $$



#### Parameter Exploration
- **Shape Influence**:
  - **Uniform**: Symmetrical, no skewness, converges quickly (by $n=10$).
  - **Exponential**: High skewness ($\gamma = 2$ for $\lambda=1$), requires $n \geq 30$ for normality.
  - **Binomial**: Moderate skewness, converges by $n=30$.
- **Variance Impact**:
  - The sampling distribution’s variance is:
    $$
    \text{Var}(\bar{X}) = \frac{\sigma^2}{n}
    $$
  - Uniform has the largest $\sigma^2 = 8.333$, followed by Binomial ($\sigma^2 = 2.5$), then Exponential ($\sigma^2 = 1$).
  - Larger $n$ reduces $\text{Var}(\bar{X})$, tightening the distribution.
- **Convergence Rate**: Skewed distributions (Exponential) converge slower than symmetric ones (Uniform, Binomial).



#### Practical Applications
1. **Estimating Population Parameters**:
   - The CLT justifies using $\bar{X} \sim \mathcal{N}(\mu, \sigma^2/n)$ for confidence intervals in surveys (e.g., estimating average income).
2. **Quality Control**:
   - In manufacturing, batch averages (e.g., screw lengths) are normally distributed, enabling statistical control charts.
3. **Financial Modeling**:
   - The CLT supports modeling average portfolio returns as normal, facilitating risk analysis despite non-normal asset returns.



### Discussion
The simulations validate the CLT: the sampling distribution of $\bar{X}$ approaches $\mathcal{N}(\mu, \sigma^2/n)$ as $n$ increases. The rate of convergence depends on population skewness, with symmetric distributions converging faster. The variance $\frac{\sigma^2}{n}$ decreases with larger $n$, enhancing estimation precision. These results align with theoretical expectations and highlight the CLT’s critical role in statistical inference across domains like polling, manufacturing, and finance.
