# PT ACCESS - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/PT_ACCESS
PT_ACCESS \- Variable and property access control flags 

## Contents

  * 1 Synopsis
    * 1.1 PA_PUBLIC
    * 1.2 PA_REFERENCE
    * 1.3 PA_PROTECTED
    * 1.4 PA_PRIVATE
    * 1.5 PA_HIDDEN
  * 2 See also
## Synopsis
    
    
    PT_ACCESS, PA_PUBLIC|PA_REFERENCE|PA_PROTECTED|PA_PRIVATE|PA_HIDDEN
    

Access to variables and properties may be restricted using these access control flags. The underlying access privileges are 

PT_N
    Only the creating module (or core) can access the property.
PT_R
    The property may be read.
PT_W
    The property may be modified.
PT_S
    The property may be saved to output.
PT_L
    The property may be loaded from input.
PT_H
    The property is hidden (not searchable).

### PA_PUBLIC

Public properties can be read, modified, saved, and loaded. 

### PA_REFERENCE

Reference properties can be read, saved, and loaded. 

### PA_PROTECTED

Protected properties can be read. 

### PA_PRIVATE

Private properties can be saved and loaded. 

### PA_HIDDEN

Hidden property are public but will not be found by searches. 

## See also

  *     * [Module globals]

