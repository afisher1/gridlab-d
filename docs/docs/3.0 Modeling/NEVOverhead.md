# NEVOverhead

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/NEVOverhead
**Work in progress/unfinished**

## Contents

  * 1 Assumptions
  * 2 Variables
  * 3 Overhead Distribution Lines
    * 3.1 Series Impedance
    * 3.2 Shunt Admittance
## Assumptions

Assumes ground is infinite plane, zero potential plane 

  


## Variables

Table 1 - Equation Notation  Variable | Definition   
---|---  
$$\displaystyle{}V_{i_{mg}}$ | Voltage at node i, phase m relative to true ground (V)   
$$\displaystyle{}I_{i_{mg}}$ | Voltage at node i, phase m relative to true ground (A)   
$$\displaystyle{}\hat z_{i-j_{nn}}$ | Element of series impedance matrix relating voltage/current relationship for line connecting nodes i and j, corresponding to self impedance of phase n. Return path (ground) impedance folded in ($\Omega /mile$)   
$$\displaystyle{}\hat z_{i-j_{nm}}$ | Element of series impedance matrix relating voltage/current relationship for line connecting nodes i and j, corresponding to phases n and m. Return path (ground) impedance folded in ($\Omega /mile$)   
$$\displaystyle{}r_i$ | Resistance of conductor i ($\Omega /mile$)   
$$\displaystyle{}\omega$ | System angular frequency ($rad/s$)   
$$\displaystyle{}f$ | System frequency ($Hz$)   
$$\displaystyle{}G=0.1609347\times 10^{-3}$ | Constant for converting from CGS units ($\Omega /mile$)   
$$\displaystyle{}RD_i$ | Radius of conductor i ($ft$)   
$$\displaystyle{}GMR_i$ | Geometric mean radius of conductor i ($ft$)   
$$\displaystyle{}D_{ij}$ | Distance between conductors i and j ($ft$)   
$$\displaystyle{}S_{ij}$ | Distance between conductor i and conductor j's image ($ft$)   
$$\displaystyle{}\theta_{ij}$ | Angle between a pair of lines drawn from conductor i to its own image and to the image of conductor j ($rad$)   
$$\displaystyle{}\rho$ | Resistivity of earth ($\Omega -meters$)   
$$\displaystyle{}\epsilon _{air}$ | Relative permittivity of air = $1.4240\times 10^{-2} (\mu F/mile)$  
  
## Overhead Distribution Lines

Overhead lines will be modeled in a flexible way such that not only neutral conductors can be included but also any number of phases and/or circuits. 

### Series Impedance

Initially neglecting shunt admittances, the voltage/current relationship between two 'nodes' 1 and 2 (corresponding to physical terminal locations in the network) with $m$ phases can be expressed in matrix form: 

    $ \displaystyle{}\begin{bmatrix}V_{1_{ag}}\\\\\downarrow \\\V_{1_{mg}}\end{bmatrix}-\begin{bmatrix}V_{2_{ag}}\\\\\downarrow \\\V_{2_{mg}}\end{bmatrix}=\begin{bmatrix}\hat z_{1-2_{aa}}&\rightarrow &\hat z_{1-2_{am}}\\\\\downarrow &\searrow &\downarrow \\\\\hat z_{1-2_{ma}}&\rightarrow &\hat z_{1-2_{mm}}\end{bmatrix}\begin{bmatrix}I_{1-2_{ag}}\\\\\downarrow \\\I_{1-2_{mg}}\end{bmatrix}$

For example, consider a distribution line with two electrically isolated feeders sharing one neutral phase. One of the feeders has all three phases $(a, b, c)$ present, while the other has only two phases $(a, c)$ present. For clarity, the two phases on the second feeder are renamed $(d, e)$. Then, the voltage drop on the line can be expressed by: 

    $\displaystyle{}\begin{bmatrix}V_{1_{ag}}\\\V_{1_{bg}}\\\V_{1_{cg}}\\\V_{1_{dg}}\\\V_{1_{eg}}\\\V_{1_{ng}}\end{bmatrix}

