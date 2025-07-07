# HowTo:Create a solver - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/HowTo:Create_a_solver
[Template:NEW30]

## Contents

  * 1 Step 1 - Stub in the implementation file
  * 2 Step 2 - Add to build files
  * 3 Step 3 - Define the data structures
  * 4 Step 4 - Implement newton_method_set
  * 5 Step 5 - Implement newton_method_get
  * 6 Step 6 - Implement newton_method_solve
  * 7 Step 7 - Document the solver
  * 8 Tips and Tricks
  * 9 Version
  * 10 See also
This **[How To]** describes the procedure for implementing a solver in GridLAB-D. The example used here illustrates a Newton's Method solver that solver N independent single variable equations. 

## Step 1 - Stub in the implementation file

Create the file `core/solvers/newton_method.cpp` and open it for editing, adding the following code: 
    
    
    // $ Id$
    #include <stdlib.h>
    #include "gridlabd.h"
    static unsigned int version = 1;
    // **TODO** structure of solver data (see Step 3)
    static unsigned int max_iterations = 100;
    EXPORT int newton_method_init(CALLBACKS *fntable)
    {
      callback=fntable;
      return 1;
    }
    EXPORT int newton_method_set(char *param,...)
    {
      int n=0;
      va_list arg;
      va_start(arg,param);
      char *tag = param;
      while ( tag!=NULL )
      {
        // **TODO** handle set params (see Step 4)
        {
          gl_error("newton_method_config(char *param='%s',...): tag '%s' is not recognized");
          return n;
        }
        tag = va_arg(arg,char*);
        n++;
        }
      return n;
    }
    EXPORT int newton_method_get(char *param,...)
    {
      int n=0;
      va_list arg;
      va_start(arg,param);
      char *tag = param;
      while ( tar!=NULL )
      {
        // **TODO** handle get params (see Step 5)
        {
          gl_error("newton_method_config(char *param='%s',...): tag '%s' is not recognized");
          return n;
        }
        tag = va_arg(arg,char*);
        n++;
        }
      return n;
    }
    EXPORT int newton_method_solve(NMDATA *data)
    {
      // **TODO** implement solver (see Step 6)
      return 0;
    }
    

## Step 2 - Add to build files

  1. In MSVC, right-click on the "solvers" project and add the implementation file. In other environments, open `core/solvers/solvers.vcproj` and add the implementation file in the appropriate place in the XML code.
  2. Open `core/solvers/Makefile.am` and add `newton_method.cpp` to `glsolvers_la_SOURCES`
## Step 3 - Define the data structures

Add the following code at the `**TODO**` comment 
    
    
    typedef struct {
      unsigned int n; // dimensions (default 1)
      double *x; // current values of x
      double (**f)(double); // functions
      double (**df)(double); // derivatives
      double *p; // precisions
      unsigned int *m; // multiplicities (default is 1)
      unsigned char *s; // status (0=failed, 1=converge, 2=non-converged)
    } NMDATA;
    static unsigned int dimension = 1; // default number of dimensions for new data
    

## Step 4 - Implement `newton_method_set`

Add the following code at the `**TODO**` comment 
    
    
        // set the maximum number of iterations
        if ( strcmp(param,"max_iterations")==0 )
          max_iterations = va_arg(arg,unsigned int);
        
        // set the number of dimensions to use for the next data allocation
        else if ( strcmp(param,"dimension")==0 )
          dimensions = va_arg(arg,unsigned int);
        else
    

## Step 5 - Implement `newton_method_get`

