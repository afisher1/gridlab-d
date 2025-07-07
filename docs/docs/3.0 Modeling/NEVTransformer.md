# NEVTransformer - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/NEVTransformer
The approach described by Roger Dugan and used in Open DSS has been adopted to model transformers and regulators. 

## Contents

  * 1 Assumptions
  * 2 Variables
  * 3 Series Impedance
    * 3.1 Generic Approach
    * 3.2 Shunt Admittance
  * 4 Examples
    * 4.1 Three-Phase Two Winding Grounded Wye to Delta Transformer
    * 4.2 Three-Phase Two Winding Grounded Wye to Grounded Wye Transformer
    * 4.3 Single-Phase Line-to-Ground to Split Phase (Triplex) Transformer
    * 4.4 Single-Phase Step Voltage Regulator
  * 5 References
## Assumptions

Assumes ground is infinite plane, zero potential plane 

  


## Variables

Table 1 - Equation Notation  Variable | Definition   
---|---  
$$\displaystyle{}z_{ij_{(p)}}$ | Short circuit impedance between windings i and j, phase p (%)   
$$\displaystyle{}z_{base}$ | Impedance base to convert to a 1V base, equal to $\frac{1}{S_{base}}$  
$$\displaystyle{}z_{ij_{(p)}}^{1V}$ | Short circuit impedance between windings i and j, phase p, on a 1V base ($\Omega$)   
$$\displaystyle{}S_{base_{i}}$ | VA base for winding i (VA)   
$$\displaystyle{}V_{rated,i}$ | Voltage rating for winding i (V)   
$$\displaystyle{}\mathbf{Z_B}$ | Short circuit impedance matrix   
$$\displaystyle{}\mathbf{B}$ | Incidence matrix relating short circuit currents   
$$\displaystyle{}\mathbf{N}$ | Incidence matrix relating short circuit currents to terminal currents and expressing voltage ratios   
$$\displaystyle{}\mathbf{A}$ | Incidence matrix relating terminal connections   
$$\displaystyle{}a$ | Tap setting coefficient for regulator/tap-changing transformer   
$$\displaystyle{}n_{taps}$ | Tap setting for regulator/tap-changing transformer   
$$\displaystyle{}w_{taps}$ | Percent change in voltage per tap change for regulator/tap-changing transformer   
  
## Series Impedance

### Generic Approach

In [1], a method is presented which claims to be generic/applicable for modeling of all (n-winding) transformer configurations, including split-phase (4-wire) types. The result of this method is a primitive nodal admittance matrix which can be used/inserted in the system bus admittance matrix. The process is outlined as follows. 

First, a short-circuit impedance matrix, $\mathbf{Z_B}$, is constructed for the transformer. $\mathbf{Z_B}$ is of order $p(n-1)$ where $p$ is the number of phases and n is the number of windings. The values in $\mathbf{Z_B}$ are all relative to one of the windings, which is why for a common two-winding transformer, there is only one submatrix. The elements for the diagonal submatrices are calculated by: 

    $ \displaystyle{}z_{B_{ii,kk}}=z_{SC_{i,i+1}} \cdot z_{base}$ for $i=1$ to $m-1$

where '$ii$' indicates the diagonal submatrix corresponding to winding $i$, '$kk$' indicates the element corresponding to phase $k$, $z_{base}$ is a multiplier to convert the impedance to a 1V base, and $m$ is the number of phases. The off diagonal submatrices (present when there are more than two windings) are calculated by: 

    $ \displaystyle{}z_{B_{ij}}=\frac{1}{2}\left [z_{B_{ii}}+z_{B_{jj}}-z_{i+1,j+1}\right ]$

The primitive nodal admittance matrix is then constructed by: 

    $ \displaystyle{}\mathbf{Y_{prim}}=\mathbf{ANBZ_B}^{-1}\mathbf{B}^T\mathbf{N}^T\mathbf{A}^T$

In the above equation, $\mathbf{B}$ is an incidence matrix relating currents in the short circuit reference frame, $\mathbf{N}$ is an incidence matrix relating the short circuit currents to the winding currents and whose non-zero elements are the corresponding voltage rating, and lastly $\mathbf{A}$ is an incidence matrix relating winding currents to actual terminal currents. 

