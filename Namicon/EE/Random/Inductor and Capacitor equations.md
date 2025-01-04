- Inductor resists changes in current and capacitor resists changes in voltage

Capacitor (voltage cannot change instantaneously, that would mean infinite current across the capacitor): 
$$
I_{C} = C \cdot \frac{dV}{dt}
$$
- In a sinusoidal voltage circuit, when the voltage is 0, that is the maximum voltage change, so the peak current will be at 0V, and at peaks of voltage, the rate of change in voltage is 0 so the current will drop to 0 (the current wave will lead the voltage wave by 90 degree)

Inductor (current cannot change instantaneously, that would mean infinite voltage across the inductor):
$$
V_{L} = L \cdot \frac{dI}{dt}
$$
- In a sinusoidal voltage circuit, when the voltage reaches the peak, the rate of current change is maximum, and when the voltage is 0, the rate of current change is 0 (the current wave will lag behind the voltage wave by 90 degree)

- Note: red is the current of the inductor, blue is the current of the capacitor

- The reason why inductor current lags behind voltage and capacitor current leads voltage is because inductor resists against change in current. The EMF force that the inductor generate will cause current to lag behind voltage, and since inductor is just a wire, change in voltage will not be resisted. As for the capacitor current, the current needs to flow into the 2 plates first and then when the charge accumulates at the plates, the voltage difference will be established.


# Mathematical proof
## AC through pure inductor
Consider a pure inductor of inductance L.

Let an alternating EMF of instantaneous value E = $E_0\cdot Sin(ωt)$ be applied to the inductor. If an instant current at any time t is, the back EMF developed in the inductor is $L\cdot\frac{𝑑I}{𝑑𝑡}$ opposes the applied EMF **E**. The applied EMF must be at least equal to this back EMF in order that a current may just flow through the circuit.

Lenz's law:
$$L\cdot\frac{𝑑I}{𝑑𝑡} = E_0\cdot Sin(ωt)$$

$$\frac{𝑑I}{𝑑𝑡} = \frac{𝐸_0}{𝐿}\cdot Sin(ωt)$$


$$dI = \frac{𝐸_0}{𝐿}\cdot Sin(ωt) \;dt$$

On integration

$$I = -\frac{𝐸_0}{𝐿ω}\cdot Cos(ωt) + K$$

where K is a constant of integration. To find K, we can use the condition that average value of i over one cycle is zero which gives the the value of K = 0

The instantaneous current through inductor is

$$I = -\frac{𝐸_0}{𝐿ω}\cdot Cos(ωt)$$

$$I = \frac{𝐸_0}{𝐿ω}\cdot Sin(ωt - \frac{π}{2})$$

The quantity $𝐸_0𝐿ω$ is the maximum current or peak current ($I_0$)

$$I = I_0\cdot Sin(ωt - \frac{π}{2})$$

Phase angle in equation of instantaneous EMF is ωt
Phase angle in equation of current is (ωt - π/2)
Phase difference between EMF and current is ωt - (ωt - π/2) = π/2 (rad)

Hence current lags behind EMF by π/2 rad or 90°

## AC through pure capacitor
Consider a capacitor of capacitance C. 

Let an alternating EMF of instantaneous value, E = $E_0\cdot Sin(ωt)$ be applied to the capacitor. When the alternating voltage is applied to the plates of the capacitor, they are first charged in one direction and then in opposite direction. An alternating current flows through the capacitor. If the instantaneous current is $I$ at any time t through the capacitor, q is the charge on the capacitor at that time t. The potential difference across the capacitor (q/c) is equal to the applied EMF

$$\frac{q}{C} = E_0\cdot Sin(ωt)$$

$$q = E_0Cω\cdot Sin(ωt)$$

Differentiate both sides with respect to time

$$\frac{dq}{dt} = E_0Cω\cdot Cos(ωt)$$

$$I = E_0Cω\cdot Cos(ωt)$$

$$I = E_0Cω\cdot Sin(ωt + \frac{π}{2})$$

The quantity $E_0Cω$ is the maximum current or peak current ($I_0$)

$$I = I_0\cdot Sin(ωt + \frac{π}{2})$$

Phase angle in equation of instantaneous EMF is ωt
Phase angle in equation of current is (ωt + π/2)
Phase difference between EMF and current is ωt - (ωt + π/2) = -π/2 (rad)

Hence current leads EMF (ahead of the EMF) by π/2 rad or 90°
