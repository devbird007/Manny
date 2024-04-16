
# How To Create a Date Calculator with the Linux `date` Command in a Shell Script

![date image](img/Date-Image.png)
### Introduction

The `date` command is a command line utility that displays the computer's current date and time. It is included in the [GNU Core Utilities](https://www.gnu.org/software/coreutils/) and is therefore present in all Linux operating systems. While the `date` command is most commonly used to simply display the current datetime (date and time) in the terminal, it is capable of so much more. The `date` command allows you to format its output in different ways, set the system's datetime, and even integrate time calculations within shell scripts.

In this tutorial, you will use the `date` command in a shell script to build a versatile date calculator. You will craft a script that accepts two datetimes as input, then performs a series of operations on them to extract valuable information. Imagine calculating the precise difference between two dates, down to the very second.

By the end of this guide, you will gain a profound understanding of the `date` command and its capabilities. You will be equipped with the necessary skills to conquer date-related calculations. Also, your ability to write and work with shell scripts will see improvements.

## Prerequisites

To complete this tutorial, you will need:

- Familiarity with using the  [terminal](https://www.digitalocean.com/community/tutorials/an-introduction-to-the-linux-terminal).

- A basic understanding of shell scripting, you can follow the guide [How To Write a Simple Shell Script on a VPS](https://www.digitalocean.com/community/tutorials/how-to-write-a-simple-shell-script-on-a-vps).
- Some familiarity with if-else statements in shell scripting. If you’d like an introduction or a refresher, you can visit the [How to Use if-else in Shell Scripts?](https://www.digitalocean.com/community/tutorials/if-else-in-shell-scripts) tutorial.

## Basic Usage of the Date Command

To access the `date` command, type the word into your terminal environment and execute.

```bash
date
```

The `date` command will display the current date and time in a standard format.

```
Thu Apr  6 11:23:31 EST 2024
```
The output shows that the current date is Thursday, April 6th, 2024 and the time is 11:23 <span style="font-variant:small-caps;">am</span> The time zone is also displayed, EST (Eastern Standard Time).
The `date` command has many options and formats that are used to customize its output. In the succeeding sections, you will see how they work.

### Options
The `date` command's options are used to specify the behavior of the `date` command. For example, the `-u` option is used to display the date and time in UTC (Coordinated Universal Time) instead of the local time zone.

```bash
date -u
```
This returns the following output:

```
Thu Apr  6 16:40:49 UTC 2024
```

Another option is `-d`. This displays the date and time as described by an attached string. Execute the following command to see how it works:

```bash
date -d "2 days ago"
```
The command's output will then be displayed on the screen:

```
Tue Apr  4 11:41:24 EST 2024
```
You will observe that the printed date is exactly two days before the earlier printed current date. The `-d` option is not case-sensitive. Here are some more examples of strings that you can parse with `-d`:

- yesterday

- tomorrow

- 1 week ago

- Friday

- 12 dec 2018

- 4 July 2022 6:00pm

- 1 apr 2019 13:14:20

>Note: `--date=STRING` is another way to use the `-d` option. It is its longer form.

### Formats
Formats are used to control the generated output of the `date` command to get specific information. For example, the `%y` format will print the last two digits of the current year. See below:

```bash
date +%y
```
Formats are represented by a percentage sign (`%`) followed by a letter of the English alphabet. When typing the `date` command alongside a format, you have to include a plus sign (`+`) before the format, otherwise, your format will not be recognized.

The output of the above command will then be:
```
24
```

You can also work with multiple formats at once. To print the current date in the form YYYY-MM-DD, execute the following command:

```bash
date +%Y-%m-%d
```

You will see an output similar to this:

```
2024-04-06
```
The `%Y` format is different from `%y` in that the former prints out the four digits of the year, while the latter prints out only the last two digits. The `%m` format prints out the number of the month, between 1-12. Finally, the `%d` format prints out the number of the day of the month between 1-31. You can use these formats individually or combine them as we've seen. The dash (-) separates the figures and can be replaced by a backslash(`/`) or colon(`:`).

To see a complete list of the options and formats of the `date` command, you can use the `--help` flag like this:

```bash
date --help
```

You can use these options and formats to customize the output of the `date` command to suit your needs.
In the following sections as you build your date calculator, you will combine several of these options and formats to get desired results.

## Step 1 — Creating a Script
Navigate to your working directory and create a new script with the following command:

```bash
nano date-calculator.sh
```

In the first line of your newly-created script, enter in the following:

```
#!/bin/sh
```

This ensures the program loader recognizes the file as a shell script and will run the commands contained within it.
Save with `CTRL+O` and exit the text editor with `CTRL+X`.

Now make the script executable with the following command:

```bash
chmod +x date-calculator.sh
```

With that out of the way, you can now get into the nitty-gritty of building your date-calculator shell script. In the next step, you will begin typing the commands to accept the input dates in your shell script.

## Step 2 — Writing the Commands to Take In Date Values
Your script will need to take in the dates (and times) that it will perform operations on. Open your script with the following command:

```bash
nano date-calculator.sh
```

Now enter these lines into your script:

```bash
# To take in date values

echo "Enter the first date: "
read -r first_date
echo "Enter the second date: "
read -r second_date
```

Adding comments to your script makes it easier to read and helps you understand at a quick glance what each set of commands do. There are several such comments you will include in different sections as you write your shell script.

The `read` command lets your shell script read lines from standard input (stdin) and stores them as strings in the specified variables, which are `first_date` and `second_date` in this case.

The `-r` flag prevents the `read` command from mangling backslashes.

The date values accepted in by your script will be in the form of strings. Throughout the rest of this tutorial, you will reference the variables(`first_date` & `second_date`) containing these date values so that date operations can then be performed on them.

## Step 3 — Turning the Date Strings Into Standard Format
The first operation your script will perform on the date values is to convert them from strings to a clear format. This format is the `yyyy-mm-dd HH:MM:SS` format. Enter the following lines into your script:

```bash
# To convert the date strings into standard format

first_dt=$(date -d "$first_date" +"%Y-%m-%d %H:%M:%S")
second_dt=$(date -d "$second_date" +"%Y-%m-%d %H:%M:%S")
```

You do not want the operations to be performed on the current date, so you'll have to use the `-d` option to specify the desired dates. In this case, these are the dates contained within the variables `first_date` and `second_date`. To specify the format of the output, you enter the plus sign (+) followed by the desired format keys.

The results are stored within two new variables `first_dt` and `second_dt`. Next, you will enter `echo` commands to print out the formatted dates. See below:

```bash
echo "\nStandard format for the first date: $first_dt"
echo "Standard format for the second date: $second_dt\n"
```

>Note: The `\n` sequence is used to print a new line. It is important to include this in your script so all your outputs don't end up clumped together on a single line when you execute your script at the end of this tutorial. It will appear in multiple places throughout this guide.


Now, the date strings will be turned into a clear format and printed out on the screen. Here is an example of what the output would look like:

```
Standard format for the first date: 2020-11-23 05:30:15
Standard format for the second date: 2023-03-16 10:15:30
...
```

Next, you will perform the operations to obtain information about the date values.

## Step 4 — Generating the Dates’ Day-Of-The-Week and Week-Of-The-Year
To obtain more information that is not immediately apparent about the date values that are accepted as input, there are specific date formats you can use. In this section, you will write commands to determine the day of the week(the weekday) and the week of the year of each date.

### Determining the Day of the Week
The date format that prints the day of the week (Monday-Sunday) is `%A`. Now, to find the weekday of our chosen dates, enter the following into your script:

```bash
# To determine the day of the week

first_weekday=$(date -d "$first_date" +%A)
second_weekday=$(date -d "$second_date" +%A)
```
Recall, the `-d` option lets you specify your desired date. The results of the operations performed in the preceding code block are stored in two new variables `first_weekday` and `second_weekday`. 

Enter the following code into your script so the results may be printed out upon execution:

```bash
echo "First date's weekday: $first_weekday"
echo "Second date's weekday: $second_weekday\n"
```
Depending on the date values, your final result for this operation when you run your script will look like this:

```
...
First date's weekday: Tuesday
Second date's weekday: Friday
...
```
### Determining the Week of the Year
There are about 52 weeks in a year. To find the week when the dates occur,  there is a format present that carries out this function. This is the `%V` format. To perform this operation, enter the following into your script:

```bash
# To determine the week of the year

week_number1=$(date -d "$first_date" +%V)
week_number2=$(date -d "$second_date" +%V)
```
Again, you will store the results into new variables (`week_number1` & `week_number2`), then you will use these variables when entering the commands to print out the final result.

```bash
echo "Week of the first date: $week_number1"
echo "Week of the second date: $week_number2\n"
```

The number that will be printed for each date will be between 1-52. The output will look like this:

```
...
Week of the first date: 12
Week of the second date: 44
...
```

Next, you will use the date values in conditional statements to extract even more interesting information about them.

## Step 5 — Determining if the Dates Are Before Noon

To determine if the time associated with your dates occur before noon of that day, i.e before 12:00, you will need an if-else statement. 

First, you need to extract the digits necessary for this operation from the accepted date strings. These are specifically the digits of the hours and minutes, `HH:MM`. Enter the following into your script:

```bash
# To determine if the dates are before noon

time_one=$(date -d "$first_date" +"%H%M")
time_two=$(date -d "$second_date" +"%H%M")
```

The values are assigned to two new variables, `time_one` & `time_two`. These are the variables that will be used in the if-else statement. 

Notice that there is no separator between the formats for hours(`%H`) and minutes(`%M`). This is so that it is possible to take them as whole numbers to compare against 12:00 which will be represented by `1200`. Enter the following lines into your script to see how it works:

```bash
if [ "$time_one" -lt 1200 ]; then
    echo "The time is before noon."
else
    echo "The time is after noon."
fi

if [ "$time_two" -lt 1200 ]; then
    echo "The time is before noon.\n"
else
    echo "The time is after noon.\n"
fi
```

If the first date has a time that is 05:12. Then the `time_one` variable will be **0512**. This is then compared against 12:00 which is represented as **1200**. The value 0512 is lower than 1200 so the statement "**The time is before noon.**" will be printed to output. This maps nicely to the fact that the time 05:12 is in fact before 12:00, so your if-else conditional statement works accurately for its intended purpose, deterimining if the time in your date value comes before or after noon.

Next, you will perform an even more complex if-else operation on the date values.

## Step 6 — Calculating the Difference Between the Dates in a Human-Readable Format

To find the difference in time between your two original date values in a readable format, you will need to first find the difference in seconds. 

The `%s` format lets you convert a given date to the number of seconds that have passed since the Unix epoch (`January 01, 1970, 00:00:00 UTC`) up until that date.

To find the difference between the dates in seconds, enter the following lines into your script:

```bash
# To calculate the difference in seconds

fdate_seconds=$(date --date="$first_date" +%s)
sdate_seconds=$(date --date="$second_date" +%s)

num=$(( sdate_seconds - fdate_seconds ))
```

The date values are converted into seconds since Unix epoch and are stored in two new variables `fdate_seconds` & `sdate_seconds`. The difference is then calculated and stored in a new variable, `num`.

Now that you have the difference in seconds contained inside `num`, you will write if-else statements to perform arithmetic operations to convert it from mere seconds into a format denoting days, hours, minutes and then seconds. This is a sample of the target output we are trying to generate:

```
84d 14h 6m 32s between Jan 12 2024 8pm and yesterday.
```

Prior to starting these operations, declare your variables and equate them to a default value of zero, like this:

```bash
# Declaring variables
sec=0
min=0
hour=0
day=0
```

You may now begin the arithmetic operations.

### If-Else Statement to Determine Seconds
You will first write an if-else statement that performs modulo and division operations to determine the final value for seconds.

```bash
# Writing the if-else statement to determine seconds

if [ "$num" -gt 59 ]; then
    sec=$(( num%60 ))
    num=$(( num/60 ))
else
    sec="$num"
fi
```

The final value of the number of seconds will be between 0-59, so the if statement determines if the `num` variable contains a value greater than 59 before performing operations on it. 

If the value is greater than 59, then the `sec` variable will be set to the result of "`num` modulo 60". This will generate a digit between 0 and 59.

Next, the `num` variable is set to the whole number generated when `num` is divided by 60. This new result for `num` is what will be used in further operations.

If however `num` is not greater than 59, the script appropriately sets the value of `sec` to the value of `num` and ends the operation.

Next, another if-else operation will be written to determine the appropriate number of minutes.

### If-Else Statement to Determine Minutes
In this step, you will write another if-else statement nested within the previous one. This will only be executed if the first condition in the previous if-else statement is true—that is, `num` is greater than 59. 

This if-else statement will take the last set value of `num` and perform a similar operation to determine the final value of the number of minutes. This is represented by the `min` variable.

<pre><code>
# Writing the if-else statement to determine seconds

if [ "$num" -gt 59 ]; then
    sec=$(( num%60 ))
   num=$(( num/60 ))

    <b># Writing the if-else statement to determine minutes

    if [ "$num" -gt 59 ]; then
        min=$(( num%60 ))
        num=$(( num/60 ))
    else
        min="$num"
    fi</b>
else
    sec="$num"
fi
</code></pre>

The highlighted section in the preceding code block is the new if-else statement nested inside the previous one. 

The final value of the number of minutes must also be within 0-59, so this if-statement checks if the new `num` value is greater than 59 or not.

If it is, it sets the `min` variable to the result of the operation `num` modulo 60, as this will result in a number between 0-59. 

`num` is again set to the result of the operation "`num`/60". This will be used in the next operation to determine the number of hours. 

If however `num` is now no longer greater than 59, the code sets the value of `min` to the value of `num` and ends the operations.

Now, you will write a final if-else statement to determine the final number of hours and days together.

### If-Else Statement to Determine Hours and Days

Here, your if-else statement will determine the values for both hours and days, represented by the variables `hour` & `day`. 

This will also be nested in the previous conditional statement and will only be triggered if the latest value of `num` is greater than 23. A single day is composed of 24 hours, so you would expect a value for `hour` to fall within 0-23. 

The value for `day` will be whatever is left at the end of the operation. Enter the highlighted section into your script:

<pre><code>
# Writing the if-else statement to determine seconds

if [ "$num" -gt 59 ]; then
    sec=$(( num%60 ))
    num=$(( num/60 ))

    # Writing the if-else statement to determine minutes

    if [ "$num" -gt 59 ]; then
        min=$(( num%60 ))
        num=$(( num/60 ))

        <b># Writing the if-else statement to determine hours

        if [ "$num" -gt 23 ]; then
            hour=$(( num%24 ))
            day=$(( num/24 ))
        else
            hour="$num"
        fi</b>
    else
        min="$num"
    fi
else
    sec="$num"
fi
</code></pre>

You will notice that instead of setting the result of "`num`/24" to `num`, it has been assigned to `day`. This is due to the fact that this is the last operation to be performed on the `num` variable, you no longer need to use it in any other statements. It is now set to the `day` variable and will be printed out as the total number of days.

Although, if `num` is less than 23, then `hour` will be equal to the value of  `num`.

Finally, you will write commands to display the results of the operations in a presentable format.

### Printing out the Result

To print out the results, you will now call the variables `day`, `hour`, `min` and `sec`. Enter the following into your script:

```bash
# To display the result

echo "${day}d ${hour}h ${min}m ${sec}s between $first_date and $second_date."
```

You will also reference the original variables `first_date` & `second_date` that store the date values whose difference you are calculating.

An example of what the output would look like is this:

```
2d 0h 0m 0s between yesterday and tomorrow.
```

Your shell script date calculator is finally complete. Run it with the following command:

```bash
./date-calculator.sh
```

Enter any two dates when prompted and watch it work effortlessly. Below is an example of what the final output of your date-calculator's operations would look like:

```
Enter the first date:
1 Jan 2016 05:12:12
Enter the second date:
yesterday 6pm

Standard format for the first date: 2016-01-01 05:12:12
Standard format for the second date: 2024-04-10 18:00:00

First date's weekday: Thursday
Second date's weekday: Monday

Week of the first date: 01
Week of the second date: 15

The time is before noon.
The time is after noon.

3021d 12h 47m 48s between 1 Jan 2016 05:12:12 and yesterday 6pm.
```

## Conclusion

In this article, you created a versatile date-calculator that takes in the values of two dates and does a lot of date-related operations on them to generate some pretty interesting results. Now, you know a lot about the `date` command, and you have improved your shell scripting skills. You can go on to tackle even more complex scripting projects.