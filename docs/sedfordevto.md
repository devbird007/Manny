
# Text Substitution with the sed command in Linux

![sedlogo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/insmcc96jaxvbq1ofso0.png)

### Introduction

`sed` is a powerful tool often used in the Linux terminal to edit text documents without opening them. The word "sed" is a contraction of the phrase "Stream Editor". It is essentially a tool for editing streams of text. `sed` takes in text from a file or text received from standard input (stdin) and lets you perform some operations on it. In this tutorial, I will demonstrate how `sed` can be used to make string substitutions in text files in the Linux command-line environment.


## Creating a File

First, open your Linux terminal and navigate to your home directory if you aren't there already. Create a new file with the `cat` command by following the succeeding code block:

```bash
cat << EOF > myfile.txt
This is my box.
It is mine.
My box is big, it is a big box.
My box is black, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the sandbox to play with my box.
EOF
```

This is the sample file which we will perform most of our `sed` operations on. In the next section, you will begin to see how the `sed` command is fundamentally used to alter and substitute text.


## The Basic Syntax of `sed`

The most common way in which you will use `sed` is this:

```bash
sed 's/OLD-WORD/NEW-WORD/' FILENAME
```

You start with typing the `sed` keyword which lets the computer know you want to access the stream editor tool. 

`s` is the search operator that looks through the file.

Next, you enter the word you want to substitute out or replace which is the `OLD-WORD`, and the word you want to substitute in which is the `NEW-WORD`. 

Then you enter the name of the target file `FILENAME` you wish to perform this `sed` operation on. 

The forward slash `/` which separates each keyword is called a delimiter, more on delimiters will be explained later.

Now, using the `sed` command in the file you created earlier, **myfile.txt**, you will attempt to replace instances of the old word **box** with a new word **bag**.

Enter the following command and press `ENTER`:

```bash
sed 's/box/bag/' myfile.txt
```

Immediately, the following output is displayed:

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big box.
My <b>bag</b> is black, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my box.
</pre>

You may notice that the word **sandbox** has been altered to **sandbag**. This occurs because the `sed` command acts on text patterns it encounters, not simply the standalone word.


You may also observe that on each line, only the first instance of **box** is changed to **bag**. The rest are left unchanged. You can look at the highlighted words in the generated output to observe this.

> Note: The file **myfile.txt** itself remains unchanged. By default the `sed` command prints the result of its operation to standard output (stdout). More on how to permanently save your edits will be covered later.


## Substituting Specific Instances

By default, only the first instance of the specified text is altered. If you wish to change the order of the affected instance, you type in the following:

```bash
sed 's/box/bag/2' myfile.txt
```

The following output is displayed in your terminal upon execution:

<pre>
This is my box.
It is mine.
My box is big, it is a big <b>bag</b>.
My box is black, my <b>bag</b> is strong, my box is the best.
My mother bough it for me.
Let's go to the sandbox to play with my <b>bag</b>.
</pre>

In the command entered, you'll notice that we've added a number **2**. What this does is, it makes it so that on each line where **box** appears, it isn't the first instance that is altered, but rather, the second instance. Looking at the generated output, you will be able to observe this.

You may replace **2** with any number to denote which instance of the old word you wish to substitute out for your new word.


## Substituting All instances

If you wish for every instance of the old word **box** in the file to be replaced with the new word **bag**, enter the following command:

```bash
sed 's/box/bag/g' myfile.txt
```

You should see this output:

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big <b>bag</b>.
My <b>bag</b> is black, my <b>bag</b> is strong, my <b>bag</b> is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my <b>bag</b>.
</pre>

The `g` flag added indictates you want to make the substitution globally, i.e everywhere the old word occurs in the file.

## Navigating Case-Insensitive Substitutions 

The `sed` command is by default case-sensitive. Using the `i` flag in your `sed` operation helps you make case-insensitive substitutions. This is for situations where you wish to find and replace instances of the old word regardless of the letter case they may be in. Below is an example:

```bash
sed 's/BOX/bag/i' myfile.txt
```
      
