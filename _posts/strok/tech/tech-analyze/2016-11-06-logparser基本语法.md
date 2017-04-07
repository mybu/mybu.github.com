---
layout: single
title: logparser基本语法
description: 工作中用到的技术
tags: 分析技术
---

# logparser介绍
>这个是一个分析软件，微软产品


# logparser命令帮助
Usage:   LogParser [-i:<input_format>] [-o:<output_format>] <SQL query> |
                   file:<query_filename>[?param1=value1+...]
                   [<input_format_options>] [<output_format_options>]
                   [-q[:ON|OFF]] [-e:<max_errors>] [-iw[:ON|OFF]]
                   [-stats[:ON|OFF]] [-saveDefaults] [-queryInfo]

         LogParser -c -i:<input_format> -o:<output_format> <from_entity>
                   <into_entity> [<where_clause>] [<input_format_options>]
                   [<output_format_options>] [-multiSite[:ON|OFF]]
                   [-q[:ON|OFF]] [-e:<max_errors>] [-iw[:ON|OFF]]
                   [-stats[:ON|OFF]] [-queryInfo]

 -i:<input_format>   :  one of IISW3C, NCSA, IIS, IISODBC, BIN, IISMSID,
                        HTTPERR, URLSCAN, CSV, TSV, W3C, XML, EVT, ETW,
                        NETMON, REG, ADS, TEXTLINE, TEXTWORD, FS, COM (if
                        omitted, will guess from the FROM clause)
 -o:<output_format>  :  one of CSV, TSV, XML, DATAGRID, CHART, SYSLOG,
                        NEUROVIEW, NAT, W3C, IIS, SQL, TPL, NULL (if omitted,
                        will guess from the INTO clause)
 -q[:ON|OFF]         :  quiet mode; default is OFF
 -e:<max_errors>     :  max # of parse errors before aborting; default is -1
                        (ignore all)
 -iw[:ON|OFF]        :  ignore warnings; default is OFF
 -stats[:ON|OFF]     :  display statistics after executing query; default is
                        ON
 -c                  :  use built-in conversion query
 -multiSite[:ON|OFF] :  send BIN conversion output to multiple files
                        depending on the SiteID value; default is OFF
 -saveDefaults       :  save specified options as default values
 -restoreDefaults    :  restore factory defaults
 -queryInfo          :  display query processing information (does not
                        execute the query)


Examples:
 LogParser "SELECT date, REVERSEDNS(c-ip) AS Client, COUNT(*) FROM file.log
            WHERE sc-status<>200 GROUP BY date, Client" -e:10
 LogParser file:myQuery.sql?myInput=C:\temp\ex*.log+myOutput=results.csv
 LogParser -c -i:BIN -o:W3C file1.log file2.log "ComputerName IS NOT NULL"

Help:
 -h GRAMMAR                  : SQL Language Grammar
 -h FUNCTIONS [ <function> ] : Functions Syntax
 -h EXAMPLES                 : Example queries and commands
 -h -i:<input_format>        : Help on <input_format>
 -h -o:<output_format>       : Help on <output_format>
 -h -c                       : Conversion help

"-------------------------------hualidefengexian--------------logparser -h -i:fs" 

Input format: FS (FileSystem properties)
Returns properties of files and folders

FROM syntax:

 <filename> [, <filename> ...]
 Comma-separated list of paths (e.g. "C:\*.*, D:\folder\*.*")

Parameters:

 -recurse             <level> : Max subdirectory recursion level (0=no
                                recurse, -1=all levels) [default value=-1]
 -preserveLastAccTime ON|OFF  : Preserve files' last access time [default
                                value=OFF]
 -useLocalTime        ON|OFF  : Use local time for dates [default value=ON]

Fields:

  Path (S)                   Name (S)                  Size (I)           
  Attributes (S)             CreationTime (T)          LastAccessTime (T) 
  LastWriteTime (T)          FileVersion (S)           ProductVersion (S) 
  InternalName (S)           ProductName (S)           CompanyName (S)    
  LegalCopyright (S)         LegalTrademarks (S)       PrivateBuild (S)   
  SpecialBuild (S)           Comments (S)              FileDescription (S)
  OriginalFilename (S)       

