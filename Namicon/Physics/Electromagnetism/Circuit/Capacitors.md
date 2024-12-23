Capacitor is basically just 2 connected metal plates separated from each other.

![](../Assets/capacitor-visualization.png)

If we put it across a voltage source, the charges will flow the sheet since they're attracted to the other side. When the charges reached the sheet, the charges on the opposite sheet will get repelled by the charges on this sheet. Those repelled charges continue on the same path. Now there's more positive charges on 1 side and more negative charges on the other.

![](../Assets/capacitor-repelled-charges-visual.png)

Overtime, the capacitor will reach an equilibrium when it has a voltage across the plates that directly opposes the voltage created by the battery.

We can calculate capacitance by calculating the charge on the plate when you apply some voltage.

$$
C = \frac{Q}{V}
$$

If we want to know exactly how much capacitance will be on the capacitor over time then we can break it down using the Kirchhoff's loop rule.

![](capacitor-kirchhoff-loop-rule.png)

So we get this equation from the Kirchhoff's loop rule:

$$
\begin{align}
C = \frac{Q}{V_c} \rightarrow V_c = \frac{Q}{C}
\\
V - \frac{Q}{C} - IR = 0
\end{align}
$$

We get that the voltage form the battery minus the voltage from the capacitor minus the voltage drop across the resistor must be equal to 0. Since the current is change in charge over change in time, we can rewrite the equation as a differential equation:

$$
V - \frac{Q}{C} - \frac{dQ}{dt}R = 0
$$

Solving it gives us this exponential decay (carrying capacity) function:

$$
Q(t) = Q_f[1-e^{\frac{-t}{RC}}]
$$

![](../Assets/capacitor-exponential-decay-graph.png)

At the beginning, the charge accumulates very quickly, then it levels off over time as the capacitor is fully charged. We can also look at the current, which is the slope of the charge curve.

$$
I(t) = \frac{Q_f}{RC} e^{\frac{-t}{RC}}
$$

![](../Assets/capacitor-charge-curve-slope.png)

At the beginning, a lot of charges is flowing through the capacitor, then it goes to 0 over time because the capacitor is opposing the voltage from the battery.

This circuit is made to account for voltage fluctuation.

![](../Assets/voltage-fluctuation-circuit.png)

When the battery's on, at first, the current goes through the capacitor, charging it up. Once the capacitor is fully charged, the voltage source's current will divert, going to the rest of the circuit. If the voltage source somehow goes out, the current will continue to go from the capacitor to the rest of the circuit. We can model the decay with the exponential decay equation:

$$
Q(t) = Q_f e^{\frac{-t}{RC}}
$$

![](../Assets/capacitor-decay-circuit.png)

The capacitor starts with a lot of charges and quickly gets rid of it through the resistor.

Generally, the capacitor works kinda like an open wire at the beginning, allowing current flow when it's charging up its plates. Over time, as it gets more charged up, it will turn into an open circuit, opposing voltage applied across it.

Geometrically speaking, let's call the area of the plate $A$ and the distance between the plates $d$:

![](../Assets/capacitor-geometric-label.png)

We know the electric field can be calculated like this:

$$
E = \frac{\sigma}{2\epsilon_0}
$$

For 2 plates, we'll get this electric field equation:

$$
E = \frac{\sigma}{\epsilon_0}
$$

The charge density is the same thing as the charge per area. So we get this:

$$
E = \frac{Q}{A \epsilon_0}
$$

To get the voltage, we can add up the product of the electric field along some length. So it's just $E$ times the length since the electric field is constant:

$$
|V| = \int_0^d{\vec{E} \cdot d \vec{s}} = \int_0^d{\frac{Q}{A \epsilon_0}dl} = \frac{Qd}{A\epsilon_0}
$$

So we can calculate the capacitance using the electric field:

$$
C = \frac{Q}{V} = \frac{Q}{\frac{Qd}{A\epsilon_0}} = \frac{\epsilon_0 A}{d}
$$

The capacitance is proportional to the area and inversely proportional to the distance between the plates. 

Capacitors in parallel effectively creates a bigger capacitor with *larger plates*.

$$
C_T = C_1 + C_2 + ... + C_n
$$

Capacitors in series effectively creates a bigger capacitor with *larger distance between the plates*.

$$
\frac{1}{C_T} = \frac{1}{C_1} + \frac{1}{C_2} + ... + \frac{1}{C_n}
$$

Energy is also a property of the capacitor that we can calculate with integrating voltage times charge, which gives us potential energy.

$$
U = \int{V dq} = \int_0^Q{\frac{q}{C}dq} = \frac{Q^2}{2C} = \frac{1}{2}CV^2
$$

We can also calculate for the energy per volume $V$ or the energy density of the electric field:

$$
\begin{align}
U = \frac{1}{2}(\frac{\epsilon_0A}{d})(Ed)^2 = \frac{1}{2}\epsilon_0 E^2 (Ad)
\\
\frac{U}{V} = \frac{1}{2}\epsilon_0E^2
\end{align}
$$

Most capacitors have an insulating material between the 2 plates called a *dielectric*. While it can't conduct charges, it can reorient the charges in itself to fit the charges in the capacitor's plates. We get an electric field from this, flowing opposite from the capacitor's. Since the voltage is derived from the electric field, a weaker electric field means higher capacitance since we need less voltage to get the same amount of charge on each plate, making the capacitor more powerful.

$$
\begin{align}
|V| = \int{\vec{E} \cdot d\vec{l}}
\\
C = \frac{Q}{V}
\end{align}
$$

The dielectric basically weakens the electric field by some factor $\kappa$. You can think of it as it changes the permeability constant ($\epsilon_0$).

$$
\begin{align}
E \rightarrow \kappa E
\\
\epsilon_0 \rightarrow \kappa \epsilon_0
\end{align}
$$