# Tape (module) - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Tape_Module_Guide

---
 
 
The **tape** module implements objects that can be used to establish and change the boundary condition on a model, and observes the properties of individual objects or the aggregate properties of a group of objects. **Player** and **shaper** tapes are used for updating the model at specified times from a file. **Recorder** and **collector** tapes are used for collecting information from the model. 

## Contents

  * 1 Synopsis
  * 2 Classes
  * 3 Globals
  * 4 Output Modes
    * 4.1 File
    * 4.2 ODBC
      * 4.2.1 HEADER_TABLE
      * 4.2.2 OBJECT_TABLE and EVENT_TABLE
  * 5 See also
# Synopsis
    
    
    module tape {
    	gnuplot_path _path_to_gnuplot_ ;
    	flush_interval 30; // seconds
    }
    

# Classes

  * [player] – Play data into the model
  * [shaper] – Generate pulsed or modulated data from averages
  * recorder – Record data to a stream 
    * [multi_recorder] – Record properties from multiple objects
    * [group_recorder] – Records properties of objects designated by class type and group id
    * [violation_recorder] – Records voltage and thermal limit violations as well as reverse flow through a substation
  * [collector] – Data aggregation recording
  * [histogram] – Property statistics
# Globals

  * [gnuplot_path] (char1024)  **TODO**: 
  * [flush_interval] (int32) effects how often output streams are flushed
# Output Modes

Tape output mode is parsed as a token in the 'file' property. The output mode and the output path are separated by a colon. 

## File

The default tape output mode is to write to CSV files. In the absence of a specified 'file' property, the tape will default to writing a CSV file with the name "[classname]-[object ID].csv". The token "file" does not need to be specified to output to a CSV file. 

## ODBC

GridLAB-D is able to write to an SQL database by means of ODBC. The current implementation supports two string formats, one for anonymous connections and one that includes login credentials. The string 'file "odbc:[DSN]:[object name]";' will write to the database specified with the name in [DSN], and will reference lines for that tape with [object name]. The string 'file "odbc:[DSN]:[username]:[password]:[object name]";' will attempt to log in to the data source with the specified username and password. Note that the double-quotes are required in both cases. 

The specified database needs to have three tables for GridLAB-D to properly communicate with it. 'HEADER_TABLE' contains the information for an output tape's run, including who ran the file and when. 'OBJECT_TABLE' contains the output lines from recorders and collectors. 'EVENT_TABLE' has the same format as 'OBJECT_TABLE', but is specifically read in by player objects. 

### HEADER_TABLE
    
    
    - HEADER_OBJECT_NAME - Text - The name of the referenced tape
    - HEADER_TIME - Text - Asctime() that the tape was opened
    - HEADER_USER - Text - Local computer username
    - HEADER_HOST - Text - Local computer hostname
    - HEADER_TARGET - Text - Target field for this tape, either and object name or aggregate definition
    - HEADER_PROPERTY - Text - Property field for this tape
    - HEADER_INTERVAL - Number - Interval for this tape recording values (-1 == on change)
    - HEADER_LIMIT - Number - Maximum number of lines written for this tape
    

### OBJECT_TABLE and EVENT_TABLE
    
    
    - EVENT_OBJECT_NAME - Text - Name of the tape object that wrote this line
    - EVENT_LINE - Number - Nth line written to or to be read by the tape
    - EVENT_TIME - Text - Timedate of the event, either in relative or absolute (YYYY-MM-DD HH:MM:SS) format
    - EVENT_VAL - Text - Value written or read to the target.  Text for a double, complex, or enumeration.
    

Note that at the beginning of each run, an output tape will remove any rows that match its object name, to prevent data collisions. To avoid this, change the object name of recorders between runs, or use a different data source. 

# See also

  * [Global variables]
  * [Modules]
  * recorder
  * [collector]
  * [player]
  * [group recorder]
  * [metrics_collector]
  * [metrics_collector_writer]

