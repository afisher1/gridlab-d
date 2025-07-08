# NEVLoads

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/NEVLoads
All loads will be described in terms of ZIP fractions and represented by voltage and current injections into the powerflow solution. The loads will be connected in a nodal manner so that any combination of wye and delta connected loads can be attached to a single bus. 

## Contents

  * 1 Parameters
  * 2 Equations
    * 2.1 Constant Impedance Loads
    * 2.2 Constant Current Loads
    * 2.3 Constant Power Loads
    * 2.4 Total Current Injection
# Parameters

The parameters required to determine the current injections due to a load connected to nodes $n$ and $m$ of a bus are listed below. 

  * $|S_{b,nm}|$ The magnitude of the base absolute power of the load connected to nodes $n$ and $m$ of a particular bus (VA) at $V_{b,nm}$.
  * $V_{nm}$ The variable voltage difference between the node to ground voltages of nodes $n$ and $m$ of a particular bus (V).
  * $|V_{b,nm}|$ The magnitude of the base voltage difference between the node to ground voltages of nodes $n$ and $m$ of a particular bus (V).
  * $pfr_{p,nm}$ The constant power fraction of the load connected to nodes $n$ and $m$ of a particular bus (unitless).
  * $pf_{p,nm}$ The power factor of the constant power fraction of the load connected to nodes $n$ and $m$ of a particular bus (-1.0 - 1.0).
  * $pfr_{i,nm}$ The constant current fraction of the load connected to nodes $n$ and $m$ of a particular bus (unitless) at $V_{b,nm}$.
  * $pf_{i,nm}$ The power factor of the constant current fraction of the load connected to nodes $n$ and $m$ of a particular bus (-1.0 - 1.0) at $V_{b,nm}$.
  * $pfr_{z,nm}$ The constant impedance fraction of the load connected to nodes $n$ and $m$ of a particular bus (unitless) at $V_{b,nm}$.
  * $pf_{z,nm}$ The power factor of the constant impedance fraction of the load connected to nodes $n$ and $m$ of a particular bus (-1.0 - 1.0) at $V_{b,nm}$.
# Equations

For the parameters described above the current injections due to each type of load fraction can be found below. 

## Constant Impedance Loads

The constant impedance of the load can be found by the following equations 

    $ \displaystyle{}S_{z,nm}=S_{b,nm}*pfr_{z,nm}*[pf_{z,nm}+j*sign(pf_{z,nm})*(1-pf_{z,nm}^{2})^{\frac{1}{2}}]$
    $ \displaystyle{}Z_{nm}=\frac{|V_{b,nm}|^{2}}{S_{z,nm}^{*}}$

The current injection due to the constant impedance can then be found from the equation below. 

    $ \displaystyle{}I_{z,nm}=\frac{V_{nm}}{Z_{nm}}$

## Constant Current Loads

The constant current of the load can be found by the following equations 

    $ \displaystyle{}S_{i,nm}=S_{b,nm}*pfr_{i,nm}*[pf_{i,nm}+j*sign(pf_{i,nm})*(1-pf_{i,nm}^{2})^{\frac{1}{2}}]$
    $ \displaystyle{}I_{nm}=(\frac{S_{i,nm}}{|V_{b,nm}|})^{*}$

The current injection due to the constant impedance can then be found from the equation below. 

    $ \displaystyle{}I_{i,nm}=I_{nm}$

## Constant Power Loads

The constant Power of the load can be found by the following equation 

    $ \displaystyle{}S_{p,nm}=S_{b,nm}*pfr_{p,nm}*[pf_{p,nm}+j*sign(pf_{p,nm})*(1-pf_{p,nm}^{2})^{\frac{1}{2}}]$

The current injection due to the constant impedance can then be found from the equation below. 

    $ \displaystyle{}I_{p,nm}=(\frac{S_{p,nm}}{V_{nm}})^{*}$

## Total Current Injection

The total current injection into a node $n$ due to loads connecting node $n$ to any number of other nodes on the same bus can be found by the following equation 

    $ \displaystyle{}I_{n}=\Sigma (I_{z,nj}+I_{i,nj}+I_{p,nj})$

where $j$ are the nodes the loads connect node $n$ to on a bus. 


