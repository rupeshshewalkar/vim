Vim Editor 
----------------------

1. Seek Help


"============================================================================
" Start reading Vim's built-in help manual
"============================================================================

    :help


"============================================================================
" Get help for a particular <topic>
"============================================================================

    :help <topic><CR>


"============================================================================
" Get a list of all help topics containing <string>
"============================================================================

    :help <string><TAB>


"============================================================================
" Search exhaustively through manual for every instance matching <pattern>
"============================================================================

    :helpgrep <pattern>

	:cnext --> go to next search
	:cprev --> go to previous search
	:cnfile --> go to search next file 
	:cpfile --> go to search previous file
	
	
-Within help files, anything in vertical bars is a hyperlink
-When the cursor is within bars, hit CTRL- ] to jump to that topic
-At any time, hit CTRL- T to back out of the sequence of links
	
"============================================================================
" Put in .vimrc Use arrow keys to navigate after a :vimgrep or :helpgrep
"============================================================================

    nmap <silent> <RIGHT>         :cnext<CR>
    nmap <silent> <RIGHT><RIGHT>  :cnfile<CR><C-G>
    nmap <silent> <LEFT>          :cprev<CR>
    nmap <silent> <LEFT><LEFT>    :cpfile<CR><C-G>

"============================================================================
" Make :help appear in a full-screen tab, instead of a window
"============================================================================

    "Only apply to .txt files...
    augroup HelpInTabs
        autocmd!
        autocmd BufEnter  *.txt   call HelpInNewTab()
    augroup END

    "Only apply to help files...
    function! HelpInNewTab ()
        if &buftype == 'help'
            "Convert the help window to a tab...
            execute "normal \<C-W>T"
        endif
    endfunction
	
"============================================================================
" Get a list of Normal mode commands and Insert mode commands
"============================================================================

    :help normal-index
    :help insert-index
	
-------------------------------------------------------------------------------------------------------------------------------


2. Learn move


"============================================================================
" Find out about configuring the statusline ruler
"============================================================================

    :help rulerformat


"============================================================================
" Find out about configuring the entire statusline
"============================================================================

    :help statusline


	
	
"============================================================================	
	Keep track of where you are
"============================================================================	

- Type <CTRL-G> to be told where you are
- Type g<CTRL-G> to be told more precisely where you are

Better still, enable the ruler :
: set ruler

Shows: <line>, <column> <percentage>
But the information the ruler shows is highly configurable

See: :help rulerformat
: help status line


"============================================================================	
 Motion commands
"============================================================================	
	
- Most vim users know the basic cursor motions of Normal mode

   k
h      l	
   j
	
	
- To move forward to the start of the next word : w
- To move backwards to the start of the previous word: b
- To move forward to the end of the next word : e
- To move backwards to the end of the previous word: ge
- "Words" are considered anything that is delimited by non-identifier characters

Line and paragraph motions
- To move to the start of the current line; 0 zero character!
- To move to the start of the first word of the current line: ^
- To move to the end of the current line: $
- To move to the start of the next line: <CR>
- To move to the start of the previous line: - minus sign!
- To move to the start of the current paragraph: {
- To move to the end of the current paragraph: }
A paragraph is delimited by empty lines not blank lines


- To move to the top of the buffer: gg
- To move to the end of the buffer : G
- To move to line n: nG or ngg
- To move to the point p percent through the buffer: g%

Matching delimiters

- To move the matching bracket of a {... }, (...), or [...] pair: %
- By default, vim only matches {...}, (...), and [...]
- But you can extend that to whatever pairs you like:

set matchpairs+=<:>,«:»

- Even to "pairs" that aren't normally considered pairs:

set matchpairs+==:;


Scrolling a large buffer

- To scroll Forward one window 's worth of text: <CTRL - F>
- To scroll Dow n a half window 's worth of text: <CTRL - D>
- To scroll Up one half window's worth of text: <CTRL - U>
- To scroll Back a full window's worth of text : <CTRL - B>

"============================================================================	
	Control your insertions
"============================================================================	

-In either editing mode most characters you type insert that character...but not all
- Many of the control characters have special insertion behaviours
- CTRL-Y dulicates the character in the same column on the preceding line
- CTRL-E duplicates the character in the same column on the following line
- CTRL-A inserts again whatever the most-recent inserted text was
- CTRL-R inserts the contents of a register more later!
- CTRL-R= evaluates an expression and inserts the result
- CTRL- T inserts a tab at the start of the line without moving the insertion point!
- CTRL -V inserts the next character verbatim even if it's normally a controlcharacter!
- CTRL-W deletes the word preceding the cursor
- CTRL-0 takes you back to Normal mode for one command

-------------------------------------------------------------------------------------------------------------------------------

3. Search 

"============================================================================	
	Search Smarter
"============================================================================	


- To search for instances of a regex within a buffer:
	/ <pattern><CR>

- Takes you to the next piece of text after the cursor that matches the pattern or To go the second match, either
	/ < CR>
	
- Or more commonly!: n

- To search backwards:
	? <pattern>< CR>

-And backwards again:
	N
-Can configure vim to wrap the search around from the end of the buffer back to the start:
	: set wrapscan

-Regexes used are sed-ish, but vim extends them considerably

"============================================================================	
	Searching within a line
"============================================================================	
Often you just want to jump to a particular character within the current line

- There's a short-cut for that: f <char > (for "find" )
- For example:
 fu

 You can jump backwards too: F<char>

"============================================================================	
	Searching for existing word
"============================================================================	

 If you have the word you mant to search for already under your cursor...
...just type * to find the next instance
...or # to find the previous

"============================================================================
	Case (in)sensitivity
"============================================================================

- Normally vim searches are case-sensitive

- If you would prefer case-insensitive set the following option:
	: set ignorecase

- Better still, if you'd like "partial sensitivity", set this option as well:

	:set smartcase

- Smartcase" overrides the "ignorecase" behaviour whenever your pattern includes any uppercase letters
- Even with these options set you can still be specific when you want
- Use the \c specifier within the pattern
- Everything after it within the pattern will be match case-insensitively, no matter what
- Likewise, there's \C to unconditionally turn case sensitivity on

"============================================================================
	Search parttern
"============================================================================	


-  The \< and \> subpatterns only match at the start/end of a word, for example:

            /\<for\>			
			
...matches "for", but not "fortune" nor "wherefor" nor "enforce"





Alternatives, synternatives, and sequences

    *  Alternatives are specified with: \|
       For example:

            /perl\|python\|php/

    *  "Synternatives" are alternatives where *both* sides have to match

    *  Specified with: \&

    *  The "and" equivalent of \|'s "or"

    *  For example, to find a line containing the word "Java" and the word "line":

            /.*Java\&.*line

    *  Sequences are successive characters that may be truncated at any point

    *  They are specified with: \%[...]

    *  Sequences are very handy when searching for terms that might be abbreviated

    *  For example:

            /fun\%[ction]

    *  ...is the same as:

            /fun\|func\|funct\|functi\|functio\|function

"=====================================================================================
	Search and replace
"=====================================================================================