Examples:

 Print the 10 largest files on the C: drive:

     LogParser "SELECT TOP 10 * FROM C:\*.* ORDER BY Size DESC" -i:FS
		 
		 
		 logparser "select FileVersion,ProductVersion, InternalName,ProductName ,CompanyName, LegalCopyright,LegalTrademarks,PrivateBuild,SpecialBuild,Comments,FileDescription,OriginalFilename from *.* " -i:fs >fileSearch.txt

"---------------------------------hualidefengexian---------------------logparser -h -i:textline" 

Input format: TEXTLINE (TextLine Format)
Parses entire lines out of generic text files

FROM syntax:

 <filename> [, <filename> ...] |
 http://<url> |
 STDIN
 Path to generic text file(s)

Parameters:

 -iCodepage      <codepage ID>     : Input codepage (0=system codepage,
                                     -1=UNICODE) [default value=0]
 -recurse        <level>           : Max subdirectory recursion level (0=no
                                     recurse, -1=all levels) [default
                                     value=0]
 -splitLongLines ON|OFF            : Split lines when longer than maximum
                                     allowed [default value=OFF]
 -iCheckpoint    <checkpoint file> : Save checkpoint information to this file
                                     [default value=no checkpoint]

Fields:

  LogFilename (S)             Index (I)             Text (S)             

"---------------------------------hualidefengexian---------------------logparser -h -i:w3c" 

Input format: W3C (Generic W3C Log Format)
Parses files compliant with the W3C Extended Format File specification

FROM syntax:

 <filename> [, <filename> ...] |
 http://<url> |
 STDIN
 Path(s) to W3C file(s)

Parameters:

 -iCodepage <codepage ID>     : Input codepage (0=system codepage,
                                -1=UNICODE) [default value=0]
 -dtLines   <number of lines> : Read this amount of lines to detect field
                                types at runtime [default value=10]
 -dQuotes   ON|OFF            : Double-quoted strings [default value=OFF]
 -separator <any character>   : Separator character between fields;
                                '<character>', 'space', 'tab' or 'auto'
                                [default value='auto']

Fields:

 Field names and types are retrieved at runtime from the specified input file(s) 

"---------------------------------hualidefengexian----------------------h GRAMMAR" 

SQL Language Grammar
-------------------------------------------------------------------------------

<query>               -> <select_clause> [ <using_clause>] [ <into_clause> ]

                         <from_clause> [ <where_clause> ] [ <group_by_clause> ]

                         [ <having_clause> ] [ <order_by_clause> ]



<select_clause>       -> SELECT [ TOP <integer> ] [ DISTINCT | ALL ]

                           <selection_list>

<selection_list>      -> <selection_list_el> [ , <selection_list> ]

<selection_list_el>   -> <field_expr> [ AS <alias> ] |

                         *



<using_clause>        -> USING <selection_list>



<into_clause>         -> INTO <into_entity>



<from_clause>         -> FROM <from_entity>



<where_clause>        -> WHERE <expression>

<expression>          -> <term1> [ OR <expression> ]

<term1>               -> <term2> [ AND <term1> ]

<term2>               -> <field_expr> <rel_op> <field_expr>                 |

                         <field_expr> [ NOT ] LIKE <like_value>             |

                         <field_expr> [ NOT ] BETWEEN <field_expr> AND

                              <field_expr>                                  |

                         <field_expr> <unary_op>                            |

                         <field_expr> <incl_op> <content>                   |

                         <field_expr> <rel_op> [ALL|ANY] <content>          |

                         ( <field_expr_list> ) <incl_op> <content>          |

                         ( <field_expr_list> ) <rel_op> [ALL|ANY] <content> |

                         NOT <term2>                                        |

                         ( <expression> )

<content>             -> ( <value_list> ) |

                         ( <query> )



<group_by_clause>     -> GROUP BY <field_expr_list> [ WITH ROLLUP ]



<having_clause>       -> HAVING <expression>



<order_by_clause>     -> ORDER BY <field_expr_list> [ ASC | DESC ] |

                         ORDER BY * [ ASC | DESC ]



<field_expr_list>     -> <field_expr> [ , <field_expr_list> ]

<field_expr>          -> <sqlfunction_expr>     |

                         <function_expr>        |

                         <value>                |

                         <alias>                |

                         <field>