Note that for voltage regulators with load tap changers, the only difference is in the elements of the $\mathbf{N}$ matrix. For source side windings, the elements of the N matrix are simply the reciprocal of the rated voltage: 

    $ \displaystyle{}n_{ii}=\frac{1}{V_{rated,i}}$

For load side windings, the elements of the N matrix can be calculated as: 

    $ \displaystyle{}n_{jj}=\frac{1}{aV_{rated,i}}$

The coefficient $a$ represents the tap setting: 

    $ \displaystyle{}a=1+n_{taps}w_{taps}$

where $n_{taps}$ is the tap number and $w_{taps}$ is the percent voltage change of one tap (usually equal to 0.625%). 

### Shunt Admittance

If they are to be included, the shunt admittances can be added directly to the primitive nodal admittance matrix. 

## Examples

The first example will be covered in detail, where others will merely give the relevant matrices. 

### Three-Phase Two Winding Grounded Wye to Delta Transformer

This example will walk through the process of constructing a primitive admittance matrix for a three-phase two winding grounded wye to delta transformer with the following specs: Winding Line-to-Line Voltages: 12.47 kV (primary); 4.16 kV (secondary) Three-phase power rating: 6000 kVA Per-Phase Percent Short Circuit Impedance: $1 + j6$ The first step is to calculate/assemble the short circuit impedance matrix, $\mathbf{Z_B}$. Since it is a two-winding transformer, there will be $(2-1) = 1$ submatrices. This means there will be no 'off-diagonal' submatrices. Since there are three phases, each submatrix will be of order 3. The neutral terminal is not yet included here since it does not have a transformer winding. Given short circuit impedances for each phase/winding pair, $z_{HL_a},z_{HL_b},z_{HL_c}$, the short circuit impedance matrix can be stated: 

    $ \displaystyle{}\mathbf{Z_B}=\begin{bmatrix}z_{HL_a}^{1V}&0&0\\\0&z_{HL_b}^{1V}&0\\\0&0&z_{HL_c}^{1V}\end{bmatrix}=\begin{bmatrix}\frac{z_{HL_a}}{z_{base}}&0&0\\\0&\frac{z_{HL_b}}{z_{base}}&0\\\0&0&\frac{z_{HL_c}}{z_{base}}\end{bmatrix}=\begin{bmatrix}\frac{0.01+j0.06}{2.0\times 10^6}&0&0\\\0&\frac{0.01+j0.06}{2.0\times 10^6}&0\\\0&0&\frac{0.01+j0.06}{2.0\times 10^6}\end{bmatrix}$

The next step is constructing the $B$ matrix, which relates the short circuit currents to the terminal currents which are defined as entering the transformer: 

    $ \displaystyle{}\begin{bmatrix}I_{H_a}^{1V}\\\I_{L_a}^{1V}\\\I_{H_b}^{1V}\\\I_{L_b}^{1V}\\\I_{H_c}^{1V}\\\I_{L_c}^{1V}\end{bmatrix}=\mathbf{B}\begin{bmatrix}I_{SC_a}\\\I_{SC_b}\\\I_{SC_c}\end{bmatrix}$
    $ \displaystyle{}\mathbf{B}=\begin{bmatrix}1&0&0\\\\-1&0&0\\\0&1&0\\\0&-1&0\\\0&0&1\\\0&0&-1\end{bmatrix}$

Next, the $N$ matrix is constructed which will apply the turns ratio multipliers to the ground-referenced 1V base quantities. Each phase-submatrix of $N$ will be a 4x2 matrix, since the (2) high and low ground-referenced currents must be translated into (4) currents for both ends of both the high and low windings: 

    $\displaystyle{}\begin{bmatrix}I_{H_a1}\\\I_{H_a2}\\\I_{L_a1}\\\I_{L_a2}\\\I_{H_b1}\\\I_{H_b2}\\\I_{L_b1}\\\I_{L_b2}\\\I_{H_c1}\\\I_{H_c2}\\\I_{L_c1}\\\I_{L_c2}\end{bmatrix}=\mathbf{N}\begin{bmatrix}I_{H_a}^{1V}\\\I_{L_a}^{1V}\\\I_{H_b}^{1V}\\\I_{L_b}^{1V}\\\I_{H_c}^{1V}\\\I_{L_c}^{1V}\end{bmatrix}=

