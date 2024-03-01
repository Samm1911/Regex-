# Understanding Regex:

Are you diving into the world of web development like I am? If so, you've probably stumbled upon this thing called regex, or regular expression. Your first thought might be a confused '...Huh?...What?' Honestly, mood.

In this tutorial, I'll guide you through the rage-inducing world of regex, breaking down its components into simple, digestible chunks. I'll also throw in a bonus because I'm so generous: deciphering a regex used for matching emails.

Let's get started!

## Summary

A regex, or regular expression, is a sequence of characters that forms a search pattern. It's used mainly for pattern matching within strings or large bodies of text. It allows you to specify conditions or patterns that are then used to find, extract, or manipulate text you've targeted.

For this specific tutorial, we'll be deciphering a regex that is used to validate whether a string of text is a email or not:

`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

Before we start though, it's important to understand the common components used in creating a regex.

## Table of Contents

- [Regex Components](#regex-components)
  - [Anchors](#anchors)
  - [Quantifiers](#quantifiers)
  - [Bracket Expressions](#bracket-expressions)
  - [Character Classes](#character-classes)
  - [The OR Operator](#the-or-operator)
  - [Grouping Constructs](#grouping-constructs)
  - [Character Escapes](#character-escapes)
  - [Flags](#flags)
- [Deciphering Time](#deciphering-time)
- [Closing](#closing)
- [Resources](#resources)

## Regex Components

### Anchors

Anchors are special characters that act as constraints, telling the regex where to perform its matching function within a string.

It's important to note that they don't match to any actual characters, but instead define the boundaries or positions for the regex to do its matching.

The four most commonly used anchors are:

- **Caret** `^`: Tells the regex to match at the beginning of the string.

  Regex: `/^\w/g`

  Translation: "Matches any word character at the beginning of the string."

  Example: apples grow from trees

  Result: **a**pples grow from trees

- **Dollar sign** `$`: Tells the regex to match at the end of the string.

  Regex: `/\w$/g`

  Translation: "Matches any word character at the end of the string."

  Example: apples grow from trees

  Result: apples grow from tree**s**

- **Word boundary** `\b`: Tells the regex to match a position where the word is not surrounded by other characters.

  Regex: `/cat\b/g`

  Translation: "Matches all occurrences of the word 'cat' that are standalone words."

  Example: cat cats catwoman

  Result: **cat** cats catwoman

- **Not word boundary** `\B`: Tells the regex to match a position where the word is surrounded by other characters.

  Regex: `/cat\B/g`

  Translation: "Matches all occurrences of the word 'cat' that are not standalone words."

  Example: cat cats catwoman

  Result: cat **cat**s **cat**woman

### Quantifiers

Quantifiers tells the regex how many times a particular character or group of characters should appear in the text for a match to occur. They help the regex know if these characters or group of characters should be option, required, or repeated a certain number of times.

- **Asterisk** `*`: Tells the regex to match zero or more occurrences of the preceding character or character group.

  Regex: `/a*/g`

  Translation: Match any sequence of zero or more 'a' characters.

  Example: a aa aaa aaa aaaa

  Result: **a** **aa** **aaa** **aaa** **aaaa**

- **Plus** `+`: Tells the regex to match one or more occurrences of the preceding character or character group.

  Regex: `/a+/g`

  Translation: Match any sequence of one or more 'a' characters.

  Example: a aa aaa aaa aaaa

  Result: **a** **aa** **aaa** **aaa** **aaaa**

  - You might look at the results of the previous two quantifiers and think "Wait! Don't they both end up looking the same? What's the difference?" The difference lies in the requirement for the preceding character or character group. Specifically, `*` allows **zero** or more occurrences, while `+` requires **one** or more occurrences. Because `*` matches zero or more occurrences, it actually counts any empty string (i.e. where the character 'a' has occurred zero times, like the white spaces) as a match as well. On the other hand, `+` only results in a match if the preceding character or character group is an 'a' as well. An easier way of understanding the difference is that `+` is more strict that `*` when it comes to producing matches.

- **Question mark** `?`: Tells the regex that its preceding character can occur 0 or 1 times, essentially making that character optional. This I find is useful if you want to match words that have alternative spelling.

  Regex: `/colou?r/g`

  Translation: Match strings that contain the word 'color' followed by zero or one occurrences of the letters 'u'.

  Example: color colour

  Result: **color** **colour**

  - There are words with alternative spelling that will require a more robust regex configuration (e.g. center and centre). But for now the above example should help you understand the use of '?'

- **Curly braces** `{}`: Tells the regex to match a specific range or exact number of occurrences of the preceding element, depending on how you configure the quantifier.

  Regex: `/a{3}/g`

  Translation: Match strings that contain the character 'a' exactly 3 times.

  Example: a aa aaa aaaa aaaaa aaaaaa

  Result: a aa **aaa** **aaa**a **aaa**aa **aaaaaa**

  - Note: Because the last group of 'a's has a length of 6, the matching will occur twice there, resulting in it looking like the whole character group was matched even though we specified the regex to only match 3 times. This is because quantifiers are 'greedy' by default, meaning the regex will attempt to match the maximum number of occurrences.

  Regex: `/b{3,}/g`

  Translation: Match strings that contain the character 'b' 3 or more times.

  Example: b bb bbb bbbb bbbbb bbbbbb

  Result: b bb **bbb** **bbbb** **bbbbb** **bbbbbb**

  Regex: `/c{3,4}/g`

  Translation: Match strings that contain the character 'c' with a minimum of 3 times and a maximum of 4 times.

  Example: c cc ccc cccc ccccc cccccc

  Result: c cc **ccc** **cccc** **cccc**c **cccc**cc

### Bracket Expressions

Bracket expressions refers to any expression enclosed within square brackets `[]` that results in a character class (I'll touch on character class in the next section). They allow you to manually configure what type of characters to match or not match.

Know that **most** metacharacters (i.e. characters that have a special function in regex) lose their function when inside a `[]` and become a literal character instead.

Some common use of bracket expressions are:

- **Matching specific characters** `[]`: Match any characters within the `[]`

  Regex: `/[aeiou1357@]/g`

  Translation: Match any occurrences of 'a' 'e' 'i' 'o' 'u' '1' '3' '5' '7' '@'.

  Example: abc 123 áéí !@#

  Result: **a**bc **1**2**3** áéí !**@**#

- **Negating specific characters** `[^]`: Match any other characters aside from 'a' 'e' 'i' 'o' 'u' '1' '3' '5' '7' '@'.

  Regex: `/[^aeiou1357@]/g`

  Translation: Match any characters in this set.

  Example: abc 123 áéí !@#

  Result: a**bc** 1**2**3 **áéí** **!**@**#**

  - Because white spaces are also considered characters, the above regex would actually results in 11 matches. If you want the regex to also ignore white spaces, just add a ' ' (i.e. `/[^aeiou1357@ ]/g`)

- **Establish a range of characters** `[-]`: Defines a range of characters for the regex to match.

  Regex: `/[a-c0-5]/g`

  Translation: Match any characters within the range of 'a' to 'c' and '0' to '5'.

  Example: abcdefg12345678

  Result: **abc**defg**12345**678

### Character Classes

Character classes are similar to bracket expressions, but they are predefined patterns so you don't need to wrap them around a '[]'.

Commonly used character classes are:

- **Word** `\w`: Equivalent to `[a-zA-Z0-9_]`. Tells the regex to match any alphanumeric characters and underscores. Note that \w will not match any accented characters or non-roman characters.

  Example: abc 123 áéí !@#

  Result: **abc** **123** áéí !@#

- **Not word** `\W`: Equivalent to `[^a-zA-z0-9_]`. Tells the regex to match any characters that are not alphanumeric or underscores.

  Example: abc 123 áéí !@#

  Result: abc 123 **áéí** **!@#**

  - Note: It will also match the white spaces between the character groups. In the example above, `\W` will result in 9 matches.

- **Digit** `\d`: Equivalent to `[0-9]`. Tells the regex to match any digits.

  Example: abc 123 áéí !@#

  Result: abc **123** áéí !@#

- **Not digit** `\D`: Equivalent to `[^0-9]`. Tells the regex to any characters that are not digits.

  Example: abc 123 áéí !@#

  Result: **abc** 123 **áéí** **!@#**

  - Note: Again, it will also match the white spaces between the character groups. In the example above, `\D` will result in 12 matches.

- **White space** `\s`: Equivalent to `[\t\n\r\f\v]`. Tells the regex to match any white space characters (i.e. tab, newline, carriage return, form feed, and vertical tab characters). It's generally preferred to just use \s.

  Example: abc 123 áéí !@#

  Result: abc 123 áéí !@#

  - Note: You can't see any differences between the example and the result because I don't even know if I can bold white spaces, but in the example above, `\s` will result in 3 matches.

- **Not white space** `\S`: Equivalent to `[^\t\n\r\f\v]`. Tells the regex to match any characters that are not white space characters (i.e. tab, newline, carriage return, form feed, and vertical tab characters). It's also generally preferred to just use \S.

  Example: abc 123 áéí !@#

  Result: **abc** **123** **áéí** **!@#**

You can wrap character classes inside a bracket expression to for further customization. For example, if you wrap /s and /S inside a bracket, the regex will match EVERYTHING!!! (i.e. `/[\s\S]/g`)

### The OR Operator

The OR operator, denoted by the `|` symbol, give a lits of choices for the regex to match.

For example, if you want write a regex that matches either "cat" or "dog" in a string, you would use `/cat|dog|/g`.

Which would result in:

**cat** bat **dog** cog **dog** log sat **cat**

Interestingly, you're not limited to only two choices. If you want to add in more alternative to the regex, simply add another `|` and then the next alternative e.g. `/cat|dog|log/g`.

Although our email matching regex does not use the OR operator, it's still good to understand what it does in case you ever want to use it when creating you own regex.

### Grouping Constructs

Grouping constructs allow you to group parts of a matching pattern together. It helps you organize and structure you regex and allows you to extract an manipulate specific groups of match strings and create more complex patterns.

Capturing group is done by wrapping parts of your regex with a `()`. For example, if I want to capture the area code of a phone number in order to gauge where my business is getting the most calls from, I would first create a regex to match a phone number:

`^\d{3}-\d{3}-\d{4}$`

Translation: Assert the start of the string. Then match exactly three digits. Match a hyphen. Match another three digits. Match another hyphen. Now match 4 digits. Assert the end of the string.

Then I would add a `()` around `\d{3}` so that I capture the area code.

`^(\d{3})-\d{3}-\d{4}$`

And there you have it. When I match a phone number, I can later call on the captured group to extract the area code of the phone number and do something to it in JavaScript.

          const phoneNumber = "123-456-7890";
          const regex = /^(\d{3})-\d{3}-\d{4}$/;
          const match = phoneNumber.match(regex);
          const areaCode = match[1]; // Extracted area code using the numerical index

You can also name you capture groups. This is especially helpful if you have multiple capture groups that you want to manipulate later and don't exactly remember the index of the group you want to capture.

Naming you captured group is done by adding `?<`name`>` before you configure you regex for the captured group.

`^(?<areaCode>\d{3})-\d{3}-\d{4}$`

Now, when you extract it, you can simply call on the name to get the area code of the phone number.

          const phoneNumber = "123-456-7890";
          const regex = /^(?<areaCode>\d{3})-\d{3}-\d{4}$/;
          const match = phoneNumber.match(regex);
          const areaCode = match.groups.areaCode; // Extracted area code using the named group

To further organize you regex, you can wrap parts of it in something called a non-capturing group. They are similar to a capture group, except they do not capture the matched content, and you wont be able to extract and manipulate it later.

To add a non-capturing group, you can wrap the part of the regex that you need to match but not use in a (?:)

`/^(?<areaCode>\d{3})-(?:\d{3}-\d{4})$/`

Now the regex will match a phone number, but will only allow me to extract and manipulate the name captured group of 'areacode' and "discard" the rest.

### Character Escapes

Some character have embedded functions within the regex syntax (i.e. `+ * ? ^ $ \ . [ ] { } ( ) | /`). We've actually seen them being used throughout this tutorial.

But what happens if you want to make a regex that matches one or more of these "metacharacters". That's where character escapes come in.

Character escapes allows you to tell the regex to treat these characters literally.

All you need to do it add a `\` before the metacharacter

For example, `\.` would tell regex to treat `.` as a literal period instead of treating it as a metacharacter that matches any character except for newline.

It's **LITERALLY** that simple! Get it? HA HA HA HA HA HA HA HA HA HA! I SAID LAUGH!!!

### Flags

Flags are like instruction you put at the end of the regex to tell it how to behave when matching. There are more than the five I'm going to show you here, but these are the more commonly used ones.

- **Global** `g`: This flag will make the regex find all matches rather than stopping after finding the first match.

- **Insensitive** `i`: This flag allows the regex to find matches regardless of letter case.

- **Multiline** `m`: This flag allows the `^` and `$` anchor to match the start and end of every line.

- **Sticky** `s`: This flag allows the `.` metacharacter to match any character, including newline characters.

- **Unicode** `u`: This flag enables support for Unicode character and properties.

You are not limited to just using one flag when creating you regex. You can stick them all in your regex and it'll still work (e.g. `/a*/gimsu`). But do make sure the flags serves the purpose of your regex or you'll just be wasting precious milliseconds.

## Deciphering Time

Now that we've covered the basics of regex, we can finally start deciphering the email match regex shown at the beginning.

Here it is again:

/^([a-z0-9_\\.-]+)@([\\da-z\\.-]+)\\.([a-z\\.]{2,6})$/

First, there are the caret and dollar sign [anchors](#anchors):

/`^`([a-z0-9_\\.-]+)@([\\da-z\\.-]+)\\.([a-z\\.]{2,6})`$`/

The caret tells the regex to match from the beginning, and ensure that the email address starts with the specified pattern.

The dollar sign tells the regex to match until the end of the string, and ensures that the email address ends with the specified pattern.

Together, they tell the regex to make sure that the entire string matches the pattern.

Next we can see that the regex has created three unnamed [capture groups](#grouping-constructs):

/^`(`[a-z0-9_\\.-]+`)`@`(`[\\da-z\\.-]+`)`\\.`(`[a-z\\.]{2,6}`)`$/

Let's start with the first capture group:

([a-z0-9_\\.-]+)

The first capture group uses a [bracket](#bracket-expressions) `[]` to configure a custom character class.

(`[`a-z0-9\_\\.-`]`+)

Inside the bracket expression, it specific two ranges; a-z and 0-9.

([`a-z``0-9`_\\.-]+)

This tells regex to match any lowercase letters and digits.

It's also tell regex to match any underscores `_`.

([a-z0-9`_`\\.-]+)

Then by using [character escapes](#character-escapes), it tell regex to treat the period `.` literally.

([a-z0-9_`\.`-]+)

The bracket ends with telling regex to match dash `-`.

([a-z0-9_\\.`-`]+)

Finally we have a [plus sign](#quantifiers), telling regex that the previous component (the character set) can occur one or more times.

([a-z0-9_\\.-]`+`)

Together, the first capture group wants regex to "Match any sequence of lowercase letters, digits, underscores, dots, and dashes."

So far so good? Awesome! Let's keep going.

Next, we see the at sign `@`. Nothing special here, it just mean we want to match a `@`.

/^([a-z0-9_\\.-]+)`@`([\\da-z\\.-]+)\\.([a-z\\.]{2,6})$/

Onto the second capture group:

([\\da-z\\.-]+)

It's mostly the same as the first capture group. Except this time the pattern uses the [character class](#character-classes) of `\d` instead of a range to match any digits.

([`\d`a-z\\.-]+)

Aside from the underscore `_` not being included, this group is almost identical to the first capture group in terms of what it wants regex to match.

Basically: "Match any sequence of lowercase letters, digits, dots, and dashes.

The second capture group is followed by a [character escape](#character-escapes) and a period `.`. Like the ones we see in the capture groups we've deciphered so far, it's telling regex to treat the period literally.

We're almost done!

The third capture group looks like this:

([a-z\\.]{2,6})

Here we see another [bracket expression](#bracket-expressions) that matches any lowercase letter from a to z, as well as an escaped period `.`

(`[a-z\\.]`{2,6})

This group also [quantifies](#quantifiers) the number of time the specified characters in the bracket expression can occur. In this case, in order to match, the number of occurrences must fall between a minimum of 2 and a maximum of 6.

With everything taken together. the whole regex translate to:

"The string should start with a group of characters that can include lowercase letters, digits, underscores, periods, and hyphens, followed by an at symbol. Then, it is followed by another group of characters that can include digits, lowercase letters, periods, and hyphens, followed by a period. Finally, it is followed by a group of lowercase letters and periods that has a length between 2 and 6 characters. The string should end after this."

## Closing

And there you have it! After 6 years, 2 failed marriages, and a burning desire to throw your computer against a wall, you've finally learned the basic of regex configuration, and successfully decipher a regex used to match emails!

This guide ended up being a lot longer than I had initially planned. I made this guide having just started learning about regex, so I wrote this from the perspective of someone who has never used or heard of regex and wanted as much guidance as possible. It's much a guide for you as it is for me.

With this being a beginner's guide, there's still a lot about regex we didn't cover here. Things like backreference constructs, boundaries, and lookaround assertion are far outside the scope of the guide.

However, I hope that this has help ease you into getting comfortable with regex and made it just a little easier for you. Good luck on your web development studies!

## Resources

[Regular Expressions](https://eloquentjavascript.net/09_regexp.html)

[MDN Web Docs on regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

[Quick-Start: Regex Cheat Sheet](https://www.rexegg.com/regex-quickstart.html)

[Regex101](https://regex101.com/)

[Untitled Pattern](https://regexr.com/)

## Author

[Hi! I'm Sam. I'm studying web developmen, and this is my GitHub profile.](https://github.com/samm1911)
