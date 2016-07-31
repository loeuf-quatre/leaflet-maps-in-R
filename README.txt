Copyright (c) 1994 - 2014, Teradata Corporation.
ALL RIGHTS RESERVED 


                              Teradata ODBC Driver 
                                      for 
                                    Windows


Product: ODBC Driver For Teradata
Version: 15.00.00.03

Product Dependencies:
    1. Shared common components for Internationalization for Teradata (tdicu)
    2. Teradata GSS client package                                    (TeraGSS)


Contents
    1.0 Introduction
    1.0.1 Customer Impact list
    2.0 Release Content
    3.0 Requirements
    4.0 ODBC Software Installation/Uninstallation
    5.0 Configuration
    6.0 Sample ODBC Applications
    7.0 Restrictions
    8.0 Limitations/Issues
    9.0 Documentation
    10.0 Support
    Appendix A - New ODBC 13.0 directory tree structure
    Appendix B - DSN Migration 
    Appendix C - New Post TTU 13.10 GCA Features
             C.1.1 - New Teradata specific descriptor fields
             C.1.2 - Server Character Set and export Width
             C.2 - New privileges
             C.3 - NoPI tables



1.0 Introduction

    This release of the ODBC driver for Teradata will support Teradata 15.00
    features in both Teradata and (Entry Level) ANSI modes, and is backwards
    compatible with previous releases of Teradata as noted in section 3.2

    The ODBC Drivers for Windows has been built using the Visual Studio 2012 
    VC++ Version 11.0.

    The ODBC 13.0 release has introduced a new ODBC directory tree structure where
    the ODBC Driver libraries are no longer placed in the system32 directory.  The
    new ODBC tree structure can be seen in Appendix B. The ODBC 13.0 installation 
    package has taken steps to make this new tree structure transparent to the 
    existing DSNs that have been created by updating the Driver Attribute in the 
    DSN to the new location of the tdata32.dll library.

1.0.1 Customer Impact List

  1)
    If using the Microsoft cursor library the SQLGetInfo call with the InfoType SQL_KEYWORDS will return a 
    truncated keyword list or SQL_ERROR depending on the buffer size when the database is 14.10. This is due 
    to a limitation in the cursor library. Microsoft recommends that the cursor library is not used because 
    it will be removed in a future version of Windows. Use of the cursor library is controlled by a connection 
    attribute set prior to connection. The default is to not use the cursor library. Please refer to 
    http://msdn.microsoft.com/en-us/library/ms713981(VS.85).aspx for more information about the cursor library. 


  2)
    DR 122470 tracks a problem that has been reported to Microsoft and there are 
    certain steps to take to work around this problem.

    DR 122470 -- 3P: W2003 client machine must be rebooted for upgrades to work; Microsoft incident SRZ080531000020 

    PROBLEM/ISSUE
    ============

    Environment Variable PATH does not get updated after installing a Custom application on 
    Windows Server 2003

    Workaround
    ==========

    1. mstsc - open session in console mode
        When installing Teradata Tools and Utilities software on a Windows Server 2003 system 
        using a terminal session opened with "mstsc", open the session in console 
        mode: "mstsc /console". Otherwise, the Windows Server 2003 system will have to be 
        rebooted after installing the software.
    2. logoff followed by logging on
    3. Enter the Environment variables dialog box and touch one of the variables and select OK.



