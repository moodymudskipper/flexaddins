
<!-- README.md is generated from README.Rmd. Please edit that file -->

# flexaddins

There’s nothing here, only ideas.

## Installation

``` r
remotes::install_github("flexaddins")
```

## Motivation

Addins are great but have flaws :

  - Need a package, not straightforward to write/itterate
  - Impossible to memorize when you don’t use often, so often not used
    at all
  - No parameter

So in practice, you might think of a convenient addin, but you think
it’s not worth its own package or its own hotkey, so you don’t
implement it.

  - {flexaddins} has a single hotkey
  - It proposes a menu, as a popup or in the console depending on option
    `flexaddins.console = TRUE`
  - The items to choose from are decided through options depending on
    the selection, they work as follow:

<!-- end list -->

``` r
options(flexaddins.items = list(
  # if selection is a comment, replace it with upper case
  "Turn comments to upper case" = 
    grepl("^#.*?\n$", .) ~ 
    fleaxaddins::replace_selection(toupper(.)),
  # if selection is a summarize call, add .groups = "drop" to a summarize call
  "Add .groups = 'drop'" = 
    is.call(call <- str2lang(.)) && as.character(call[[1]]) == "" ~ 
    fleaxaddins::replace_selection(deparse(as.call(c(as.list(str2lang(.)), .groups = 'drop')))),
  # beep, by default
  "beep" = TRUE ~ beepr::beep(),
  # reprex of selection, mapped to existing addin, by default
  "render reprex" = TRUE ~ flexaddins::run_addin("reprex", "Reprex selection")
))
```

If the above is used then the option to beep will always be shown, the
rest will depend on the selection.

We’ll have the helpers `flexaddins::add_option()` and
`flexaddins::rm_option()` to make it easier.

Options can be set through the R Profile, through custom functions or
packages, or directly in the session.

Dedicated packages could set these options on attach and do nothing
else.

I don’t think we really need to attach the package in practice.

Some use cases on the top of my head :

  - translating comments from a language to another
  - call `debugonce` on selected function
  - open source of .Internal or .Primitive call using
    `pryr::show_c_source()`
  - suggest running `usethis::use_package("foo")` if we select
    `foo::bar`
  - if selection is an installed package name, update, or detach, or
    remove, or open CRAN page or repo

To address running addins with parameters the addin could call a second
prompt and we might have `flexaddins::reprompt()` to make this easy.

So in the end we get :

  - One hotkey only
  - An “intelligent” system, showing only options which may apply
  - A very flexible tool, easy to add an addin and it won’t be
    forgotten.
  - Because possible actions are shown only when relevant we’re not
    overwhelmed by choices.
  - hopefully we get features that are easy to share among users

Maybe also find a cuter and shorter name than {flexaddins} ?
