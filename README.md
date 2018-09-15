# utl_make_xml_file_more_compact_remove_crlf_and_space
Make xml file more compact by remove crlf and space. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Make xml file more compact by removing crlf and space.
    XML can be very bloated.

    Problem: Given a 'pretty' formatted XML file, 'prett' is the accepted term remove
    CRLF and spaces after xml version and encoding.
    I suspect Perl.Python is better suited for creating and manipulating XML,JSON and HTML?

       Two Solutions

             1. PERL post process (I suspect python can do this I like PERL for this?)
             2. SAS tagsets (does the work up front - better - however does not help with existing XML)

    A little Perl/Python knowledge will not hurt, has helped my career.

    github
    https://tinyurl.com/y9ejmh7j
    https://github.com/rogerjdeangelis/utl_make_xml_file_more_compact_remove_crlf_and_space

    SAS Forum
    https://communities.sas.com/t5/ODS-and-Base-Reporting/How-to-output-XML-without-Pretty-format/m-p/495429

    tagset by Vince_SAS
    https://communities.sas.com/t5/user/viewprofilepage/user-id/13635


    INPUT
    =====

    1. Python post process

       d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space.xml

       <?xml version="1.0" encoding="windows-1252" ?>
       <TABLE>
          <TESTDATA>
             <NAME>Alfred</NAME>
             <SEX>M</SEX>
             <AGE>14</AGE>
             <HEIGHT>69</HEIGHT>
             <WEIGHT>112.5</WEIGHT>
          </TESTDATA>
          <TESTDATA>
             <NAME>Alice</NAME>
             <SEX>F</SEX>
             <AGE>13</AGE>
             <HEIGHT>56.5</HEIGHT>
             <WEIGHT>84</WEIGHT>
          </TESTDATA>
         ...

    2. SAS tagsets (does the work up front - better)

       see tagset on end


    EXAMPLE OUTPUT
    --------------

    COMPACT IT By removing CRLF abd spaces - long single line file

    <?xml version="1.0" encoding="windows-1252" ?><TABLE><TESTDATA><NAME>Alfred</NAME><SEX>M</SEX><AGE>14</AGE>...


    PROCESS
    =======

    1. Perl post processes the xml and removes CRLF and spaces while
       preserving encoding options '<?xml version="1.0" encoding="windows-1252" ?>'
    --------------------------------------------------------------------------------------


       %utl_submit_pl64('
       #!/usr/bin/perl`
       local $/=undef;`
       open(FILE, "d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space.xml");`
       $string = <FILE>;`
       $hed=substr($string, 0, 46);`
       $string =~ s/[\n\r\s]+//g;;`
       $string=$hed . substr($string, 43);`
       my $filename = "d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space_perl.xml";`
       open(my $fh, ">", $filename);`
       print $fh "$string";`
       close $fh;`
       print "$string";`
       ');

       * CHECK Compated XML file;

       libname XMLData xmlv2 "d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space_perl.xml";

       data  tst;
          set xmldata.testdataset;
       run;quit;

       NOTE: There were 19 observations read from the data set XMLDATA.testdataset.
       NOTE: The data set WORK.TST has 19 observations and 5 variables.



       %utl_submit_py64("
       with open('d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space.xml', 'r') as myfile:;
       .    data=myfile.read().rstrip('\n\r');
       data=data.replace('/[\n\r\s]+//g','');
       with open('d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space_sqz.xml', 'w') as f:;
       .    f.write(data);
       ");


    2. SAS tagsets (does the work up front - better note the tagset)
    ----------------------------------------------------------------

       * directly produces compact XML;

       libname xmldata xmlv2 "d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space_sas.xml"
       tagset=tagsets.uglyxml;

       data xmldata.testdataset;
          set sashelp.class;
       run;quit;

       * CHECK Compated XML file;

       libname xmldata xmlv2 "d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space_sas.xml";

       data  tst;
          set xmldata.testdataset;
       run;quit;

       NOTE: There were 19 observations read from the data set XMLDATA.testdataset.
       NOTE: The data set WORK.TST has 19 observations and 5 variables.

    OUTPUT
    ======

      Both output COMPACT XML that can be converted to a SAS dataset

      COMPACT IT By removing CRLF abd spaces - long single line file

      <?xml version="1.0" encoding="windows-1252" ?><TABLE><TESTDATA><NAME>Alfred</NAME><SEX>M</SEX><AGE>14</AGE>...


    *                _                         _
     _ __ ___   __ _| | _____  __  ___ __ ___ | |
    | '_ ` _ \ / _` | |/ / _ \ \ \/ / '_ ` _ \| |
    | | | | | | (_| |   <  __/  >  <| | | | | | |
    |_| |_| |_|\__,_|_|\_\___| /_/\_\_| |_| |_|_|

    ;

    libname XMLData xmlv2 "d:/xml/utl_make_xml_file_more_compact_remove_crlf_and_space.xml";

    data xmldata.TestDataSet;
          set SASHelp.class;
    run;quit;

    *                _                       _
     ___  __ _ ___  | |_ __ _  __ _ ___  ___| |_
    / __|/ _` / __| | __/ _` |/ _` / __|/ _ \ __|
    \__ \ (_| \__ \ | || (_| | (_| \__ \  __/ |_
    |___/\__,_|___/  \__\__,_|\__, |___/\___|\__|
                              |___/
    ;

    ods path(prepend) work.tmplmst(update);

    proc template;
    define tagset tagsets.uglyxml;
     parent=tagsets.Sasioxml;
      notes "Ugly SAS-XML generic XML-Data";


       define event XMLversion;
          put "<?xml version=""1.0""";
          putq " encoding=" ENCODING;
          put " ?>";
          break;
       end;


       define event SASTable;
          start:
             put "<TABLE>" ;
             break;

          finish:
             put "</TABLE>" ;
             break;
       end;

       define event SASRow;
          start:
             put "<";
             put UPCASE(NAME);
             put ">";
             break;

          finish:
             put "</";
             put UPCASE(NAME);
             put ">";
             break;
       end;

       define event SASColumn;
          start:
             put "<";
             put "COLUMN" /if cmp( XMLDATAFORM, "ATTRIBUTE");
             put " name=""" /if cmp( XMLDATAFORM, "ATTRIBUTE");
             put NAME;
             put """" /if cmp( XMLDATAFORM, "ATTRIBUTE");
             break;

          finish:
             break /if exists( MISSING);
             put " />" /if cmp( XMLDATAFORM, "ATTRIBUTE");
             put NL /if cmp( XMLDATAFORM, "ATTRIBUTE");
             break /if cmp( XMLDATAFORM, "ATTRIBUTE");
             put "</";
             put NAME;
             put ">";
             break;
       end;

       define event MLEVDAT;
          putq " missing=" MISSING;
          put " />" /if exists( MISSING);
          put NL /if exists( MISSING);
          break /if exists( MISSING);
          put " rawvalue=""" /if exists( RAWVALUE);
          put RAWVALUE /if exists( RAWVALUE);
          put """" /if exists( RAWVALUE);
          put " value=""" /if cmp( XMLDATAFORM, "ATTRIBUTE");
          put VALUE /if cmp( XMLDATAFORM, "ATTRIBUTE");
          put """" /if cmp( XMLDATAFORM, "ATTRIBUTE");
          break /if cmp( XMLDATAFORM, "ATTRIBUTE");
          put ">";
          put VALUE;
          break;
       end;

       private;
    end;
    run; quit;


