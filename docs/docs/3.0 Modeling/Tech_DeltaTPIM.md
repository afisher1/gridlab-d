# Tech:DeltaTPIM

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Tech:DeltaTPIM
## Contents

  * 1 Three Phase Induction Motors
    * 1.1 Synopsis
    * 1.2 Classes
    * 1.3 Variables
    * 1.4 Dynamic Phasor Equations
    * 1.5 Simulation study
      * 1.5.1 Case 1
      * 1.5.2 Case 2
  * 2 References
# Three Phase Induction Motors

Implement three phase induction motor dynamic phasor model into GridLAB-D. 

## Synopsis

This work will incorporate the ability to model three phase induction motor and other more advanced power flow capabilities into GridLAB-D. 

## Classes

## Variables

Variable | Definition   
---|---  
$$ V_{p} $ | Positive sequence voltage   
$$ V_{n}^{*} $ | Complex conjugate of negative sequence voltage   
$$ r_{s} $ | Stator resistance   
$$ L_{s}$ | Stator inductance   
$$ w_{s}$ | Angular speed   
$$ L_{m}$ | Magnetizing inductance   
$$ L_{r}$ | Rotor inductance   
$$ \Omega _{r,0}$ | Mechanical speed dc component   
$$ \Omega _{r,2} $ | Mechanical speed second harmonic component   
$$ I_{P}^{s} $ | Positive sequence of stator current   
$$ I_{n,s}^{*}$ | Complex conjugate of negative sequence stator current   
$$ I_{n,r}^{*}$ | Complex conjugate of negative sequence rotor current   
$$ J $ | Inertia   
$$ B $ | Damping constant   
$$ \psi _{f}^{R} + j. \psi _{f}^{I}$ | Forward rotating flux   
$$ \psi _{f}^{R} + j. \psi _{f}^{I}$ | Backward rotating flux   
$$ \phi $ | Voltage phasor angle   
  
## Dynamic Phasor Equations

$$ V_{p} = [r_{s}+j.w_{s}.L_{s} +L_{s} \frac{\mathrm{d} }{\mathrm{d} t} ]I_{P}^{s}+ [j.w_{s}.L_{m} +L_{m} \frac{\mathrm{d} }{\mathrm{d} t}]I_{P}^{r} $$

$$0 = [r_{r}+j.w_{s}.L_{r} +L_{r} \frac{\mathrm{d} }{\mathrm{d} t} ]I_{P}^{r}+ [j.w_{s}.L_{m} +L_{m} \frac{\mathrm{d} }{\mathrm{d} t}]I_{P}^{s}- j.\Omega_{r,0} \frac{\mathrm{P} }{\mathrm{2}} [L_{m} I_{P}^{s}+ L_{r}.I_{P}^{r}] - j.\Omega_{r,2} \frac{\mathrm{P} }{\mathrm{2}} [L_{m} I_{n,s}^{*}+ L_{r}.I_{n,r}^{*}]$$

$$V_{n}^{*} = [r_{s} - j.w_{s}.L_{s} + L_{s} \frac{\mathrm{d} }{\mathrm{d} t} ]I_{n,s}^{*}+ [-j.w_{s}.L_{m} +L_{m} \frac{\mathrm{d} }{\mathrm{d} t}]I_{n,r}^{*}$$

$$ 0 = [- j.w_{s}.L_{m} +L_{m} \frac{\mathrm{d} }{\mathrm{d} t}]I_{n,s}^{*} \+ [r_{r} - j.w_{s}.L_{r} + L_{r} \frac{\mathrm{d} }{\mathrm{d} t} ]I_{n,r}^{*} - j.\Omega_{r,0} \frac{\mathrm{P} }{\mathrm{2}} [L_{m} I_{n,s}^{*} + L_{r}.I_{n,r}^{*}] - j.\Omega_{r,2}^{*} \frac{\mathrm{P} }{\mathrm{2}} [L_{m} I_{P}^{s}+ L_{r}.I_{P}^{r}] 
    
    
    $$
    

$$ J.\frac{\mathrm{d} }{\mathrm{d} t}.\Omega_{r,0} = \frac{\mathrm{P} }{\mathrm{2}}.L_{m}(I_{P}^{s}.I_{P}^{r} + I_{n,s}^{*}.I_{n,r}^{*})- B.\Omega_{r,0} - T_{L} $$

