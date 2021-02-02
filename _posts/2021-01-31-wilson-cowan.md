---
layout: post
title: The Wilson-Cowan model
subtitle: A dynamical model for the mean activity of populations of neurons
tags: [neuronal dynamics]
---


The brain is a complex system, and as all complex systems, its dynamics is highly nonlinear with complexity at many different scales. Large scale properties emerge from interactions at smaller scales but cannot be understood from the mechanisms of the smaller scales only. For example, at the molecular level one can study the mechanisms that generate action potentials in neurons but a perfect understanding of these mechanisms is not enough to understand how the brain is able to perform complex cognitive abilities such as memory, reasoning, spatial navigation, language, etc. Instead, the large scale of brain networks is more appropriate to gain insights on how the brain achieves such remarkable abilities. This is why it is important to model the dynamics of large networks of neurons.

The problem is that the brain contains millions of neurons and that it is impossible to simulate the dynamics at the level of single neuron when considering such a large number of them. Physics faced the same problem since it is impossible to solve the equations of motion for each of the $$10^{27}$$ atoms of a gas. Statistical physics provides a procedure to go from a theory of the constituents at the microscale, to a macroscale description of the system. The main idea is to define macroscopic observables as statistical properties of the microstates of the system. For example the internal energy of a gas is defined as an average of the energy over all the microstates. A similar approach can be taken to study the dynamics of the brain at large scales. Indeed, large populations of neurons with similar properties can be found in the cortex and in such populations, one can simulate the mean firing activity of the population of neurons instead of the activity of individual neurons, which greatly reduces the computational cost. 

