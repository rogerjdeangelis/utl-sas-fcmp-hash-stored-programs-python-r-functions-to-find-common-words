# utl-sas-fcmp-hash-stored-programs-python-r-functions-to-find-common-words
sas fcmp hash stored programs python r functions to find common words
   %let pgm=utl-sas-fcmp-hash-stored-programs-python-r-functions-to-find-common-words;

    sas fcmp hash stored programs python r functions to find common words

    github
    https://tinyurl.com/2p88599m
    https://github.com/rogerjdeangelis/utl-sas-fcmp-hash-stored-programs-python-r-functions-to-find-common-words


    Latest python and R drop down from sas macros on end of this message

    Fcmp with single arguments is well documented.

    Problem: Intersection of words in two lists

    Two Solutions

        1. SAS FCMP (unusual interface multiple character arguments)
           Bartosz Jablonski
           yabwon@gmail.com
           Checkout https://github.com/yabwon on SAS packages
           https://listserv.uga.edu/scripts/wa-UGA.exe?A2=SAS-L;ca832da1.2210A&S=

        2. SAS stored program.

        3. DOSUBL solution is not shown.
           Note you can use peek and poke to send blocks of storage from
           parent datastep to child dosubl routine (currently very slow)

        4. SAS FCMP (unusual interface multiple numeric arguments)
           Bartosz Jablonski
           yabwon@gmail.com

        5. SAS FCMP (unusual interface multiple numeric arguments)
           Bartosz Jablonski
           yabwon@gmail.com

        6. FCMP Subroutine with multiple character inputs and outputs
           https://sasnrd.com/sas-proc-fcmp-function-subroutine/

        7. Bonus SAS HASH Solution
           Bartosz Jablonski
           yabwon@gmail.com

        8. Drop down to python.

        9. Drop down to R.

    As a side note you can solve this with sql joins after making the strings long and skinny.

    github
    https://tinyurl.com/bd9x7tfn
    https://github.com/rogerjdeangelis/utl-sas-FCMP-functions-and-stored-programs-with-multiple-character-arguments

    Related repos

    github  (best solution)
    https://tinyurl.com/v8ratsjn
    https://github.com/rogerjdeangelis/utl-intersection-of-words-in-two-lists

    https://tinyurl.com/y9dzonq2
    https://github.com/rogerjdeangelis/utl_sharing_a_block_of_memory_with_dosubl

    https://tinyurl.com/2vda4rc2
    https://github.com/rogerjdeangelis/utl-simple-datastep-replacement-for-fcmp-use-stored-program-

    FCMP LIMITATIONS

    Limitations on fcmp functions and stored sas programs when using multiple arguments
    These obsevations are my obsevations and may not be correct. Documentation is limited.

       1. Common storage with fcmp requires multiple arguments be passed using an array.
       2. The FCMP language does not support all sas operators or statements. This can be madening.
       3. FCMP is not supported in WPS World Programming System
       4. Stored program solutions are very slow (stored sas programs unfortunately are not executables)

    Problem

    Find intersection common of words in two lists;
                                                                      |   Rules
                                                                      |   Common Words
      SRC                              TRG                            |   RESULT
                                                                      |
      one two three four               two three                      |   two three
      five six seven eight             one two three four             |
      one two three four five          five                           |   five
      roger suzie roger suzie roger    roger roger suzie suzie roger  |   roger suzie
      sas wps                          sas wps                        |   sas wps
      taco taco taco                   taco                           |   taco
      taco                             taco taco                      |   taco

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    libname sd1 "d:/sd1";
    data sd1.have;
      informat src trg $200.;
      infile cards delimiter=',';
      input src trg;
    cards4;
    one two three four,two three
    five six seven eight,one two three four
    one two three four five,five
    roger suzie roger suzie roger,roger roger suzie suzie roger
    sas wps,sas wps
    taco taco taco,taco
    taco,taco taco
    ;;;;
    run;quit;

    /*
    Up to 40 obs SD1.HAVE total obs=7 27SEP2022:15:01:52
    Obs    SRC                              TRG
     1     one two three four               two three
     2     five six seven eight             one two three four
     3     one two three four five          five
     4     roger suzie roger suzie roger    roger roger suzie suzie roger
     5     sas wps                          sas wps
     6     taco taco taco                   taco
     7     taco                             taco taco
    */

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */
    Up to 40 obs from WANT_SASFCMP total obs=7 27SEP2022:15:06:33
                                                                             COMMON WORDS
    Obs    SRC                              TRG                              COMWRD

     1     one two three four               two three                        two three
     2     five six seven eight             one two three four
     3     one two three four five          five                             five
     4     roger suzie roger suzie roger    roger roger suzie suzie roger    roger suzie
     5     sas wps                          sas wps                          sas wps
     6     taco taco taco                   taco                             taco
     7     taco                             taco taco                        taco
    */
    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
     _      __
    / |    / _| ___ _ __ ___  _ __
    | |   | |_ / __| `_ ` _ \| `_ \
    | |_  |  _| (__| | | | | | |_) |
    |_(_) |_|  \___|_| |_| |_| .__/
                             |_|
    */
    * YOU HAVE TO COMPILE THE FCMP FUNCTION BELOW BEFORE RUNNING THIS DATASTEP;
    * CLUMSY INTERFACE?;

    options cmplib=work.funcs;
     proc fcmp outlib=work.funcs.char;
       function commonwords(w[*] $) $ 200;
        length src trg result $200;
        src    = w[1] ;
        trg    = w[2] ;
        result = w[3] ;
         do idxSrc = 1 to countw(src);
             srcIdx = scan(src,IdxSrc);
             if index(trg,strip(srcIdx)) > 0 then do;
                  if index(result,strip(srcIdx)) = 0 then result=catx(" ",result,srcIdx);
             end;
         end;
       return(result);
       endfunc;
    run;quit;

    * YOU CAN USE BOTH TEMPORARY OR PERMANENT ARRAYS;

    * PERMANENT ARRAYS;
    options compress=no;
    data want_sasFCMP;
       set sd1.have;
       array wrds[3] $ 200 wrd1-wrd3;
       wrds[1] = src ;
       wrds[2] = trg ;
       wrds[3] = " " ;  /* the commn words */
       comWrd  = commonwords(wrds);
       drop wrd1-wrd3;
       put comWrd;
    run;

    * TEMPORARY ARRAYS;
    options compress=no;
    data want_sasFCMPtmp;
       set sd1.have;
       array wrds[3] $ 200 _temporary_;
       wrds[1] = src ;
       wrds[2] = trg ;
       wrds[3] = " " ;  /* the commn words */
       comWrd  = commonwords(wrds);
       drop wrd1-wrd3;
       put comWrd;
    run;


    /*
     ____          _                     _
    |___ \     ___| |_ ___  _ __ ___  __| |  _ __   __ _ _ __ ___
      __) |   / __| __/ _ \| `__/ _ \/ _` | | `_ \ / _` | `_ ` _ \
     / __/ _  \__ \ || (_) | | |  __/ (_| | | |_) | (_| | | | | | |
    |_____(_) |___/\__\___/|_|  \___|\__,_| | .__/ \__, |_| |_| |_|
                                            |_|    |___/
    */

    libname sto "d:/sto";
    data sto.utl_commonWords / pgm=sto.utl_commonWords;

            length arg_trg arg_src $200;

            arg_trg = symgetc("arg_trg");
            arg_src = symgetc("arg_src");

            /*-- none of the variables created by this macro will be in the output   --*/
            /*-- temporary arrays are not written to output datasets                 --*/

            array __c[2] $ 200 _temporary_;
            array __n[1] 8     _temporary_;  /*-- for saving _n_                     --*/

            /*-- unfortunately the do statement does not support _temporary_ arrays  --*/
            /*-- save currenr temprary _n_ value to retore later                     --*/
            __n[1] = _n_;

            /*--  hold _temporary_ intermediate values
               __c[1] temp storage for single words from source
               __c[2] remp storage for intermediate results
            --*/

            /*-- cannot use a temp variable in do statement                          --*/
            /*-- iterate over each word in source string                             --*/

            do  _n_ = 1 to  countw(arg_src)   ;

                __c[1] = scan(arg_src,_n_) ; /*--get each word in the source list --*/

                /*-- if the source word in target string then proceed                --*/
                if index(arg_trg,strip(__c[1])) > 0 then do ;

                     /* -- do not add word to result list if already present         --*/
                     if index(__c[2],strip(__c[1])) = 0 then
                        __c[2]=catx(" ",__c[2],__c[1]);

                end ; /* adding one word that is not aready in result list           --*/

            end; /* end adding distinct words in source and target to result list    --*/

            call symputx("arg_result",__c[2],"G")  ;

            put __C[2]=;

            __c[2] = "";

            * restore _n_; /*-- restore original bact to calling program             --*/
            _n_ = __n[1] ;

    run;quit;

    * COMPILE THE STORED PROGRAM BELOW BEFORE RUNNING THIS DATASTEP;
    * CLEANER INTERFACE BUT VERY SLOW;

    proc datasets lib=work mt=data kill nodetails nolist;
    run;quit;

    %symdel arg_src arg_trg arg_resulr / nowarn;

    * slow;
    data want_stored;

      length arg_result $200;

      set sd1.have;

      call symputx('arg_src',src);
      call symputx('arg_trg',trg);

      rc=dosubl('
         data pgm=sto.utl_commonWords;
         run;quit;
         ');

      arg_result = symgetc('arg_result');

      drop rc;

    run;quit;


    /*___      __                                              _
    | ___|    / _| ___ _ __ ___  _ __    _ __  _   _ _ __ ___ | |__   ___ _ __ ___
    |___ \   | |_ / __| `_ ` _ \| `_ \  | `_ \| | | | `_ ` _ \| `_ \ / _ \ `__/ __|
     ___) |  |  _| (__| | | | | | |_) | | | | | |_| | | | | | | |_) |  __/ |  \__ \
    |____(_) |_|  \___|_| |_| |_| .__/  |_| |_|\__,_|_| |_| |_|_.__/ \___|_|  |___/
                                |_|
    */

    proc fcmp outlib=work.funcs.nums;
      function PRODUKT (_arr_[*]) VARARGS;
          _prd_ = 1;
          _missings_ = 0;

          do i = 1 to DIM(_arr_);
              _missings_ += ( if _arr_[i] = . then 1 else 0 );
              _prd_ = _prd_ * coalesce(_arr_[i], 1);
          end;

          if _missings_ = dim(_arr_) then _prd_ = .;
          return(_prd_);
      endsub;
    run;quit;

    data _null_;
      x1 = .;
      x2 = 3;
      x3 = 2;
       a = "ABC";
      x4 = 4;
      x5 = 5;
      x6 = .;

      array testx[*] x:;

      prd=Produkt(testx);
      put prd=;
    run;

    /*__       __                                  _                     _   _
     / /_     / _| ___ _ __ ___  _ __    ___ _   _| |__  _ __ ___  _   _| |_(_)_ __   ___
    | `_ \   | |_ / __| `_ ` _ \| `_ \  / __| | | | `_ \| `__/ _ \| | | | __| | `_ \ / _ \
    | (_) |  |  _| (__| | | | | | |_) | \__ \ |_| | |_) | | | (_) | |_| | |_| | | | |  __/
     \___(_) |_|  \___|_| |_| |_| .__/  |___/\__,_|_.__/|_|  \___/ \__,_|\__|_|_| |_|\___|
    */

    options cmplib=work.funcs;
    proc fcmp outlib=work.funcs.sumpy;
    proc fcmp outlib=work.funcs.sumpy;
       subroutine sumpy (in1, in2, out1, out2);
          outargs out1, out2;
          out1=in1 + in2;
          out2=in1 * in2;
       endsub;
    run;quit;

    options cmplib=work.funcs;
    data _null_;
       num1 = 2 ;
       num2 = 3 ;
       call sumpy(num1, num2, sum, prod);
       put sum= / prod=;
    run;

    /*
    SUM=5
    PROD=6
    */

    /*____   _                             _               _
    |___  | | |__   ___  _ __  _   _ ___  | |__   __ _ ___| |__
       / /  | `_ \ / _ \| `_ \| | | / __| | `_ \ / _` / __| `_ \
      / /_  | |_) | (_) | | | | |_| \__ \ | | | | (_| \__ \ | | |
     /_/(_) |_.__/ \___/|_| |_|\__,_|___/ |_| |_|\__,_|___/_| |_|

    */

    data have;
      length str1 str2 $200;
      informat str1 str2 $200.;
      infile cards delimiter=',';
      input str1 str2;
    cards4;
    one two three four,two three
    five six seven eight,one two three four
    one two three four five,five
    roger suzie roger suzie roger,roger roger suzie suzie roger
    sas wps,sas wps
    taco taco taco,taco
    taco,taco taco
    ;;;;
    run;quit;

    data want;

    dcl hash h (ordered: "a") ;
      h.definekey ("word") ;
      h.definedata ("word", "count") ;
      h.definedone ();

    do until(eof);
    h.clear();

    set have end = eof;

    count = 0;
    countw_str1 = countw(str1);
    do i=1 to countw_str1;
     word = scan(str1, i);
     if h.find() then h.add();
    end;

    countw_str2 = countw(str2);
    do i=1 to countw_str2;
     word = scan(str2, i);
     if h.find() = 0 then do; count = count + 1;                     h.replace(); end;
                     else do; count = -sum(countw_str1,countw_str2); h.add();     end;
                     /* it could be done like that: "count = .;"
                        but then you have: "NOTE: Missing values were generated..." in the log
                        and it is a ugly one ;-)
                     */
    end;


    declare hiter ih('h');
    count_of_common=0; length common_words $ 200; common_words = "";
    count_of_union=0;  length union_words $ 200; union_words = "";

    _rc_ = ih.first();
    do while(_rc_ = 0);
        if count>0 then do;
                        common_words = catx(" ", common_words, word);
                        count_of_common +1;
                      end;
                      union_words = catx(" ", union_words, word);
                      count_of_union +1;
        _rc_ = ih.next();
    end;
    output;

    /**/
    end;

    stop;
    keep str1 str2 common_words count_of_common union_words count_of_union;
    run;

    options ls = max ps=max;
    proc print data = want;
    run;

    Output:
    Obs|str1                          |str2                         |common|words      |of_union|union_words
    ===|==============================|=============================|======|===========|========|=======================================
     1 |one two three four            |two three                    |  2   |three two  |    4   |four one three two
     2 |five six seven eight          |one two three four           |  0   |           |    8   |eight five four one seven six three two
     3 |one two three four five       |five                         |  1   |five       |    5   |five four one three two
     4 |roger suzie roger suzie roger |roger roger suzie suzie roger|  2   |roger suzie|    2   |roger suzie
     5 |sas wps                       |sas wps                      |  2   |sas wps    |    2   |sas wps
     6 |taco taco taco                |taco                         |  1   |taco       |    1   |taco
     7 |taco                          |taco taco                    |  1   |taco       |    1   |taco


    Bart

    /*___                 _   _
     ( _ )    _ __  _   _| |_| |__   ___  _ __
     / _ \   | `_ \| | | | __| `_ \ / _ \| `_ \
    | (_) |  | |_) | |_| | |_| | | | (_) | | | |
     \___(_) | .__/ \__, |\__|_| |_|\___/|_| |_|
             |_|    |___/
    */

    * The quoting will drive you nuts. Since double quotes are reserved for
    the macro, you cannot use double quotes.
    Quotes are doubled at each level.;

    %*inc "c:/oto/utl_submit_py64_310.sas";

    proc datasets lib=work mt=data kill nodetails nolist;
    run;quit;

    %symdel src trg conWrds / nowarn;

    libname sd1 "d:/sd1";
    libname sto "d:/sto";

    %symdel src trg conWrds / nowarn;

    * CREATE STORED PROGRAM WITH PYTHON CODE;
    data sd1.want_py;

      set sd1.have;

      call symputx('src',cats("['",src,"']"));
      call symputx('trg',cats("['",trg,"']"));

      rc = dosubl('
          %utl_submit_py64_310(''
          import pyperclip;
          src = &src;
          trg = &trg;
          src=  '''' ''''.join(src).split('''' '''');
          trg = '''' ''''.join(trg).split('''' '''');
         output = list(set(src).intersection(trg));
         pyperclip.copy(str(output));
         '',return=comWrds)
         ');

     result = symget('comWrds');
     put result=;

    run;quit;


    Up to 40 obs from SD1.WANT_PY total obs=7 03OCT2022:13:28:46

    Obs    SRC                              TRG                              RC    RESULT

     1     one two three four               two three                         0    ['two', 'three']
     2     five six seven eight             one two three four                0    []
     3     one two three four five          five                              0    ['five']
     4     roger suzie roger suzie roger    roger roger suzie suzie roger     0    ['suzie', 'roger']
     5     sas wps                          sas wps                           0    ['wps', 'sas']
     6     taco taco taco                   taco                              0    ['taco']
     7     taco                             taco taco                         0    ['taco']

    /*___     ____
     / _ \   |  _ \
    | (_) |  | |_) |
     \__, |  |  _ <
       /_(_) |_| \_\

    */

    * it appears that you need macro tslit when using SAs macro variables or
      R functions. R does not seem to make it easy to get objects into or out of R.
      SAS/WPS are desiged to get integrate with other languages.;

    proc datasets lib=work mt=data nodetails nolist kill;
    run;quit;

    %symdel src trg conRwrds result / nowarn;

    data want_r;

    set sd1.have;

    call symputx('src',src);
    call symputx('trg',trg);

    rc=dosubl('
      %utl_submit_r64(''
      library(stringr);
      a <-%tslit(&src);
      b <-%tslit(&trg);
      vec1 <- c(a,b);
      res<-Reduce(%tslit(intersect),str_extract_all(vec1,%tslit(\\w+)));
      res= paste(res, collapse = '''' '''');
      res;
      writeClipboard(as.character(res));
      '',return=result,resolve=Y);');

    result=symget('result');

    put result=;

    run;quit;


    /*
    Up to 40 obs from WANT_R total obs=7 04OCT2022:11:56:04

    Obs    SRC                              TRG                              RC    RESULT

     1     one two three four               two three                         0    two three
     2     five six seven eight             one two three four                0
     3     one two three four five          five                              0    five
     4     roger suzie roger suzie roger    roger roger suzie suzie roger     0    roger suzie
     5     sas wps                          sas wps                           0    sas wps
     6     taco taco taco                   taco                              0    taco
     7     taco                             taco taco                         0    taco
    */


    /*     _   _               _               _ _                     __   _  _
     _   _| |_| |    ___ _   _| |__  _ __ ___ (_) |_      _ __  _   _ / /_ | || |
    | | | | __| |   / __| | | | `_ \| `_ ` _ \| | __|    | `_ \| | | | `_ \| || |_
    | |_| | |_| |   \__ \ |_| | |_) | | | | | | | |_     | |_) | |_| | (_) |__   _|
     \__,_|\__|_|___|___/\__,_|_.__/|_| |_| |_|_|\__|____| .__/ \__, |\___/   |_|
               |_____|                             |_____|_|    |___/
    */
    %macro utl_submit_py64_310(
          pgm
         ,return=  /* name for the macro variable from Python */
         )/des="Semi colon separated set of python commands - drop down to python";

      * delete temporary files;
      %utlfkil(%sysfunc(pathname(work))/py_pgm.py);
      %utlfkil(%sysfunc(pathname(work))/stderr.txt);
      %utlfkil(%sysfunc(pathname(work))/stdout.txt);

      filename py_pgm "%sysfunc(pathname(work))/py_pgm.py" lrecl=32766 recfm=v;
      data _null_;
        length pgm  $32755 cmd $1024;
        file py_pgm ;
        pgm=resolve(&pgm);
        semi=countc(pgm,";");
          do idx=1 to semi;
            cmd=cats(scan(pgm,idx,";"));
            if cmd=:". " then
               cmd=trim(substr(cmd,2));
             put cmd $char384.;
             putlog cmd ;
          end;
      run;quit;
      %let _loc=%sysfunc(pathname(py_pgm));
      %let _stderr=%sysfunc(pathname(work))/stderr.txt;
      %let _stdout=%sysfunc(pathname(work))/stdout.txt;
      filename rut pipe  "d:\Python310\python.exe &_loc 2> &_stderr";
      data _null_;
        file print;
        infile rut;
        input;
        put _infile_;
      run;
      filename rut clear;
      filename py_pgm clear;

    data _null_;
        file print;
        infile "%sysfunc(pathname(work))/stderr.txt";
        input;
        put _infile_;
      run;
      filename rut clear;
      filename py_pgm clear;

      * use the clipboard to create macro variable;
      %if "&return" ^= "" %then %do;
        filename clp clipbrd ;
        data _null_;
         length txt $200;
         infile clp;
         input;
         putlog "*******  " _infile_;
         call symputx("&return",_infile_,"G");
        run;quit;
      %end;

    %mend utl_submit_py64_310;

    /*     _   _               _               _ _             __   _  _
     _   _| |_| |    ___ _   _| |__  _ __ ___ (_) |_     _ __ / /_ | || |
    | | | | __| |   / __| | | | `_ \| `_ ` _ \| | __|   | `__| `_ \| || |_
    | |_| | |_| |   \__ \ |_| | |_) | | | | | | | |_    | |  | (_) |__   _|
     \__,_|\__|_|___|___/\__,_|_.__/|_| |_| |_|_|\__|___|_|   \___/   |_|
               |_____|                             |_____|
    */

    %macro utl_submit_r64(
          pgmx
         ,return=N
         ,resolve=N
         )/des="Semi colon separated set of R commands - drop down to R";

      %utlfkil(%sysfunc(pathname(work))/r_pgm.txt);

      /* clear clipboard */
      filename _clp clipbrd;
      data _null_;
        file _clp;
        put " ";
      run;quit;

      * write the program to a temporary file;
      filename r_pgm "%sysfunc(pathname(work))/r_pgm.txt" lrecl=32766 recfm=v;

      data _null_;
        length pgm $32756;
        file r_pgm;
        if substr(upcase("&resolve"),1,1)="Y" then do;
            pgm=resolve(&pgmx);
         end;
        else do;
            pgm=&pgmx;
         end;
        put pgm;
        putlog pgm;
      run;

      %let __loc=%sysfunc(pathname(r_pgm));

      * pipe file through R;
      filename rut pipe "c:\progra~1\r\R-4.2.1\bin\r.exe --vanilla --quiet --no-save < &__loc";
      data _null_;
        file print;
        infile rut recfm=v lrecl=32756;
        input;
        put _infile_;
        putlog _infile_;
      run;

      filename rut clear;
      filename r_pgm clear;

      * use the clipboard to create macro variable;
      %if %upcase(%substr(&return.,1,1)) ne N %then %do;
        filename clp clipbrd ;
        data _null_;
         infile clp;
         input;
         putlog "macro variable &return = " _infile_;
         call symputx("&return.",_infile_,"G");
        run;quit;
      %end;

    %mend utl_submit_r64;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
