# BDFD-TextSplit-Variables

This guide will teach how you can store multiple variables inside a single variable, with the use of some functions. This way you can exceed the amount of maximum amount of variables by storing much information inside a variable.


In this guide we'll learn to setup some basic economy commands along with other useful commands a normal bot would have (levelling up, shop, balance, deposit and withdraw + bank limit).
I'm looking forward to see people actually understanding what this Guide is, so I will do my best to explain how everything based on "TextSplit" Variables works.

 ## TextSplit Functions
- [$textSplit](#textsplit)
- [$editSplitText](#editsplittext)
- [$joinSplitText](#joinsplittext)
- [$splitText](#splittext)

 ## Balance, Deposit & Withdraw
- [Balance Command](#balance-command)
- [Deposit Command](#deposit-command)
- [Withdraw Command](#withdraw-command)



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

Never forget to use `$joinSplitText` inside `$setVar`. Make sure the joiner inside the argument should be whatever you're splitting the variable with, or it would throw an error later.

For example:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;200] $c[setting value for "money" as 200.]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
```

Always remember to use `$joinSplitText` inside $setVar with what you're splitting the avriable with.



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

### Balance Command

This is how a simple Balance code would look like:

```markdown
$textSplit[$getVar[VARIABLE;$mentioned[1;yes]];.]
Balance: $numberSeparator[$splitText[1]]
Bank: $numberSeparator[$splitText[2]]/$numberSeparator[$splitText[3]]
```

This command shows how much balance and Bank balance you have, along with Bank Capacity.

Always remember to use:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
```

At the top of your code wherever you use TextSplit Variables, so the bot understands what variable to be split and to be retrieved.


### Deposit Command

Now let's see how we can make a Deposit command with TextSplit Usage. For this, start off with `$textSplit`:

```markdown
$nomention
$textSplit[$getVar[VARIABLE;$authorID];.]
$onlyIf[$isNumber[$message]==true;You can only deposit money in numbers.]
$onlyIf[$message>0;Your amount should be greater than 0.]
$onlyIf[$splitText[2]<$splitText[3];Your maximum Bank Limit reached. You cannot deposit anymore.]
$onlyIf[$splitText[2]!=$splitText[3];Your maximum Bank Limit reached. You cannot deposit anymore.]
$onlyIf[$splitText[1]>0;You don't have any money in balance to deposit.]
$reply
$allowUserMentions[]
$username, you deposited **$message** into the bank.
$editSplitText[2;$sum[$splitText[2];$message]]
$editSplitText[1;$sub[$splitText[1];$message]]
$setvar[VARIABLE;$joinSplitText[.];$authorID]
$c[always remember to use $joinSplitText after setting values.]
```

That's it, pretty easy right? The withdraw command will be easy too. Just like this, vice versa and not much limiters related to bank limit, since we're not gonna need that for withdrawing.

### Withdraw Command

A simple withdraw command would look like this:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
