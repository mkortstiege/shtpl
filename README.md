  
## tinytpl - a tiny shell template engine

    syntax:
    #% shell-command
    #%# comment
    #%include file
    #slurp (removes trailing newline)
    
    options:
    -ass/-allow-subshell: does not mask $(, ´
    
    usage: sh -c "$( tinytpl [options] template )"
     
    (License: GPLv3+)

#### Installation:

    sudo make install

#### Example Templates: 
    $ cat testfile
      #%include testfile2
    
      #% for i in first "second\"" third; do 
      #% str=super; [ "$i" = "third" ] && str="not so super" 
         blubber test $i ist $str, #slurp
         keine'\\" frage
      #% done 
        blub!
    
      #%#include testfile2

    $ cat testfile2
      #% date="$( date )"
         $date
         $( date )
         ` date `
         ./*.sh 
         <( ls )

#### tinytpl's output:
    $ tinytpl testfile
    date="$( date )"
    printf "%s\n" "   $date"
    printf "%s\n" "   \$( date )"
    printf "%s\n" "   \` date \`"
    printf "%s\n" "   ./*.sh "
    printf "%s\n" "   <( ls )"
    printf "%s\n" ""
     for i in first "second\"" third; do 
     str=super; [ "$i" = "third" ] && str="not so super" 
    printf "%s" "   blubber test $i ist $str, "
    printf "%s\n" "   keine'\\\\\" frage"
     done 
    printf "%s\n" "  blub!"
    printf "%s\n" ""
    #include testfile2

#### Results in:
    $ sh -c "$( tinytpl testfile )"
      Sam Jun  2 22:59:29 CEST 2012
      $( date )
      ` date `
      ./*.sh
      <( ls )
    
      blubber test first ist super,    keine'\\" frage
      blubber test second" ist super,    keine'\\" frage
      blubber test third ist not so super,    keine'\\" frage
     blub!
    