<sqlfunction_expr>    -> <sqlfunction> ( [ DISTINCT | ALL ] <field_expr> )   |

                         <prop_sqlfunction> ( <field_expr> ) [ <on_fields> ] |

                         COUNT ( [ DISTINCT | ALL ] * )                      |

                         COUNT ( [ DISTINCT | ALL ] <field_expr_list> )      |

                         PROPCOUNT ( * ) [ <on_fields> ]                     |

                         PROPCOUNT ( <field_expr_list> ) [ <on_fields> ]

<function_expr>       -> <function> ( <field_expr_list> ) |

                         <case_statement>

<value_list>          -> <value_list_row> [ ; <value_list> ]

<value_list_row>      -> <value> [ , <value_list_row> ]

<sqlfunction>         -> SUM | AVG | MAX | MIN | GROUPING

<prop_sqlfunction>    -> PROPSUM

<on_fields>           -> ON ( <field_expr_list> )

<function>            -> ADD | BIT_AND | BIT_NOT | BIT_OR | BIT_SHL          |

                         BIT_SHR | BIT_XOR | COALESCE | COMPUTER_NAME | DIV  |

                         EXP | EXP10 | EXTRACT_EXTENSION | EXTRACT_FILENAME  |

                         EXTRACT_PATH | EXTRACT_PREFIX | EXTRACT_SUFFIX      |

                         EXTRACT_TOKEN | EXTRACT_VALUE | FLOOR               |

                         HASHMD5_FILE | HASHSEQ | HEX_TO_ASC | HEX_TO_HEX16  |

                         HEX_TO_HEX32 | HEX_TO_HEX8 | HEX_TO_INT             |

                         HEX_TO_PRINT | IN_ROW_NUMBER | INDEX_OF             |

                         INT_TO_IPV4 | IPV4_TO_INT | LAST_INDEX_OF | LOG     |

                         LOG10 | LTRIM | MOD | MUL | OUT_ROW_NUMBER          |

                         QNTFLOOR_TO_DIGIT | QNTROUND_TO_DIGIT | QUANTIZE    |

                         REPLACE_CHR | REPLACE_IF_NOT_NULL | REPLACE_STR     |

                         RESOLVE_SID | REVERSEDNS | ROT13 | ROUND | RTRIM    |

                         SEQUENCE | SQR | SQRROOT | STRCAT | STRCNT | STRLEN |

                         STRREPEAT | STRREV | SUB | SUBSTR | SYSTEM_DATE     |

                         SYSTEM_TIME | SYSTEM_TIMESTAMP | SYSTEM_UTCOFFSET   |

                         TO_DATE | TO_HEX | TO_INT | TO_LOCALTIME            |

                         TO_LOWERCASE | TO_REAL | TO_STRING | TO_TIME        |

                         TO_TIMESTAMP | TO_UPPERCASE | TO_UTCTIME | TRIM     |

                         URLESCAPE | URLUNESCAPE | WIN32_ERROR_DESCRIPTION

<case_statement>      -> CASE <field_expression> <when_statement_list>

                           [ <else_statement> ] END

<when_statement_list> -> <when_statement> [ , <when_statement_list> ]

<when_statement>      -> WHEN <field_expression> THEN <field_expression>

<else_statement>      -> ELSE <field_expression>

<value>               -> <string_value> |

                         <real>         |

                         <integer>      |

                         <timestamp>    |

                         NULL

<rel_op>              -> < | > | <> | = | <= | >=

<incl_op>             -> IN | NOT IN

<unary_op>            -> IS NULL | IS NOT NULL

<timestamp>           -> TIMESTAMP ( <string_value> , <timestamp_format> )

<timestamp_format>    -> ' *( <timestamp_separator> ) *( <timestamp_element>

                           *( <timestamp_separator> ) )'

<timestamp_element>   -> 1*4 y        |

                         1*4 M        |

                         MX | MP      |

                         1*4 d        |

                         dx | dp      |

                         1*2 h        |

                         hx | hp      |

                         1*2 m        |

                         mx | mp      |

                         1*2 s        |

                         sx | sp      |

                         1*2 l        |

                         lx | lp      |

                         1*2 n        |

                         nx | np      |

                         tt

