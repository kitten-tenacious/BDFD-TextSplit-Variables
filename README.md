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

 ## Levelling System
- [XP & Level Checker](#level-up-checker)

---

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


So now we have made 6 variables, we can now modify the values, and use it accordingly.
If you ever want to add more variables, just add `.` (the splitter to split the values) and then add values for each variable. Remember to keep an eye on the values because since we're using Global variables, the maximum limit is 499 characters.

---

### $editSplitText

This is how we modify each values once a variable is split using $textSplit. This is how the function is used:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[Index;Value]
```

This function has 2 arguments, with first being "Index" and the second being "Value". 

#

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

#

#### Value

The **Value** of an index is the text you want to replace with.
The example for this can be seen above this section.

---

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

The output is: `I.love.Anime`

Never forget to use `$joinSplitText` inside `$setVar`. Make sure the joiner inside the argument should be whatever you're splitting the variable with, or it would throw an error later.

For example:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;200] $c[setting value for "money" as 200.]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
```

Always remember to use `$joinSplitText` inside $setVar with what you're splitting the variable with.

---

### $splitText

$splitText returns the value of the Index set inside the arguments. For example:

```markdown
$textSplit[I-Love-Anime;-]
$splitText[2]
```

It will return: `Love`.
This will be pretty useful when using TextSplit Variables and to return values of a specific Index.
Now let's try retrieveing the Value of our variable we created before:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
Money: $splitText[1]
Bank: $splitText[2]
```

The output will be:

`Money: 1000
Bank: 0`


Now that we've learned some functions of $textSplit and it's relatives, let's now try making the commands using these.

---

### Balance Command

This is how a simple Balance code would look like:

```markdown
$textSplit[$getVar[VARIABLE;$mentioned[1;yes]];.]
$if[$splitText[3]==0]
$editSplitText[3;1000]
$c[set the current bank limit value to 1000 if the value is 0. (thats the default, you can change it)]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
$endif
Balance: $$numberSeparator[$splitText[1]]
Bank: $$numberSeparator[$splitText[2]]/$$numberSeparator[$splitText[3]]
```

This command shows how much balance and Bank balance you have, along with Bank Capacity.
I also set if so that the current value of maximum bank limit is 0, it will be set to 1000 (you can change this value, or set in manually through the app and remove that part of the code).


Always remember to use:

```markdown
$textSplit[$getVar[VARIABLE;$authorID];.]
```

At the top of your code wherever you use TextSplit Variables, so the bot understands what variable to be split and to be retrieved.

---

### Deposit Command

Now let's see how we can make a Deposit command with TextSplit Usage. For this, start off with `$textSplit`:

```markdown
$nomention
$textSplit[$getVar[VARIABLE;$authorID];.]
$if[$splitText[3]==0]
$editSplitText[3;1000]
$c[set the current bank limit value to 1000 if the value is 0. (thats the default, you can change it)]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
$endif
$onlyIf[$sum[$splitText[2];$message]<=$splitText[3];You cannot exceed the bank limit.]
$onlyIf[$message<=$splitText[1];Your amount should not exceed how much money you have!]
$onlyIf[$isNumber[$message]==true;You can only deposit money in numbers.]
$onlyIf[$message>0;Your amount should be greater than 0.]
$onlyIf[$splitText[2]<$splitText[3];Your maximum Bank Limit reached. You cannot deposit anymore.]
$onlyIf[$splitText[2]!=$splitText[3];Your maximum Bank Limit reached. You cannot deposit anymore.]
$onlyIf[$splitText[1]>0;You don't have any money in balance to deposit.]
$onlyIf[$message<=$splitText[3];You cannot exceed your bank limit.]
$reply
$allowUserMentions[]
$username, you deposited **$message** into the bank.
$editSplitText[2;$sum[$splitText[2];$message]]
$editSplitText[1;$sub[$splitText[1];$message]]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
$c[always remember to use joinSplitText after setting values.]
```

That's it, pretty easy right? The withdraw command is easy too. Just like this, vice versa and not much limiters related to bank limit, since we're not gonna need that for withdrawing.

---

### Withdraw Command

A simple withdraw command would look like this:

```markdown
$nomention
$textSplit[$getVar[VARIABLE;$authorID];.]
$if[$splitText[3]==0]
$editSplitText[3;1000]
$c[set the current bank limit value to 1000 if the value is 0. (thats the default, you can change it)]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
$endif
$onlyIf[$splitText[2]>0;You don't have any money in the bank to withdraw.]
$onlyIf[$message<=$splitText[2];You cannot withdraw more than you have in the bank.]
$onlyIf[$message>0;Your amount should be greater than 0.]
$onlyIf[$isNumber[$message]==true;Your amount should be in numbers.]
$username, you withdrawn **$$message** from the bank.

$editSplitText[1;$calculate[$splitText[1]+$message]]
$editSplitText[2;$calculate[$splitText[2]-$message]]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
```

Just that's it, now you have a working withdraw, deposit and balance command with bank limit all in a single variable!
Now, let's learn how we can make Levelling System with the rest of the variables we made before.


---

Now let's see how we can make a Level Up checker, that checks if the user has reached X amount of exp, so that the user will advance to the next level, using the variables made inside the main variable.


### Level Up Checker

```markdown
$nomention
$textSplit[$getVar[VARIABLE;$authorID];.]
$if[$splitText[6]==0] $c[Since the default value is set 0, let's check if it's 0, the level is set to 1, and Required EXP is set to the amount you want. For me, I'm setting "100" as the Level 1 required EXP.]
$editSplitText[6;1]
$editSplitText[5;100] $c[required EXP set to 100 now.]
$setVar[VARIABLE;$joinSplitText[.];$authorID] $c[saving the variable]
$endif
$c[I'm also going to copy paste the "bank limit" checker below so it checks if it's 0, it sets to 1000. You can defaultly change the values of these variables right in the app so you don't want to put these lines of code everytime you make a command.]
$if[$splitText[3]==0]
$editSplitText[3;1000]
$c[set the current bank limit value to 1000 if the value is 0. (thats the default, you can change it)]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
$endif
$if[$splitText[4]>=$splitText[5]]
$c[if XP reaches or goes more than Required EXP]
$editSplitText[6;$sum[$splitText[6];1]] $c[Increases Level by 1 each time user levels up.]
$editSplitText[4;0] $c[EXP set to 0 once leveled up.]
$editSplitText[5;$sum[$splitText[5];100]] $c[Required XP set to +100 while every time user levels up.]
$setVar[VARIABLE;$joinSplitText[.];$authorID]
$endif
```

That's it, it was this simple. If you don't want to always copy paste the "bank limit" and Level checker everytime when making a new command, you can defaultely set the values to whatever you want in the app, so you don't want to get annoyed, like this:

#

![Click Me](https://cdn.discordapp.com/attachments/1349964745519665273/1350397269949808650/IMG_20250315_144614.jpg?ex=67d696f6&is=67d54576&hm=e468e2e318a52fbafedab398b188eec78b4cbab74fd098de0e637f361175dbfa&)

#

In this image, I've set default values of Bank Limit to 1000, Required EXP to 100 and Level to 1, so I don't want to copy paste the code to manually check it.

That's it, now let's focus on the final command: **Shop Command**.

---

Ok.
