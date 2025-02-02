
## Learning Objectives

* Describe the concept of 'looping' to iterate commands over multiple items
* Automate a task by using a loop inside of a shell script

## Recap
+ You can save commands in files (usually called shell scripts) for re-use.
+ `sh` [filename] will run the command saved within the shell script
+ shell script end in `.sh`
+ You should place variables in backticks (`) if the values might have spaces in them


***

**Class Activity #1**

Before moving on, please complete the following class activity below. You will have ~5 minutes to answer both questions.

[Class-activity](https://forms.gle/vng9b1HvgabFr8Y66)

***

## Loops

Typically, when you are running analyses on the cluster, you are running multiple commands which correspond to individual steps in your workflow. We learned earlier that we can compile these commands into a single shell script to make this process more efficient. What if we could further increase our efficiency so that the same series of commands could be easily repeated for each sample in our dataset? We can do this with the use of loops in Shell!

Looping is a concept shared by several programming languages, and its implementation in bash is very similar to other languages. 

The structure or the syntax of (*for*) loops in bash is as follows:

```bash
for (variable_name) in (list)
do
 (command1 $variable_name)
 (command2 $variable_name)
done
```

The text that is **bold, are parts of the loop structure that remain constant**. That is, for every loop your create you will need to have the words: `for`, `in`, `do` and `done`. *This syntax/structure is virtually set in stone.* The text that goes in between those words will change depending on what it is you want your loop to do.

<p align="center">
<img src="../img/for_loop.png" width="400">
</p>

### How do loops work?

Let's use the example below to go through step-by-step how a loop is actually working.


**Class Exercise #2**  

Open a Jupyter Notebook session and create a script in `unit1_unix` called `loop.sh`. Type the following below. After you are done run the script! 

```bash
cd raw_fastq/

for x in Mov10_oe_1.subset.fq Mov10_oe_2.subset.fq Mov10_oe_3.subset.fq
do
 echo $x
 wc -l $x
done
```

```
Mov10_oe_1.subset.fq
 1223600 Mov10_oe_1.subset.fq
Mov10_oe_2.subset.fq
 1110016 Mov10_oe_2.subset.fq
Mov10_oe_3.subset.fq
  690816 Mov10_oe_3.subset.fq
```

|    Loop component      |      Value          |
| ---------------- | ---------------------- |
| ***variable_name*** | `x` |
| **list** | `Mov10_oe` FASTQ files |
| ***body (commands to be executed)*** | `echo` and `wc -l` |

### Loop.sh explained 

1. When we start the loop, the temporary variable is initialized by taking the value of the first item in the list. 

	> **We don't explicitly see this, but the variable has been defined as `x=Mov10_oe_1.subset.fq`.**

2. Next, all of the commands in the body of the loop (between the `do` and `done`) are executed. Usually, the commands placed here will be using the temporary variable as input. **Remember, if you are using the value stored in the variable you need to use $ to reference it!** In the example, we are running two commands:

	* `echo $x`: print out the value stored in `x`
	* `wc -l $x`: count/report the number of lines in `x`

3. Once those two commands are complete, the temporary variable is assigned a new value. It now takes the value of the second item in the list.

	> **The variable is reassigned a value `x=Mov10_oe_2.subset.fq`.**

4. Once again, all of the commands in between the `do` and `done` are executed. This time they are using the new value stored in `x` as input.

5. The temporary variable then takes on the value of the third item in the list.

	> **The variable is reassigned a value `x=Mov10_oe_3.subset.fq`.**

6. Once again, all of the commands in between the `do` and `done` are executed using the new value stored in `x`. 

7. Now that we have gone through every item in the list, the loop is `done` and it exits. 

Essentially, **the number of items in the list = number of times the code will loop through**. So in our case, we had three files listed and so the series of commands in the body of the loop were repeated three times. If we had provided all six files, the series of commands would be repeated six times.

> #### Running loops at the command prompt
> In our materials, the for loop is written out using multiple lines rather than the single line commands we have been running so far. When running this at the command prompt begin by typing out the `for` statement, then press the return key. You will notice that you are not back at your command prompt. Rather than a `$`, you should see a `>`. The shell has acknowledged that you have started a for loop and is waiting for you to complete it. Continue to type code line by line. Once you type in `done` and press return the shell will know you are done and will run the loop. 

### Creating loops using best practices

#### Meaningful variable names
It doesn't matter what variable name we use, but it is advisable to make it something more intuitive. In the long run, it's best to use a name that will help point out a variable's functionality, so your future self will understand what you are thinking now.

#### Using the wildcard to define the list 
In the example above, we typed out each item in the list leaving a space in between each item. This is usually fine for one or two items, but with larger lists this can become tedious and error-prone. If the list you are iterating over share some similarities in the naming we recommend using the wildcard shortcut to specify the list. 

***

**Class Exercise #3**  
+ Rewrite the for loop above using a more meaningful variable name and using the wildcard.
+ Change the for loop example above so that it runs on all six FASTQ files.
+ Change the for loop example above so that it prints out the first line of all six files.

> **Don't forget to change the name of the variable being referenced inside of the loop!**

***


## Automating more with Scripts

Imagine, if you will, a script that would do the following for us each time we get a new data set:

- Use for loop to iterate over each FASTQ file
- Generate a prefix to use for naming our output files
- Dump out bad reads into a new file
- Get the count of the number of bad reads and report it to a running log file

It might seem daunting, but everything outlined above is something that you know how to do. Let's get started...

***


---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*


