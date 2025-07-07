# Timezone - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Timezone
In GridLAB-D timezones follow the [Posix TZ standard](http://www.gnu.org/s/hello/manual/libc/TZ-Variable.html). Each timezone is described with a string using the form 
    
    
     STZ[hh[:mm][DTZ][,M#[#].#.#/hh:mm,M#[#].#.#/hh:mm]]
    

where STZ is the 3-digit standard time zone specification and DTZ is the 3 digit daylight time zone specification, hh:mm describes the offset from GMT with negative for each and positive for west, the first M-spec describes the month, week and weekday on which daylight savings starts, and the second on which it ends. 

Because GridLAB-D must run historical simulations, the timezone rules may change from year to year. Consequently, there are different section specify the year in which the timezone specification goes into effect. Each year section is described with the string 
    
    
    [YYYY]
    

The following timezones are currently supported in `tzinfo.txt` for the United States: 
    
    
    UTC0 ; Coordinated Universal Time ~ never uses DST
    GMT0 ; Greenwich Mean Time, no DST
    EST5 ; Eastern no DST
    CST6 ; Central no DST
    MST7 ; Mountain no DST
    PST8 ; Pacific no DST 
    
    [1970] ; Rules as of 1967
    GMT0GMT,M3.5.0/02:00,M10.5.0/2:00 ; GMT, DST last Sun/Mar to last Sun/Oct
    EST+5EDT,M4.5.0/02:00,M10.5.0/02:00 ; Eastern, DST last Sun/Apr to last Sun/Oct
    CST+6CDT,M4.5.0/02:00,M10.5.0/02:00 ; Central, DST last Sun/Apr to last Sun/Oct
    MST+7MDT,M4.5.0/02:00,M10.5.0/02:00 ; Mountain, DST last Sun/Apr to last Sun/Oct
    PST+8PDT,M4.5.0/02:00,M10.5.0/02:00 ; Pacific, DST last Sun/Apr to last Sun/Oct
    
    [1986] ; Rules as of 1986
    EST+5EDT,M4.1.0/02:00,M10.5.0/02:00 ; Eastern, DST first Sun/Apr to last Sun/Oct
    CST+6CDT,M4.1.0/02:00,M10.5.0/02:00 ; Central, DST first Sun/Apr to last Sun/Oct
    MST+7MDT,M4.1.0/02:00,M10.5.0/02:00 ; Mountain, DST first Sun/Apr to last Sun/Oct
    PST+8PDT,M4.1.0/02:00,M10.5.0/02:00 ; Pacific, DST first Sun/Apr to last Sun/Oct
    
    [2007] ; Rules as of 2007
    EST+5EDT,M3.2.0/02:00,M11.1.0/02:00 ; Eastern, DST second Sun/Mar to first Sun/Nov
    CST+6CDT,M3.2.0/02:00,M11.1.0/02:00 ; Central, DST second Sun/Mar to first Sun/Nov
    MST+7MDT,M3.2.0/02:00,M11.1.0/02:00 ; Mountain, DST second Sun/Mar to first Sun/Nov
    PST+8PDT,M3.2.0/02:00,M11.1.0/02:00 ; Pacific, DST second Sun/Mar to first Sun/Nov

## Synopsis
    
    
    clock {
      timezone _tz-spec_ ;
    }
    

## Description

The timezone [clock directive] determines which timezone to use during the simulation. The timezone must be known before any [timestamps] can be interpreted. The timezone rules are used to determine the offset from UTC for all time calculations, as well as determine daylight or summer time shifts. 

Prior to [Hassayampa (Version 3.0)]
    If the timezone is not set, the system will assume all [timestamps] are in UTC.

As of [Hassayampa (Version 3.0)]
    If the timezone is not set, the system will assume all [timestamps] are in local time.

Time zones are specified using the [POSIX timezone standard](http://www.gnu.org/software/libc/manual/html_node/TZ-Variable.html). 

### GLM

The timezone is usually set using the [clock directive] as follows: 
    
    
     clock {
       timezone PST8PDT;
       starttime '2000-01-01 00:00:00 PST';
       stoptime '2001-01-01 00:00:00 PST';
     }
    

### Command line

The timezone can be set using the command line: 
    
    
    host% export TZ=PST8PDT
    

On Windows machines, the syntax is 
    
    
    C:\> set TZ=PST8PDT
    

If the timezone is not set either using a [clock directive] or the [TZ] [environment variable], GridLAB-D may be unable to interpret [timestamps] and fatal errors may occur. 

### Locale names

As of [Hassayampa (Version 3.0)] you may use locale names instead of the timezone codes. Locale names are listed in the `tzinfo` file and take the form 
    
    
    Country/Region/City
    

  
For example, instead of coding 
    
    
    timezone PST+8PDT;
    

you can code 
    
    
    timezone US/CA/Los Angeles;
    

For a listing of country and region codes, see [ISO Std 3166-2](http://en.wikipedia.org/wiki/ISO_3166-2). 

Timezones and daylight-savings/summer time rules can be found at [www.worldtimezone.com](http://http://www.worldtimezone.com/). 

## Version

Prior to [Hassayampa (Version 3.0)]
    Only US timezones were distributed with GridLAB-D. City names are not supported.

As of [Hassayampa (Version 3.0)]
    All officially recognized international timezones are implemented. Many city names are supported. The list of recognized cities and timezones can be found in the [timezone file](http://gridlab-d.svn.sourceforge.net/viewvc/gridlab-d/trunk/core/tzinfo.txt).

## Caveats

* Most Asia and Africa timezones are not yet implemented in the [timezone file](http://gridlab-d.svn.sourceforge.net/viewvc/gridlab-d/trunk/core/tzinfo.txt). 

* It is not clear whether 1/2 and 1/4 hours offset timezones always work properly. 

* Many of the historical rules for summer time around the world are not supported. In some cases the current summer time rules may be inappropriately applied to past years. 
