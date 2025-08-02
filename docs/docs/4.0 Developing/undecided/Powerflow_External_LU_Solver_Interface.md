# Powerflow External LU Solver Interface

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Powerflow_External_LU_Solver_Interface
# Powerflow External LU Solver Interface

## Contents

  * 1 Powerflow Module External LU Matrix Solver Overview
  * 2 General Specifications
  * 3 GridLAB-D Implementation
  * 4 Functional Interface
    * 4.1 LU_init
    * 4.2 LU_alloc
    * 4.3 LU_solve
    * 4.4 LU_destroy
  * 5 Linux Build
## Powerflow Module External LU Matrix Solver Overview

The `powerflow` module has the option to utilize a Newton-Raphson-based solver to obtaine the final system values. As part of this algorithm, a matrix inversion and the solving of a system of equations must occur ($Ax=b \to x=A^{-1}b$). By default, GridLAB-D uses the SuperLU package to perform these operations. However, other solvers may offer faster computations, or be better suited for a particular system being examined. As such, the ability to interface with an external solver is included in the GridLAB-D `powerflow` module. 

## General Specifications

Any matrix-equation solving package must be able to properly handle the data types passed. The specifics of the interfacing structure will be outlined below, but some general guidelines must hold for the selected solver. 

All of the matrix manipulations are handled using double-precision floating point numbers, so the solver must support that capability. As part of the Newton-Raphson algorithm implemented, complex numbers are separated into their individual double-precision components. 