2.0 Release Content

    2.1 New Features included in this release
        a) New Features included in TTU 15.00 GCA release (see ODBC User Guide for description)

    ODBC-188   -- Support client side default security mechanism
    ODBC-3531  -- The Teradata ODBC Driver now supports leap seconds
    ODBC-4604  -- Support for 64-bit row count (activity count)
    ODBC-11065 -- Support for "small" large objects (SLOBs) has been implemented
    ODBC-11067 -- Support for JSON (JavaScript Object Notation) integration has been implemented
    ODBC-11069 -- Support for 3D Geospatial data types, including supportive analytic methods and functions has been implemented
    ODBC-11071 -- Support for SQL-H enhancements has been implemented
    ODBC-11072 -- Support for Aster analytical functions has been implemented
    ODBC-11075 -- Extended type information for UDT columns in a result set is now returned
    ODBC-11239 -- The Teradata ODBC Driver is no longer supported on Windows XP, RedHat Linux 4.x, Oracle Enterprise Linux 4.x, and Solaris 9
    ODBC-11360 -- Use of the common text field for password and Wallet String in the runtime connect dialog has been implemented
    ODBC-11381 -- Support for ANSI SQL Temporal syntax has been implemented
    ODBC-12031 -- The latest Teradata ODBC Driver 13.0, 13.10, 14.0, and 14.10 eFixes have been certification tested with DBS 15.0
    ODBC-12032 -- The Teradata ODBC Driver has been certification tested with the latest DBS 13.0, 13.10, 14.0, 14.10, and 15.0 versions
    ODBC-12033 -- The Teradata ODBC Driver is now supported on Windows Server 2012
    ODBC-12034 -- The Teradata ODBC Driver is now supported on Windows 8.1

    ODBC 15.00.00.03
    ================
    ODBC-3531  -- Support leap seconds



    2.2 ODBC Driver Problems fixed in this release

    ODBC 15.00.00.03
    ================
    
    ODBC-235     -- Fix to return proper SQLState for conversions from SQL_C_TYPE_DATE to SQL_CHAR when invalid date literal is used
    ODBC-538     -- Fix to return proper SQLStates for conversions SQL_C_TYPE_DATE to SQL_TYPE_TIMESTAMP/SQL_TYPE_DATE
    ODBC-796     -- Fix covnversions from SQL_C_UTINYINT and SQL_C_UTINYINT to Single Field Interval Types
    ODBC-809     -- Fix catalog functions to return proper errors when Schema, Table or Column Name = NULL and NameLength > 1
    ODBC-832     -- Fix NULL value conversions from SQL_C_SBIGINT & SQL_C_UBIGINT to Numeric Destinations
    ODBC-881     -- Fix error code returned with conversions from C_UnsignedNumeric Type to Single Field Interval Types
    ODBC-1419    -- Rank the diagnostic messages as per the ODBC specification
    ODBC-1528    -- Fix SQLExecDirect/SQLExecute connection reset issue with conversion of parameters to SQL LOB types
    ODBC-1569    -- Fix SQLGetDescField to return correct SQL_DESC_CONCISE_TYPE values for Date, Time and TimeStamp
    ODBC-1598    -- Fix conversions from SQL_C_S/TINYINT/SQL_C_S/SHORT/SQL_C_S/LONG to Single Field Interval Types
    ODBC-1717    -- Fix NULLABLE column of catalog functions SQLColumns, SQLTables and SQLGetTypeInfo
    ODBC-3049    -- Fix inconsistent results of SQLTables
    ODBC-3117    -- Fix to insert NULL values for SQL_DECIMAL/SQL_NUMERIC parameters.
    ODBC-3127    -- Fix SQLGetConnectAttr to return correct sqlstate for SQL_ATTR_SIMULATE_CURSOR
    ODBC-3176    -- Fix invalid precision descriptor consistency check for floating point types
    ODBC-3180    -- Fix retrieval of fractional values of Interval types
    ODBC-3260    -- Fix SQLGetDescField attributes SQL_DESC_PARAMETER_TYPE and SQL_DESC_UNNAMED
    ODBC-3324    -- Fix insertion of character and binary (non-LOB) data in parts using SQLPutData
    ODBC-3536    -- driver now accepts connection-string values enclosed in brackets {}
                    Note : value enclosed in {} cannot have } character.
    ODBC-4732    -- Fix incorrect statement state after SQLFetch in ODBC 2
    ODBC-4811    -- Fix SQLGetStmtAttr, when invalid attribute with NULL buffer and BufferLength < 1
    ODBC-4969    -- Fix conversions of values > 2147483647 to SQL_C_ULONG buffers
    ODBC-4999    -- Fix SQLGetDiagField for diagnostic fields SQL_DIAG_DYNAMIC_FUNCTION and SQL_DIAG_DYNAMIC_FUNCTION_CODE
    ODBC-6577    -- Fix insertion of parameter arrays with multi insert.
    ODBC-6668    -- Fix SQLColumns to return correct precision for INTERVAL columns
    ODBC-7877    -- Fix conversions from SQL_INTERVAL_SECOND to SQL C Integer Types
    ODBC-7973    -- Fix SQLState for conversions from SQL_C_CHAR to SQL_TYPE_TIMESTAMP for invalid strings.
    ODBC-9951    -- Fix retrieval of valid numeric literals to DECIMAL.
    ODBC-11306   -- The syntax error 3704 for a request with an unmatched quoted string and LF character has been resolved
    ODBC-11378   -- MS Access query using date parameter fails
    ODBC-11469   -- Fix conversions from SQL_C_UBIGINT to SQL_FLOAT with values > max BIGINT
    ODBC-11681   -- The ODBC Driver for Teradata now transforms the IN-list when the list contains ODBC escapes
    ODBC-12273   -- Fix code dump issue, when parameter array insert of bound to LOB columns
    ODBC-12548   -- Fix conversions from SQL CHAR to C INTERVALs, when the source column has trailing spaces
    ODBC-12647   -- Fix SQLExecute, when the last value in array of parameters is NULL
    ODBC-12999   -- Fix retrieval of fractional seconds in INTERVAL_DAY_TO_SECONDS and INTERVAL_MINUTE_TO_SECONDS
    ODBC-12963   -- SQLStatistics returns SQL_TABLE_STAT row with CARDINALITY and PAGES as NULL for non-updatable views

    ODBC 15.00.00.02
    ================

    ODBC-12273 -- Parameter array insert of bound LOB columns core dumps
    ODBC-7809  -- SQLTables TableType filter doesn't always work
    ODBC-11554 -- ODBC DSN setup defaults to ASCII if session-character-set name is very long

    ODBC 15.00.00.01
    ================

    ODBC-12284 -- ODBC JSON datatype returns -1 with SQLColAttributes when using attributes SQL_COLUMN_EXPORT_WIDTH and SQL_COLUMN_EXPORT_WIDTH_ADJ    
    ODBC-12405 -- The parser now preserves the ON COMMIT clause when creating volatile and global temporary tables

    ODBC 15.00.00.00
    ================

    ODBC-860   -- Some keyboard shortcuts in the Teradata ODBC Driver dialogs are now functioning properly
    ODBC-861   -- The SQLProcedureColumns() function no longer returns an incorrect timestamp precision
    ODBC-897   -- A memory leak in the SQLConnect() function has been resolved
    ODBC-3343  -- The Diag_Map_SqlState() function is now mapped correctly for HY009 errors
    ODBC-6203  -- The Teradata ODBC Driver now ignores the Catalog NameLength when the CatalogName=NULL
    ODBC-6487  -- Users can now specify fractional seconds in parameters bound to an SQL_INTERVAL_STRUCT
    ODBC-9789  -- The SQLSpecialColumns() function no longer returns an "HY092 - Invalid attribute" error on UNIX and Linux platforms
    ODBC-9921  -- When the RetryOnEINTR connection option is on, SQLExecDirect now returns if it is interrupted by an alarm
    ODBC-10305 -- When connecting with UTF-16, there is no longer a difference in behavior when using APIs SQLDescribeParam and SQLDescribeCol for date columns
    ODBC-10897 -- The SQLStatistics API is fixed for the cases when SchemaName and TableName returns a NULL, blank, or invalid name
    ODBC-11035 -- The “Message too long” socket error that is returned during SQLDisconnect has been resolved
    ODBC-11306 -- The syntax error 3704 for a request with an unmatched quoted string and LF character has been resolved
    ODBC-11546 -- SQLGetDiagField with DiagIdentifier "SQL_DIAG_ROW_COUNT" now provides row count for statements following the first one in a multi-statement request
    ODBC-11559 -- The “Online Help initialization failed” error on Japanese Windows clients has been resolved
    ODBC-11681 -- The Teradata ODBC Driver now transforms the IN-list when the list contains ODBC escapes


    * This release also includes all features, improvements and bug fixes committed up to Release 14.10.00.02
    ODBC 14.10.00.02
    ================

    ODBC-860     -- Some keyboard shortcuts in the ODBC dialogs no longer work
    ODBC-10897   -- SQLStatistics returns incorrect output when TableName is NULL or a space or an invalid name
    ODBC-11360   -- Updates to connect dialog
    ODBC-11521   -- ODBC Driver not recognizing Security Policy coming from database



    2.3 Driver Manager Problems fixed in this release

        None.