\begin{bmatrix} 
    
    
    \frac{1}{7200}&0&\vdots &0&0&\vdots &0&0\\
    

-\frac{1}{7200}&0&\vdots &0&0&\vdots &0&0\\\ 0& \frac{1}{4160}&\vdots &0&0&\vdots &0&0\\\ 0&-\frac{1}{4160}&\vdots &0&0&\vdots &0&0\\\ \cdots &\cdots && \cdots &\cdots && \cdots &\cdots \\\ 0&0&\vdots & \frac{1}{7200}&0&\vdots &0&0\\\ 0&0&\vdots &-\frac{1}{7200}&0&\vdots &0&0\\\ 0&0&\vdots &0& \frac{1}{4160}&\vdots &0&0\\\ 0&0&\vdots &0&-\frac{1}{4160}&\vdots &0&0\\\ \cdots &\cdots && \cdots &\cdots && \cdots &\cdots \\\ 0&0&\vdots &0&0&\vdots & \frac{1}{7200}&0\\\ 0&0&\vdots &0&0&\vdots &-\frac{1}{7200}&0\\\ 0&0&\vdots &0&0&\vdots &0& \frac{1}{4160}\\\ 0&0&\vdots &0&0&\vdots &0&-\frac{1}{4160} \end{bmatrix}\begin{bmatrix}I_{H_a}^{1V}\\\I_{L_a}^{1V}\\\I_{H_b}^{1V}\\\I_{L_b}^{1V}\\\I_{H_c}^{1V}\\\I_{L_c}^{1V}\end{bmatrix}$ Note that line-to-line voltage ratings have been converted to line-to-ground values. Finally, the $\mathbf{A}$ matrix is constructed to represent the transformer terminal connections (e.g. wye/delta configuration). At this point the neutral conductor/terminal can be included. Here, again the currents are defined as entering the transformer: 

    $\displaystyle{}\begin{bmatrix}I_{H_a}\\\I_{H_b}\\\I_{H_c}\\\I_{H_n}\\\I_{L_a}\\\I_{L_b}\\\I_{L_c}\end{bmatrix}=

\mathbf{A} \begin{bmatrix}I_{H_a1}\\\I_{H_a2}\\\I_{L_a1}\\\I_{L_a2}\\\I_{H_b1}\\\I_{H_b2}\\\I_{L_b1}\\\I_{L_b2}\\\I_{H_c1}\\\I_{H_c2}\\\I_{L_c1}\\\I_{L_c2}\end{bmatrix} = \left [ \begin{matrix} 1&0&0&0&0&0& \\\ 0&0&0&0&1&0& \\\ 0&0&0&0&0&0& \\\ 0&1&0&0&0&1& \\\ 0&0&1&0&0&0& \\\ 0&0&0&1&0&0& \\\ 0&0&0&0&0&0& \end{matrix} \begin{matrix} 0&0&0&0&0&0 \\\ 0&0&0&0&0&0 \\\ 0&0&1&0&0&0 \\\ 0&0&0&1&0&0 \\\ 0&0&0&0&0&1 \\\ 1&0&0&0&0&0 \\\ 0&1&0&0&1&0\end{matrix} \right ] \begin{bmatrix}I_{H_a1}\\\I_{H_a2}\\\I_{L_a1}\\\I_{L_a2}\\\I_{H_b1}\\\I_{H_b2}\\\I_{L_b1}\\\I_{L_b2}\\\I_{H_c1}\\\I_{H_c2}\\\I_{L_c1}\\\I_{L_c2}\end{bmatrix} $ Multiplying the assembled matrices yields the primitive nodal admittance matrix for the transformer: 

    $ \displaystyle{}\mathbf{Y_{prim}}=\mathbf{ANBZ_B}^{-1}\mathbf{B}^T\mathbf{N}^T\mathbf{A}^T=$

$$\displaystyle{} \begin{bmatrix} 
    
    
    0.1043-j0.6256& 0             & 0             &-0.1043+j0.6256&-0.1805+j1.0828& 0.1805-j1.0828& 0\\
    0             & 0.1043-j0.6256& 0             &-0.1043+j0.6256& 0             &-0.1805+j1.0828& 0.1805-j1.0828\\
    0             & 0             & 0.1043-j0.6256&-0.1043+j0.6256& 0.1805-j1.0828& 0             &-0.1805+j1.0828\\
    

