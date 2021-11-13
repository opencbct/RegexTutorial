# A Tutorial Explaining a Specific RegEx 

Regular expressions are patterns used to match character combinations in strings.

## Summary

Construct a regular expression in one of two ways:

Using a regular expression literal, which consists of a pattern enclosed between slashes, as follows:
  let re = /ab+c/;

Regular expression literals provide compilation of the regular expression when the script is loaded. If the regular expression remains constant, using this can improve performance.
Or calling the constructor function of the RegExp object, as follows:
  let re = new RegExp('ab+c');

Using the constructor function provides runtime compilation of the regular expression. Use the constructor function when you know the regular expression pattern will be changing, or you don't know the pattern and are getting it from another source, such as user input.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors
Anchors are a different breed. They do not match any character at all. Instead, they match a position before, after, or between characters. They can be used to “anchor” the regex match at a certain position. The caret ^ matches the position before the first character in the string. Applying ^a to abc matches a. ^b does not match abc at all, because the b cannot be matched right after the start of the string, matched by ^. See below for the inside view of the regex engine.

Similarly, $ matches right after the last character in the string. c$ matches c in abc, while a$ does not match at all.

Useful Applications
When using regular expressions in a programming language to validate user input, using anchors is very important. If you use the code if ($input =~ m/\d+/) in a Perl script to see if the user entered an integer number, it will accept the input even if the user entered qsdf4ghjk, because \d+ matches the 4. The correct regex to use is ^\d+$. Because “start of string” must be matched before the match of \d+, and “end of string” must be matched right after it, the entire string must consist of digits for ^\d+$ to be able to match.

It is easy for the user to accidentally type in a space. When Perl reads from a line from a text file, the line break is also be stored in the variable. So before validating input, it is good practice to trim leading and trailing whitespace. ^\s+ matches leading whitespace and \s+$ matches trailing whitespace. In Perl, you could use $input =~ s/^\s+|\s+$//g. Handy use of alternation and /g allows us to do this in a single line of code.

Using ^ and $ as Start of Line and End of Line Anchors
If you have a string consisting of multiple lines, like first line\nsecond line (where \n indicates a line break), it is often desirable to work with lines, rather than the entire string. Therefore, most regex engines discussed in this tutorial have the option to expand the meaning of both anchors. ^ can then match at the start of the string (before the f in the above string), as well as after each line break (between \n and s). Likewise, $ still matches at the end of the string (after the last e), and also before every line break (between e and \n).

In text editors like EditPad Pro or GNU Emacs, and regex tools like PowerGREP, the caret and dollar always match at the start and end of each line. This makes sense because those applications are designed to work with entire files, rather than short strings. In Ruby and std::regex the caret and dollar also always match at the start and end of each line. In Boost they match at the start and end of each line by default. Boost allows you to turn this off with regex_constants::no_mod_m when using the ECMAScript grammar.

In all other programming languages and libraries discussed on this website , you have to explicitly activate this extended functionality. It is traditionally called “multi-line mode”. In Perl, you do this by adding an m after the regex code, like this: m/^regex$/m;. In .NET, the anchors match before and after newlines when you specify RegexOptions.Multiline, such as in Regex.Match("string", "regex", RegexOptions.Multiline).


### Quantifiers
Possessive quantifiers are a way to prevent the regex engine from trying all permutations. This is primarily useful for performance reasons. You can also use possessive quantifiers to eliminate certain matches.
You can make a quantifier possessive by placing an extra + after it. * is greedy, *? is lazy, and *+ is possessive. ++, ?+ and {n,m}+ are all possessive as well.
Basically, instead of X*+, write (?>X*). It is important to notice that both the quantified token X and the quantifier are inside the atomic group. Even if X is a group, you still need to put an extra atomic group around it to achieve the same effect. (?:a|b)*+ is equivalent to (?>(?:a|b)*) but not to (?>a|b)*. The latter is a valid regular expression, but it won’t have the same effect when used as part of a larger regular expression.

To illustrate, (?:a|b)*+b and (?>(?:a|b)*)b both fail to match b. a|b matches the b. The star is satisfied, and the fact that it’s possessive or the atomic group will cause the star to forget all its backtracking positions. The second b in the regex has nothing left to match, and the overall match attempt fails.

In the regex (?>a|b)*b, the atomic group forces the alternation to give up its backtracking positions. This means that if an a is matched, it won’t come back to try b if the rest of the regex fails. Since the star is outside of the group, it is a normal, greedy star. When the second b fails, the greedy star backtracks to zero iterations. Then, the second b matches the b in the subject string.

### OR Operator

### Character Classes
A character class matches only a single character. gr[ae]y does not match graay, graey or any such thing. The order of the characters inside a character class does not matter. The results are identical.
Character classes are one of the most commonly used features of regular expressions. You can find a word, even if it is misspelled, such as sep[ae]r[ae]te or li[cs]en[cs]e. You can find an identifier in a programming language with [A-Za-z_][A-Za-z_0-9]*. You can find a C-style hexadecimal number with 0[xX][A-Fa-f0-9]+.
The order of the characters inside a character class does not matter. gr[ae]y matches grey in Is his hair grey or gray?, because that is the leftmost match. We already saw how the engine applies a regex consisting only of literal characters. Now we’ll see how it applies a regex that has more than one permutation. That is: gr[ae]y can match both gray and grey.

### Flags

### Grouping and Capturing
By placing part of a regular expression inside round brackets or parentheses, you can group that part of the regular expression together. This allows you to apply a quantifier to the entire group or to restrict alternation to part of the regex.

Only parentheses can be used for grouping. Square brackets define a character class, and curly braces are used by a quantifier with specific limits.

### Bracket Expressions

### Greedy and Lazy Match

### Boundaries

### Back-references

### Look-ahead and Look-behind

## Author

Benjamin Kim 
https://github.com/opencbct/RegexTutorial/blob/main/README.md
opencbct@gmail.com