# Optimize

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Optimize
# Optimize

The optimize module implements optimization classes based on the idea that optimization is a component of GridLAB-D as opposed to the idea that one uses GridLAB-D to do optimization. The optimizer provides functionality that modelers can use rather than requiring modelers to provide models that optimization experts can use. 

The optimizer is designed to meet the expectations of GridLAB-D modelers, rather than those of traditional optimization experts. For example, the terminology of "target property" (instead of _objective_) and "changing property" (instead of _decision variable_) to facility accessibility and understanding for GridLAB-D modelers who are not necessarily well-versed in the language of optimization. 

Because of the structure of GridLAB-D models, it is easy to create discontinuous functions or even non-numeric values. The models usually cannot be solved with classical optimization methods. For this reason, it is not expected that the optimizer can always determine the global optimum. 

All the classes of optimize implement at least the following properties. 

Target property
    The target the optimization objective. The general form of the objective is either **[max|min](_expression_)** or **[_object_ :]_property_ =_expression_ , where _expression_ is a mathematical expression conforming to the GridLAB-D loader syntax.**

Changing properties
    This specifies the properties that may be changed to achieve the objective. The general form of the decision variables list is **[_object1_ :]_property1_[,[_object2_ :]_property2_[,...,[_objectN_ :]_propertyN_]]**.

Constraints
    This specifies the constraints that are applied. Constraints are described as comma-separated a list of inequalities of the form **[_object_ :]_property_ [<|<=|=|>=|>|<>] (_expression_)**.

Maximum trials
    This limits the number of trials before the optimizer quits trying.

Precision
    This limits the precision with which the objective must be determined.

Models
    This enumeration allows the specification of modeling assumptions such as linear, non-negative, and automatic scaling.

Methods
    Specific method options such as the use of tangent or quadratic estimates, forward or central derivatives, and Newton or conjugate searching.

  


# 

Classes

  * [Simple] is a simple optimizer that uses Newton's method. It only works with continuous behaviors.
# 

See also

  * [Modules]
  * Optimize module 
    * [Requirements]
    * [Specifications]
    * [Developer notes]
    * [Technical support document]
    * [Validation]