-0.1043+j0.6256&-0.1043+j0.6256&-0.1043+j0.6256& 0.3128-j1.8769& 0 & 0 & 0\\\ -0.1805+j1.0828& 0 & 0.1805-j1.0828& 0 & 0.6247-j3.7482&-0.3124+j1.8741&-0.3124+j1.8741\\\ 
    
    
    0.1805-j1.0828&-0.1805+j1.0828& 0             & 0             &-0.3124+j1.8741& 0.6247-j3.7482&-0.3124+j1.8741\\
    0             & 0.1805-j1.0828&-0.1805+j1.0828& 0             &-0.3124+j1.8741&-0.3124+j1.8741& 0.6247-j3.7482
    \end{bmatrix}
$$

### Three-Phase Two Winding Grounded Wye to Grounded Wye Transformer

This example is for the same transformer as above, but with both sides connected in grounded wye. Therefore, only the A matrix is altered: 

    $\displaystyle{}\begin{bmatrix}I_{H_a}\\\I_{H_b}\\\I_{H_c}\\\I_{H_n}\\\I_{L_a}\\\I_{L_b}\\\I_{L_c}\\\I_{L_n}\end{bmatrix}=

\mathbf{A} \begin{bmatrix}I_{H_a1}\\\I_{H_a2}\\\I_{L_a1}\\\I_{L_a2}\\\I_{H_b1}\\\I_{H_b2}\\\I_{L_b1}\\\I_{L_b2}\\\I_{H_c1}\\\I_{H_c2}\\\I_{L_c1}\\\I_{L_c2}\end{bmatrix} = \left [ \begin{matrix} 1&0&0&0&0&0& \\\ 0&0&0&0&1&0& \\\ 0&0&0&0&0&0& \\\ 0&1&0&0&0&1& \\\ 0&0&1&0&0&0& \\\ 0&0&0&0&0&0& \\\ 0&0&0&0&0&0& \\\ 0&0&0&1&0&0& \end{matrix} \begin{matrix} 0&0&0&0&0&0 \\\ 0&0&0&0&0&0 \\\ 0&0&1&0&0&0 \\\ 0&0&0&1&0&0 \\\ 0&0&0&0&0&0 \\\ 1&0&0&0&0&0 \\\ 0&0&0&0&1&0 \\\ 0&1&0&0&0&1 \end{matrix} \right ] \begin{bmatrix}I_{H_a1}\\\I_{H_a2}\\\I_{L_a1}\\\I_{L_a2}\\\I_{H_b1}\\\I_{H_b2}\\\I_{L_b1}\\\I_{L_b2}\\\I_{H_c1}\\\I_{H_c2}\\\I_{L_c1}\\\I_{L_c2}\end{bmatrix} $$

    $ \displaystyle{}\mathbf{Y_{prim}}=\mathbf{ANBZ_B}^{-1}\mathbf{B}^T\mathbf{N}^T\mathbf{A}^T=$

$$\displaystyle{} \begin{bmatrix} 
    
    
    0.1043-j0.6256& 0             & 0             &-0.1043+j0.6256&-0.1805+j1.0828& 0             & 0             & 0.1805-j1.0828\\
    0             & 0.1043-j0.6256& 0             &-0.1043+j0.6256& 0             &-0.1805+j1.0828& 0             & 0.1805-j1.0828\\
    0             & 0             & 0.1043-j0.6256&-0.1043+j0.6256& 0             & 0             &-0.1805+j1.0828& 0.1805-j1.0828\\
    

-0.1043+j0.6256&-0.1043+j0.6256&-0.1043+j0.6256& 0.3128-j1.8769& 0.1805-j1.0828& 0.1805-j1.0828& 0.1805-j1.0828&-0.5414+j3.2484\\\ -0.1805+j1.0828& 0 & 0 & 0.1805-j1.0828& 0.3124-j1.8741& 0 & 0 &-0.3124+j1.8741\\\ 
    
    
    0             &-0.1805+j1.0828& 0             & 0.1805-j1.0828& 0             & 0.3124-j1.8741& 0             &-0.3124+j1.8741\\
    0             & 0             & 0.1805-j1.0828& 0.1805-j1.0828& 0             & 0             & 0.3124-j1.8741&-0.3124+j1.8741\\
    0.1805-j1.0828& 0.1805-j1.0828& 0.1805-j1.0828&-0.5414+j3.2484&-0.3124+j1.8741&-0.3124+j1.8741&-0.3124+j1.8741& 0.9371-j5.6223
    \end{bmatrix}
