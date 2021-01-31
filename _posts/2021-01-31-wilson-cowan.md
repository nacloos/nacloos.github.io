---
layout: post
title: The Wilson-Cowan model
tags: [neuronal dynamics]
---


The brain is a complex system, and as all complex systems, its dynamics is highly nonlinear with complexity at many different scales. Large scale properties emerge from interactions at smaller scales but cannot be understood from the mechanisms of the smaller scales only. For example, at the molecular level one can study the mechanisms that generate action potentials in neurons. But a perfect understanding of the molecular mechanisms is not enough to understand how the brain is able to perform complex cognitive abilities such as memory, reasoning, spatial navigation, language, etc. Instead, the large scale of brain networks is more appropriate to gain insights on how the brain achieves such remarkable abilities. This is why it is important to model the dynamics of large networks of neurons.

The problem is that the brain contains millions of neurons and that it is impossible to simulate the dynamics of individual neurons when considering such a large number of them. Physics faced the same problem where it is impossible to solve the equations of motion for each of the $$10^{27}$$ atoms of a gas. Statistical physics provides a procedure to go from a theory of the constituents at the microscale, to a macroscale description of the system. The main idea is to define macroscopic observables as statistical properties of the microstates of the system. For example the internal energy of a gas is defined as the average of the energy over all the microstates, i.e. positions and momenta of each atoms, compatible with the macrostate, i.e. thermodynamic state, of the gas. A similar approach can be taken to study the dynamics of the brain at larger scales than individual neurons. Indeed, large populations of neurons with similar properties can be found in the cortex and in such populations, one can simulate the mean firing activity of the population of neurons instead of the activity of individual neurons, which greatly reduces the computational cost.


## The rate model
The rate model describes the dynamics of the mean activity of a homogeneous population of neurons. The population activity, $$A(t)$$, can be seen as the proportion of active neurons at time $$t$$. If the neurons in the population are disconnected, the dynamics is the following

$$
\tau \dfrac{dA}{dt} = -A + F(I)
$$

where $$\tau$$ is a time constant, $$I$$ is the external input current, and $$F$$ is a nonlinear activation function relating the input of a neuron to its activity. The equilibrium is $$A^* = F(I)$$, and there is no bifurcation. Recurrent connections can be added within the population of neurons to get a more interesting behaviour. We assume that the population is fully-connected with the same weight $$w$$ for each connection. This assumption allows us to write the dynamics of the population activity as

$$
\tau \dfrac{dA}{dt} = -A + F(wA + I)
$$

The additional term $$wA$$ inside $$F$$ accounts for the inputs that each neuron receives from all the others. 
Now the equilibria are the solutions of the equation $$A = F(wA + I)$$, which depend on the external input $$I$$. To study the bifurcations of this dynamical system, it is needed to choose a particular form for the activation function $$F$$. A popular choice is the sigmoid activation function, which is given by

$$
F(x) = \dfrac{1}{1+e^{-a(x-\theta)}} - \dfrac{1}{1+e^{a\theta}}
$$

where the parameter $$\theta$$ is the position of the inflection point of the sigmoid, and $$\frac{a}{4}$$ is the slope at $$\theta$$.

#### Saddle-node bifurcation
The system undergoes two saddle-node bifurcations as the parameter $$I$$ varies. To see why, the figure below shows the graphs $$y=x$$ and $$y=F(wx+I)$$ for different values of the external input $$I$$. The parameters $$w$$, $$a$$, and $$\theta$$ are kept fixed. The equilibria are given by the intersections of these two graphs. Let's start with the leftmost plot, $$I$$ is below the first critical threshold and there is only one equilibrium. Increasing $$I$$ moves the graph of $$F$$ towards the left, and at some point its upper part intersects with the line $$y=x$$. There are now three equilibria. If $$I$$ keeps increases above the second critical threshold, the two lower equilibria disappear. The second figure gives another visualization of these saddle-node bifurcations, where the stability of the equilibria is also represented. We recognize a hysteresis phenomenon.

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/rate_bif.png" class="center">
<img src="{{ site.baseurl }}/assets/img/wilson_cowan/rate_hysteresis.png" width="500" class="center">


## The Wilson-Cowan model
The Wilson-Cowan model describes the dynamics of two populations of neurons, one is excitatory and the other is inhibitory. It is assumed that all the neurons within a population have identical characteristics, so that their dynamics can be described by the mean population activity. The population activity is denoted by $$E$$ for the excitatory neurons, and by $$I$$ for the inhibitory neurons. 
The dynamics of each population is taken to be the dynamics of the rate model described in the previous section.
In order to simplify the calculations, it is further assumed that the two populations have the same parameters, i.e. $$\tau$$, $$a$$, and $$\theta$$. If needed this assumption can be relaxed. The two populations have recurrent connections and are connected to each other, as shown on figure. They also receive their own external input current, $$E_{ext}$$ and $$I_{ext}$$ for the $$E$$ and $$I$$ population, respectively. We obtain the following coupled dynamical equations

