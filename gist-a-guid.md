# It's Gist a Guid... 


Don't worry. It doesn't bite.

This is a not-exaclyt-brief synopsis of using a "regular expression" (henceforth called  **regex**) to validate/enforce a globally unique identifier (henceforth called a **guid**). Note: Sometimes it is also called a universally unique ID (UUID), but GUID is what we'll be using (mostly because it sounds close to squid).

## Summary

There are two things we need to summarize for this to make sense.

 What makes up a GUID?

 What is the RegEx expression that we will use?

#### Part 1: What makes up a GUID?
---
 - Before all this... Why use a GUID in the first place?
 
  One of the most widely adopted uses for GUIDs are creating necessary unique identifiers (sometimes called keys) for relational databases. Since every row in an RDBMS requires a unique key, many developers use this format to ensure this uniqueness. Auto-incremented integers are more easily grasped/implemented, but they have a finite limit and are difficult to replicate/scale into a hybrid or cloud environment.

 The format has the following 4 basic conditions:
 1. It should be a 128-bit number.
 2. It should be 36 characters (32 hexadecimal characters and 4 hyphens) long.
 3. It should be displayed in five groups separated by hyphens (-).
 4. Microsoft GUIDs are sometimes represented with surrounding braces () or {}.

, PLUS 1 special format condition if you're going to make it work with almost anything....

5. The five groups mentioned in requirement 3, should be divided with the following character combination. (8)-(4)-(4)-(4)-(12)  **highly recommended**

Examples:

    Ex1: ABCDEF-1234567890AB-abcdef-012345-67
    Ex2: abcdef01012345678901aAbBcCdDeEfF
    Ex3: {abcdef01-0123-4567-8901-aAbBcCdDeEfF}

Example 1 is "technically" a GUID, but it doesn't follow with the Microsoft format, nor does it have the 5 groups in the preferred format.

Example 2 is more common. It has all the requirements for a GUID, but doesn't include the hyphens or the brackets. Some programs/databases prefer this format over the next example.

Example 3 has all the requirements for a GUID and in the preferred/highly recommended format for full compatibility with most platforms.

#### Part 2: RegEx Validation Expression
---

Here's the regex expression that we will use to validate a GUID: 

***^[({]?[a-fA-F0-9]{8}[-]?([a-fA-F0-9]{4}[-]?){3}[a-fA-F0-9]{12}[})]?$***

Nasty isn't it? Actually, it's not as bad as you might think, and we'll show you why.


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
Simply put, regex anchors are used to ensure that the FULL expression must be matched. The ^ and $ symbols in our expression signify the beginning and end of the string. They could be removed to create an unanchored string which could then be used to match partial strings, but since GUIDs are so specific in their format, it is much better to use the anchors.

To illustrate, given a string: 
    Input: "yadda{abcdef01-0123-4567-8901-aAbBcCdDeEfF}blah"
    
    RegEx without anchors = True
    RegEx with anchors = False

### Quantifiers
The quantifiers used in this expression are all GREEDY! Which really means that we are trying to match as many occurrences of the pattern as possible. Lazy quantifiers try to match as few occurrences as possible.

To illustrate, from our example:
    RegEx Partial: [({]?[a-fA-F0-9]{8}.....

    The single ? is looking for a possible match within the brackets. In this case, it's looking for a left curly bracket or parenthesis character either zero or one times.

    The {8} is looking for exactly 8 instances of a valid hexadecimal character.

So, using the following input examples:

    Input 1: [Qx99- 
    Input 2: (aBcDeF01 

    Input 1 would be False because it starts with a left bracket, it has an invalid hexadecimal character "Q", and it doesn't have exactly 8 characters.
    Input 2 would be True.

### OR Operator
We're not using any logical OR "||" elements in this expression, but we could if we wanted to.... You can research this further, if you need to.

### Character Classes
These can also be called "character sets". It's a way to match a specific "set" of characters in a given location. In our example, we use this extensively to make sure the characters are valid hexadecimal values.

Using our RegEx expression partial:
    Expression: [a-fA-F0-9]
    
    Valid matches are only 3 types. Uppercase A~F, lowercase a~f, and numbers 0~9.
    Input 1: 3  - Valid 
    Input 2: x  - Not valid

### Flags
We're not using any flags in this example, but if you're SUPER keen to learn about them, I suggest looking at: https://javascript.info/regexp-introduction#flags

### Grouping and Capturing
While we didn't have to, our expression uses grouping to make it more compact. We have a grouping of 3 sets of 4 characters in the middle of the GUID right? So, instead of typing out each one individually(which would work), we used the (3) option to group the middle 3 validations.

From our RegEx example: ...***(***[a-fA-F0-9]{4}[-]?***)***{3}...

The bolded ***()*** show that we are grouping this validation. We are requiring 4 hexadecimal characters, followed by an optional hyphen, AND we are grouping this all together and requiring it 3 times.

    Input 1: ..."a0b9-B4b2-9991"... is valid.
    Input 2: ..."A5b922310cdF"... is valid.
    Input 3: ..."a0z9-0908"... is not valid because it has invalid characters and does not have the required 3 groupings. 

### Bracket Expressions
Take a look at the character classes section above. It shows how we used the brackets [] to match the specific set of hexadecimal characters.

### Greedy and Lazy Match
Take a look at the quantifiers section above. It shows how we are using the greedy methods to match the GUID string.

### Boundaries
Since GUIDs don't use words, we don't get into matching whole words like you would if using boundaries.

### Back-references
Since the whole point of a GUID is to be unique, we don't really want our groups to be repeated. So, we won't be using any back references.

### Look-ahead and Look-behind
You may want to use this feature if you want to create/force a GUID to use/not use certain character sequences (like maybe you don't want 4 characters to be identical). This is really useful for software serial numbers where you might not want to use both 0 and the character O. In our case, since hexadecimal characters really don't have an overlap, it's not nearly as useful, so I won't go into it in depth here.

## Author

An overweight, balding IT developer with a penchant for coffee, a piquant wit, dual lazy eyes, and a lousy singing voice. Visit if you wish...
https://gist.github.com/Mohlenkamp