\- \begin{bmatrix}V_{2_{ag}}\\\V_{2_{bg}}\\\V_{2_{cg}}\\\V_{2_{dg}}\\\V_{2_{eg}}\\\V_{2_{ng}}\end{bmatrix} = \begin{bmatrix} \hat z_{1-2_{aa}}&\hat z_{1-2_{ab}}&\hat z_{1-2_{ac}}&\hat z_{1-2_{ad}}&\hat z_{1-2_{ae}}&\hat z_{1-2_{an}}\\\ \hat z_{1-2_{ba}}&\hat z_{1-2_{bb}}&\hat z_{1-2_{bc}}&\hat z_{1-2_{bd}}&\hat z_{1-2_{be}}&\hat z_{1-2_{bn}}\\\ \hat z_{1-2_{ca}}&\hat z_{1-2_{cb}}&\hat z_{1-2_{cc}}&\hat z_{1-2_{cd}}&\hat z_{1-2_{ce}}&\hat z_{1-2_{cn}}\\\ \hat z_{1-2_{da}}&\hat z_{1-2_{db}}&\hat z_{1-2_{dc}}&\hat z_{1-2_{dd}}&\hat z_{1-2_{de}}&\hat z_{1-2_{dn}}\\\ \hat z_{1-2_{ea}}&\hat z_{1-2_{eb}}&\hat z_{1-2_{ec}}&\hat z_{1-2_{ed}}&\hat z_{1-2_{ee}}&\hat z_{1-2_{en}}\\\ \hat z_{1-2_{na}}&\hat z_{1-2_{nb}}&\hat z_{1-2_{nc}}&\hat z_{1-2_{nd}}&\hat z_{1-2_{ne}}&\hat z_{1-2_{nn}} \end{bmatrix} \begin{bmatrix}I_{1-2_{ag}}\\\I_{1-2_{bg}}\\\I_{1-2_{cg}}\\\I_{1-2_{dg}}\\\I_{1-2_{eg}}\\\I_{1-2_{ng}}\end{bmatrix}$$

The hat notation, taken from [Kersting], indicates that the return path, i.e. the ground impedance, has been folded into the other impedances. According to Carson's equations, the elements of the primitive impedance matrix can be calculated by: 

    $ \displaystyle{}\hat z_{1-2_{ii}}=r_i+4\omega P_{ii}G + j\left(X_i + 2\omega G\ln{\frac{S_{ii}}{RD_i}} + 4\omega Q_{ii}G\right)\Omega /mi$
    $ \displaystyle{}\hat z_{1-2_{ij}}=4\omega P_{ii}G + j\left(2\omega G\ln{\frac{S_{ij}}{D_{ij}}} + 4\omega Q_{ij}G\right)\Omega /mi$

Wherein: 

    $ X_i = 2\omega{}G\ln{\frac{RD_i}{GMR_{i}}}\Omega /mi$
    $ P_{ij} = \frac{\pi}{8}-\frac{1}{3\sqrt{2}}k_{ij}\cos{\theta_{ij}}+\frac{k_{ij}^2}{16}\cos{2\theta_{ij}}\left(0.6728+\ln{2}{k_{ij}}\right)+\frac{k_{ij}^2}{16}\theta_{ij}\sin{2\theta_{ij}}$
    $ Q_{ij} = -0.0386 + \frac{1}{2}\ln{\frac{2}{k_{ij}}}+\frac{1}{3\sqrt{2}}k_{ij}\cos{\theta_{ij}}$
    $ k_{ij} = 8.565\times 10^{-4}S_{ij}\sqrt{\frac{f}{\rho}}$

The primitive series impedance matrix can be inverted to yield the primitive series admittance matrix for the line. For a more complete model of the line, the primitive shunt admittance matrix can then be added. 

### Shunt Admittance

The method in [Kersting] uses Carson's equations to construct shunt admittance matrices for overhead lines. Carson's equations make use of conductor 'images' which are modeled as being parallel underground conductors parallel to each conductor, equidistant from the ground surface. The primitive potential coefficient matrix $P$ is assembled such that for each element: 

    $ \displaystyle{}P_{ii}= \frac{1}{2\pi\epsilon_{air}}\ln{\frac{S_{ii}}{RD_{i}}}mile/\mu F$
    $ \displaystyle{}P_{ij}= \frac{1}{2\pi\epsilon_{air}}\ln{\frac{S_{ij}}{D_{ij}}}mile/\mu F$

Having constructed matrix $P$, the primitive shunt admittance matrix can then be constructed simply by: 

    $ \displaystyle{}Y_{shunt}=j\omega 10^{-6}P^{-1} Siemens/mi$