$$
 \begin{aligned}
\tau \dfrac{dE}{dt} &= -E + F(w_{EE}E - w_{EI}I + E_{ext})\\
\tau \dfrac{dI}{dt} &= -I + F(w_{IE}E - w_{II}I + I_{ext})
\end{aligned}
$$

where all the weights $$w_{EE}$$, $$w_{EI}$$, $$w_{IE}$$, $$w_{II}$$ are positive constants, and the inhibitory connections contribute negatively when summing the inputs of a population. This dynamical system is called the Wilson-Cowan model \cite{wilson_cowan_1972}. Its state space is two-dimensional, which allows for richer dynamics than the one-dimensional rate model as we will see in the subsequent sections. 

#### Analysis of the equilibria
In this section I compute analytically the equation of the curve of equilibria where the trace, and then the determinant, of the Jacobian vanishes. These results will be useful in the next section, where we will analyse the bifurcations of the Wilson-Cowan model occurring when varying the external inputs $$E_{ext}$$ and $$I_{ext}$$. The approach taken to compute the bifurcation curves is similar to the one described in the book \emph{Weakly connected neural networks} \cite{hoppensteadt_izhikevich_1997}, p.45. However I provide more detailed calculations here, and I use a more general form for the sigmoid activation function.

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/weights.png" width="400" class="center">
<img src="{{ site.baseurl }}/assets/img/wilson_cowan/nullclines_annotated.png" width="500" class="center">
#### Stability & bifurcation diagram
In this section we will try to understand the dynamical repertoire of the Wilson-Cowan model, and, in particular, how the dynamics depends on the external inputs $$E_{ext}$$ and $$I_{ext}$$. To this end, we will analyse the bifurcations occurring in the 2-dimensional parameter space $$(E_{ext}, I_{ext})$$. Without losing interesting behaviours, the external inputs can be restricted to positive values.

In the previous section, we computed analytically bifurcation curves. The Hopf bifurcation curve is given by the equilibria where the trace of the Jacobian vanishes, and where its determinant is positive. This curve in the $$(E_{ext}, I_{ext})$$ plane is shown on figure below, in solid line. The saddle-node bifurcation curve is given by the equilibria where the determinant of the Jacobian vanishes, which is depicted by the dotted line. To better visualize the behaviour of the system in the diagram of the figure, I added information about the equilibria for a regular grid of points in the $$(E_{ext}, I_{ext})$$ plane. For a particular point on this grid, the number of dots gives the number of equilibria, their shape represents their stability, and their color indicates whether their eigenvalues are real or complex conjugate. These results were obtained by computing numerically the intersections of the two nullclines, and by looking at the eigenvalues of the Jacobian evaluated at these intersections.

Now, let's take a closer look at the trajectories of the system for different regions of this diagram. More specifically, we will see that when the external input of the inhibitory population is large, the dynamics is similar to that of the rate model. We will also encounter a region where we can prove the existence of a limit cycle. And finally, we will see what happens near a Bogdanov-Takens bifurcation point.

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/bifurcation_diagram.png" width="700" class="center">

#### Hysteresis
We can see on the diagram that when $$I_{ext}$$ is larger than more or less 11, the Hopf bifurcation curve stops, and there remains only the two saddle-node bifurcations. This situation is similar to that of the rate model, which means that there is a hysteresis phenomenon. Indeed, the right plot of the figure \ref{fig:wilson_cowan_hysteresis} shows a hysteresis for the excitatory activity $$E$$ when $$E_{ext}$$ varies. The external input $$E_{ext}$$ is slowly increased and crosses the two saddle-node bifurcation curves before decreasing slowly back to its starting point, while $$I_{ext}$$ is kept at a fixed value of 12. The left part of the figure \ref{fig:wilson_cowan_hysteresis} shows what happens for the nullclines before, between, and after the two saddle-node bifurcations. 

Note that a small discrepancy between the simulated bifurcation and the theoretical one (in dotted lines, on the right plot) can be observed. This is due to the incomplete relaxation to the equilibrium.
<div style="text-align: center">
	<div style="display: inline-block">
    	<img src="{{ site.baseurl }}/assets/img/wilson_cowan/sadddle_node.png" width="350" class="center">
	</div>
	<div style="display: inline-block">
   		<img src="{{ site.baseurl }}/assets/img/wilson_cowan/hysteresis.png" width="450" class="center">
   	</div>
