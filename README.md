# BDFD-TextSplit-Variables

This guide will teach how you can store multiple variables inside a single variable, with the use of some functions. This way you can exceed the amount of maximum amount of variables by storing much information inside a variable.


In this guide we'll learn to setup some basic economy commands along with other useful commands a normal bot would have (levelling up, shop, balance, deposit and withdraw + bank limit).
I'm looking forward to see people actually understanding what this Guide is, so I will do my best to explain how everything based on "TextSplit" Variables works.

## TextSplit Functions 
- $textSplit
- $editSplitText
- $joinSplitText
- $splitText



### $textSplit

This is the very beginning of using a Textsplit variable.
For this, first of all you have to use the $textSplit function to split the values of a variable.
You'll have to make a variable named anything preferable to you. This will be the variable we'll use to make multiple variables. This is how the function would look like, when splitting the values inside the variable:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
```

Since the variable we're using will have multiple variables stored, this is how the default value should look:

![Click Me](https://cdn.discordapp.com/attachments/1349964745519665273/1349964786217259048/IMG_20250314_094112.jpg?ex=67d5042e&is=67d3b2ae&hm=a9a67c1e2e2b10f325f442ea4294ca80e308b3be67c5f770be763724c15c1600&)


This is how the value should look like, with each variables getting split by `.`

Each 0 represent each variable, so let's just memorize what each variable is.

- The first two zeroes are "money" and "bank".
- The next two zeroes are "bank limit" and "xp" (xp of user).
- The rest of the zeroes are "req-exp" (required exp) and "level" (current level).


So now we have made 6 variables, we can now modify the values, and use it acocordingly.
If you ever want to add more variables, just add `.` (the splitter to split the values) and then add values for each variable. Remember to keep an eye on the values because since we're using Global variables, the maximum limit is 499 characters.



### $editSplitText

This is how we modify each values once a variable is split using $textSplit. This is how the function is used:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[Index;Value]
```

This function has 2 arguments, with first being "Index" and the second being "Value". 

#### Index

An index of a variable is easy to learn. Suppose you have to edit the value of "money" variable, stored inside the main variable.

Since the first 0 is "money", we're going to use:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;1000]
```

This sets the value of the "money" variable to "1000". So now we have the "money" variable with value "1000".
An **Index** is a number that tells you which part of a split you want to use/modify.
Since "money" is 1, "bank" is 2, "bank-limit" is 3 and so on, each split has a number.



#### Value

The **Value** of an index is the text you want to replace with.
The example for this can be seen above this section.



### $joinSplitText

$joinSplitText joins the split text with whatever you specify inside the value. Usage:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;1000]
$joinSplitText[.]
```

For example, if I use:

```markdown
$textSplit[I-love-Anime;-]
$joinSplitText[.]
```

The output would be: `I.love.Anime`


### $splitText

$splitText returns the value of the Index set inside the arguments. For example:

```markdown
$textSplit[I-Love-Anime;-]
$splitText[2]
```

It would return: `Love`.
This will be pretty useful when using TextSplit Variables and to return values of a specific Index.
Now let's try retrieveing the Value of our variable we created before:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
Money: $splitText[1]
Bank: $splitText[2]
```

The output would be:

`Money: 1000
Bank: 0`


Now that we've learned some functions of $textSplit and it's relatives, let's now try making the commands using these.

