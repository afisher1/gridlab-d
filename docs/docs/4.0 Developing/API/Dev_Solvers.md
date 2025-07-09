# Dev:Solvers

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Dev:Solvers
Solvers \- C++ API for general solvers [Template:NEW30]

## Synopsis
    
    
    class glsolver {
    public:
      int (*init)(CALLBACKS *_core-function-table_);
      int (*solve)(void *_data_);
      int (*set)(char *_parameter-name_ , ..., NULL);
      int (*get)(char *_parameter-name_ , ..., NULL);
      glsolver(char *_solver-name_);
    };
    

## Description

General purpose solvers are deployed in the **glsolvers.dll** (windows) or **glsolvers.so** (linux/MacOSX) library. The _solver-name_ parameter determines which solver is loaded from the library. 

The **init** function is automatically called by the constructor and does not need to be called explicitly. 

The **set** and **get** functions are used to access solver parameters and data. In particular, every solver should implement a `get("version",&version,NULL)` and a `set("init",&data,NULL)` to allow solver users to verify they are using the same version of the data structure and to initialize the data structure. Often, the initialization procedure will depend on some parameters being set in the data structure (e.g., the number of dimensions). 

The **solve** function is called with the data structure as the argument. It returns one of three values: 

  * 0 means the solver failed without obtaining a solution
  * 1 means the solver obtained a solution within the specified criteria, e.g., precision
  * 2 means the solver obtained a solution but not within the specified criteria, e.g., iteration limit reached.
### [etp]

    The [etp solver] computes the time to the next solution of the equation $f(t)=ae^{nt}+be^{mt}+c=0$ using Newton's method. The parameter $p$ can be used to specify the precision to which the solution is obtained. If the pointer $*e$ is non-null, the value $p/[df(t)/dt]$ is written to the double to which it points. The solver handles a number of special/degenerate cases used reduced (non-iterative) methods. If there is not solution, the values returned in NaN.

    The [etp solver] returns 0 when convergence failed, 1 when a value is returned. The value is NaN when no solution is found, otherwise a positive real number representing the time is returned. The value of the time depends on the units of the units of a,b,c,n, and m.

### [newton-raphson]

**TODO**: 

### [forward-backsweep]

**TODO**: 

### [modified-euler]

**TODO**: 

### [runga-kutta]

**TODO**: 

## Version

The general solvers library was introduced in [Hassayampa (Version 3.0)]. 

Note
    A limited implementation of the Solvers C++ API is available in [Grizzly (Version 2.3)] as of <https://sourceforge.net/p/gridlab-d/code//3302> for implementation of the [Microgrids] module.

## See also

  * Solver API
  * [How to create a solver]
  * Technical manuals 
    * [Equivalent Thermal Parameters (ETP)]
    * [Newton Raphson (NR)]
    * [Forward-backsweep (FBS)]
    * [Modified Euler (ME)]
    * [Runga Kutta (RK)]
  * [USE_GLSOLVERS]