<timestamp_separator> -> <any_char_except_timestamp_element> |

                         '?'

<field>              -> '[' <field_name> ']'    |

                         <field_name>

<like_value>          -> ' *( <any_char> | % | _ ) '

<string_value>        -> ' *( <any_char> ) '

<comment>             -> '/*' <text> '*/'    |

                         '//' <text> CRLF



"---------------------------------hualidefengexian-----------------------h FUNCTIONS" 

Functions Syntax
-------------------------------------------------------------------------------

ADD( addend1 <any type>, addend2 <any type> )

BIT_AND( arg1 <INTEGER>, arg2 <INTEGER> )

BIT_NOT( arg <INTEGER> )

BIT_OR( arg1 <INTEGER>, arg2 <INTEGER> )

BIT_SHL( arg1 <INTEGER>, arg2 <INTEGER> )

BIT_SHR( arg1 <INTEGER>, arg2 <INTEGER> )

BIT_XOR( arg1 <INTEGER>, arg2 <INTEGER> )

CASE <field_expression>

  WHEN <field_expression> THEN <field_expression>

  [ ... ]

  [ ELSE <field_expression> ]

 END

COALESCE( arg1 <any type>, arg2 <any type> [, ....] )

COMPUTER_NAME()

DIV( dividend <INTEGER | REAL>, divisor <INTEGER | REAL> )

EXP( argument <INTEGER | REAL> )

EXP10( argument <INTEGER | REAL> )

EXTRACT_EXTENSION( filepath <STRING> )

EXTRACT_FILENAME( filepath <STRING> )

EXTRACT_PATH( filepath <STRING> )

EXTRACT_PREFIX( argument <STRING>, index <INTEGER>, separator <STRING> )

EXTRACT_SUFFIX( argument <STRING>, index <INTEGER>, separator <STRING> )

EXTRACT_TOKEN( argument <STRING>, index <INTEGER> [ , separator <STRING> ] )

EXTRACT_VALUE( argument <STRING>, key <STRING> [ , separator <STRING> ] )

FLOOR( argument <REAL> )

HASHMD5_FILE( filePath <STRING> )

HASHSEQ( value <STRING> )

HEX_TO_ASC( hexString <STRING> )

HEX_TO_HEX16( hexString <STRING> [ , bigEndian <INTEGER> ] )

HEX_TO_HEX32( hexString <STRING> [ , bigEndian <INTEGER> ] )

HEX_TO_HEX8( hexString <STRING> )

HEX_TO_INT( hexString <STRING> )

HEX_TO_PRINT( hexString <STRING> )

IN_ROW_NUMBER()

INDEX_OF( string <STRING>, searchStr <STRING> )

INT_TO_IPV4( ipV4Address <INTEGER> )

IPV4_TO_INT( ipV4Address <STRING> )

LAST_INDEX_OF( string <STRING>, searchStr <STRING> )

LOG( argument <INTEGER | REAL> )

LOG10( argument <INTEGER | REAL> )

LTRIM( string <STRING> )

MOD( dividend <INTEGER | REAL>, divisor <INTEGER | REAL> )

MUL( multiplicand <INTEGER | REAL>, multiplier <INTEGER | REAL> )

OUT_ROW_NUMBER()

QNTFLOOR_TO_DIGIT( value <INTEGER>, digits <INTEGER> )

QNTROUND_TO_DIGIT( value <INTEGER>, digits <INTEGER> )

QUANTIZE( argument <INTEGER | REAL | TIMESTAMP>, 

          quantization <INTEGER | REAL> )

REPLACE_CHR( string <STRING>, searchCharacters <STRING>, 

             replaceString <STRING> )

REPLACE_IF_NOT_NULL( argument <any type>, replaceValue <any type> )

REPLACE_STR( string <STRING>, searchString <STRING>, replaceString <STRING> )

RESOLVE_SID( sid <STRING> [ , computerName <STRING> ] )

REVERSEDNS( ipAddress <STRING> )

ROT13( string <STRING> )

ROUND( argument <REAL> )

RTRIM( string <STRING> )

SEQUENCE( [ startValue <INTEGER> ] )

SQR( argument <INTEGER | REAL> )

SQRROOT( argument <INTEGER | REAL> )

