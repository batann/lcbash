# `lcbash` – A Modular Bash Function and Utility Library

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/batann/lcbash/images/logo.png">
    <img src="images/logo.png" alt="Logo" width="720" height="720">
  </a>
</div>




## Overview

`lcbash` is a modular and scalable collection of Bash utilities, functions, and scripts. It is designed for ease of use, maintainability, and efficient function sourcing. The project is split into manageable components, ensuring that each function can be sourced dynamically, and each file has a clear purpose.

## Project Structure

The project is organized into several directories:

- **`bashrc/`**:        Contains `.bashrc` files (located in the `files/` subdirectory). This is where you can experiment and develop custom shell configurations.
- **`dotfiles/`**:      Backup or template files for other dotfiles like `.vimrc`, `.bash_profile`, and others.
- **`misc/`**:          A collection of random scripts and utilities.
- **`functions/`**:     Each file in this directory contains a single function, written in foldable chunks for easy sourcing.
- **`README.md`**:      This file, providing documentation and system overview.

## Folder Breakdown

### `bashrc/`
- **`files/`**:         This directory contains the development and experimental `.bashrc` files. It is meant to be used for creating custom aliases, functions, and other shell configurations.

### `functions/`
- This directory holds the core functions for the system. Functions are written in foldable sections for easy sourcing and modular use. Functions are added here by naming each script after the function it performs (e.g., `str_trim.sh` for the `str_trim` function).

---

## Project Imperatives

### LC-Folds

- Folds-Level-1: #{{{ >>>   TITLE [>#N (marker and script relative line number)] \
                 #}}} [<#N (marker and script relative line number)]

- Folds-Level-2: #{{{ >>    TITLE [>#N (marker and script relative line number)] \
                 #}}} [<#N (marker and script relative line number)]

- Folds-Level-3: #{{{ >     TITLE [>#N (marker and script relative line number)] \
                 #}}} [<#N (marker and script relative line number)]



  <sub> *markers with script relative line numbers indicate the position
    of either Fold-beginning or Fold-ending within the current buffer and are
    detected,added and/or edited according to current positionby vim autocommand
    on :w execution.*</sub>

  <sub> *Folds Levels are from 1-3 are indicated by >>>
    1 = >>>
    2 = >>
    3 = >
    to maintain a consistent layout Space [ ] replaces each [>]*</sub>

#### vim auto command
###### positionning fold start
```
  autocmd BufWritePre * call UpdateFoldMarkersStart()
function! UpdateFoldMarkersStart()
    let l:save_cursor = getpos(".")
    let l:lines = getline(1, '$')
    for idx in range(len(l:lines))
        let line = l:lines[idx]
        if line =~ '#{{{' " Check for the first marker >#61
            if line =~ '>\#\d\+' " If the second marker with a number exists, replace the number
                let newline = substitute(line, '>\#\d\+', '>#' . (idx + 1), '')
            elseif line =~ '>\#' " If the second marker exists but no number, add the line number
                let newline = line . (idx + 1)
            else " If the second marker doesn't exist, add it with the line number at the end
                let newline = line . ' >#' . (idx + 1)
            endif
            call setline(idx + 1, newline)
        endif
    endfor
    call setpos('.', l:save_cursor)
endfunction
```

###### positioning fold end
```
    autocmd BufWritePre * call UpdateFoldMarkersEnd()
function! UpdateFoldMarkersEnd()
    let l:save_cursor = getpos(".")
    let l:lines = getline(1, '$')
    for idx in range(len(l:lines))
        let line = l:lines[idx]
        if line =~ '#}}}' " Check for the first marker >#10 <#82
            if line =~ '<\#\d\+' " If the second marker with a number exists, replace the number
                let newline = substitute(line, '<\#\d\+', '<#' . (idx + 1), '')
            elseif line =~ '<\#' " If the second marker exists but no number, add the line number
                let newline = line . (idx + 1)
            else " If the second marker doesn't exist, add it with the line number at the end
                let newline = line . ' <#' . (idx + 1)
            endif
            call setline(idx + 1, newline)
        endif
    endfor
    call setpos('.', l:save_cursor)
endfunction
```

---

### Dot files
 - Path: $HOME/lcbash/dotfiles


---

### Bashrc files
 - Path: $HOME/lcbash/bashrc


---

### Functions
 - Path: $HOME/lcbash/functions/