Add the following code at the `**TODO**` comment 
    
    
        // get the current version of the solver
        if ( strcmp(param,"version")==0 )
          *va_arg(arg,unsigned int*) = version;
        
        // get the maximum number of iterations currently in use
        else if ( strcmp(param,"max_iterations")==0 )
          *va_arg(arg,unsigned int*) = max_iterations;
        
        // get the number of dimension currently is use
        else if ( strcmp(param,"dimensions")==0 )
          *va_arg(arg,unsigned int*) = dimensions;
        
        // get a new solver data block using the current number of dimensions
        else if ( strcmp(param,"init_data")==0 )
        {
          NMDATA *data = (NMDATA*)va_arg(arg,NMDATA*);
          if ( data->n==0 ) data->n = dimensions;
          data->df = (double(**)(double))malloc(sizeof(void*)*dimensions);
          data->f = (double(**)(double))malloc(sizeof(void*)*dimensions);
          data->x = (double*)malloc(sizeof(double)*dimensions);
          data->m = (unsigned int*)malloc(sizeof(unsigned int)*dimensions);
          data->p = (double*)malloc(sizeof(double)*dimensions);
          data->s = (unsigned char*)malloc(sizeof(unsigned int)*dimensions);
          int i;
          for ( i=0 ; i<dimensions ; i++ )
          {
            data->x[i] = 0;
            data->df[i] = NULL;
            data->f[i] = NULL;
            data->m[i] = 1;
            data->p[i] = 1e-8;
            data->s[i] = 1;
          }
        }
        else
    
    

## Step 6 - Implement `newton_method_solve`

Add the following code at the `**TODO**` comment 
    
    
      // n is an index, s is the return status
      int n, s=1;
      
      // for each equation
      for ( n=0 ; n<data->n ; n++ )
      {
        // map the solution data to local variables
        double &p = data->p[n];
        double &x = data->x[n];
        double (*f)(double) = data->f[n];
        double (*df)(double) = data->df[n];
        unsigned int &m = data->m[n];
        
        // dx is the correction to the solution, i is the iteration counter
        double dx;
        unsigned int i=0;
        
        // iterate until the correction is less than the precision given (status=1)
        do {
        
          // slope at the current solution
          double dydx = (*df)(x);
          
          // if slope is not usable
          if ( isnan(dydx) || dydx==0 )
          {
            // no solution (status=0)
            x = NaN;
            s = data->s[n] = 0;
            break;
          }
          
          // if slope is infinite
          else if ( !isfinite(dydx) )
            // current solution is used (status=1)
            break;
          
          // compute new correction
          else
          {
            dx = m*(*f)(x)/dydx;
            x -= dx;
          }
          
          // if iteration limit reached current solution is used (status=2)
          if ( i++>max_iterations )
          {
             if ( s>0 ) s=2; // only flag if not already flagged for failure
             data->s[n] = 2;
             break;
          }
        } while ( fabs(dx)>p );
      }
    

## Step 7 - Document the solver

  1. Add the solver to the [Xref:Solvers] _See Also_ list by adding a new link of the form `[[Tech:Newton's Method|Newton's Method]]`
  2. Click on the new link and write the page. Document the _Synopsis_ , _Description_ , _Version_ , _See also_ sections at least. Don't forget to document the _set_ /_get_ parameters and the data structure used to access the solver.
  3. Add a _brief_ description of the solver to the [Dev:Solvers] page with a link to the new _Tech_ page.
## Tips and Tricks

Check the version
    Each time the data structure is changed, the version number should be changed to prevent inconsistent data usage and mysterious crashes.

Do not use globals for anything other than defaults
    Global variables can cause problems if more than once instance of the solver is used, or if multithreading is used.

Use on `throw(const char[])` or `throw (const char *)`
    No other `catch` is certain to be available.

Remember that the `solve` function has 3 return states
    0 means at least one of the equations had no solution
    1 means all the equations converged to a solution
    2 means at least one of the equations did not converge before the maximum iteration limit was reached.

## Version

The general solvers library was introduced in [Hassayampa (Version 3.0)]. 

## See also

  * [Solver API]
  * How to create a solver
  * Technical manuals 
    * [Equivalent Thermal Parameters (ETP)]
    * [Newton Raphson (NR)]
    * [Forward-backsweep (FBS)]
    * [Modified Euler (ME)]
    * [Runga Kutta (RK)]
  * [USE_GLSOLVERS]