3.0 Requirements

    3.1 System

            Microsoft Windows 2003(including R2)
            Microsoft Windows Vista Enterprise
            Microsoft Windows Server 2008
            Microsoft Windows Server 2012
            Microsoft Windows 7
            Microsoft Windows 8.x

    3.2 Supported Teradata Database versions

            Teradata Database 13.0
            Teradata Database 13.10
            Teradata Database 14.0
            Teradata Database 14.10
            Teradata Database 15.00

    3.3 ODBC Dependencies

        1. Shared common components for Internationalization for Teradata (tdicu)
        2. Teradata GSS client package                                    (TeraGSS)

    3.4 ODBC Application development

        When developing an ODBC C++ application, the developer is encouraged to
        use the recommended compiler Visual Studio 2012 VC++ Version 11.0.

        The ODBC 13.0 release has deploy tdsql.h which contains definitions to
        Teradata specific attributes for the ODBC connection, statement and 
        descriptor objects. These are extensions to the ODBC standard.  The
        tdsql.h file can be found in the install directory of the
        ODBC Driver for Teradata.  For example,

             C:\Program Files\Teradata\Client\13.0\ODBC Driver for Teradata


4.0 ODBC Software Installation/Uninstallation

    Starting the ODBC 13.10 release, co-existence of both the 32-bit and 64-bit ODBC Drivers
    on a single Windows system is supported.  Each ODBC Driver has its own installation package
    so two installations would be required to get both drivers onto the system.


    4.1 ODBC Dependencies

        Prior to installing the "ODBC Driver for Teradata", the TTU dependency products
        that are listed in section 3.3 must be installed first and in the order listed
        below.
            TTU Products
            ============
            1. Shared common components for Internationalization for Teradata (tdicu)
            2. Teradata GSS client package                                    (TeraGSS)

        On 64-bit versions of Windows, it is recommended that the user installs both the 32-bit
        and 64-bit versions of the ODBC Driver and its Dependencies.


    4.2 Installation

        Before installing the "ODBC Driver for Teradata", it is recommended that
        all applications are closed, including Microsoft Office, and to disable 
        any virus protection software running on the system. If any of the 
        required system DLLs are used by a running applications, the installation 
        of the Teradata ODBC Driver may fail.

        The ODBC install package is available on the TTU GCA CD-ROM or can be 
        downloaded from the Patch Server or the Teradata Down Load Center when 
        an EFIX is required.  

        There are two types of installation format.  The one found on the TTU GCA 
        CD-ROM which is a installation directory containing all the InstallShield files 
        with a setup.exe program to be execute. And the other format that is downloaded
        is a single zip file contaning the contents of the installation directory.
        Once the zip file has been unzipped, then the setup.exe program can be executed.

        Note: If the installation shows any errors, open the Setup.log file that
              is created in the directory to find out what the errors are.  The 
              User Guide will provide you more information about the instructions,
              possible errors and their descriptions and solutions.

        4.2.1 TTU CD-ROM

              When installing from the TTU CD-ROM, the user will be guided 
              through the installation for the "ODBC Driver for Teradata".  Just 
              follow the guidelines on the screen and it will automatically 
              determine the type of system hardware (32-Bit or 64-Bit) and install 
              the approriate version of the driver.

        4.2.2 Install (Setup of ODBC Driver for Teradata)
	      

	      Dialog Windows 
	      ==============

	      - Welcome to the InstallShield Wizard for ODBC Driver for Teradata
	      - License Agreement
              - Choose Destination Location
                   Default: "C:\Program Files\Teradata\Client\15.00\ODBC Driver for Teradata"
              - Start Copying Files
              - InstallShield Wizard Complete
                   Yes, I want to view the ReadMe file
                   Yes, I want to run the ODBC Administrator now	 	

        
        4.2.3 Custom Install (Setup of ODBC Driver for Teradata)

	4.2.4 Silent Install


              4.2.4.1 Customizing an InstallShield Response file
                        N/A

              4.2.4.2 Invoking a Silent Install
		
              	From a DOS window, the user will change directories to where the
                              "ODBC Driver for Teradata.msi" is located and use any of the below 
              	commands to silently install the ODBC Driver for Teradata Package.
                   
              	In case of New Install or Upgrading to a Major Release

              	Default Directory:
	
                     setup /S /v"/qn"
                           (or)		
                     msiexec /i "ODBC Driver for Teradata.msi" /qn

              (Default: "<Drive>:\Program Files\Teradata\Client\15.00\ODBC Driver for Teradata")
              (Note: "ODBC Driver for Teradata nt-x8664.msi" is for the 64-bit Driver)

              	User Specified Directory:

                     setup /S /v"/qn INSTALLDIR=<user specified directory>"
                              (or)   
                     msiexec /i "ODBC Driver for Teradata.msi" /qn INSTALLDIR=<user specified directory>


                In case of upgrade to an E-fix Release, please use following commands.
	
                     setup /S /v"/qn"
                            or		
                     msiexec /i "ODBC Driver for Teradata.msi" /qn REINSTALL=ALL REINSTALLMODE=voums

              (Note: "ODBC Driver for Teradata nt-x8664.msi" is for the 64-bit Driver)
        
    4.3 Uninstallation

        To uninstall the "ODBC Driver for Teradata" the user needs to use the
        Add/Remove Programs panel
                Start->Settings->Control Panel->Add/Remove Programs 
        and 
                select the "ODBC Driver for Teradata package"
        for removal.

    4.4 Upgrades

    4.5 List Software Product

        The following steps are used to display information about the 
        "ODBC Driver for Teradata" installed.   The user needs to use the
        Add/Remove Programs panel
                Start->Settings->Control Panel->Add/Remove Programs 
        and 
                select the "ODBC Driver for Teradata package"