### Misc
 - Path: $HOME/lcbash/Misc

---

## How to Use

1. **Adding Functions:**
   - Functions are placed in the `functions/` directory. Each function should be contained within its own script file.
   - Functions are written in foldable blocks (i.e., `#{{{` for the start of a fold and `#}}}` for the end). This allows functions to be sourced dynamically based on need. >#108 <#108
   - Use the markers `>#` followed by the line number to specify where the fold begins for easier sourcing and navigation.

2. **Source a Function:**
   - To source a function, use:
     ```bash
     source /path/to/function/file.sh
     ```

3. **Modify `.bashrc`:**
   - You can experiment with your `.bashrc` files in the `bashrc/files/` directory. Remember that these files should contain your custom aliases, environment variables, and other Bash settings.

4. **Vim Auto-update:**
   - Whenever you modify a function file in Vim, the line numbers marked with `>#` are updated automatically to reflect the real line position.

---

## Utilities

### `lcbash/tools/`
- **`fold_find.sh`**:          Find the start and end lines of a function fold.
- **`fold_extract.sh`**:       Extract and source a function by name.
- **`fold_validate.sh`**:      Validate that all folds are correctly matched with start and end markers.

---

## Project Directory Structure

```
├── home                                     	│     ├── bashrc
│   └── batan                               	│     │   ├── bash_logout
│       ├── .config                          	│     │   ├── bash_profile
│       │   ├── dunst                        	│     │   ├── bashrc
│       │   ├── i3                           	│     │   ├── bashrc.aliases
│       │   ├── kitty                        	│     │   ├── bashrc.batan
│       │   ├── lc-bashrc                    	│     │   ├── bashrc.finish
│       │   ├── lc-clipboard                 	│     │   ├── BASHRC.FIRST.LC
│       │   ├── lc-fm                        	│     │   └── bashrc.navigation
│       │   ├── lc-gutenberg                 	│     ├── dotfiles
│       │   ├── lc-mashpodder                	│     │   ├── fzf.bash
│       │   ├── lc-ranger                    	│     │   ├── inputrc
│       │   ├── lists                        	│     │   ├── lc-cd
│       │   ├── menu-100-scripts             	│     │   ├── lc-sign
│       │   ├── picom                        	│     │   ├── megarc
│       │   ├── polybar                     	│     │   ├── music.kdl
│       │   ├── ranger                      	│     │   ├── oolite-starter.conf
│       │   ├── rofi                         	│     │   ├── profile
│       │   ├── simple_menu                 	│     │   ├── rss.opml
│       │   ├── tmux                        	│     │   ├── taskrc
│       │   ├── vim                          	│     │   ├── tkremind
│       │   ├── vimwiki                      	│     │   ├── tmux.conf
│       │   ├── YAD                          	│     │   ├── vimrc
│       │   └── yt-dlp                       	│     │   ├── Xauthority
│       ├── lcbash                           	│     │   ├── xboardrc
│       │   ├── bashrc                       	│     │   ├── Xdefaults
│       │   ├── dotfiles                     	│     │   ├── xinitrc
│       │   ├── functions                    	│     │   └── Xresources
│       │   ├── images                      	│     ├── functions
│       │   ├── misc                         	│     ├── images
│       │   └── tools                        	│     │   └── logo.png
│       ├── .task                            	│     ├── lcbash.wiki
│       │   ├── assets                       	│     ├── misc
│       │   ├── hooks                        	│     ├── README.md
│       │   └── notes                        	│     └── tools
│       └── .vim                             	│         └── fold_find.sh
│           ├── pack                         	│
│           │   └── plugins                  	│
│           │       └── start                	│
│           │           ├── moonfly.nvim     	│
│           │           ├── vim-taskwarrior  	│
│           │           └── vimwiki          	│
│           └── templates                    	│
└── media                                    	│
    └── batan                                	│
        ├── 100                              	│
        ├── 200                              	│
        ├── 300                             	│
        ├── 400                              	│
        ├── 500                              	│
        ├── 600                              	│
        ├── 700                              	│
        ├── 800                              	│
        └── 900                               |
```

---

## Development Notes

- **Modularization**:         The project is designed with modularity in mind. Functions can be added and updated without interfering with other parts of the project.
- **Easy Sourcing**:          With the fold system and the `>#` markers, sourcing functions is made simple and efficient.

---

