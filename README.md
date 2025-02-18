# utl-move-even-numbered-columns-to-the-end-of-the-sas-pdv-datastep-and-sql-r-excel
 Move even numbered columns to the end of the sas pdv datastep and sql r excel
    %let pgm=utl-move-even-numbered-columns-to-the-end-of-the-sas-pdv-datastep-and-sql-r-excel;

    %stop_submission;

    Move even numbered columns to the end of the sas pdv datastep and sql r excel

    github
    https://tinyurl.com/2s36s6md
    https://github.com/rogerjdeangelis/utl-move-even-numbered-columns-to-the-end-of-the-sas-pdv-datastep-and-sql-r-excel

           SOLUTIONS (macros in https://tinyurl.com/y9nfugth )

               1 sas dictionary datastep
               2 sas bart and teds macros
               3 sas dictionary sql
               4 r excel sql
                 (you could use cbind in base re - however sql is more universal?)
               5 python. I could not figure it out
                 SOAPBOX OFF

                    Gave up using base python to create the odd column names.

                    In Python, indexing starts at 0, so the
                    even-numbered columns will actually be at
                    odd-numbered indices (1, 3, 5, etc.).

                    Also gave up on python because python cannot access the
                    sqldf pragma dictionary, which has column numbers that start with 1.

                    I worked at IBM when PL/1 language was developed.
                    PL/I (Programming Language One) was developed by IBM in the 1960s
                    as an ambitious attempt to create a universal programming language.

                    There were rumors that team meetings were roucous.

                    Even that a psycharist was invoved to try to get to common ground.
                    There were also thousands of pages related to the decisions and
                    the final documentation.

                    The first version and the bones of SAS were written in PL/1.

                    I don't believe the PL/1 team would make the default starting column index zero.

                    When SAS was rewritten in C the Classic 1980's editor lost functionality?

                    In spit of this, the python packaes are essential for any programmer.

                 SOAPBOX OFF

    NOTE:

    I included Barts array macro because it suppots a function to define the odd indecies.
    It expands the functionality of Ted Clays array macro.

    Bartosz Jablonski
    yabwon@gmail.com

    Ted Clay, M.S.
    tclay@ashlandhome.net

    FOR INSTANCE

          * this creates 4 odd indecies;
          %barray(o[1:4], function = (2*_I_-1))

          %put &=O1; * 1 ;
          %put &=O2; * 3 ;
          %put &=O3; * 5 ;
          %put &=O4; * 7 ;

          %put &=ON 4 * array dimension;


    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories


    /*************************************************************************************************************************/
    /*                           |                                            |                                              */
    /*       INPUT               |       PROCESS                              |               OUTPUT                         */
    /*       =====               |       =======                              |               ======                         */
    /*                           |                                            |                                              */
    /*                           |                                            |                                              */
    /*      SD1.HAVE             |                                            |               SD1.WANT                       */
    /*                           |  1 SAS DICTIONARY DATASTEP                 |                                              */
    /*  A C1 B C2 C C3 D C4      |  =========================                 |          A B C D   C1 C2 C3 C4               */
    /*                           |                                            |                                              */
    /*  1  1 2  2 3  3 4  4      |  %arraydelete(name);                       |          1 2 3 4    1  2  3  4               */
    /*                           |                                            |                                              */
    /*  options                  | proc sql;                                  |                                              */
    /*   validvarname=upcase;    |  select                                    |                                              */
    /*  libname sd1 "d:/sd1";    |      name                                  |                                              */
    /*  data sd1.have;           |  into                                      |                                              */
    /*   retain                  |     :name separated by ' '                 |                                              */
    /*      a c1 1               |  from                                      |                                              */
    /*      b c2 2               |     dictionary.columns                     |                                              */
    /*      c c3 3               |  where                                     |                                              */
    /*      d c4 4;              |     libname = "SD1"   and                  |                                              */
    /*  run;quit;                |     memname = "HAVE"  and                  |                                              */
    /*                           |  mod(varnum,2)=1                           |                                              */
    /*                           | ;quit;                                     |                                              */
    /*                           |                                            |                                              */
    /*                           | %put &=name; * A B C D ;                   |                                              */
    /*                           |                                            |                                              */
    /*                           | data want;                                 |                                              */
    /*                           |   retain &name;                            |                                              */
    /*                           |   set sd1.have;                            |                                              */
    /*                           | run;quit;                                  |                                              */
    /*                           |                                            |                                              */
    /*                           |                                            |                                              */
    /*                           |                                            |                                              */
    /*                           |  2 TED CLAY AND BARTS MACROS               |                                              */
    /*                           |  ===========================               |                                              */
    /*                           |                                            |                                              */
    /*                           |  * get variable list                       |                                              */
    /*                           |                                            |                                              */
    /*                           |  %let vars=%utl_varlist(sd1.have);         |                                              */
    /*                           |  %put &=vars;                              |                                              */
    /*                           |                                            |                                              */
    /*                           |  VARS=A C1 B C2 C C3 D C4                  |                                              */
    /*                           |                                            |                                              */
    /*                           |  * get positions of odd variables          |                                              */
    /*                           |                                            |                                              */
    /*                           |  * in case we rerun;                       |                                              */
    /*                           |  %arraydelete(o);                          |                                              */
    /*                           |                                            |                                              */
    /*                           |  * this creates 4 indecies                 |                                              */
    /*                           |  %barray(o[1:4],function=(2*_I_-1) )       |                                              */
    /*                           |                                            |                                              */
    /*                           |  %put &=O1; * 1 ;                          |                                              */
    /*                           |  %put &=O2; * 3 ;                          |                                              */
    /*                           |  %put &=O3; * 5 ;                          |                                              */
    /*                           |  %put &=O4; * 7 ;                          |                                              */
    /*                           |                                            |                                              */
    /*                           |  %put &=ON  * 4 array dimension;           |                                              */
    /*                           |                                            |                                              */
    /*                           |  * scan varlist for odd columns            |                                              */
    /*                           |                                            |                                              */
    /*                           |  %MACRO DOIT(idx);                         |                                              */
    /*                           |    %scan(&vars,&idx,' ')                   |                                              */
    /*                           |  %MEND doit;                               |                                              */
    /*                           |                                            |                                              */
    /*                           |  * odd variable first;                     |                                              */
    /*                           |                                            |                                              */
    /*                           |  data want;                                |                                              */
    /*                           |    retain %do_over(o,macro=doit);          |                                              */
    /*                           |    set sd1.have;                           |                                              */
    /*                           |  run;quit;                                 |                                              */
    /*                           |                                            |                                              */
    /*                           |                                            |                                              */
    /*                           |  3 SAS DICTIONARY SQL                      |                                              */
    /*                           |  ====================                      |                                              */
    /*                           |                                            |                                              */
    /*                           |  %arraydelete(name);                       |                                              */
    /*                           |                                            |                                              */
    /*                           |   proc sql;                                |                                              */
    /*                           |    select                                  |                                              */
    /*                           |        name                                |                                              */
    /*                           |    into                                    |                                              */
    /*                           |       :name separated by ','               |                                              */
    /*                           |    from                                    |                                              */
    /*                           |       dictionary.columns                   |                                              */
    /*                           |    where                                   |                                              */
    /*                           |       libname = "SD1"   and                |                                              */
    /*                           |       memname = "HAVE"  and                |                                              */
    /*                           |       mod(varnum,2)=1                      |                                              */
    /*                           |   ;quit;                                   |                                              */
    /*                           |                                            |                                              */
    /*                           |  %put &=name; * A,B,C,D ;                  |                                              */
    /*                           |                                            |                                              */
    /*                           |  proc sql                                  |                                              */
    /*                           |     create                                 |                                              */
    /*                           |        table want as                       |                                              */
    /*                           |     select                                 |                                              */
    /*                           |        &name                               |                                              */
    /*                           |       ,*                                   |                                              */
    /*                           |  from                                      |                                              */
    /*                           |       sd1.have                             |                                              */
    /*                           |  ;quit;                                    |                                              */
    /*                           |                                            |                                              */
    /*                           |                                            |                                              */
    /*                           |  4 R EXCEL SQL                             |                                              */
    /*                           |  =============                             |                                              */
    /*                           |                                            |                                              */
    /*                           |  %utl_rbeginx;                             |                                              */
    /*                           |  parmcards4;                               |                                              */
    /*                           |  library(sqldf)                            |                                              */
    /*                           |  library(haven)                            |                                              */
    /*                           |  source("c:/oto/fn_tosas9x.R")             |                                              */
    /*                           |  have<-read_sas("d:/sd1/have.sas7bdat")    |                                              */
    /*                           |  oddvec<-have[,(1:ncol(have)) %% 2==1]     |                                              */
    /*                           |  evnvec<-have[,(1:ncol(have)) %% 2==0]     |                                              */
    /*                           |  oddstr<-paste(names(oddvec),collapse=",") |                                              */
    /*                           |  evnstr<-paste(names(evnvec),collapse=",") |                                              */
    /*                           |  query<-paste('select'                     |                                              */
    /*                           |    ,oddstr,",",evnstr, "from have")        |                                              */
    /*                           |  query                                     |                                              */
    /*                           |  want<-sqldf(query)                        |                                              */
    /*                           |  want                                      |                                              */
    /*                           |  fn_tosas9x(                               |                                              */
    /*                           |        inp    = want                       |                                              */
    /*                           |       ,outlib ="d:/sd1/"                   |                                              */
    /*                           |       ,outdsn ="rwant"                     |                                              */
    /*                           |       )                                    |                                              */
    /*                           |  ;;;;                                      |                                              */
    /*                           |  %utl_rendx;                               |                                              */
    /*                           |                                            |                                              */
    /*                           |  proc print data=want;                     |                                              */
    /*                           |  run;quit;                                 |                                              */
    /*                           |                                            |                                              */
    /*                           |                                            |                                              */
    /*                           |  5 PYTHON COULD NOT FIGURE IT OUT          |                                              */
    /*                           |  ================================          |                                              */
    /*                           |                                            |                                              */
    /*                           |                                            |                                              */
    /*************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