</div>




#### Limit cycle
There is a region in the $$(E_{ext}, I_{ext})$$ plane where there is only one equilibrium that is an unstable focus (an orange circle in the diagram of figure \ref{fig:bif_diagram}). We can prove using the Poincaré-Bendixson theorem that, for external inputs in that region, the trajectories of the Wilson-Cowan model converge towards a periodic orbit, e.g. a limit cycle.

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/limit_cycle.png" width="1000" class="center">

#### Bogdanov-Takens bifurcation
The Bogdanov-Takens bifurcation \cite{Guckenheimer_2007} is a bifurcation of co-dimension two, i.e. two control parameters, with a curve of saddle-node bifurcation, a curve of Hopf bifurcation, and a curve of homoclinic bifurcation. Its bifurcation point is characterised by a double zero eigenvalue. Therefore, we can look at the intersections between the Hopf and the saddle-node bifurcation curves shown on the figure \ref{fig:bif_diagram}. The lowermost and the upper right intersections are Bogdanov-Takens bifurcations. But the one around $(6.5,3)$ is not because the Hopf bifurcation doesn't involve the node of the saddle-node bifurcation.
% A cusp of the two saddle-node bifurcation curves.

Let's analyse the upper right Bogdanov-Takens bifurcation, which is around (13.5, 11) in the diagram of figure \ref{fig:bif_diagram}. After investigating a few simulations, we can determine that the Hopf bifurcation is subcritical, which consists in a stable focus absorbing an unstable limit cycle to become unstable. The homoclinic bifurcation curve is not represented in figure \ref{fig:bif_diagram} because it is difficult to compute it analytically. Nevertheless, with the knowledge of the Bogdanov-Takens bifurcation, we know that it is located just on the left of the subcritical Hopf bifurcation curve. The figure \ref{fig:BT_bif} shows some orbits in the phase plane to better understand what happens in this Bogdanov-Takens bifurcation. Each color corresponds to an orbit whose starting point is denoted by a dot. The solid and dotted gray lines in the background are the E-nullcline and I-nullcline, respectively. The external input of the inhibitory population is kept fixed to $\Iext=9$. While the external input $\Eext$ starts on the left of the homoclinic bifurcation, and it will be increased so as to cross the homoclinic bifurcation first, the Hopf bifurcation second, and the saddle-node bifurcation last. 


Let's start with the leftmost plot. There are two equilibria, a saddle-point and a stable focus (the third equilibrium, the stable node, is not of interest here). We can observe that the left unstable manifold of the saddle-point is attracted by the stable focus. Increasing a bit $\Eext$ and we arrive at the homoclinic bifurcation. The homoclinic orbit is shown in black on the second plot, and it is unstable as the orange trajectory starting inside it converges towards the stable focus. Just after this bifurcation, an unstable limit cycle appears around the stable focus, depicted in orange on the third plot. 
Until the Hopf bifurcation, the unstable limit cycle shrinks, and finally, collapses on the focus. At this point, the focus loses its stability as it is observed on the fourth plot. The green trajectory spirals away from the node and, in this case, converges towards the stable manifold of the saddle-node.
As the saddle-node bifurcation is crossed, the focus and the saddle-point collide and the two equilibria disappear. Finally, on the last plot, there is a saddle-node ghost that keeps influencing the trajectories.

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/homoclinic.png" class="center">

## Conclusion
We have seen that the Wilson-Cowan model exhibits a rich dynamical repertoire, and that we can move from a dynamical regime to another with two control parameters, for example the external input of the excitatory neurons and the one of the inhibitory neurons. We found hysteresis and limit cycles, which are thought to have significant functions in the brain. For example the hysteresis is a mechanism of short-term memory that might be used in the brain. It is also known that oscillatory patterns play an important role in the hippocampus for abilities such as spatial navigation. 


With the two models presented here, the dynamics at the scale of neuronal populations is obtained by averaging the fast dynamics of individual neurons, and by considering as constant the slow dynamics of synaptic weights. But they are better ways to take into account the dynamics at other scales. For example higher order statistics of the neural activity can be considered with an explicit modelisation of the noise using stochastic dynamics. They are also adaptive models of neurons that take into account the slow dynamics of ion channels.
The analysis of such models is more challenging and requires other tools than those used here. It would also be interesting to directly compare the activity of a simulated network of neurons, with the activity predicted by a population model such as the Wilson-Cowan model. For example, one could ask which bifurcations can still be observed in the simulated network.

There is still a long way to go to understand the emergence of cognitive abilities from the dynamics of neurons, but this is a road that I find fascinating to pursue.



### Details of the calculations (optional)
