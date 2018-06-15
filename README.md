# utl_create_new_variable_names_based_on_existing_variable_names
Create_new_variable_names_based_on_existing_variable_names.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Create new variable names based on existing variable names

    github
    https://tinyurl.com/y7bk4x72
    https://github.com/rogerjdeangelis/utl_create_new_variable_names_based_on_existing_variable_names

    see
    https://tinyurl.com/y72ofg2h
    https://stackoverflow.com/questions/50837407/sas-how-to-work-with-columns-whose-name-consists-of-two-parts-first-is-the-same


    INPUT
    =====
                             |   RULES Add new variables based on existing names
     WORK.HAVE total obs=2   |   We want to prefix existing names with 'E'
                             |
      S_A    S_B    S_C      |    ES_A    ES_B    ES_C
                             |
       1      0      1       |    yes             yes
       0      1      0       |            yes
                             |

    EXAMPLE OUTPUT

     WORK.LOG total obs=1

               STATUS

      Meta data extract completed


     WORK.WANT total obs=2

        S_A    S_B    S_C  ES_A    ES_B    ES_C

         1      0      1   yes             yes
         0      1      0           yes


    PROCESS
    =======

    %symdel RC ES_A ES_B ES_C / nowarn; * just in case global - not needed;

    data want(drop=status) log(keep=status);

      * get the meta data - often more work then the mainline;
      if _n_=0 then do;
         %let rc=%sysfunc(dosubl('
            proc transpose data=have(obs=1) out=havXpo;
              var _all_;
            run;quit;
            proc sql;
              select cats("E",_name_) into :nams separated by " " from havXpo
            ;quit;
        '));
      array new[*] $3 &nams;
      end;

      if _n_ = 1 then do;
        if &rc=0 then status="Meta data extract completed";
        else status="Meta data failed";
        output log;
      end;

      set have;

      array vars[*]  _numeric_;

      do i= 1 to dim(vars);
       if vars[i] = 1 then
          new[i] = "yes";
      end;
      output want;
    drop i;
    run;


    OUTPUT
    ======

     WORK.LOG total obs=1

               STATUS

      Meta data extract completed


     WORK.WANT total obs=2

        S_A    S_B    S_C  ES_A    ES_B    ES_C

         1      0      1   yes             yes
         0      1      0           yes

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
    S_a = 1;
    S_b = 0;
    S_c = 1;
    output;
    S_a = 0;
    S_b = 1;
    S_c = 0;
    output;
    run;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    see process


