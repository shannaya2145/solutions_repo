# Problem 1


#### 1. **Setup and Measurements**
- **Materials**: A string (1.5 m long), a small weight (e.g., a key chain), a stopwatch, and a ruler.
- **Length Measurement**: Suppose the length $L$ of the pendulum (from the suspension point to the center of the weight) is measured as $L = 1.50 \, \text{m}$. Assume the ruler has a resolution of $1 \, \text{mm} = 0.001 \, \text{m}$.
  - Uncertainty in length:
  
   $\Delta L = \frac{\text{Ruler Resolution}}{2} = \frac{0.001}{2} = 0.0005 \, \text{m}$.

- **Time Measurement**: Displace the pendulum by a small angle ($<15^\circ$) and measure the time for 10 full oscillations ($T_{10}$). Repeat this 10 times. Suppose the measurements are:
  - $T_{10}$ values (in seconds): 24.5, 24.6, 24.4, 24.7, 24.5, 24.6, 24.5, 24.4, 24.6, 24.5.
  - Mean time for 10 oscillations: 
    $$
    \overline{T}_{10} = \frac{24.5 + 24.6 + 24.4 + 24.7 + 24.5 + 24.6 + 24.5 + 24.4 + 24.6 + 24.5}{10} = 24.53 \, \text{s}
    $$
  - Standard deviation ($\sigma_T$) of the $T_{10}$ measurements:
    - Variance: $\sigma_T^2 = \frac{\sum (T_{10,i} - \overline{T}_{10})^2}{n-1}$
    - Differences from mean: $-0.03, 0.07, -0.13, 0.17, -0.03, 0.07, -0.03, -0.13, 0.07, -0.03$

    - Squared differences: $0.0009, 0.0049, 0.0169, 0.0289, 0.0009, 0.0049, 
    
    0.0009, 0.0169, 0.0049, 0.0009$
    - Sum: $0.0801$
    - Variance: $\frac{0.0801}{9} = 0.0089$
    - Standard deviation: $\sigma_T = \sqrt{0.0089} \approx 0.094 \, \text{s}$.
  - Uncertainty in the mean time: 
    $$
    \Delta T_{10} = \frac{\sigma_T}{\sqrt{n}} = \frac{0.094}{\sqrt{10}} \approx 0.030 \, \text{s}
    $$

#### 2. **Calculations**

1. **Calculate the Period**:
   $$
   T = \frac{\overline{T}_{10}}{10} = \frac{24.53}{10} = 2.453 \, \text{s}
   $$
   $$
   \Delta T = \frac{\Delta T_{10}}{10} = \frac{0.030}{10} = 0.003 \, \text{s}
   $$

2. **Determine $g$**:
   The period of a simple pendulum is given by:
   $$
   T = 2\pi \sqrt{\frac{L}{g}}
   $$
   Rearrange for $g$:
   $$
   g = \frac{4\pi^2 L}{T^2}
   $$
   Substitute $L = 1.50 \, \text{m}$, $T = 2.453 \, \text{s}$:
   $$
   T^2 = (2.453)^2 \approx 6.017 \, \text{s}^2
   $$
   $$
   g = \frac{4 \pi^2 (1.50)}{6.017} = \frac{4 \times (3.1416)^2 \times 1.50}{6.017} \approx \frac{59.22}{6.017} \approx 9.84 \, \text{m/s}^2
   $$

3. **Propagate Uncertainties**:
   The uncertainty in $g$ is:
   $$
   \Delta g = g \sqrt{\left(\frac{\Delta L}{L}\right)^2 + \left(\frac{2 \Delta T}{T}\right)^2}
   $$
   - $\frac{\Delta L}{L} = \frac{0.0005}{1.50} \approx 0.000333$
   - $\frac{2 \Delta T}{T} = \frac{2 \times 0.003}{2.453} \approx 0.00245$
   - Combined: 
     $$
     \sqrt{(0.000333)^2 + (0.00245)^2} \approx \sqrt{0.00000011 + 0.000006} \approx \sqrt{0.00000611} \approx 0.00247
     $$
   - $\Delta g = 9.84 \times 0.00247 \approx 0.024 \, \text{m/s}^2$

   So, $g = 9.84 \pm 0.02 \, \text{m/s}^2$.

#### 3. **Analysis**

1. **Comparison with Standard Value**:
   The standard value of
    $g$ is $9.81 \, \text{m/s}^2$. The measured 
    
    $g = 9.84 \pm 0.02 \, \text{m/s}^2$ is slightly higher but within a reasonable range, considering the uncertainty overlaps with the standard value 
    
    ($9.84 - 0.02 = 9.82$, which is close to 9.81).

2. **Discussion**:
   - **Effect of Measurement Resolution on $\Delta L$**: The ruler's resolution ($0.001 \, \text{m}$) leads to a small $\Delta L = 0.0005 \, \text{m}$, contributing minimally to the uncertainty in $g$ ($\frac{\Delta L}{L} \approx 0.000333$). A more precise tool (e.g., a caliper) could reduce this further.
   - **Variability in Timing and Impact on $\Delta T$**: The standard deviation in $T_{10}$ ($\sigma_T \approx 0.094 \, \text{s}$) reflects variability in timing, likely due to human reaction time or slight variations in the pendulum's motion. This contributes more significantly to $\Delta g$ via $\frac{2 \Delta T}{T} \approx 0.00245$.
   - **Assumptions and Limitations**:
     - Assumes small-angle approximation 

     ($<15^\circ$) for the pendulum equation to hold.
     - Neglects air resistance and the mass of the string, which could slightly affect the period.
     - Human error in timing with a stopwatch may introduce systematic bias.

#### 4. **Deliverables**

1. **Tabulated Data**:

| Parameter         | Value            |
|-------------------|------------------|
| $L$           | 1.50 m           |
| $\Delta L$    | 0.0005 m         |
| $T_{10}$ (measurements) | 24.5, 24.6, 24.4, 24.7, 24.5, 24.6, 24.5, 24.4, 24.6, 24.5 s |
| $\overline{T}_{10}$ | 24.53 s          |
| $\sigma_T$    | 0.094 s          |
| $\Delta T_{10}$ | 0.030 s          |
| $T$           | 2.453 s          |
| $\Delta T$    | 0.003 s          |
| $g$           | 9.84 m/s²        |
| $\Delta g$    | 0.02 m/s²        |

2. **Discussion on Sources of Uncertainty**:
   - The uncertainty in $L$ ($\Delta L$) is small due to the high precision of the ruler, but it still contributes to $\Delta g$.
   - The uncertainty in timing ($\Delta T$) is more significant due to variability in the stopwatch measurements, likely from reaction time errors.
   - Assumptions like neglecting air resistance and assuming a point mass may lead to a slight overestimation of $g$. These factors suggest the result is reasonably accurate but could be improved with more precise timing methods (e.g., a photogate) and a controlled environment.

---

### Final Answer
- Measured $g = 9.84 \pm 0.02 \, \text{m/s}^2$, which is close to the standard $9.81 \, \text{m/s}^2$.
- Main uncertainties arise from timing variability, with minor contributions from length measurement. Limitations include the small-angle approximation and neglected effects like air resistance.