STRCAT( string1 <STRING>, string2 <STRING> )

STRCNT( string <STRING>, token <STRING> )

STRLEN( string <STRING> )

STRREPEAT( string <STRING>, count <INTEGER> )

STRREV( string <STRING> )

SUB( minuend <any type>, subtrahend <any type> )

SUBSTR( string <STRING>, start <INTEGER> [ , length <INTEGER> ])

SYSTEM_DATE()

SYSTEM_TIME()

SYSTEM_TIMESTAMP()

SYSTEM_UTCOFFSET()

TO_DATE( timestamp <TIMESTAMP> )

TO_HEX( argument <INTEGER | STRING> )

TO_INT( argument <any type> )

TO_LOCALTIME( timestamp <TIMESTAMP> )

TO_LOWERCASE( string <STRING> )

TO_REAL( argument <any type> )

TO_STRING( argument <INTEGER | REAL> ) |

         ( timestamp <TIMESTAMP>, format <STRING> )

TO_TIME( timestamp <TIMESTAMP> )

TO_TIMESTAMP( dateTime1 <TIMESTAMP>, dateTime2 <TIMESTAMP> ) |

            ( string <STRING>, format <STRING> )

            ( seconds <INTEGER | REAL> )

TO_UPPERCASE( string <STRING> )

TO_UTCTIME( timestamp <TIMESTAMP> )

TRIM( string <STRING> )

URLESCAPE( url <STRING> [ , codepage <INTEGER> ] )

URLUNESCAPE( url <STRING> [ , codepage <INTEGER> ] )

WIN32_ERROR_DESCRIPTION( win32ErrorCode <INTEGER> )


Input format: REG (Registry properties)
Returns properties of registry keys and values

FROM syntax:

 [\\<server_name>]\[<root_name>[\<subkey_path>]] [, ...]
 Path(s) of registry keys to enumerate; <root_name> is one of: HKCR, HKCU,
 HKLM, HKU, HKCC

Parameters:

 -recurse      <level>       : Max subkey recursion level (0=no recurse,
                               -1=all levels) [default value=-1]
 -multiSZSep   <any string>  : Separator between values of MULTI_SZ types
                               [default value='|']
 -binaryFormat ASC|PRINT|HEX : Format of binary fields [default value=ASC]

Fields:

  ComputerName (S)     Path (S)      KeyName (S)           ValueName (S)
  ValueType (S)        Value (S)     LastWriteTime (T)     

Examples:

 Load a portion of the registry into a SQL table:

     LogParser "SELECT * INTO MyTable FROM \HKLM" -i:REG -o:SQL
     -server:MyServer -database:MyDatabase -driver:"SQL Server"
     -username:TestSQLUser -password:TestSQLPassword -createTable:ON

 Display the distribution of registry value types:

     LogParser "SELECT ValueType, COUNT(*) INTO DATAGRID FROM \HKLM GROUP BY
     ValueType"

"--------------------------------------hualidefengexian--------------------etw" 

Input format: ETW (Event Tracing for Windows)
Parses ETW binary logs or live trace sessions

FROM syntax:

 <session_name> | <filename_list>
 Name of an ETW tracing session or comma-separated list of .etl filenames

Parameters:

 -providers        <file>|<names>           : Path to a file containing a
                                              list of provider names or
                                              GUIDs, or comma-separated list
                                              of provider names or GUIDs
                                              [default value=not specified]
 -dtEventsLog      <number of events>       : Read this amount of events from
                                              trace logs to detect providers
                                              at runtime [default value=3000]
 -dtEventsLive     <number of events>       : Read this amount of events from
                                              live sessions to detect
                                              providers at runtime [default
                                              value=20]
 -flushPeriod      <flush period>           : Number of milliseconds between
                                              trace flushes (-1=no flush)
                                              [default value=500]
 -ignoreEventTrace ON|OFF                   : Ignore EventTrace events
                                              [default value=ON]
 -fMode            Full|Compact|FNames|Meta : Field mode (Full=fields for
                                              each property; Compact=single
                                              field for all properties;
                                              FNames=Compact plus field
                                              names; Meta=dump event
                                              metainformation only) [default
                                              value=FNames]
 -compactModeSep   <any string>             : Separator of property values in
                                              'compact' or 'fnames' field
                                              modes; '<separator>', 'space',
                                              or 'tab' [default value='|']
 -expandEnums      ON|OFF                   : Expand enumeration fields
                                              [default value=ON]
 -ignoreLostEvents ON|OFF                   : Ignore lost events [default
                                              value=ON]
 -schemaServer     <computer name>          : Name of computer with event
                                              schema information (if not
                                              specified, will use the
                                              computer on which the
                                              from-entity is located)
                                              [default value=not specified]