In this blog post, I will present two such models, the rate model and the Wilson-Cowan model, and with the tools of the theory of nonlinear dynamical systems, we will analyse the dynamics of these models. Notably, we will see that the dynamics of the Wilson-Cowan model can exhibit hysteresis, limit cycles, and even a homoclinic orbit. The code used to make all the figures is available [here](https://github.com/nacloos/Wilson-Cowan-model) and don't hesitate to [contact me](mailto:cloosnathan@gmail.com) by email if you have questions, remarks or if you want more details.


## The rate model
The first model is a one-dimensional differential equation describing the dynamics of the mean activity of a population of neurons. This mean activity, also called firing rate or population activity, is denoted by $$A(t)$$. Assuming that the population receives an external input $$I$$ and that the neurons are not connected together, the rate model is the following

$$
\tau \dfrac{dA}{dt} = -A + F(I)
$$

where $$\tau$$ is a time constant and $$F$$ is a nonlinear activation function relating the input of a neuron to its activity. The solution at equilibrium is defined by the equation $$\frac{dA}{dt}=0$$, which simply gives $$A = F(I)$$. Now, let's consider a fully-connected population of neurons, with the same weight $$w$$ for each connection. Then the rate model becomes

$$
\tau \dfrac{dA}{dt} = -A + F(wA + I)
$$

The additional term $$wA$$ inside $$F$$ accounts for the inputs that each neuron receives from all the others. To found the equilibria, we can look at the solutions of the equation $$A = F(wA + I)$$ but we have to be careful because the number of solutions depend on the form of the activation function $$F$$ and on the external input $$I$$. Let's take a sigmoid activation function given by

$$
F(x) = \dfrac{1}{1+e^{-a(x-\theta)}} - \dfrac{1}{1+e^{a\theta}}
$$

where the parameter $$\theta$$ is the position of the inflection point of the sigmoid, and $$\frac{a}{4}$$ is the slope at $$\theta$$. You can see the shape of this function in red on the plots just below. We will now study the equilibrium solutions as a function of the external input $$I$$. 

#### Saddle-node bifurcations and hysteresis

We can understand qualitatively how the solutions $$A = F(wA + I)$$ vary with $$I$$ by plotting the graphs $$y=x$$ and $$y=F(wx+I)$$ and looking at their intersections. The plots on the figure below show these graphs for different values of the external input $$I$$. Let's start with the leftmost plot, $$I$$ is below a first critical threshold and there is only one equilibrium. Increasing $$I$$ moves the graph of $$F$$ towards the left, and at some point its upper part intersects with the line $$y=x$$. There are now three equilibria. If $$I$$ keeps increasing above a second critical threshold, the two lower equilibria disappear. So the behaviour of the system, i.e. towards which equilibrium state it will converge, depends critically on the value of $$I$$. This abrupt change of behaviour when a parameter is changed is called a [bifurcation](https://en.wikipedia.org/wiki/Bifurcation_theory) of the system. For systems of dimensions less or equal to two, all the possible bifurcations can be classified. In this case, we have what is called two [saddle-node bifurcations](https://en.wikipedia.org/wiki/Saddle-node_bifurcation). We will encounter other types of bifurcations in the next section. 

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/rate_bif.png" class="center">

The next figure gives another visualisation of these saddle-node bifurcations. the x-axis is the external input $$I$$ and the y-axis gives the equilibria of the system. We can see the two critical values of $$I$$ (dotted lines) for which there is an abrupt change of behaviour. The stability of the equilibria is also represented with two stable equilibria (solid lines) and one unstable (dashed lines).

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/rate_hysteresis.png" width="500" class="center">

Even though this model is a crude approximation of the dynamics of real neurons in the brain, it can actually play the role of a short-term memory. To see this, let's consider that the neural activity is initially at rest, i.e. $$A$$ is small, and that the external input $$I$$ is situated between the two critical thresholds (dotted lines on the figure above). An external stimulation will increase the value of $$I$$ but the activity $$A$$ will remained attracted to the stable equilibrium of low activity, until $$I$$ crosses the second critical threshold. At this point the low activity equilibrium disappears and the state converges towards the only equilibrium remaining, which is an equilibrium of high neural activity. When the external stimulation ceases, $$I$$ comes back to its initial value but the neurons remain highly activity, playing the role of a memory of the stimulation. This phenomenon is also well-know in physics under the name of [hysteresis](https://en.wikipedia.org/wiki/Hysteresis).




## The Wilson-Cowan model
The second model that we will analyse is called the Wilson-Cowan model ([original paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1484078/pdf/biophysj00727-0011.pdf)). This model also describes the dynamics of the mean activity but of two separate populations of neurons, one is excitatory and the other is inhibitory. The mean activity is denoted by $$E$$ for the excitatory neurons, by $$I$$ for the inhibitory neurons, and the dynamics for each population is taken to be the dynamics of the rate model described in the previous section. The two populations have recurrent connections and are connected to each other, as shown on the figure below. They also receive their own external input current, $$E_{ext}$$ and $$I_{ext}$$ for the $$E$$ and $$I$$ population, respectively.

<img src="{{ site.baseurl }}/assets/img/wilson_cowan/weights.png" width="400" class="center">

We obtain the following coupled dynamical equations

$$
\begin{aligned}
\tau \dfrac{dE}{dt} &= -E + F(w_{EE}E - w_{EI}I + E_{ext})\\
\tau \dfrac{dI}{dt} &= -I + F(w_{IE}E - w_{II}I + I_{ext})
\end{aligned}
$$

where all the weights $$w_{EE}$$, $$w_{EI}$$, $$w_{IE}$$, $$w_{II}$$ are positive constants, and the inhibitory connections contribute negatively when summing the inputs of a population. The activation function $$F$$ is the sigmoid function used in the previous section. The state space is two-dimensional, which allows for richer dynamics than the one-dimensional rate model as we will see in the subsequent sections. 

#### Nullclines
The equilibria of the system can be found by solving the equations $$\frac{dE}{dt}=0$$ and $$\frac{dI}{dt}=0$$, but these equations are often difficult to solve analytically for nonlinear dynamical systems. As in the previous section we can use a graphical method to understand qualitatively the solutions, and use numerical methods to obtain quantitative results. The equation $$\frac{dE}{dt}=0$$  defines a curve in the $$(E,I)$$ plane which is called the *E-nullcline*, it is shown in solid line on the figure below. Similarly, $$\frac{dI}{dt}=0$$ defines a curve called the *I-nullcline* and it is in dashed line on the figure. An equilibrium satisfies both of these equations and so it is an intersection of the E-nullcline and the I-nullcline. Note that while their intersections cannot be calculated directly, the equations of these nullclines can be obtained analytically and used to plot them. For the example of the figure below where the external inputs $$E_{ext}$$ and $$I_{ext}$$ are null, there is only one equilibrium corresponding to a state of low neural activity. We will use this graphical method again later on.
<img src="{{ site.baseurl }}/assets/img/wilson_cowan/nullclines.png" width="500" class="center">

<!--In this section I compute analytically the equation of the curve of equilibria where the trace, and then the determinant, of the Jacobian vanishes. These results will be useful in the next section, where we will analyse the bifurcations of the Wilson-Cowan model occurring when varying the external inputs $$E_{ext}$$ and $$I_{ext}$$. The approach taken to compute the bifurcation curves is similar to the one described in the book \emph{Weakly connected neural networks} \cite{hoppensteadt_izhikevich_1997}, p.45. However I provide more detailed calculations here, and I use a more general form for the sigmoid activation function.-->


#### Stability & bifurcation diagram
Let's try to understand the dynamical repertoire of the Wilson-Cowan model and in particular, what kind of dynamics can be found when varying the external inputs $$E_{ext}$$ and $$I_{ext}$$. To this end, we will analyse the bifurcations occurring in the 2-dimensional parameter space $$(E_{ext}, I_{ext})$$. Without losing interesting behaviours, the external inputs can be restricted to positive values.

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



Describe the mean activity and don't model the fluctuations. Ok if the the number of neurons is very large, the fluctuations become negligeable (law of large number). Stochastic Wilson-Cowan, requires tools from physics such as path integrals and Feymann diagrams. This will be for another post.





There is still a long way to go to understand the emergence of cognitive abilities from the dynamics of neurons, but this is a fascinating road to pursue.



## Materials

* An introductory book on the theory of nonlinear dynamical systems
* The original paper of the Wilson-Cowan model
* A book an neuronal dynamics for more advanced models of neurons and populations of neurons
* A notebook with exercices in python on network dynamics provided by Neuromatch Academy