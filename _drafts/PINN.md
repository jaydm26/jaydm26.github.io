Physics Informed Neural Networks (PINNs) are the latest attempt in merging two of my favourite communities: machine learning and partial differential equations. PINNs are really useful at solving PDEs at remarkable speed and accuracy based of very little learning data.

The remarkable results are as a result of using the data in a wise manner: the data is used to reduce the loss in prediction of the variable, and further to reduce the residual losses from the PDE. This double use of data allows this network to predict shocks from the Viscous Burgers Equation with great accuracy. It can also predict field variables a substantial time into the future without performing intermediate step calculations by the use a huge RK method jump [Maziar et. al. used more than 50]. This circumvents the CFL condition, and makes simulating the future incredibly fast.

Additional advantages of the PINNs are its ability to predict parameters of a PDE with incredible accuracy too. Moreover, the framework of the PINN allows for calculating additional quantities like gradients and fluxes with relative ease (gradients are freely available since they are coded using Automatic Differentiation).

PINNs however are sensitive to the data it receives as input.

I will update this with an example to show what this means.