5.0 Configuration

    For first time users, one or more DSNs will need to be configured.  This is 
    done through the ODBC Administrator. There is an icon for the ODBC 
    Administrator in the ODBC driver's program folder.

    5.1 ODBC and ODBCINST INI Configuration

    5.2 DSN Configuration

        After starting the ODBC Administrator, click on the Add... button to 
        configure a new data source.  This will bring up a dialog with the names 
        of all installed ODBC drivers. Select Teradata and then continue. 
        (You may use the Help button to get information on how the various fields 
         are used.)

        Note: Your TCP/IP must be functional through Microsoft Windows(r) in 
              order to fully configure data sources.


        5.2.1 ODBC Driver Setup for Teradata Database (Dialog Box)

        Fill in the information in the first ODBC Teradata Driver Setup dialog 
        box.

        1. A data source name is mapped to a combination of a Teradata system
           running Teradata Database such as (but not limited to) a 34xx, 35xx, 
           3600, 43xx, 44xx, 47xx, 51xx and so on, and a default database within 
           that system.

        2. One can optionally configure multiple data source entries for the same 
           Teradata system to associate different user logons, default databases, 
           or options. Data Source

        3. Description is an optional comment field.

        4. Enter the alias name or IP address of the Teradata Server you will be
           using. If you are using the alias name of a server, you must either be 
           using Domain Name Services, or a local hosts file to resolve the named
           alias. Setup will automatically add "COPn" (where n is a number) to 
           the name in order to search for other host IP addresses with similar 
           aliases, following the standard Teradata naming conventions. Multiple 
           server names may be entered by putting entries on separate lines.

        5. The ODBC Driver Setup for Teradata Database dialog contains a new 
           check box in the Teradata Server Info section. If this box is checked,
           the alias names entered will not be resolved to IP addresses at setup 
           time. Instead the driver will resolve the names at connect time. 
           Entering user information is optional.

        6. IMPORTANT: Note that although the password is encrypted, storing a 
           password is never 100% secure.


        5.2.2 Teradata ODBC Driver Options (Dialog Box)

        The first optional dialog box which contains options that are most useful. 


        5.2.3 Teradata ODBC Driver Advanced Options (Dialog Box)

        The second optional dialog box, Advanced, contains options that are very 
        risky to change; if you do not understand the effects of changing these 
        default settings, you should not make any changes.


