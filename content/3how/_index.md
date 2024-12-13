+++
title = "3How?"
+++

So, how does it work you may ask?

It's all in the documentation. The algorythm is very simple and can be dumbed-down so much that anybody with the slightest knowledge of programming can build the whole thing back from scratch in any language since it has no specificities what-so-ever. So, let's dumb-it-down.

## Explain Like I'm 5 years old

In order to obfuscate your seedphrase, we use the mnemonic (the dictionary of words your seedphrase comes from) and we switch the position of the words in the mnemonic. To do so, we take the index of the word in the mnemonic (its position in the dictionary) and add a certain number to it, giving us a different word. When desobfuscating, we subtract this same number to the position of the obfuscated word in the same dictionary.

The numbers we add or subtract comes from a password you define. Every character in your password has a position in the Unicode table (so it has an index, therefor a number uniquely characterizing it) and by processing the positions of every character through a function that makes extensive uses of modulo, which is a lossy operation (meaning we lose information so the operation can't be reversed), we gather a new position for every word of your seedphrase.

## Lossy algorythm

In order to properly obfuscate your seedphrase, we are not just offsetting it globally, meaning we would shift every indexes by a single value. We are in fact generating a specific value for each word and this value is not only corelated to the characters in your password but also to their positions in the password. Because of that, a single character change in the password results in a totally different obfuscation. This lossyness means we can even disclose several characters in the password, it doesn't give an edge to the attacker because it is not directly corelated to one position of one word in your seedphrase. 

## Hints and recovery

Because of this previously mentioned lossyness, you can - and should - give your future self hints on the password protecting your seedphrase. If you set a password "abcdefgh", you can use "****e***************" as a hint (never give away the total length of your password) and you still have a heavily protected obfuscated seedphrase. The knowledge of the letter in the 5th position doesn't give you the index-shifting of the fifth word in your seedphrase. It is corelated in such a way that you can't do anything with this information than just trying to bruteforce every other possible character in the password.

## Why stop at one?

The real magic happens when you add more than one password to your obfuscation. ObfusKey is designed to re-obfuscate the output of an obfuscation with the next password. This means if you set three passwords, an attacker can't access the "in-between" obfuscations. In order to brute-force a multiple password obfuscation, an attacker will have to try breaking ALL the obfuscations AT THE SAME TIME. And because of this very simple but clever trait, this method of protecting your seedphrase becomes virtually unbreakable when using more than one password since even if all your passwords are known and can be found in a password listing, an attacker will have to try every possible combination of passwords at the same time and for every outcome, try to open the wallet and see if it is the one they are looking for.

The mathematics of it are all described clearly in the documentation but to make it simple:
an attacker will waste far less resources (and therefore money) by trying to randomly generate seedphrases then by actually trying to break a multiple password obfuscation.

## How well does it protect my seedphrase?

We are going to talk about probabilities in a very simple manner here using the Bitcoin blockchain for the example. First, let's acknowledge the fact that if you have a bitcoin seedphrase, nothing stops anyone from opening your wallet by simply be lucky enough to enter the words of your seedphrase in the right order. Because of the very big number of possible bitcoin private keys (i.e wallets, i.e valid seedphrases), it is very much unlikely but not impossible. This is the base of the security of the network and by using it you just have to accept this fact. This number of possible bitcoin private keys is very big, roughly 1.46x10⁴⁸. Let's consider this number as "pretty big" since we accept it to secure our wallets by making it very improbable for anyone to open our wallet.

Now, if you are using ObfusKey, you have to set one or more (but preferably more) passwords that can be of virtually any length and use any of the Unicode characters. The Unicode table consists of 155,063 characters but for the sake of argument let's say we are using A-Z, a-z, 0-9 and 10 non-alphranumeric characters -_.:,?1+=@. If we are using a 24 characters password, there are 3.77x10⁴⁴ possibilities for this password. But as said before, we don't stop at one, let's add two more passwords. Because you can't break the passwords one by one to break ObfusKey, you need to find the three correct passwords at the same time, meaning you are now dealing with 5.4x10¹³³. If we considered earlier that 1.46x10⁴⁸ was already a very big number, then this one is truely gigantic. This means actually brute-forcing a propery obfuscated seedphrase has dramatically less chances of success over simply trying to generate random seedphrases and try opening ny wallet instead.

Of course, in the end, it all comes down to crafting yourself a set of unique, very private passwords because using basic "abcdefg" and giving out a couple of characters as hints will help attackers breaking it very easily which defies the whole purpose.

## How would someone attack it?

In order to understand how it works, let's see how someone would go about attacking an obfuscated seedphrase. Let's say someone found your ObfusKey text output and sees that you have the following passwords:

{{< paige/code >}}
Passwords:
    1.: *****f************
    2.: ***l**************
    3.: ******w***********
{{< /paige/code >}}

The first thing they would probably do would be to look into a password listing and take all the passwords that have the known characters at their known position. This makes it pretty clear that you shouldn't give too many indications because the more known characters, the less potential words they have to try but also, not giving out characters towards the end of the passwords so they don't have indications of the minimum length of the password.

Now, in this case, if your passwords were

{{< paige/code >}}
Passwords:
    1.: abcdefgh
    2.: ijklmnop
    3.: qrstuvw
{{< /paige/code >}}

The attacker would probably have found it pretty easily. But now, let's imagine you actually used something a lot more personal like

{{< paige/code >}}
Passwords:
    1.: Park-florida_1998
    2.: Cool-And-The-Gang
    3.: BLACK&white_Australia-015429
{{< /paige/code >}}

Even though the passwords are composed of common words, the way they are put together is already a layer of difficulty for the brute-forcing but on top of that, the fact that only these three passwords tried at the same time will give back your desobfuscated seedphrase makes it immensly unlikely that anyone can brute force it.

The strength of ObfusKey resides entirely in the passwords you are setting. How many are they, how long are they and how personal are they is what will make them impossible to brute-force.

{{< paige/center >}}
Now, let's see [how do you use ObfusKey](../4howto)
{{< /paige/center >}}
