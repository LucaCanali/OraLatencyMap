
OraLatencyMap, a performance widget to visualize the latency drilldown for Oracle wait events using Heatmaps

Scope: Performance investigations of event latency. For example single block read latency with OraLatencyMap.sql

Author: Luca.Canali@cern.ch

Version: v1.2, March 2014. 
         Original version 1.0, Aug 2013.

Credits: Brendan Gregg for "Visualizing System Latency", Communications of the ACM, July 2010
         Tanel Poder (snapper, moats, sqlplus and color), Marcin Przepiorowski (topass)

Notes: These scripts need to be run from sqlplus from a terminal supporting ANSI escape codes. 
       Run from a privileged user (select on v$event_histogram and execute on dbms_lock.sleep)
       Tested on 11.2.0.3, 11.2.0.4, 12.1.0.1, 12.1.0.2 for Linux x86_64.
       A workaround may be needed for 10g clients: 
            increase set lines to 13000 in OraLatencyMap_advanced.sql (thanks to Paul Kelly).
       Best not wrap sqlplus with rlwrap or else graphics smoothness will suffer.

How to start:
     sqlplus / as sysdba
     SQL> @OraLatencyMap 

More examples:
     SQL> @Example2_commit_time.sql
     SQL> @OraLatencyMap_advanced 5 "db file sequential read" 12 80 "and inst_id=1"


Output: Two latency heatmaps of the given wait event
        The top heatmap represents the number of waits per second and per latency bucket
        The bottom heatmap represents the estimated time waited per second and per latency bucket
        with the advanced script it is possible to customize sampling time, event name, screen size 
        moreover in RAC, the default is to aggregate histogram data over all nodes, but this is 
        customizable too to display data only for a given instance and/or set of sessions

        
Related: OraLatencyMap_advanced.sql      -> this is the main script for generic investigation of event latency with heat maps
         OraLatencyMap_internal.sql      -> the slave script where all the computation and visualization is done
         OraLatencyMap_internal_loop.sql -> the slave script that runs several dozens of iterations of the tool's engine 
         Example*.sql                    -> a series of wrapper and exmaple scripts

Change list: 
         1.2  OraLatencyMap_internal: added legend, added average latency value, additional minor changes
         1.1b OraLatencyMap_internal: fixed a small mistake in the formular to compute v_color  
         1.1a OraLatencyMap_internal: use of ceil instead of round to compute g_table_wait_count rate. (thanks to Coskan)
         1.1a OraLatencyMap_internal: Add print date and time. (thanks to Coskan)
         1.1a OraLatencyMap_internal: minor, change of position for the code to compute g_latest_time_sec


Documentation, blog entries at: 
                 http://externaltable.blogspot.com/2014/03/recent-updates-of-oralatencymap-and.html
                 http://externaltable.blogspot.com/2013/05/latency-heat-map-in-sqlplus-with.html


