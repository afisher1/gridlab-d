# NEVUnderground - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/NEVUnderground
## Contents

  * 1 Underground Distribution Lines
    * 1.1 Variables
    * 1.2 Series Impedance
      * 1.2.1 Concentric Neutral
      * 1.2.2 Tape-Shielded
    * 1.3 Shunt Admittance
      * 1.3.1 Concentric Neutral
      * 1.3.2 Tape-Shield
# Underground Distribution Lines

Underground lines will be modeled in a flexible way such that not only neutral conductors can be included but also any number of phases and/or circuits. 

## Variables

Table 1 - Equation Notation  Variable | Definition   
---|---  
$$\displaystyle{}V_{i_{mg}}$ | Voltage at node i, phase m relative to true ground (V)   
$$\displaystyle{}I_{i_{mg}}$ | Voltage at node i, phase m relative to true ground (A)   
$$\displaystyle{}\hat z_{i-j_{nn}}$ | Element of series impedance matrix relating voltage/current relationship for line connecting nodes i and j, corresponding to self impedance of phase n. Return path (ground) impedance folded in ($\Omega /mile$)   
$$\displaystyle{}\hat z_{i-j_{nm}}$ | Element of series impedance matrix relating voltage/current relationship for line connecting nodes i and j, corresponding to phases n and m. Return path (ground) impedance folded in ($\Omega /mile$)   
$$\displaystyle{}r_{i,c}$ | Resistance of the phase conductor for cable i ($\Omega /mi$)   
$$\displaystyle{}r_{i,cn}$ | The effective resistance of the concentric neutral ring for cable i ($\Omega /mi$)   
$$\displaystyle{}r_{i,sh}$ | The effective resistance of the tape-shield for cable i ($\Omega /mi$)   
$$\displaystyle{}\omega$ | System angular frequency ($rad/s$)   
$$\displaystyle{}f$ | System frequency ($Hz$)   
$$\displaystyle{}G=0.1609347\times 10^{-3}$ | Constant for converting from CGS units ($\Omega /mi$)   
$$\displaystyle{}d_{i,c}$ | Diameter of the phase conductor for cable i ($in$)   
$$\displaystyle{}d_{i,s}$ | Diameter of a neutral strand for cable i ($in$)   
$$\displaystyle{}d_{i,sh}$ | Diameter of the tape-shield for cable i ($in$)   
$$\displaystyle{}d_{i,od}$ | Outer diameter of cable i ($in$)   
$$\displaystyle{}R_{i}$ | Radius of the circle passing through the concentric neutral strands for cable i ($in$)   
$$\displaystyle{}T_{i}$ | Thickness of the tape-shield for cable i ($mil$)   
$$\displaystyle{}k_{i}$ | The number of neutral strands for cable i ($unitless$)   
$$\displaystyle{}GMR_{i,c}$ | Geometric mean radius of the phase conductor for cable i ($ft$)   
$$\displaystyle{}GMR_{i,s}$ | Geometric mean radius of a neutral strand for cable i ($ft$)   
$$\displaystyle{}GMR_{i,sh}$ | Geometric mean radius of the tape-shield for cable i ($ft$)   
$$\displaystyle{}GMR_{i,cn}$ | The effective Geometric mean radius of the concentric neutral ring for cable i ($ft$)   
$$\displaystyle{}D_{ij}$ | Distance between conductors i and j ($ft$)   
$$\displaystyle{}S_{ij}$ | Distance between conductor i and conductor j's image ($ft$)   
$$\displaystyle{}\theta_{ij}$ | Angle between a pair of lines drawn from conductor i to its own image and to the image of conductor j ($rad$)   
$$\displaystyle{}\rho$ | Resistivity of earth ($\Omega -meters$)   
$$\displaystyle{}\rho_{i,sh}$ | Resistivity of the tape-shield for cable i at a temperature of 50\Celsius ($\Omega -meters$)   
$$\displaystyle{}\epsilon_{0}$ | The permittivity of free space = $1.4240\times 10^{-2} (\mu F/mi)$  
$$\displaystyle{}\epsilon_{i,r}$ | The relative permittivity of the insulation medium for cable i ($unitless$)   
$$\displaystyle{}C_{1}=\frac{1}{63360}$ | conversion factor for converting inches to miles for cable i ($mi/in$)   
$$\displaystyle{}C_{2}=\frac{1}{63360000}$ | conversion factor for converting mils to miles for cable i ($mi/mil$)   
$$\displaystyle{}C_{3}=\frac{1}{1609.344}$ | conversion factor for converting meters to miles for cable i ($mi/m$)   
  
## Series Impedance

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

The primitive series admittance matrix can be inverted to yield the primitive series admittance matrix for the line. For a more complete model of the line, the primitive shunt admittance matrix can then be added. 

The effective resistance and GMR of the tape-shield and concentric neutral ring need to be calculated in order to use the previously defined equations. 

### Concentric Neutral

The effective geomentric mean of the concentric neutral ring can be found using the following equation. 

    $ \displaystyle{}GMR_{i,cn}=(GMR_{i,s}k_{i}R_{i}^{k_{i}-1})^{\frac{1}{k_{i}}} ft$

$$R$ is the radius of the circle passing through the center of the concentric neutral strands in ft and can be found using the equation below. 

    $ \displaystyle{}R_{i}=\frac{d_{i,od}-d_{i,s}}{24} ft$

The effective resistance of the concentric neutral ring is calculated using the following equation. 

    $ \displaystyle{}r_{i,cn}=\frac{r_{i,s}}{k_{i}} \Omega/mi$

Because the distance between cables is much greater than $R_{i}$ it is a good approximation to treat the concentric neutral strands as a single conductor located a distance $R$ above the center of the cable when determining distances between adjacent conductor cables. 

### Tape-Shielded

The GMR of the tape shield is given the equation below. 

    $ \displaystyle{}GMR_{i,sh}=\frac{\frac{d_{i,sh}}{2}-\frac{T_{i}}{2000}}{12} ft$

The resistance of the tape sheild is the given in the equation below. 

    $ \displaystyle{}r_{i,sh}=\frac{C_{3}\rho_{i,sh}}{(C_{1}C_{2}d_{i,sh}T{i}+(C_{2}T_{i})^2)} \Omega/mi$

The distance between the tape shield and it's own phase conductor is $GMR_{sh}$. 

## Shunt Admittance

The equations presented below assume that the electric field created by the charge on the phase conductor is is confined to the boundary of the insulation. Because of this assumption there is no cross coupling of the admittances between conductors. 

### Concentric Neutral

The shunt admittance between a conductor and the concentric neutral ring for a single cable is defined by the equation below. 

    $ \displaystyle{}y_{in}=0+j\frac{2\pi\omega\epsilon_{0}\epsilon_{i,r}}{10^{6}(ln(\frac{2R_{i}}{d_{i,c}})-\frac{1}{k_{i}}ln(k_{i}\frac{d_{i,s}}{2R_{i}}))} S/mi$

### Tape-Shield

The shunt admittance between a conductor and the tape-shield for a single cable is defined by the equation below. 

    $ \displaystyle{}y_{in}=0+j\frac{2\pi\omega\epsilon_{0}\epsilon_{i,r}}{10^{6}ln(\frac{2R_{i}}{d_{i,c}})} S/mi$


