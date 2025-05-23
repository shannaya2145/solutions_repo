# Problem 1


### Deliverable 1: Tabulated Data in Markdown

Let's assume the following measurements based on the procedure:

- **Length of the pendulum ($L$)**: Measured as 1.000 m using a ruler with a resolution of 0.001 m (1 mm).  
  - Uncertainty in length ($\Delta L$) = (Ruler Resolution) / 2 = 0.001 / 2 = 0.0005 m.

- **Time for 10 oscillations ($T_{10}$)**: Measured 10 times using a stopwatch. Let's assume the following values (in seconds):  
  20.1, 20.2, 20.0, 20.3, 20.1, 20.2, 20.0, 20.1, 20.2, 20.1.

- **Mean time for 10 oscillations ($\bar{T}_{10}$)**:  
  $\bar{T}_{10} = \frac{20.1 + 20.2 + 20.0 + 20.3 + 20.1 + 20.2 + 20.0 + 20.1 + 20.2 + 20.1}{10} = \frac{201.3}{10} = 20.13 \, \text{s}$.

- **Standard deviation of $T_{10}$ ($\sigma_T$)**:  
  First, calculate the variance:  

  $\sigma_T^2 = \frac{(20.1-20.13)^2 + (20.2-20.13)^2 + (20.0-20.13)^2 + \ldots + (20.1-20.13)^2}{10}$  

  $= \frac{(-0.03)^2 + (0.07)^2 + (-0.13)^2 + (0.17)^2 + (-0.03)^2 + (0.07)^2 + (-0.13)^2 + (-0.03)^2 + (0.07)^2 + (-0.03)^2}{10}$ 

  $= \frac{0.0009 + 0.0049 + 0.0169 + 0.0289 + 0.0009 + 0.0049 + 0.0169 + 0.0009 + 0.0049 + 0.0009}{10} = \frac{0.0811}{10} = 0.00811$.  

  So, $\sigma_T = \sqrt{0.00811} \approx 0.090 \, \text{s}$.



- **Uncertainty in the mean time ($\Delta T_{10}$)**:  

  $\Delta T_{10} = \frac{\sigma_T}{\sqrt{n}} = \frac{0.090}{\sqrt{10}} \approx \frac{0.090}{3.162} \approx 0.028 \, \text{s}$.

- **Period ($T$) and its uncertainty ($\Delta T$)**:  

  $T = \frac{\bar{T}_{10}}{10} = \frac{20.13}{10} = 2.013 \, \text{s}$,  

  $\Delta T = \frac{\Delta T_{10}}{10} = \frac{0.028}{10} = 0.0028 \, \text{s}$.

- **Gravitational acceleration ($g$)**: 

  $g = \frac{4\pi^2 L}{T^2} = \frac{4 \times (3.1416)^2 \times 1.000}{(2.013)^2} = \frac{4 \times 9.8696 \times 1.000}{4.0522} \approx \frac{39.4784}{4.0522} \approx 9.74 \, \text{m/s}^2$.

- **Uncertainty in $g$ ($\Delta g$)**:  

  $\Delta g = g \sqrt{\left( \frac{\Delta L}{L} \right)^2 + \left( 2 \frac{\Delta T}{T} \right)^2}$  

  $\frac{\Delta L}{L} = \frac{0.0005}{1.000} = 0.0005$,  

  $\frac{\Delta T}{T} = \frac{0.0028}{2.013} \approx 0.00139$, 

  $\Delta g = 9.74 \sqrt{(0.0005)^2 + (2 \times 0.00139)^2} = 9.74 \sqrt{0.00000025 + (0.00278)^2} = 9.74 \sqrt{0.00000025 + 0.00000773} \approx 9.74 \sqrt{0.00000798} \approx 9.74 \times 0.00283 \approx 0.028 \, \text{m/s}^2$.

Now, let's tabulate the data in Markdown:

| Parameter         | Value         | Uncertainty      |
|-------------------|---------------|------------------|
| $L$           | 1.000 m       | $\Delta L$ = 0.0005 m  |
| $T_{10}$ (10 measurements) | 20.1, 20.2, 20.0, 20.3, 20.1, 20.2, 20.0, 20.1, 20.2, 20.1 s | -                |
| $\bar{T}_{10}$| 20.13 s       | $\Delta T_{10}$ = 0.028 s |
| $\sigma_T$    | 0.090 s       | -                |
| $T$           | 2.013 s       | $\Delta T$ = 0.0028 s |
| $g$           | 9.74 m/s²     | $\Delta g$ = 0.028 m/s² |

---

### Deliverable 2: Discussion on Sources of Uncertainty and Their Impact

#### Comparison with Standard Value
The calculated $g = 9.74 \pm 0.028 \, \text{m/s}^2$ can be compared to the standard value of $g = 9.81 \, \text{m/s}^2$. The measured value is slightly lower by about 0.07 m/s², but the standard value lies just outside the uncertainty range (9.74 + 0.028 = 9.768 m/s²). This suggests possible systematic errors or environmental factors affecting the measurement.

#### Sources of Uncertainty
1. **Measurement Resolution of Length ($\Delta L$)**:  
   The ruler's resolution (1 mm) contributes to $\Delta L = 0.0005 \, \text{m}$. This uncertainty propagates to $g$ through the term $\frac{\Delta L}{L}$. For $L = 1.000 \, \text{m}$, this relative uncertainty is small (0.05%), but it still impacts the final value of $g$. Using a more precise tool, like a caliper with a resolution of 0.01 mm, would reduce $\Delta L$ to 0.000005 m, lowering the contribution to $\Delta g$.

2. **Timing Uncertainty ($\Delta T_{10}$)**:  
   The stopwatch measurements have variability, reflected in $\sigma_T = 0.090 \, \text{s}$. Human reaction time in starting and stopping the stopwatch likely contributes to this. The uncertainty in the mean ($\Delta T_{10} = 0.028 \, \text{s}$) propagates to $T$, and since $g \propto \frac{1}{T^2}$, the relative uncertainty in $T$ is doubled in $g$. Using an automated timing system (e.g., a photogate) could reduce this error.

3. **Systematic Errors**:  
   - **Air Resistance**: The pendulum's motion may be slightly damped, increasing the measured period and lowering $g$.  
   - **Angle of Displacement**: The procedure specifies a small angle (<15°), but if the angle is too large, the small-angle approximation 
   
   ($T = 2\pi \sqrt{\frac{L}{g}}$) becomes less accurate, leading to errors in $g$.  
   - **Non-Uniform $g$**: The local value of $g$ depends on altitude and latitude. At sea level, $g \approx 9.81 \, \text{m/s}^2$, but it decreases with altitude, which might explain the slightly lower measured value.

#### Impact on Results
- The uncertainty in $T$ has a larger impact on $\Delta g$ because of the factor of 2 in the error propagation formula ($2 \frac{\Delta T}{T}$). In this case, $2 \frac{\Delta T}{T} \approx 0.00278$, which dominates over $\frac{\Delta L}{L} = 0.0005$.  
- Improving the timing precision would have the most significant effect on reducing $\Delta g$. For example, reducing $\sigma_T$ to 0.045 s (e.g., with better equipment) would halve $\Delta T_{10}$, reducing $\Delta g$ to approximately 0.021 m/s², bringing the measured $g$ closer to the standard value within uncertainty.

In conclusion, while the experiment yields a reasonable estimate of $g$, refining the timing method and accounting for systematic effects like air resistance and local variations in $g$ would improve accuracy and precision.
