# Electro-Thermal Circuit With Heat Flow, Temperature-Dependent Resistance, and Feedback

This model couples an electrical resistor to a first-order thermal network.

## Electrical domain

- Supply voltage: \(V_s\)
- Temperature-dependent resistance: \(R(T)\)
- Current: \(i = \frac{V_s}{R(T)}\)
- Electrical dissipation to heat:

\[
P_e = i^2 R(T) = \frac{V_s^2}{R(T)}
\]

## Thermal domain

- Thermal resistance to ambient: \(R_{th}\)
- Thermal capacitance: \(C_{th}\)
- Ambient temperature: \(T_{amb}\)
- Temperature state: \(T\)

Dynamic equation:

\[
C_{th}\frac{dT}{dt} = P_e - \frac{T - T_{amb}}{R_{th}}
\]

Steady-state temperature rise:

\[
T = T_{amb} + P_e R_{th}
\]

## Feedback mechanism

Temperature feeds back into the electrical domain because resistance depends on temperature:

\[
R = R(T)
\]

A common linear approximation is:

\[
R(T) = R_0\left[1 + \alpha (T - T_0)\right]
\]

where \(\alpha\) is the temperature coefficient of resistance.

## Interpretation

- Higher current increases \(P_e\), heating the element.
- Heat flows to ambient through \(R_{th}\), while \(C_{th}\) sets thermal time lag.
- The resulting temperature changes \(R(T)\), which changes current and power.
- This creates electro-thermal feedback that can be stabilizing or destabilizing depending on \(\alpha\), drive conditions, and thermal path.