$$ J.\frac{\mathrm{d} }{\mathrm{d} t}.\Omega_{r,2} = \frac{\mathrm{P} }{\mathrm{4}}.L_{m}(I_{P}^{s}.I_{n}^{r} + I_{n,s}^{*}.I_{p,r})-( B + j. 2.J.w_{s}).\Omega_{r,2} $$

  


## Simulation study

### Case 1

Case 1 simulates a step change in mechanical torque at constant grid voltage. A torque step is applied at time t=10 sec. Fig 1. demonstrates constant quadrature axis voltage. Fig.2 indicates step change in torque. Following are the simulation results. Fig. 3 shows the impact of a step change in torque on direct axis current. Fig. 4 shows the impact of a step change in torque on quadrature axis current. Fig. 5 and Fig.6 show dc and second harmonic component of speed respectively. 

  


[![](//images.shoutwiki.com/gridlab-d/thumb/c/ca/Q_axisvotage_Tstep.png/300px-Q_axisvotage_Tstep.png)](/wiki/File:Q_axisvotage_Tstep.png)

[]

Figure 2. Q axis voltage

[![](//images.shoutwiki.com/gridlab-d/thumb/e/ec/Torque_Tstep.png/300px-Torque_Tstep.png)](/wiki/File:Torque_Tstep.png)

[]

Figure 2. Step change in torque

[![](//images.shoutwiki.com/gridlab-d/thumb/e/ee/Ids_Tstep.png/300px-Ids_Tstep.png)](/wiki/File:Ids_Tstep.png)

[]

Figure 3. Direct axis current

[![](//images.shoutwiki.com/gridlab-d/thumb/d/d0/Iqs_Tstep.png/300px-Iqs_Tstep.png)](/wiki/File:Iqs_Tstep.png)

[]

Figure 4. Quadrature axis current

[![](//images.shoutwiki.com/gridlab-d/thumb/b/b7/Wrze_Tstep.png/300px-Wrze_Tstep.png)](/wiki/File:Wrze_Tstep.png)

[]

Figure 5. Speed

[![](//images.shoutwiki.com/gridlab-d/thumb/a/af/Wrtw_Tstep.png/300px-Wrtw_Tstep.png)](/wiki/File:Wrtw_Tstep.png)

[]

Figure 6. Second harmonic component of Speed

### Case 2

Case 2 simulates a step change in grid voltage. A constant load torque is considered. Following are the simulation results. Fig. 9 shows impact of step change in voltage on direct axis current. Fig. 10 shows impact of step change in voltage on quadrature axis current. Fig.11 and Fig.12 shows dc and second harmonic component of speed respectively. 

  


[![](//images.shoutwiki.com/gridlab-d/thumb/d/d0/Q_axisvotage_Vstep.png/300px-Q_axisvotage_Vstep.png)](/wiki/File:Q_axisvotage_Vstep.png)

[]

Figure 7. Q axis voltage

[![](//images.shoutwiki.com/gridlab-d/thumb/9/90/Torque_Vstep.png/300px-Torque_Vstep.png)](/wiki/File:Torque_Vstep.png)

[]

Figure 8. Step change in torque

[![](//images.shoutwiki.com/gridlab-d/thumb/f/f8/Ids_Vstep.png/300px-Ids_Vstep.png)](/wiki/File:Ids_Vstep.png)

[]

Figure 9. Direct axis current

[![](//images.shoutwiki.com/gridlab-d/thumb/1/17/Iqs_Vstep.png/300px-Iqs_Vstep.png)](/wiki/File:Iqs_Vstep.png)

[]

Figure 10. Quadrature axis current

[![](//images.shoutwiki.com/gridlab-d/thumb/e/e1/Wrze_Vstep.png/300px-Wrze_Vstep.png)](/wiki/File:Wrze_Vstep.png)

[]

Figure 11. Speed

[![](//images.shoutwiki.com/gridlab-d/thumb/a/a4/Wrtw_Vstep.png/300px-Wrtw_Vstep.png)](/wiki/File:Wrtw_Vstep.png)

[]

Figure 12. Second harmonic component of Speed

# References

P. Krause et al. Analysis of electric machinery and drive systems. Vol. 75. John Wiley & Sons, 2013. 