6.0 Sample ODBC Applications


7.0 Restrictions

    Additional restrictions are listed in the ODBC Driver for Teradata User Guide

    7.1 ODBC restrictions

    7.2 Binary Compatibilites


8.0 Limitations/Issues
	
    8.1 DSN migration to new TTU directory tree structure.

	In Release 13.0, All Teradata DSN Driver values are migrated to the new tdata32.dll path 
        ( Default: "C:\Program Files\Teradata\Client\13.0\ODBC Driver for Teradata\lib\tdata32.dll" )
	
	Note : All Teradata DSN Driver values prior to 13.0 will be migrated and previous
	Teradata DSN Driver values are available in %SYSTEMROOT%\temp\Teradata_DSN_Migration_[type]_[timestamp].txt
	
	For restoration of previous values , use tdodbcdsn.vbs file located at
	( Default: "C:\Program Files\Teradata\Client\13.0\ODBC Driver for Teradata ) directory.
	with the back up file.
		
	USAGE :	cscript tdodbcdsn.vbs %SYSTEMROOT%\temp\Teradata_DSN_Migration_[type]_[timestamp].txt
		
	On 64-bit machine, User DSN's will be migrated based on location of tdata32.dll 
	i.e system32 or syswow64 when a 64bit or 32bit ODBC Driver for Teradata Package
	is installed respectively.

	New User DSN's will not be migrated with the tdodbcdsn.vbs script.
	
    8.2 Japanese Online Help

        In place of the Japanese Online Help, the ODBC 14.0 GCA release will deploy the English version.
        When the Japanese Online Help becomes available, it will be included in a subsequent ODBC 14.0.0.x
        EFIX.

	

9.0 Documentation

    1) ODBC Driver for Teradata User Guide
          (Release 15.00.00 Under Construction)

    2) www.info.teradata.com