The output:

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big box.
My <b>bag</b> is black, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my box.
</pre>

You'll observe that even though **BOX** is used in the command, instances of its lower-case, **box**, in the file are altered. The substitution wouldn't work without the `i` flag, you can try this out in your terminal environment to see.

In the next section, we will see how to make changes on designated lines.

## Substituting Text on Specific Lines
To find and replace a text pattern on a specific line, instead of all the lines in the file, use the following command:

```bash
sed '3 s/box/bag' myfile.txt
```

Generated output:

<pre>
This is my box.
It is mine.
My <b>bag</b> is big, it is a big box.
My box is black, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the sandbox to play with my box.
</pre>

In the preceding generated output, you will observe that only the first instance of **box** on the third line has been substituted out. Placing the number `3` before `s` tells the computer that in `myfile.txt` you want to find and replace the word **box** for **bag** in the third line only. 

Also, you can change that number to whatever line number you want your substitution made instead.

>Note: If you have a large file, to display the line numbers, enter this command:  
> 	`sed = myfile.txt`


## Substituting Text in a Range of Lines
To find and replace text in a range of lines:

```bash
sed '1,3 s/box/bag/' myfile.txt
```

The displayed output

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big box.
My box is black, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the sandbox to play with my box
</pre>

In the `sed` command entered, the first number is followed by the second number and a comma separates them. This command lets the computer know that you want your `sed` substitutions to be made within the range of line 1 to line 3.

In the next section, we will experiment with different ways of how to execute multiple `sed` substitutions at a go.

## Making Multiple `sed` Substitutions
At times, you may wish to use the `sed` command multiple times on the same text file, there are various ways for you to execute this in one command entry in the Linux terminal.

To demonstrate this, you will substitute the words **box** with **bag** and **black** with **red** in the text file we created, **myfile.txt**. This can be done using any of the following methods:  

### 1. Using Pipe `|`

Enter in the following command:

```bash
sed 's/box/bag/' myfile.txt | sed 's/black/red/'
```

The output:

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big box.
My <b>bag</b> is <b>red</b>, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my box
</pre>

Remember, `sed` is a **Stream Editor**. It prints out to standard output(stdout) and can also take in text piped in from standard input(stdin). In the above scenario, the output from the first `sed` substitution is piped in to the next `sed` command to make its own substitutions too. This lets us make multiple `sed` substitutions at once.

### 2. Using the `-e` Option
You may use the `-e` option provided by `sed` to conveniently execute multiple `sed` substitutions at a go. See below:

```bash
sed -e 's/box/bag/' -e 's/black/red/' myfile.txt
```

Output:

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big box.
My <b>bag</b> is <b>red</b>, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my box
</pre>

### 3. Using the Semicolon `;`

The semicolon `;` allows you to quickly conjoin your multiple `sed` commands and execute them at once on the same file. See below:

```bash
sed 's/box/bag/;s/black/red/' myfile.txt
```

Printed output:

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big box.
My <b>bag</b> is <b>red</b>, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my box
</pre>

We've seen the different ways available that help facilitate executing multiple `sed` substitutions at a go. In the next section we will go over what happens when you only want the lines you've altered returned to output. 

## Printing Edited Lines Only
If you are working on a huge file, and you don't want the output of your `sed` commands on the file filling up your terminal environment each time you make an execution. There is a method you can use to isolate and output only the lines of the file your command has altered. See below:

```bash
sed -n 's/box/bag/p' myfile.txt
```

This is the generated output:

<pre>
This is my <b>bag</b>.
My <b>bag</b> is big, it is a big box.
My <b>bag</b> is black, my box is strong, my box is the best.
Let's go to the <b>sandbag</b> to play with my box
</pre>

In the command executed we use the `-n` option and the `p` flag. The `-n` option suppresses automatic printing of the output. Meanwhile, the `p` flag causes double printing of the edited lines. The combination of these two will cause the command to print out the edited lines only.

