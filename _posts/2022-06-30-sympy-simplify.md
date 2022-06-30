---
layout: post
title: "Organize and simplify complex expressions using Python"
date: 2022-06-30
description: How to organize and simplify complex expressions using sympy
share: true
tags:
 - python
---

Using sympy to simplify following equation with regards to $\gamma$:

$$ R_dL*[g(1 + \frac{Lq_v^c}{R_dT})+q_v^cL\frac{\varepsilon\gamma}{\delta+\gamma}]=(R_vR_dT^2\gamma+gR_vT)(C_p+\frac{q_v^cL^2}{R_vT^2}) $$

$$ R_dL*[g(R_dT + Lq_v^c)(\delta+\gamma)+q_v^cL\varepsilon R_dT\gamma]*R_vT^2=(R_vR_dT^2\gamma+gR_vT)(C_pR_vT^2 + q_v^cL^2)(\delta+\gamma)R_dT $$

$$ R_dL*[g(R_dT + Lq_v^c)(\delta+\gamma)+q_v^cL\varepsilon R_dT\gamma]*R_vT^2 - (R_vR_dT^2\gamma+gR_vT)(C_pR_vT^2 + q_v^cL^2)(\delta+\gamma)R_dT=0 $$

python code:

```python
from sympy import *
x, Rd, Rv, L, g, qvc, Cp, epsilon, sigma, T = symbols('x Rd Rv L g qvc Cp epsilon sigma T')
expr = Rd*L*((g*Rd*T + g*L*qvc)*(sigma + x) + qvc*L*epsilon*Rd*T*x)*Rv*T**2 - (Rv*Rd*T**2*x + g*Rv*T)*(Cp*Rv*T**2 + qvc*L**2)*(sigma + x)*Rd*T
expr2 = expr.expand()  #expand expression
expr3 = collect(expr2, x)   # collect expression with respect to x
simplify(expr3)  # simplify expression with respect to x
```

result:
$$Rd*Rv*T**3*(-Cp*Rv*T*g*sigma + L*Rd*g*sigma - Rd*x**2*(Cp*Rv*T**2 + L**2*qvc) - x*(Cp*Rd*Rv*T**2*sigma + Cp*Rv*T*g - L**2*Rd*epsilon*qvc + L**2*Rd*qvc*sigma - L*Rd*g))$$

Note, in the code, $\gamma$ is set to x!

last update: 2022/06/30