10.0 Support

    10.1 Contact Information

         Customers with support contracts may report problems with the ODBC Driver to
         Teradata Customer Services either by phone via the regional support center or
         on the web via Teradata@YourSevices
             www.teradata.com

         Contact your Teradata Customer Services Representative for details.


Appendix A - New ODBC 15.00 directory tree structure

    The new ODBC Driver tree structure is shown in the diagram below.

        C:\Program Files\Teradata\Client\15.00\ODBC Driver for Teradata\
                                                   Bin
                                                   Help
                                                   Lib
                                                   Samples
                                                   Pbodb0x0.txt
                                                   Readme.txt
                                                   tdodbcdsn.vbs
												   tdsql.h


Appendix B - DSN Migration

    DSN Migration will be automatically done by the ODBC Driver for Teradata 
    Package.

    For explicit DSN Migration , from a DOS window, the user will change directories
    to where the tdodbcdsn.vbs is located and use the below commandline to change all
    Teradata DSN's to the present installed version.

	For 32bit Package : cscript tdodbcdsn.vbs 32bit
	For 64bit Package : cscript tdodbcdsn.vbs 64bit

    For Backward DSN Migration, to restore the previous Teradata DSN driver
    values , change directories to where the tdodbcdsn.vbs is available and use
    the below command line .
   
    Note : The below command only restores the DSN values which are available
           at the time of installation.
	
    cscript tdodbcdsn.vbs %SYSTEMROOT%\temp\Teradata_DSN_Migration_[type]_[timestamp].txt
    


Appendix c - New Post TTU 15.00 GCA Features
             ==============================