$$

### Single-Phase Line-to-Ground to Split Phase (Triplex) Transformer

The important modeling consideration with this transformer is that there are effectively three windings. The high side winding will be denoted winding H, and the two split phases 'L' and 'T'. The transformer data is given here: Winding Line-to-Ground Voltages: 7.2 kV (primary); 0.12 kV (each secondary) Power rating: 25 kVA Percent Short Circuit Impedance: $0 + 2.04j$ (winding H to winding L); $0 + 2.04j$ (winding H to winding T); $0 + 1.36$ (winding L to winding T) 

First, constructing the short circuit impedance matrix, the matrix will be of order $(3-1)=2$ since there are three windings. Using the impedance data: 

    $\displaystyle{}\displaystyle{}\mathbf{Z_B}=

\begin{bmatrix}\frac{z_{HL}^{1V}}{z_{base}}&\frac{1}{2}\left (\frac{z_{HL}^{1V}}{z_{base}}+\frac{z_{HT}^{1V}}{z_{base}}-\frac{z_{LT}^{1V}}{z_{base}}\right )\\\\\frac{1}{2}\left (\frac{z_{HL}^{1V}}{z_{base}}+\frac{z_{HT}^{1V}}{z_{base}}-\frac{z_{LT}^{1V}}{z_{base}}\right )&\frac{z_{HT}^{1V}}{z_{base}}\end{bmatrix} = \begin{bmatrix}\frac{j0.0204}{25\times 10^3}&\frac{1}{2}\left (\frac{j0.0204}{25\times 10^3}+\frac{j0.0204}{25\times 10^3}-\frac{j0.0136}{25\times 10^3}\right )\\\\\frac{1}{2}\left (\frac{j0.0204}{25\times 10^3}+\frac{j0.0204}{25\times 10^3}-\frac{j0.0136}{25\times 10^3}\right )&\frac{j0.0204}{25\times 10^3}\end{bmatrix} = \begin{bmatrix}j0.816\times 10^{-6}&j0.544\times 10^{-6}\\\j0.544\times 10^{-6}&j0.816\times 10^{-6}\end{bmatrix}$$

Then, constructing the $\mathbf{B}$ matrix to relate short circuit currents is assembled such that the short circuit current matrix (where winding 'H' is short circuited) equals the product $\mathbf{B}I_{sc}$: 

    $ \displaystyle{}I_H=\mathbf{B}I_{sc}$
    $ \displaystyle{}\begin{bmatrix}I_H\\\I_L\\\I_T\end{bmatrix}=\mathbf{B}\begin{bmatrix}I_{L}\\\I_{T}\end{bmatrix}_{SC}$
    $ \displaystyle{}\begin{bmatrix}I_H\\\I_L\\\I_T\end{bmatrix}=\begin{bmatrix}1&1\\\\-1&0\\\0&-1\end{bmatrix}\begin{bmatrix}I_{L}\\\I_{T}\end{bmatrix}_{SC}$

The $\mathbf{N}$ matrix relates both sides of each winding, so here the turns ratios are reflected. N will be $2m\times m-1=6\times 3$: 

    $ \displaystyle\begin{bmatrix}I_{H1}\\\I_{H2}\\\I_{L1}\\\I_{L2}\\\I_{T1}\\\I_{T2}\end{bmatrix}=\mathbf{N}\begin{bmatrix}I_H\\\I_L\\\I_T\end{bmatrix}$
    $\displaystyle\begin{bmatrix}I_{H1}\\\I_{H2}\\\I_{L1}\\\I_{L2}\\\I_{T1}\\\I_{T2}\end{bmatrix}=

\begin{bmatrix}\frac{1}{7200}&0&0\\\\-\frac{1}{7200}&0&0\\\0&\frac{1}{120}&0\\\0&-\frac{1}{120}&0\\\0&0&\frac{1}{120}\\\0&0&-\frac{1}{120}\end{bmatrix} \begin{bmatrix}I_H\\\I_L\\\I_T\end{bmatrix}$$