The external matrix solver must accept sparse matrices in a compressed column sparse matrix representation. A brief description can be found at [Netlib's compressed column representation page](http://netlib.org/linalg/html_templates/node92.html#SECTION00931200000000000000). Other modes can be translated inside the external matrix solver, but input data will come in this format. Consider a very simple 4x4 matrix: 

$$ \begin{bmatrix} 2 & 3 & 0 & 0 \\\ 3 & 0 & 4 & 0 \\\ 0 & -1 & -3 & 2 \\\ 0 & 0 & 1 & 0 \end{bmatrix} $. 

In compressed column format, this matrix is fed in as three separate arrays: 

$$Data = \begin{bmatrix} 2 & 3 & 3 & -1 & 4 & -3 & 1 & 2 \end{bmatrix}$$

$$Rows = \begin{bmatrix}0 & 1 & 0 & 2 & 1 & 2 & 3 & 2 \end{bmatrix}$$

$$Columns = \begin{bmatrix} 0 & 2 & 4 & 7 & 8 \end{bmatrix}$. 

The specific arrays that contain the $Data$, $Rows$, and $Columns$ information are outlined below. As long as the information is passed in this format and in a double-precision number, it will interface with the `powerflow` module. 

## GridLAB-D Implementation

To utilize an appropriate external matrix solver with the `powerflow` module, two steps must occur. First, the appropriate library file (.dll file in windows or .so file in Linux) must be located in a place where GridLAB-D can access it. This is typically in the same folder all of the module library files are located. If the file is not located in an accessible location, GridLAB-D will fail to load it and continue utilizing the internal superLU solver for the Newton-Raphson powerflow. 

In addition to being accessible, GridLAB-D must also know to look for the solver. This is done with a `powerflow` module-level variable `lu_solver`. If a compiled version of the KLU solver were utilized, it would be implemented with 
    
    
    module powerflow {
    	lu_solver "KLU";
    	solver_method NR;
    	}
    

where "KLU" represents the name of the appropriate library file (e.g., solver_KLU.dll on Windows) and the `solver_method NR` line indicates the Newton-Raphson solver should be utilized. If `solver_method NR` is not specified, `powerflow` defaults to the Forward-Back Sweep method and does not utilize the matrix solver. 

## Functional Interface

To properly integrate into the `powerflow` module Newton-Raphson solver, four functions must be exported or exposed from the appropriate library file. If any one of these functions is not found by the `powerflow` module during initialization, the solver reverts to the internal superLU solver. 

The four function names required are: 

Name  | Brief Description   
---|---  
LU_init  | Initialization function prior to Y-matrix formation or allocations   
LU_alloc  | Allocation and memory handling function   
LU_solve  | Solver function   
LU_destroy  | Memory freeing and temporary variable cleaning function   
  
### LU_init

The function prototype for `LU_init` is given as 
    
    
    void *LU_init(void *ext_array)
    

where `ext_array` is a void pointer kept by the Newton-Raphson solver. On success the return value is a pointer to the new `ext_array`. This implementation allows an easier exit for NULL conditions, particularly if a memory allocation fails. Upon completion of the `LU_init` routine, the `powerflow` NR solver checks this variable for a NULL condition. As such, if no intermediate variable storage is needed, `ext_array` must point to some dummy, non-NULL location to allow the solver to proceed. 

The `ext_array` variable is meant to provide persistence between calls to the Newton-Raphson solver and store any variables the particular external solver needs between runs. A pointer to a structure is suggested as a way to keep individual variables of the external solver separate. 

This function is intended to initialize any "start-up" parameters for the matrix solver, such as default options or reset flop calculations. 

### LU_alloc

The function prototype for `LU_alloc` is given as 
    
    
    void LU_alloc(void *ext_array, unsigned int rowcount, unsigned int colcount, bool admittance_change)
    

where `ext_array` is the same void pointer utilized in `LU_init`. `rowcount` and `colcount` provide information on the admittance matrix that is expected to pass into the solver. At this time, `rowcount` and `colcount` are the same value (square admittance matrix), but the general functionality is prototyped here in case future matrix solver uses in `powerflow` require it. The `admittance_change` flag occurs when the admittance matrix has had a value updated, or the overall size changed. This variable is set after the particular Newton-Raphson call finishes (inside the `SWING` node), but could be reset inside `LU_alloc` to prevent multiple memory allocations. 

This function is intended to initialize any working arrays required by the external solver, such as L or U decomposition matrices or permutation matrices. 

### LU_solve

The function prototype for `LU_solve` is given as 
    
    
    int LU_solve(void *ext_array, NR_SOLVER_VARS *system_info_vars, unsigned int rowcount, unsigned int colcount)
    

where `ext_array` is again the same void-pointered storage array utilized in the previous functions. `rowcount` and `colcount` serve similar roles to those in `LU_alloc`, but `colcount` is passed the solution vector's column count (1 for current implementations of the Newton-Raphson solver). The `system_info_vars` parameter is a structure that contains the compressed column sparse matrix formatted data, as well as the solution vector. The structure `NR_SOLVER_VARS` is defined in the C-code as 
    
    
    typedef struct {
    	double *a_LU;
    	double *rhs_LU;
    	int *cols_LU;
    	int *rows_LU;
    } NR_SOLVER_VARS;
    

where `a_LU` contains the non-zero data elements($Data$ in the example above), `rhs_LU` contains the solution vector (the $b$ portion of the $Ax=b$ equation), `cols_LU` contains the column transition points ($Columns$ in the above example), and `row_LU` contains the row locations of the data ($Rows$ in the above example). Note that the solution, $x$ is returned in the `rhs_LU` portion of this structure, so $b$ will need to be stored separately if this will affect the solver operation. 

The `LU_solve` function returns an integer status indicating its success or failure. Any value returned that is non-zero will be interpreted as a solver failure by the `powerflow` module. The return value will be explained via a `gl_verbose` call, so solver failures can be passed via this variable for debugging and diagnostic purposes. 

This function is intended to contain the actual matrix equation solver. If explicit knowledge of the data being passed in (in the compressed column sparse format) is required for solver variable initialization, it may be performed in this function as well. 

### LU_destroy

The function prototype for `LU_destroy` is given as 
    
    
    void LU_destroy(void *ext_array, bool new_iteration)
    

where `ext_array` is the null pointer variable passed into the other functions and `new_iteration` is a flag indicating if another solver pass is scheduled. If `new_iteration` is false, the Newton-Raphson solution meets the convergence criterion and the solver is preparing to exit back to `powerflow`. 

This function is intended to remove any iteration-specific or intermediate memory spaces between calls to the external solver routine (in `LU_solve`). It can be used to clean up values upon a successful solution, if the `new_iteration` flag is utilized. 

## Linux Build

To build the KLU shared library in Linux you must modify the Makefile in `solver_klu/source/KLU/Lib` to add the following targets to the `library` target 
    
    
     install: library libklu.so libklu.la
           cp libklu.{so,la} /usr/local/lib/gridlabd
     
     libklu.la:
           libtool --mode=link gcc -g -O -o libklu.la *.o
     
     libklu.so:
           ar -x libklu.a
           gcc -shared *.o -o libklu.so
    

On some systems you may need to change the compiler flags to support relocation during loader operations. If so, change the following line: 
    
    
     C = $(CC) $(CFLAGS) -fPIC
    

to include the `-fPIC` option. 

After building the library using the main makefile in the KLU folder, run make in the KLU/Lib folder and copy the targets to the gridlabd library folder, naming them `solver_klu,{la,so}`. Then you can use the `lu_solver "klu"` in the model to enable use of the KLU solver, i.e., 
    
    
    module powerflow {
    #ifexist "/usr/local/lib/gridlabd/libklu.so"
    	lu_solver klu;
    #endif
    	solver_method NR;
    	}
    


  
