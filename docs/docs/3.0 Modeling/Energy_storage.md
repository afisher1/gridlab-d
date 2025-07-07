# Energy storage - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Energy_Storage

---
 
 
[![Workinprogress.jpg](//images.shoutwiki.com/gridlab-d/d/d5/Workinprogress.jpg)](/wiki/File:Workinprogress.jpg)EDITING IN PROGRESS[![Workinprogress.jpg](//images.shoutwiki.com/gridlab-d/d/d5/Workinprogress.jpg)](/wiki/File:Workinprogress.jpg)

energy_storage – generic? distributed storage 

## Synopsis
    
    
    module generators;
    class energy_storage {
    	enumeration {SUPPLY_DRIVEN=4, CONSTANT_PF=3, CONSTANT_PQ=2, CONSTANT_V=1, UNKNOWN=0} generator_mode;
    	enumeration {ONLINE=2, OFFLINE=1} generator_status;
    	enumeration {DC=0, AC=1} power_type;
    	double Rinternal;
    	double V_Max[V];
    	complex I_Max[A];
    	double E_Max;
    	double Energy;
    	double efficiency;
    	double Rated_kVA[kVA];
    	complex V_Out[V];
    	complex I_Out[A];
    	complex VA_Out[VA];
    	complex V_In[V];
    	complex I_In[A];
    	complex V_Internal[V];
    	complex I_Internal[A];
    	complex I_Prev[A];
    	set {S=112, N=8, C=4, B=2, A=1} phases;
    }
    

## Properties

**TODO**: 

## Remarks

**TODO**: 

## Example

**TODO**: 

## See Also

  * [Generator Module Guide]
  * [Generators (module)]
    * [Battery]
    * [Diesel dg]
    * Energy storage
    * inverter
    * [Microturbine]
    * [Power electronics]
    * [Rectifier]
    * [Solar]
    * [Windturb dg]