Finally, the $\mathbf{A}$ matrix relates the connections of the transformer. This is where the reverse polarity of the tertiary winding is reflected. Here the primary neutral terminal, $N_P$, and secondary neutral terminal, $N_S$, (common to both the 'low' and 'tertiary' windings) are reflected. 

    $ \displaystyle{}\begin{bmatrix}I_H\\\I_{N_P}\\\I_L\\\I_T\\\I_{N_S}\end{bmatrix}=\mathbf{A}\begin{bmatrix}I_{H1}\\\I_{H2}\\\I_{L1}\\\I_{L2}\\\I_{T1}\\\I_{T2}\end{bmatrix}$
    $\displaystyle{}\begin{bmatrix}I_H\\\I_{N_P}\\\I_L\\\I_T\\\I_{N_S}\end{bmatrix}=

\begin{bmatrix}1&0&0&0&0&0\\\0&1&0&0&0&0\\\0&0&1&0&0&0\\\0&0&0&0&0&1\\\0&0&0&1&1&0\end{bmatrix} \begin{bmatrix}I_{H1}\\\I_{H2}\\\I_{L1}\\\I_{L2}\\\I_{T1}\\\I_{T2}\end{bmatrix}$$

The primitive admittance matrix can then be calculated using the assembled matrices: 

    $ \displaystyle{}\begin{bmatrix}0-j0.0284&0+j0.0284&0+j0.8510&0-j0.8510&0\\\0+j0.0284&0-j0.0284 &0-j0.8510&0+j0.8510&0\\\0+j0.8510&0-j0.8510&0-j153.19&0-j102.12&0+j255.31\\\0-j0.8510&0+j0.8510&0-j102.12&0-j153.19&0+j255.31\\\0&0&0+j255.31&0+j255.31&0-j510.62\end{bmatrix}$

### Single-Phase Step Voltage Regulator

Rated (nominal) line-to-neutral voltage: 2400V Rated power: 1666.66 kVA Tap width: 0.625% Tap setting: 10 Short circuit percent impedance: 0.01 + j0.01 Note that even if the regulator impedance/losses are to be neglected, a small value must be used. As described above, the tap changes are included in the N matrix. The set of matrices can be constructed: 

    $ \displaystyle\mathbf{Z_B}=\begin{bmatrix}\frac{.0001+j.0001}{1666.66kVA}\end{bmatrix}=\begin{bmatrix}6.002\times 10^{-11}+j6.002\times 10^{-11}\end{bmatrix}$
    $ \displaystyle\mathbf{B}=\begin{bmatrix}1\\\\-1\end{bmatrix}$
    $ \displaystyle\mathbf{N}=\begin{bmatrix}\frac{1}{2400}&0\\\\-\frac{1}{2400}&0\\\0&\frac{1}{(2400)(1+0.00625*10)}\\\0&-\frac{1}{(2400)(1+0.00625*10)}\end{bmatrix}=\begin{bmatrix}4.167\times 10^{-2}&0\\\\-4.167\times 10^{-2}&0\\\0&3.922\times 10^{-2}\\\0&-3.922\times 10^{-2}\end{bmatrix}$
    $ \displaystyle\mathbf{A}=\begin{bmatrix}1&0&0&0\\\0&1&0&0\\\0&0&1&0\\\0&0&0&1\end{bmatrix}$

The primitive admittance matrix can then be calculated using the assembled matrices: 

    $ \displaystyle{}\begin{bmatrix}1446.2-j1446.2&-1446.2+j1446.2&-1361.1+j1361.1&1361.1-j1361.1\\\\-1446.2+j1446.2&1446.2-j1446.2 &1361.1-j1361.1&-1361.1+j1361.1\\\\-1361.1+j1361.1&1361.1-j1361.1&1281.0-j1281.0&-1281.0+j1281.0\\\1361.1-j1361.1&-1361.1+j1361.1&-1281.0+j1281.0&1281.0-j1281.0\end{bmatrix}$

## References

  1. Dugan, Roger C., "An Example of 3-phase Transformer Modeling for Distribution System Analysis," IEEE T&D Conference, Vol. 3, 2003.