> Note: `sed` provides a whole lot of options and flags you can use to make all kinds of tweaks to your substitutions. To access them, you simply enter `sed --help` into your terminal or `man sed` for the full user manual.


## Delimiters
So far, we've been using the forward-slash `/` as a delimiter to separate the relevant categories of our substitution commands in the terminal. Although, there will be situations where this will not suffice. 

For example if you're trying to use `sed` on a URL, a URL already has forward slashes `/` so an error will occur if you try to use a forward-slash(/) as a delimiter. Fortunately, other special characters can be used as delimiters too. 

Let us try to make a `sed` substitution on the URL `https://www.wikipedia.org/homepage.html`. We will take the text pattern **org/homepage** and replace it with **inc/search**. See below:

```bash
echo "https://www.wikipedia.org/homepage.html" | sed 's$org/homepage$inc/search$'
```

The resulting output:

<pre>
https://www.wikipedia.<b>inc/search</b>.html
</pre>

In the command entered, notice that we now use a dollar-sign `$` as a delimiter to separate the text we want to substitute out **org/homepage** and the text to be substituted in **inc/search**. If we had tried using the forward-slash `/` as a delimiter, the operation wouldn't have been successful.

## Saving Changes
It is not enough to only make our edits in stream, you may wish to have the files altered permanently. To make the changes permanent, you must pass the `-i` option. See below for an example acting on the earlier created **myfile.txt** file:

```bash
sed -i 's/black/red/' myfile.txt
```

To confirm that the file has been altered, you will open it with the `cat` command like this:

```bash
cat myfile.txt
```

The result:

<pre>
This is my box.
It is mine.
My box is big, it is a big box.
My box is <b>red</b>, my box is strong, my box is the best.
My mother bought it for me.
Let's go to the sandbox to play with my box.
</pre>

Owing to the `-i` option, we have now confirmed that the old word `black` has been replaced with a new word `red` permanently.

>Note: It is not advised to use the `-i` option casually because doing so could cause you to permanently lose your data. Use it only when you are certain you want to make irreversible changes.


## Saving Changes to a New File

Suppose instead you wanted to save your changes to a new file leaving the first one unaltered. Enter the following command:

```bash
sed 's/box/bag/g' myfile.txt > secondfile.txt
```

If the file **secondfile.txt** did not exist, it is created automatically in your present working directory with the output generated from the `sed` command executed. 

If you open the file with the command `cat secondfile.txt`, you should see the following:

<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big <b>bag</b>.
My <b>bag</b> is red, my <b>bag</b> is strong, my <b>bag</b> is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my <b>bag</b>.
</pre>


## Making a Backup
You can also pass an option that tells the computer to permanently edit the file but create a backup of the original document. To do this, you type out a variation of the `-i` option, which is the `-i.bak` option. It is used like this:

```bash
sed -i.bak 's/box/bag/g' myfile.txt
```

The above command will save your changes to the **myfile.txt** file, but also creates a backup of the original file with a **.bak** extension in the same directory. So the backup created from the command above will be **myfile.txt.bak**.

Running `cat myfile.txt` gets you:
<pre>
This is my <b>bag</b>.
It is mine.
My <b>bag</b> is big, it is a big <b>bag</b>.
My <b>bag</b> is red, my <b>bag</b> is strong, my <b>bag</b> is the best.
My mother bought it for me.
Let's go to the <b>sandbag</b> to play with my <b>bag</b>.
</pre>

Running `cat myfile.txt.bak` gets you:
<pre>
This is my <b>box</b>.
It is mine.
My <b>box</b> is big, it is a big <b>box</b>.
My <b>box</b> is red, my <b>box</b> is strong, my <b>box</b> is the best.
My mother bought it for me.
Let's go to the <b>sandbox</b> to play with my <b>box</b>.
</pre>

You can see that **myfile.txt** is permanently altered while **myfile.txt.bak** contains its original content.


## Conclusion
Now you know the different ways of using `sed` to make basic substitutions in text files in the linux command line, how to make multiple sed changes in one entry, and how to save your changes permanently.