C.1.1 - New Teradata specific descriptor fields
        =======================================

    A new set of Teradata specific descriptor fields have been added to
    expose the server character and export width values for character columns.

    The descriptor fields are read-only and can be retrieved using
    SQLGetDescField.  The value will be an SQLINTEGER. The fields are only
    meaningful for character types and only when the EnableStatementInfo option
    is set or when LOB support is enabled.

    The new fields are also available through the SQLColAttribute API call. The
    function will return a numeric value in the NumericAttributePtr, which will
    be the value of the corresponding descriptor field. Note that on 64-bit
    platforms the integer value is a 64-bit integer.

    These definitions are available in the tdsql.h header file included with the
    Teradata ODBC package.

c.1.2 - Server Character Set and export Width
        =====================================

    Server Character Set
    ===================

    The server character set for a given column is available through the
    descriptor field named SQL_DESC_TD_CHARACTER_SET.

    The following values are available:

    SQL_TD_CS_UNDEFINED
    SQL_TD_CS_LATIN
    SQL_TD_CS_UNICODE
    SQL_TD_CS_KANJISJIS
    SQL_TD_CS_GRAPHIC
    SQL_TD_CS_KANJI1

    If both the EnableStatementInfo option and LOB support are disabled the
    ODBC driver will be unable to determine the server character set and the
    value will be SQL_TD_CS_UNDEFINED.


    Export Width Values
    ====================

    The ODBC driver now exposes Teradata export width and export width
    adjustment values at the descriptor level. For for each IRD and IPD there
    will be two new fields called SQL_DESC_TD_EXPORT_WIDTH and
    SQL_DESC_TD_EXPORT_WIDTH_ADJ. These fields contain the export width and
    export width adjustment values associated with the column character set
    given the session character in use.

    Note that these fields are only valid if the column character set is
    different from SQL_TD_CS_UNDEFINED. Otherwise they will have the value -1
    indicating an undefined value.

    The fields can be used to calculate the number of bytes that will be
    exported by Teradata for a given character column.

    The formula is:

       <Number of bytes > = <export width * N > + <export width adjustment>

    where N is the number of characters in the column.

    A third new descriptor field named SQL_TD_EXPORT_BYTES can be used to return
    the number of bytes exported by Teradata using the above formula. It has the
    value -1 if the value is undefined.
       
    There is a known issue with the KANJI1 (KanjiOne) character columns using an
    UTF8 session character set. The descriptor fields retrieved through
    SQLGetDescField for SQL_DESC_TD_EXPORT_WIDTH is incorrectly set to 2, where
    it should be set to 1. The issue is caused by the fact that the database
    does not return export width values for this server character set in the
    HELP SESSION command. The ODBC driver is therefore unable to support the
    correct value for both UTF-8 and UTF-16 sessions and it was decided to
    provide the correct value for UTF-16 session character set.

    It is recommended to use the SQLColAttribute function when some columns have
    the Kanji1 server character set. This will precisely return the number of
    bytes that the database exports (SQL_DESC_TD_EXPORT_BYTES) for a given
    character field in all cases.


C.2 - New privileges
      ==============

    The ODBC catalog functions SQLTablePrivileges and SQLColumnPrivileges are
    displaying various database object privileges. Support of for following new
    privileges has been added to the ODBC driver.  The list extends the existing
    list of privileges listed in the ODBC User Guide.

        Privelege name
        ---------------------------
        SHOW
        STATISTICS
        CREATE OWNER PROCEDURE
        ALTER EXTERNAL PROCEDURE
        ALTER FUNCTION
        ANY PRIVILEGE
        ALTER PROCEDURE
        CREATE EXTERNAL PROCEDURE
        CREATE FUNCTION
        CREATE ROLE
        DROP FUNCTION
        EXECUTE FUNCTION


C.3 - NoPI tables
      ===========

    Tables with no primary index ("NoPI tables") are supported in Teradata 13.0.

    NoPI tables are includes in the result set from SQLTables with the
    TABLE_TYPE "TABLE" like tables with primary index.

    A NoPI table can be recognized by the absence of a row for the primary
    index in the result set from SQLStatistics: The SQLStatistics result set for
    a NoPi table has no rows with index TYPE SQL_INDEX_CLUSTERED (value 1).


