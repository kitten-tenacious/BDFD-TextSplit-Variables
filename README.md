# BDFD-TextSplit-Variables

This guide will teach how you can store multiple variables inside a single variable, with the use of some functions. This way you can exceed the amount of maximum amount of variables by storing much information inside a variable.


In this guide we'll learn to setup some basic economy commands along with other useful commands a normal bot would have (levelling up, shop with buy, balance, deposit and withdraw + bank limit).
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

 ## Shop & Buy Commands
- [Shop Command](#shop-command)
- [Buy Command](#buy-command)

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
$onlyIf[$isNumber[$message]==true;You can only deposit money in numbers.]
$onlyIf[$message!=;Include how much do you want to deposit.]
$onlyIf[$message<=$splitText[1];Your amount should not exceed how much money you have!]
$onlyIf[$message>0;Your amount should be greater than 0.]
$onlyIf[$splitText[2]<$splitText[3];Your maximum Bank Limit reached. You cannot deposit anymore.]
$onlyIf[$splitText[2]!=$splitText[3];Your maximum Bank Limit reached. You cannot deposit anymore.]
$onlyIf[$splitText[1]>0;You don't have any money in balance to deposit.]
$onlyIf[$message<=$splitText[3];You cannot exceed your bank limit.]
$onlyIf[$sum[$splitText[2];$message]<=$splitText[3];You cannot exceed the bank limit.]
$reply
$allowUserMentions[]
$username, you deposited **$$message** into the bank.
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

$onlyIf[$message!=;Include how much do you want to withdraw.]
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

This checks if the User has exceeded the amount of "Required XP" variable, and if they did, they are advanced to the next level.


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

### Shop Command

For this, we have to make a seperate variable for adding the following items:

- Apple
- Banana
- Watermelon
- Orange
- Mango

These are some example items that I'm going to add in the shop, you can customize or rename these items to what you want. This is just for an example as this is a Guide. So now, I'm going to create a new variable named "ITEMS" with the Value set the following:

![Click Here](https://cdn.discordapp.com/attachments/1349964745519665273/1350402981694144542/IMG_20250315_150813.jpg?ex=67d69c48&is=67d54ac8&hm=f851f3a6832e8caafaa3805c5c5e0bee7d52fd987038425d9e5d88d9d4b45ca7&)


There are 5 variables, with them being seperated again with `.`
Since there are 5 fruits that are being added in the shop, 5 variables with each having a seperator as `.` between them.

TO REMEMBER:

**DO NOT** add the seperator at the beginning of the Variable Value (Before the first variable) and at the end of the variable (After the last variable).

This is how the code looks like, with adding basic embed examples:

```markdown
$nomention
$reply
$allowUserMentions[]
$title[Shop]
$description[

Mango: $30
Apple - $50
Banana - $60
Orange - $90
Watermelon - $120]
$footer[$username]
$addTimestamp
$color[00ff00] $c[adding color for a clean shop]
```

There. Since it's a pretty visual code and there's nothing to be done in it, there's only a title, description, color and a timestamp put in the code, not too much detail. You can make the shop command anyway you like.

---

### Buy Command

This is a basic buy command that checks if the item exists in the shop, and if not then it outputs an error message.

```markdown
$nomention
$reply
$allowUserMentions[]
$textSplit[$getVar[VARIABLE;$authorID];.]
$var[money;$splitText[1]]
$onlyIf[$message!=;What do you want to buy from the Shop?]
$if[$toLowercase[$message]==mango]
$onlyIf[$var[money]>=30;You need $30 to buy a Mango.] $c[checking if the user has money.]
$username, you bought a **Mango** for **$30**.
$textSplit[$getVar[ITEMS;$authorID];.]
$editSplitText[1;$sum[$splitText[1];1]] $c[Increases 1 per each purchase.]
$setVar[ITEMS;$joinSplitText[.];$authorID] $c[saving it]
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;$sub[$splitText[1];30]] $c[Removing $30 from User's balance.]
$setVar[VARIABLE;$joinSplitText[.];$authorID] $c[saving the variable.]

$elseif[$toLowercase[$message]==apple]
$onlyIf[$var[money]>=50;You need $50 to buy an Apple.] $c[checking if the user has money.]
$username, you bought an **Apple** for **$50**.
$textSplit[$getVar[ITEMS;$authorID];.]
$editSplitText[2;$sum[$splitText[2];1]] $c[Increases 1 per each purchase.]
$setVar[ITEMS;$joinSplitText[.];$authorID] $c[saving it]
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;$sub[$splitText[1];50]] $c[Removing $50 from User's balance.]
$setVar[VARIABLE;$joinSplitText[.];$authorID] $c[saving the variable.]

$elseif[$toLowercase[$message]==banana]
$onlyIf[$var[money]>=60;You need $60 to buy a Banana.] $c[checking if the user has money.]
$username, you bought a **Banana** for **$60**.
$textSplit[$getVar[ITEMS;$authorID];.]
$editSplitText[3;$sum[$splitText[3];1]] $c[Increases 1 per each purchase.]
$setVar[ITEMS;$joinSplitText[.];$authorID] $c[saving it]
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;$sub[$splitText[1];60]] $c[Removing $60 from User's balance.]
$setVar[VARIABLE;$joinSplitText[.];$authorID] $c[saving the variable.]

$elseif[$toLowercase[$message]==orange]
$onlyIf[$var[money]>=90;You need $90 to buy an Orange.] $c[checking if the user has money.]
$username, you bought an **Orange** for **$90**.
$textSplit[$getVar[ITEMS;$authorID];.]
$editSplitText[4;$sum[$splitText[4];1]] $c[Increases 1 per each purchase.]
$setVar[ITEMS;$joinSplitText[.];$authorID] $c[saving it]
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;$sub[$splitText[1];90]] $c[Removing $90 from User's balance.]
$setVar[VARIABLE;$joinSplitText[.];$authorID] $c[saving the variable.]

$elseif[$toLowercase[$message]==watermelon]
$onlyIf[$var[money]>=120;You need $120 to buy a Watermelon.] $c[checking if the user has money.]
$username, you bought a **Watermelon** for **$120**.
$textSplit[$getVar[ITEMS;$authorID];.]
$editSplitText[5;$sum[$splitText[5];1]] $c[Increases 1 per each purchase.]
$setVar[ITEMS;$joinSplitText[.];$authorID] $c[saving it]
$textSplit[$getVar[VARIABLE;$authorID];.]
$editSplitText[1;$sub[$splitText[1];120]] $c[Removing $120 from User's balance.]
$setVar[VARIABLE;$joinSplitText[.];$authorID] $c[saving the variable.]

$else
Couldn't find the item you're looking for.
$endif
```

This is a basic buy command with the existing items I added in my shop command. You can replace my Items with your Items and Price, since the Shop and Buy Command are pretty self explanatory. If you ever felt like making an advance Server Shop Setup that the Shop can be customized for each server, I had posted a wiki in the Official BDFD Server. You can integrate those codes with this to make this more better.

---

I guess that's it, thank you for following the whole Guide, and those who read the whole Guide until here, you're a real one. Whole Guide done by: `kitten_tenacious`

If you ever face any error, please DM me or create a forum in Support then ping me. I hope you understand this Guide, because this can be a pretty save for your variables.
