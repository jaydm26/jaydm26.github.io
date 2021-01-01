---
layout: post
title: Kalman Filter
date: 31-12-2020 13:00:00
type: post
categories: Computational Math and Engineering
tags: system dynamics, control systems
permalink: "/portfolio/computational-math-and-engineering/kalman-filter"
---

Kalman Filters offer a great way to estimate the state of a linear dynamic system with noisy measurements and inputs. But what is a dynamic system? In the simplest form, a dynamic system is given by:

\\[ \\dot{x} = Ax + Bu + \\mathcal{N}_x \\]

\\[ y = Cx + Du + \\mathcal{N}_y \\]

where $latex \\mathcal{N}_x \\sim \\mathcal{N}(0,\\sigma_x^2) $ and $latex \\mathcal{N}_y \\sim \\mathcal{N}(0,\\sigma_y^2) $

To express the above equation in a physical example, consider a room that has a heater and is also losing heat to the surrounding by convection. The temperature in the room is modelled as a single lumped model. The temperature of the room is obtained through a temperature sensor. In mathematical form, the model is expressed as:

\\[m C_p \\dfrac{\\partial T}{\\partial t} = -h A (T - T_o) + \\lambda Q \\]

where each term carries their usual meaning in heat transfer and $latex \\lambda $ is either 0 or 1 based on the state of the heater. Performing a variable transformation and rewriting the equation as a dynamic system,

\\[ \\dot{\\theta} = -\\dfrac{hA}{m C_p} \\theta + \\dfrac{\\lambda}{m C_p} Q\\]

\\[ y = \\theta\\]

By comparing, it is clear that:
\\[A = -\\dfrac{hA}{m C_p} \\]
\\[B = \\dfrac{\\lambda}{m C_p}  \\]
\\[C = 1 \\]
\\[D = 0 \\]

This model is fairly easy to solve should I ask for some specifics (say maintaining the temperature at $latex 25^{\\circ} C$). One needs to specify when to switch $latex \\lambda $ from 0 to 1 and vice versa. However, what if the sensor used for taking measurements is noisy in nature. Further, to complicate matters, what if the heater itself was not a fixed source of heat.


Let us add some numbers to this- let $latex A = -0.1$, $latex B = 0.5 \\lambda$. Instead of writing $latex Q \\sim \\mathcal{N}(Q_{mean},Q_{var})$,  I have chosen to split it into the mean and the noise, and then absorbing the $latex B $ of the noise component within the noise itself. $latex C$ is obviously 1 as the temperature sensor measure temperature. The noise associated with the sensor for the time being can be considered standard gaussian noise.

Thus, recasting the equation with its values:

\\[\\dot{\\theta} = -0.1 \\theta + \\lambda (0.5 Q_{mean} + \\epsilon_1 Q_{var})\\]

\\[y = \\theta + \\epsilon_2 sensor_{var}\\]

Here, $latex \\epsilon_1, \\epsilon_2 \\sim \\mathcal{N}(0,1)$

Let $latex Q_{mean} = 1, Q_{var} = 1, sensor_{var} = 0.04$.

To make for a comparative study, one can follow along with the code in the [Jupyter Notebook on Github](https://github.com/jaydm26/Kalman-Filter/blob/master/Kalman_Filter.ipynb). Alternatively, one can follow the code below.

First, we solve the model without any filters. Let us call this model the "no-filter model". Employing the forward Euler to solve the model, we have:

    t = np.linspace(0,100,num=1000)
    dt = t[1] - t[0]
    theta = np.zeros(t.size) # observed (includes all noises)
    theta_hat = np.zeros(t.size) # predicted by model (without any noise)
    theta_real = np.zeros(t.size) # real temperature with the heater noise
    np.random.seed(seed_val)
    for i in range(t.size-1):

      lam = int(theta[i] <= T_set)
      e1 = np.random.normal()
      e2 = np.random.normal()

      theta_hat[i+1] = (1 + A * dt) * theta_hat[i] + lam * (B * Q_mean) * dt
      theta_real[i+1] = (1 + A * dt) * theta_real[i] + lam * (B * Q_mean + e1 * np.sqrt(Q_var)) * dt
      theta[i+1] = theta_real[i+1] + e2 * np.sqrt(sensor_var)

|![]({{ site.baseurl }}/assets/img/No-Filter Model (Default Case).png)|

Without the filter, the predicted temperatures and the real temperatures differ by a lot. Further, the sensor readings are nowhere close to what the model predicts.

Now, let us try to use the Kalman Filter algorithm.

The Kalman Filter algorithm updates the prior estimate of the state space through a gain such that it minimises the expected value of the error covariance matrix. In other words, it tries to minimise the mean square error between the observed state and the prior estimate. It does so in a recursive fashion:

1. Calculate Kalman Gain $latex K_k = P_k'C^T(CP_k'C^T + R)^{-1}$ where $latex P_k'$ is the prior estimate of the error, $latex R$ is the covariance matrix of the observation noise.
2. Update the prediction using $latex \\hat{\\theta} = \\hat{\\theta}' + K_k(z_k - C\\hat{\\theta}')$, where $latex z_k$ is the observed measurement.
3. Update the estimate of the error $latex P_k = (I - K_k C) P_k'$
4. Move to the next time step through a projection step:

$latex \\hat{\\theta}_{k+1}' = A \\hat{\\theta}_k $, and  

$latex P_{k+1}' = A P_k A^T + Q$,
where $latex Q$ is the covariance matrix of the model noise.

    t = np.linspace(0,100,num=1000)
    dt = t[1] - t[0]
    theta = np.zeros(t.size) # observed
    theta_hat_prime = np.zeros(t.size) # predicted state (prior estimate)
    theta_hat = np.zeros(t.size) # posterior estimate of theta_hat_prime
    theta_real = np.zeros(t.size) # real theta including heater noise
    P = np.zeros(t.size) # Posterior estimate of error covariance
    P_prime = np.zeros(t.size) # Prior estimate of error covariance
    K = np.zeros(t.size) # Kalman Gain
    lam = np.zeros(t.size)
    P_prime[0] = 1
    np.random.seed(seed_val)
    for i in range(t.size-1):

      lam[i] = int(theta[i] <= T_set)
      e1 = np.random.normal()
      e2 = np.random.normal()

      K[i] = P_prime[i] * C / (C * P_prime[i] * C + sensor_var)
      theta_hat[i] = theta_hat_prime[i] + K[i] * (theta[i] - C * theta_hat_prime[i])
      P[i] = (1 - K[i] * C) * P_prime[i]
      theta_hat_prime[i+1] = (1 + A * dt) * theta_hat[i] + lam[i] * (B * Q_mean) * dt
      theta_real[i+1] = (1 + A * dt) * theta_real[i] + lam[i] * (B * Q_mean + e1 * np.sqrt(Q_var)) * dt
      theta[i+1] = theta_real[i+1] + e2 * np.sqrt(sensor_var)
      P_prime[i+1] = (1 + A * dt) * P[i] * (1 + A * dt) + Q_var

    i = t.size-1
    K[i] = P_prime[i] * C / (C * P_prime[i] * C + sensor_var)
    theta_hat[i] = theta_hat_prime[i] + K[i] * (theta[i] - C * theta_hat_prime[i])
    P[i] = (1 - K[i] * C) * P_prime[i]

|![]({{ site.baseurl }}/assets/img/Kalman Model (Default Case).png)|

While the graph for Kalman Filter looks quite chaotic, it actually performs better than the no-filter model.

||No-Filter Model| Kalman Filter|
|Norm Error between Real and Predicted | 13.7618 | 6.3947 |

This concludes that the Kalman Filter performs better than a no-filter model.

However, there are some caveats to the forgoing conclusion. The Kalman Filter is highly susceptible to the sensor noise covariance matrix. For example, let us increase $latex sensor_{var} = 0.49$. The results are as follows:

||No-Filter Model| Kalman Filter|
|Norm Error between Real and Predicted | 12.5632 | 17.7904 |

The error is greater than the no-filter model.

Alternatively, consider increasing $latex Q_{var} = 4$. The results are as follows:

||No-Filter Model| Kalman Filter|
|Norm Error between Real and Predicted | 28.9569 | 6.5673 |

The graphs for these cases can be found [here](https://github.com/jaydm26/Kalman-Filter).

Thus, the Kalman Filter provides an excellent means to predict the temperature accurately assuming there is input noise in the system. It is however susceptible to sensor noise and thus, it should be ensured that white noise from sensors is minimised.

To go over the derivation of the Kalman Filter, I recommend reading the [tutorial from MIT](http://web.mit.edu/kirtley/kirtley/binlustuff/literature/control/Kalman%20filter.pdf). While the Kalman Filter works for linear models, there are other filters that can account for the non-linearity in the projection of states (i.e. $latex A$ need not necessarily be linear). For non-linear filters, refer to Extended Kalman Filters, Unscented Kalman Filters, etc.