Fields:

  EventNumber (I)    EventName (S)    EventTypeName (S)    Timestamp (T)
  UserData (S)       

Examples:

 Create a CSV file containing basic information from an ETW .etl binary log
  file:

     LogParser "SELECT * INTO Trace.csv FROM myFile.etl"

 List from an ETW trace all the filepaths accessed by IIS:

     LogParser "SELECT FileName, COUNT(*) AS Total INTO filenames.w3c FROM
     iistrace.etl GROUP BY FileName ORDER BY Total DESC" -providers:"IIS: WWW
     Server" -fmode:full -o:W3C -encodeDelim:ON

 Show all the IIS events from an ETW trace, grouped together by request:

     LogParser "SELECT EventName, EventTypeName, Timestamp,
     HASHSEQ(ContextId) AS Id INTO DATAGRID FROM iistrace.etl WHERE ContextId
     IS NOT NULL ORDER BY Id, Timestamp ASC" -providers:myfile.guid
     -fmode:full

"--------------------------------------hualidefengexian--------------------evt" 

Input format: EVT (Windows Event Log)
Parses the Windows Event Log

FROM syntax:

 <EventLog> [, <EventLog> ...]
 <EventLog> = [\\<machinename>\]<Name> | <.evt filename>
 <Name> can be a standard EventLog (e.g.'System', 'Application', 'Security')
 or a custom EventLog

Parameters:

 -fullText      ON|OFF            : Retrieve the full text message [default
                                    value=ON]
 -resolveSIDs   ON|OFF            : Resolve SID's into full account names
                                    [default value=OFF]
 -formatMsg     ON|OFF            : Format the message as a single line
                                    [default value=ON]
 -msgErrorMode  NULL|ERROR|MSG    : Behavior when event messages or event
                                    category names cannot be resolved (NULL:
                                    return NULL value for Message and/or
                                    EventCategoryName field; ERROR: return a
                                    parse error; MSG: return an error message
                                    for Message and/or EventCategoryName
                                    field) [default value=MSG]
 -fullEventCode ON|OFF            : Return the full event code instead of the
                                    friendly event code [default value=OFF]
 -direction     FW|BW             : Events read direction [default value=FW]
 -stringsSep    <any string>      : Separator between values of the Strings
                                    field [default value='|']
 -iCheckpoint   <checkpoint file> : Save checkpoint information to this file
                                    [default value=no checkpoint]
 -binaryFormat  ASC|PRINT|HEX     : Format of binary fields [default
                                    value=HEX]

Fields:

  EventLog (S)              RecordNumber (I)          TimeGenerated (T)    
  TimeWritten (T)           EventID (I)               EventType (I)        
  EventTypeName (S)         EventCategory (I)         EventCategoryName (S)
  SourceName (S)            Strings (S)               ComputerName (S)     
  SID (S)                   Message (S)               Data (S)             

Examples:

 Create an XML report file containing logon account names and dates from the
  Security Event Log messages:

     LogParser "SELECT TimeGenerated AS LogonDate, EXTRACT_TOKEN(Strings, 0,
     '|') AS Account INTO Report.xml FROM Security WHERE EventID NOT IN
     (541;542;543) AND EventType = 8 AND EventCategory = 2"

 Get the distribution of EventID values for each Source:

     LogParser "SELECT SourceName, EventID, MUL(PROPCOUNT(*) ON (SourceName),
     100.0) AS Percent FROM System GROUP BY SourceName, EventID ORDER BY
     SourceName, Percent DESC"

 Create TSV files containing Event Messages for each Source in the
  Application Event Log:

     LogParser "SELECT SourceName, Message INTO myFile_*.tsv FROM
     \\MYSERVER1\Application, \\MYSERVER2\Application"