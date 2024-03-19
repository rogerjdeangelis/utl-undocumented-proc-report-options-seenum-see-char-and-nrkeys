# utl-undocumented-proc-report-options-seenum-see-char-and-nrkeys
Undocumented proc report options seenum see char and nrkeys  
    %let pgm=utl-undocumented-proc-report-options-seenum-see-char-and-nrkeys;

    Undocumented proc report options seenum see char and nrkeys


       Undocumented  options

               1. Seenum and Seechar
               2, Nrkeys

    /*                                                        _
     ___  ___  ___ _ __  _   _ _ __ ___    ___  ___  ___  ___| |__   __ _ _ __
    / __|/ _ \/ _ \ `_ \| | | | `_ ` _ \  / __|/ _ \/ _ \/ __| `_ \ / _` | `__|
    \__ \  __/  __/ | | | |_| | | | | | | \__ \  __/  __/ (__| | | | (_| | |
    |___/\___|\___|_| |_|\__,_|_| |_| |_| |___/\___|\___|\___|_| |_|\__,_|_|

    */

    PUTLOG IN A PROC REPORT COMPUTE BLOCK SEENUM AND SEECHAR FUNCTIONS VERY USEFUL FOR DEBUGGING

    This used to put

    ods listing;
    proc report data=sashelp.class nowd;
        columns _all_;
        compute weight;
           _n_ + 1;
           rcN = seenum(_n_,          'NOTE: OBS');
           rcN = seenum(weight.sum,'      WEIGHT');
           rcC = seechar(name,'             NAME');
           endcomp;
        run;quit;
    ods listing close;

    NOTE: Multiple concurrent threads will be used to summarize data.
    3508!         quit;
    NOTE: OBS 1
          WEIGHT 112.5
                 NAME Alfred   (8)
    NOTE: OBS 2
          WEIGHT 84
                 NAME Alice    (8)
    NOTE: OBS 3
          WEIGHT 98
                 NAME Barbara  (8)
    NOTE: OBS 4
          WEIGHT 102.5

    ....

    /*          _
     _ __  _ __| | _____ _   _ ___
    | `_ \| `__| |/ / _ \ | | / __|
    | | | | |  |   <  __/ |_| \__ \
    |_| |_|_|  |_|\_\___|\__, |___/
                         |___/
    */

    Does not seem to do anything, but is accepted as an option on the proc report statement?
    The option is not echoed back using the list option in proc report.
    Maybe it is short for norowkeys?


    WITH NORKEYS
    ============

    ods listing;
    proc report data=sashelp.cars(obs=5) split='#' nowd nocenter ls=132 ps=64 norkeys list;
      column make mpg_city mpg_highway;
      define make / group ;
      define mpg_city / analysis;
      define mpg_highway / analysis;
    run;
    ods listing close;

                                                               ================
    GENERATED CODE MISSING NORKEYS NOTE ALL OTHER OPTIONS PRESENT BUT NOT NORKEYS
    ===========================================================================

    PROC REPORT DATA=SASHELP.CARS LS=132 PS=64  SPLIT="#" CENTER ; ==> MISSING NORKEYS OPTION;
    COLUMN  ( Make MPG_City MPG_Highway );

    DEFINE  Make / GROUP FORMAT= $13. WIDTH=13    SPACING=2   LEFT "Make" ;
    DEFINE  MPG_City / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "MPG (City)" ;
    DEFINE  MPG_Highway / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "MPG (Highway)" ;
    RUN;




    WITHOUTOUT NORKEYS
    ==================

    DOES NOT EVEN HAVE IT IN THE GENERATE DCODE;
    ods listing;
    proc report data=sashelp.cars(obs=5) split='#' nowd nocenter ls=132 ps=64 list;
      column make mpg_city mpg_highway;
      define make / group ;
      define mpg_city / analysis;
      define mpg_highway / analysis;
    run;
    ods listing close;


    EXACTLY THE SAME GENERATE DCODE WITH and WITHOUT NORKEYS
    ==========================================================


    PROC REPORT DATA=SASHELP.CARS LS=132 PS=64  SPLIT="#" CENTER;
    COLUMN  ( Make MPG_City MPG_Highway );

    DEFINE  Make / GROUP FORMAT= $13. WIDTH=13    SPACING=2   LEFT "Make" ;
    DEFINE  MPG_City / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "MPG (City)" ;
    DEFINE  MPG_Highway / SUM FORMAT= BEST9. WIDTH=9     SPACING=2   RIGHT "MPG (Highway)" ;
    RUN;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
