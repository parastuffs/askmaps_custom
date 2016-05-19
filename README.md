# Custom askmaps

This is a custom implementation of the TeX package *askmaps* from [Jesse op den Brouw](https://www.ctan.org/tex-archive/macros/latex/contrib/askmaps).

What it changes is twofold:
1. It corrects the Hamming distances of the 5 variables K-maps so that it's compliant with EPB/BEAMS courses.

   When using more than four variables, Hamming distances can not be respected anymore and the table needs to be split into 4x4 sub-tables.
   Still, n-cubes can span over sub-tables, but their aspect will vary depending on the Hamming distances.
   Two cells belonging to different sub-tables will be *adjacent* (within the meaning of Hamming distance) when they are superimposed after the sub-tables have either been folded or stacked. Jesse op den Brouw used folding, we prefer stacking.
   
   This was changed by updating `\put` coordinates in `askmapv`.
   
2. In the default implementation, each variable needs to be a single character.

   However, we would like set variables like y<sub>1</sub> (`$y_1$`), which is composed of five characters. To do so, we need to understand how the arguments are parsed by the package.
   
   The parsing is composed of three functions:
   - `askmapargumentstring`: get the argument and put it inside a dummy string.
   - `askmapgetonechar`: extract the first character of the dummy string and redefine said string without the extracted character.
   - `askmapgetchar`: get the next character of the argument defined in `askmapargumentstring` (internally calling `askmapgetonechar`).

   In order to allow for longer variables, we need a new method to separate them in the argument string.
   Let's choose a space (`\space` in TeX).
   We thus need to create three new similar functions, the most specific being `getonechar`.
   This new function will extract the first token (*i.e.* everything up to the space) from the dummy string.
   Note that a minor workaround has been needed: when defining the dummy string using `preparestr`, we need to append an extra token so that the last useful token of the argument can be processed by the character extraction function.
   
## How can I use this?
The only thing changing is the second argument.
The input variables now need to be separated by a space:
```
\askmapiv{$F=\overline{$y_2$}+\overline{a$y_1$}$}{$y_1$ $y_2$ a b}{}{1111110011110000}{}
```

### Note
The documentation is that of the original package, it has not been modified to reflect the changes.
   
   