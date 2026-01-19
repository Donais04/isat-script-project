# ISAT Script Project Contribution Guide

welcome to the ISAT Script Project Contribution Guide! i've been working on making a version of the ISAT Script Project with the game's Japanese script, but it's a lot of work even with all the automating i tried to do, so i'm making this guide and asking for help in the hopes that someone decides to help out. if you decide to do so, thank you!

this guide will assume you have a GitHub account and know how to use Git and a code editor. if any of that isn't true but you'd still like to contribute, there are plenty of guides you can look up online.

contributing consists of looking through a bunch of lines and determining whether or not their automatically-matched translations look correct. if they do, great! mark it as such and move on. if they don't, either find the right translation yourself, or skip it and move on.
you can check as many or as little lines as you want to, and you don't have to correct any, either. you can also make a pull request now, and add to it or make more later. anything's welcome!

# how to contribute

start by making a fork of the japanese-translation branch (this one) and cloning the repository.

**`line_associations/`** has automatically-matched translations for each page that need to be checked to make sure they're correct. \
**`translations/`** has copies of those files that are already checked, or are in the process of being checked.

first, find a page to start checking. you can check in `translations/` to see if there are any unchecked ones. alternatively, you can copy a file from `line_associations/` to `translations/` to start working on a new page. to find lines that need to be checked, <kbd>Ctrl</kbd> <kbd>F</kbd> for lines that say `"checked": false`.

each unchecked line will look like this:
```json
{
    "en_raw": "original site line",
    "en_clean": "cleaned up site line",
    "jp_raw": ["list of all original japanese game line matches"] OR null,
    "jp_clean": ["list of all cleaned up japanese game line matches"] OR null,
    "checked": "false"
}
```
- if everything looks correct, great! change that "false" to "true" and move on.
- leave `en_raw` and `en_clean` unchanged, even if it looks wrong
    - if `jp_raw` has html formatting (eg. `<span class=\"shake\">`) where `en_raw` does not, leave it be[^mistakes]
- if `jp_raw` and `jp_clean` are lists or null:
    - if it's "(...)" exactly in the english version, make it that in the japanese version as well
    - otherwise, you'll need to find the line yourself.

to find the line yourself, check the game code. you can do this by searching through `Translations.json` most of the time, which is included in this repository. if this doesn't work, try opening up the game in RPG Maker MV[^rpgmaker] or searching through the contents of the files in the `www` directory[^search]. if you don't own the game and can't find the line in `Translations.json`, leave it unchecked and move on. if you've checked using all of those methods and still can't find the text, leave a note about it in your PR and move on.

when pulling lines from the game code, they'll often contain formatting codes. most of the important ones are listed [here](https://github.com/felikatze/isat-script-project/wiki/Plugins), and variables are listed [here](https://isatinformation.neocities.org/lists/isat/variables). these need to be either removed or reformatted to work with the site. you can often figure out what you're supposed to do based on the english version of the line, but regardless, here's some notes on how to reformat specific codes (note that backslashes will be doubled if you got them from a file instead of RPG Maker):
- if you see the unicode character U+200E (an invisible character, the left-to-right mark), leave a note about it and we'll get to it ourselves. these are a little more complicated and i don't feel like explaining it lol
- `\v[286]` is either a period or exclamation mark, depending on the act. the japanese translation uses the characters `！` and `。`. follow what the english line has written down. most of the time it'll be `[!/.]`, in which you should replace it with `[！/。]`. if it's just one or the other, replace it with the appropriate character - it's probably on an act-specific page where the value of the variable is known.
- `\{` turns into `<span class=\"big\">`
    - remember to close the span at the end of the line with `</span>`
    - if there's also a `\shake` or `\wave` that starts at the same spot, and there's no `\resetshake`, you can combine the classes into the same span, eg. `class=\"shake big\"`
- `\}` turns into `<span class=\"small\">`
    - remember to close the span at the end of the line with `</span>`
    - if there's also a `\shake` or `\wave` that starts at the same spot, and there's no `\resetshake`, you can combine the classes into the same span, eg. `class=\"shake small\"`
- `\.` generally turns into a non-breaking space or something. copy what you see in the english line
- `\|` generally turns into a couple non-breaking spaces or something. copy what you see in the english line
- `\!` or `\!\` can be removed if following a period. that last backslash in `\!\` is to escape the following space. if there's not a space after it, it's probably part of another code. if it's not following a period, it's probably a pause within a sentence, which is usually turned into a non-breaking space of some sort. copy what you see in the english line
- `\fb` turns into `<b>`. remember to close the tag at the end
- `\fi` turns into `<i>`. remember to close the tag at the end
- `\i[n]` is an icon, and is probably replaced with an image in the english line. copy what you see there
- `\shake` turns into `<span class=\"shake\">`. remember to close the span either at `\resetshake` or the end of the line with `</span>`
- `\wave` turns into `<span class=\"shake\">`. remember to close the span either at `\resetshake` or the end of the line with `</span>`
- `\m[choicevar]` is a variable containing Siffrin's dialogue. go look at the game code for this one and see what the variable gets set to. cannot be found in `Translations.json`.
- `\m[wishon]` turns into `<span class\"wish\">`. remember to close the span at the end if there's no `\m[wishoff]`
- `\m[wishoff]` closes the wish span
- the following can be removed without replacing: `\>`, `\<`, `\^`, `\n<x>`, `\lson`, `\lsoff`, `\lspi[n]`, `\lspiv[n]`, `\lsi[n]` `\m[slide]`, `\m[clear]`, `\m[v___]` where `___` is anything, `\m[rb]`, `\m[choicewait]`, `\m[choicespace]`
- if you see any of the following: `\c[n]`, `\oc[n]`, `\ow[n]`, `\fs[n]`, `\m[dot]`
    - if you're confident in what you think you should turn it into based on the english line, go ahead
    - if you're not confident, leave the line unchecked and move on
    - either way, leave a note about it in your PR so we can check it

once you've checked a line and confirmed it's correct, change the value of `"checked"` to `"true"`.

## when you're done

go ahead and push & commit your changes, and then make a pull request (PR) to the branch you forked. leave any comments you need to. we'll take a look at it when we get a chance, make any changes we need to, and then merge it when we've reviewed it. thank you for helping out!

[^mistakes]: leave us a note if you find anything like this! it usually means there's a mistake on the english site, and if you let us know about it, we'll add it to our list of typos to fix. you can let us know when you submit your PR.
[^rpgmaker]: generally a lot easier to do if you know where something is, but it takes some time to learn that. RPG Maker costs money and you can buy it [here](https://store.steampowered.com/app/363890/RPG_Maker_MV) on steam. it [goes on sale regularly](https://steamdb.info/app/363890) for 12 USD or less.
[^search]: easier to access but harder to read. you can do this with <kbd>Ctrl</kbd> <kbd>Shift</kbd> <kbd>F</kbd> in VSCode, and if you're using something else where that doesn't work, you probably know another way. if not, uh, look it up