# utl-collapse-dataset-to-contain-only-distinct-levels-and-counts_wps
Nice way to communicate the metric of a table in a usedull way
    %let pgm=utl-collapse-dataset-to-contain-only-distinct-levels-and-counts_wps;

    Collapse dataset to contain only distinct levels and counts

    Nice way to communicate the metric of a table in a usedull way

    github
    http://tinyurl.com/mry6p3pw
    https://github.com/rogerjdeangelis/utl-collapse-dataset-to-contain-only-distinct-levels-and-counts_wps

    PROBLEM

    /**************************************************************************************************************************/
    /*                        |                             |                                                                 */
    /*     IMPUT              |       PROCESS               |                  OUTPUT (ADDED FREQUENCY)                       */
    /*                        |                             |                                                                 */
    /* SD1.HAVE total obs=19  |                             |                                                                 */
    /*                        |                             |                                                                 */
    /* NAME    SEX AGE HEIGHT | %array(_vs,values=          |   NAME   NAME# SEX SEX#   AGE AGE#   HEIGHT HEIGHT#             */
    /*                        |  %utl_varlist(sd1.class));  |                                                                 */
    /* Alfred   M   14  69.0  |                             |   Alfred     1  F     9   11     2    51.3     1                */
    /* Alice    F   13  56.5  |  %do_over(_vs,phrase=%str(  |   Alice      1  M    10   12     5    56.3     1                */
    /* Barbara  F   13  65.3  |   proc sql;                 |   Barbara    1        .   13     3    56.5     1                */
    /* Carol    F   14  62.8  |    create                   |   Carol      1        .   14     4    57.3     1                */
    /* Henry    M   14  63.5  |      view ? as              |   Henry      1        .   15     4    57.5     1                */
    /* James    M   12  57.3  |    select                   |   James      1        .   16     1    59       1                */
    /* Jane     F   12  59.8  |       cats(?)  as ?         |   Jane       1        .          .    59.8     1                */
    /* Janet    F   15  62.5  |      ,count(?) as ?_cnt     |   Janet      1        .          .    62.5     2                */
    /* Jeffrey  M   13  62.5  |    from                     |   Jeffrey    1        .          .    62.8     1                */
    /* John     M   12  59.0  |       sd1.have              |   John       1        .          .    63.5     1                */
    /* Joyce    F   11  51.3  |    group                    |   Joyce      1        .          .    64.3     1                */
    /* Judy     F   14  64.3  |       by ?                  |   Judy       1        .          .    64.8     1                */
    /* Louise   F   12  56.3  |  ;quit;));                  |   Louise     1        .          .    65.3     1                */
    /* Mary     F   15  66.5  |                             |   Mary       1        .          .    66.5     2                */
    /* Philip   M   16  72.0  |  data sd1.want;             |   Philip     1        .          .    67       1                */
    /* Robert   M   12  64.8  |  merge %utl_varlist         |   Robert     1        .          .    69       1                */
    /* Ronald   M   15  67.0  |       hsd1.have)            |   Ronald     1        .          .    72       1                */
    /* Thomas   M   11  57.5  |                             |   Thomas     1        .          .             .                */
    /* William  M   15  66.5  |                             |   William    1        .          .             .                */
    /*                        |                             |                                                                 */
    /**************************************************************************************************************************/


    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
       set sashelp.class;
    run;quit;


    /**************************************************************************************************************************/
    /*  _                   _                                                                                                 */
    /* (_)_ __  _ __  _   _| |_                                                                                               */
    /* | | `_ \| `_ \| | | | __|                                                                                              */
    /* | | | | | |_) | |_| | |_                                                                                               */
    /* |_|_| |_| .__/ \__,_|\__|                                                                                              */
    /*         |_|                                                                                                            */
    /*                                                                                                                        */
    /* SD1.HAVE total obs=19                                                                                                  */
    /*                                                                                                                        */
    /* Obs    NAME       SEX    AGE    HEIGHT    WEIGHT                                                                       */
    /*                                                                                                                        */
    /*   1    Alfred      M      14     69.0      112.5                                                                       */
    /*   2    Alice       F      13     56.5       84.0                                                                       */
    /*   3    Barbara     F      13     65.3       98.0                                                                       */
    /*   4    Carol       F      14     62.8      102.5                                                                       */
    /*   5    Henry       M      14     63.5      102.5                                                                       */
    /*   6    James       M      12     57.3       83.0                                                                       */
    /*   7    Jane        F      12     59.8       84.5                                                                       */
    /*   8    Janet       F      15     62.5      112.5                                                                       */
    /*   9    Jeffrey     M      13     62.5       84.0                                                                       */
    /*  10    John        M      12     59.0       99.5                                                                       */
    /*  11    Joyce       F      11     51.3       50.5                                                                       */
    /*  12    Judy        F      14     64.3       90.0                                                                       */
    /*  13    Louise      F      12     56.3       77.0                                                                       */
    /*  14    Mary        F      15     66.5      112.0                                                                       */
    /*  15    Philip      M      16     72.0      150.0                                                                       */
    /*  16    Robert      M      12     64.8      128.0                                                                       */
    /*  17    Ronald      M      15     67.0      133.0                                                                       */
    /*  18    Thomas      M      11     57.5       85.0                                                                       */
    /*  19    William     M      15     66.5      112.0                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit
    
    %array(_vs,values=%utl_varlist(sd1.have));

    %put &_vs4; /*----  HEIGHT                                               ----*/
    %put &_vsn; /*----  5                                                    ----*/

    proc datasets lib=work kill;
    run;quit;

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    options validvarname=any;
    %do_over(_vs,phrase=%str(
     proc sql;
      create
        view ? as
      select
         cats(?)  as ?
        ,count(?) as ?_cnt
      from
         sd1.have
      group
         by ?
    ;quit;));

    data sd1.want;
      merge name merge %utl_varlist(sd1.class);
    run;quit;
    ");

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* p to 40 obs from SD1.WANT total obs=19 12MAY2022:12:41:57                                                              */
    /*                                                                            HEIGHT_              WEIGHT_                */
    /* bs    NAME       NAME_cnt    SEX    SEX_cnt    AGE    AGE_cnt    HEIGHT      cnt      WEIGHT      cnt                  */
    /*                                                                                                                        */
    /*  1    Alfred         1        F         9      11        2        51.3        1       102.5        2                   */
    /*  2    Alice          1        M        10      12        5        56.3        1       112          2                   */
    /*  3    Barbara        1                  .      13        3        56.5        1       112.5        2                   */
    /*  4    Carol          1                  .      14        4        57.3        1       128          1                   */
    /*  5    Henry          1                  .      15        4        57.5        1       133          1                   */
    /*  6    James          1                  .      16        1        59          1       150          1                   */
    /*  7    Jane           1                  .                .        59.8        1       50.5         1                   */
    /*  8    Janet          1                  .                .        62.5        2       77           1                   */
    /*  9    Jeffrey        1                  .                .        62.8        1       83           1                   */
    /* 10    John           1                  .                .        63.5        1       84           2                   */
    /* 11    Joyce          1                  .                .        64.3        1       84.5         1                   */
    /* 12    Judy           1                  .                .        64.8        1       85           1                   */
    /* 13    Louise         1                  .                .        65.3        1       90           1                   */
    /* 14    Mary           1                  .                .        66.5        2       98           1                   */
    /* 15    Philip         1                  .                .        67          1       99.5         1                   */
    /* 16    Robert         1                  .                .        69          1                    .                   */
    /* 17    Ronald         1                  .                .        72          1                    .                   */
    /* 18    Thomas         1                  .                .                    .                    .                   */
    /* 19    William        1                  .                .                    .                    .                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
