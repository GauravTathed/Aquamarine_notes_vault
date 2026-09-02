This page describes methods for detecting and compensating excess micromotion in the Aquamarine linear four-rod Paul trap.

# Micromotion

## Introduction

A linear Paul trap uses an oscillating radio-frequency (RF) quadrupole field for radial confinement and static electric fields for axial confinement. The motion of a trapped ion therefore contains two characteristic components: relatively slow **secular motion** in the effective trapping potential and fast **micromotion** driven at the RF frequency. Some intrinsic micromotion is a normal and unavoidable part of RF trapping. However, if static stray fields displace the ion from the RF-field null, the ion experiences a nonzero oscillating electric field and develops **excess micromotion (EMM)**. RF phase differences between opposing electrodes, RF pickup on DC electrodes, and axial RF-field components can also produce EMM [(Berkeland et al., 1998)](zotero://select/library/items/GZU9KR3P) and [(Leibfried et al., 2003)](zotero://select/library/items/FGR9DM39).

Micromotion compensation aims to minimize this excess component by moving the ion's equilibrium position onto the RF null and, where necessary, correcting RF phase imbalances. This is important because EMM can:

- modulate and broaden optical transitions through the first-order Doppler effect;
- produce second-order Doppler (time-dilation) and AC Stark shifts;
- reduce Doppler-cooling efficiency and increase the ion's kinetic energy;
- degrade state preparation, coherent control, and gate fidelity; and
- reduce trap performance and disturb precision spectroscopy or ion-atom collision experiments.

The RF field has a null only at a particular position, so the compensation should be checked along every experimentally accessible direction. Common diagnostics include observing the ion's position while changing the RF confinement, measuring fluorescence correlated with the RF phase, measuring micromotion sidebands on a narrow transition, and monitoring parametric heating. The best method depends on the available imaging directions, laser-beam wavevectors, transition linewidths, and detector timing resolution [(Berkeland et al., 1998)](zotero://select/library/items/GZU9KR3P), [(Keller et al., 2015)](zotero://select/library/items/6TLULRJS), and [(Gloger et al., 2015)](zotero://select/library/items/P5K6L2CD).

## References
- [(Berkeland et al., 1998)](zotero://select/library/items/GZU9KR3P) - Foundational treatment of position-shift, resolved-sideband, and RF-fluorescence-correlation methods. [DOI](https://doi.org/10.1063/1.367318)
- [(Keller et al., 2015)](zotero://select/library/items/6TLULRJS) - Quantitative comparison of photon-correlation, resolved-sideband, and parametric-heating methods. [DOI](https://doi.org/10.1063/1.4930037)
- [(Gloger et al., 2015)](zotero://select/library/items/P5K6L2CD) - Imaging-based RF-null determination by tracking the ion as the RF confinement is varied. [DOI](https://doi.org/10.1103/PhysRevA.92.043421)
- [(Leibfried et al., 2003)](zotero://select/library/items/FGR9DM39) - Broad background on ion motion and control in RF Paul traps. [DOI](https://doi.org/10.1103/RevModPhys.75.281)

## Imaging-based Micromotion compensation
Here we look at the Ion position on the camera and try to align the RF null and DC nulls by reducing the RF confining potential so the Ion moves towards the DC null and then change the DC bias voltages so that the DC null is close to the RF null indicated by Ion not changing position when the RF confinement is weakened. 

```tikz
\begin{document}
\begin{tikzpicture}[
    scale=1.05,
    >=stealth,
    rod/.style={
        circle,
        draw=black,
        line width=1.2pt,
        minimum size=1.45cm,
        font=\small\bfseries,
        align=center
    },
    fieldline/.style={
        ->,
        blue!70!black,
        line width=1.15pt
    }
]

% Electrode rods at the corners of a square, numbered clockwise
\node[rod, fill=red!18]  (R1) at (-2.35, 2.35) {Rod 1\\$+V_{\mathrm{RF}}$};
\node[rod, fill=blue!16] (R2) at ( 2.35, 2.35) {Rod 2\\$-V_{\mathrm{RF}}$};
\node[rod, fill=red!18]  (R3) at ( 2.35,-2.35) {Rod 3\\$+V_{\mathrm{RF}}$};
\node[rod, fill=blue!16] (R4) at (-2.35,-2.35) {Rod 4\\$-V_{\mathrm{RF}}$};

% Instantaneous RF electric-field lines for cos(Omega t) > 0
\draw[fieldline] (-1.82, 1.82)
    .. controls (-0.80, 1.08) and ( 0.80, 1.08) .. ( 1.82, 1.82);
\draw[fieldline] (-1.82, 1.82)
    .. controls (-1.08, 0.80) and (-1.08,-0.80) .. (-1.82,-1.82);
\draw[fieldline] ( 1.82,-1.82)
    .. controls ( 1.08,-0.80) and ( 1.08, 0.80) .. ( 1.82, 1.82);
\draw[fieldline] ( 1.82,-1.82)
    .. controls ( 0.80,-1.08) and (-0.80,-1.08) .. (-1.82,-1.82);

% RF null and coordinate axes
\draw[->, gray!75, line width=0.8pt] (-0.75,0) -- (0.75,0)
    node[right, black] {$x$};
\draw[->, gray!75, line width=0.8pt] (0,-0.75) -- (0,0.75)
    node[above, black] {$y$};
\fill[orange!85!black] (0,0) circle (0.10);
%%\node[below right] at (0.08,-0.08) {RF null: $\mathbf{E}_{\mathrm{RF}}=0$};%%

\node[align=center] at (0,4.15)
    {Radial cross-section at $\cos(\Omega t)>0$};
\node[align=center, blue!70!black] at (0,-4.15)
    {Instantaneous RF electric field\\(direction reverses every half RF cycle)};

\end{tikzpicture}
\end{document}
```

*Idealized radial cross-section of the four-rod linear Paul trap. Opposing rods are driven in phase, while adjacent rods have opposite RF phase. 

