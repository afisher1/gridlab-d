# Tape Histogram Guide - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Histogram

---
 
 
## Contents

  * 1 Glossary
  * 2 Usage
    * 2.1 Uniform Bins
    * 2.2 Non-Uniform Bins
      * 2.2.1 Sample Object
      * 2.2.2 Properties
The Histogram class is designed to watch either a single object or a group of objects with a common property and generate rows with the number of times the watched objects values' were within certain ranges. 

## Glossary

Sample
    refers to the values collected at a specific interval.

Count
    refers to a collection of samples across a specific interval.

Row
    aludes to the underlying structure of the histogram's output, in which the date precedes a comma separated list of counts for the previous counting interval.

Bin
    is used to reference a column, such as "between ten and twenty".

# Usage

There are two overt ways of organizing the bins with the Histogram class. The first method will automatically partition a single range into several uniform bins. The second method requires the explicit delimitation of the bins, which may be single values (or enumerated states), single-ended infinite ranges, or finite ranges. 

## Uniform Bins

To set up uniform bins, the Histogram object must define the properties: 
    
    
    object histogram {
      // [...]
      bin_count 4;
      min 0;
      max 100;
    }
    

The first bin will count values 0 ≤ x < 25, the second counts 25 ≤ x < 50, third counts 50 ≤ x < 75, and the fourth counts 75 ≤ x ≤ 100. 

## Non-Uniform Bins

The specific bin definitions are of the general form **[a..b]**. Both inclusive square brackets **[ ]** and exclusive parentheses **( )** are valid ways to bound either side of the bin. The omission of the brackets will implicitly create an inclusive range, such as with **25..50**. If one of the two values is excluded, the bin will implicitly extend to positive or negative infinity, exclusively. 

A dash may replace the .. in defining bin ranges. Possible ambiguity may arise because "-100" is synonymous to the parser as "..100". To count everything up to -100, "..-100" or "--100" should be used. 

Explicit bins may overlap freely. An example is a cumulative function: 
    
    
    bins ..20,..40,..60,..80,..100;
    

#### Sample Object

A simple histogram that looks at a set of houses and accumulates the internal air temperatures would look like 
    
    
    object histogram {
      name histogram_test;
      filename /here/there/hist_test.csv;
      bins ..66, ..68, ..70, ..72, ..74, ..;
      limit 30;
      samplerate 3600;
      countrate 3600;
      group class=house;
      property temperature;
    };
    

This will sample all of the houses in a model once an hour and write the accumulated temperature results thirty times to the file at /here/there/hist_test.csv (c:\here\there\hist_test.csv under Windows). 

### Properties

filename - char1024
    Used for a combination of defining the name of the output file, and for specifying the output mode. Read as either _fname_ or _ftype_ :_fname_ , where valid values of _ftype_ are **file** , **memory** , or **odbc**.

group - char1024
    Defines the group that the histogram should collect values for, see [Finding objects] for more details.

bins - char1024
    If nonempty, the string to parse to define the histogram bins. Only used if input bin_count is non-positive or not inititalized, or if min < max or not defined.

property - char32
    The property name to look for and sample in the parent object or the object group. Complex values must use "prop.mag", "prop.ang", "prop.real", or "prop.imag" to specify which part to sample.

min - double
    Minimum of the range to use for uniform bins.

max - double
    Maximum of the range to use for uniform bins.

samplerate - int32
    The interval for sampling the parent or the object group and incrementing histogram bins appropriately, in seconds. -1 means "every iteration", 0 means "every timestep".

countrate - int32
    The interval for writing histogram counts to the output stream, in seconds. -1 means "every iteration", 0 means "every timestep".

bin_count - int32
    [input] the number of uniform bins to create. [output] the number of bins used by the histogram.

limit - int32
    The maximum number of lines to write to the output stream.


