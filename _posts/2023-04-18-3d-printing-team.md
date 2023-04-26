---
title: 3D Printing (Team)
categories: [Technical Logs, Team]
tags: []
math: true
date: 2023-04-24 13:46:27 -0600

authors: [adriz, salma]

image:
    path: https://codejam.googleapis.com/dashboard/get_file/AQj_6U1yXmbP6Nf5PONAMbqVd5eyM5BBSbjggzDn9H6vS3ATQiqbGVrfZ0ABoAbBkn8IWocYoj1rdJim6VkTTOP4/3d_printing.png
    alt: "3 printers with their own ink levels."
---

Try this problem by yourself first! Check the [Code Jam Page](https://codingcompetitions.withgoogle.com/codejam/round/0000000000876ff1/0000000000a4672b).

## Overview

In this technical log, we will discuss different algorithmic solutions to a Google Code Jam problem where we are given three printers with different amounts of ink in their cartridges. The objective is to find a color that can be printed by all three printers using the ink they have. The color must be composed of four non-negative integers representing the amount of cyan, magenta, yellow, and black ink needed, adding up to the number of units needed to print a D.

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Context

We are given three printers with their own ink levels. Ink levels are measured in units. Each printer has four different colors: Cyan, Magenta, Yellow and Black. We want to select a color defined by the number of units of cyan, magenta, yellow and black needed to form the color. Every printer must have sufficient ink to print a big D that requires $10^6$ units of ink to be printed. Representing this problem in mathematical terms will help us find the solution.

Suppose the printer $i$ has a cyan ink level of $C_i$, a magenta ink level of $M_i$, yellow ink level of $Y_i$ and black ink level of $K_i$. What we want to find is $c$, $m$, $y$ and $k$ integers such that

$$
\begin{eqnarray}
&   c \leq C_i \qquad
    m \leq M_i \qquad
    y \leq Y_i \qquad
    k \leq K_i , \qquad
    \forall i , \ 1 \leq i \leq 3 .
\end{eqnarray}
$$

Notice that if $c \leq C_1$, $c \leq C_2$, $c \leq C_3$, then $c \leq \min \\{ C_1,\ C_2,\ C_3 \\}$. Thus, it is sufficient to find $c$, $m$, $y$ and $k$ integers such that

$$
\begin{eqnarray}
&   c \leq \min \{ C_1,\ C_2,\ C_3 \} , \qquad
    m \leq \min \{ M_1,\ M_2,\ M_3 \} , \\
&   y \leq \min \{ Y_1,\ Y_2,\ Y_3 \} , \qquad
    k \leq \min \{ K_1,\ K_2,\ K_3 \} .
\end{eqnarray}
$$

On the other hand, we also need that

$$
\begin{eqnarray}
&   c + m + y + k = 10^6 .
\end{eqnarray}
$$

Finally, we have an extra restraint. Every cartridge has between 0 and $10^6$ units of ink. This is everything we need to find a solution to our problem.

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Solution

The main idea for the solution to this problem is to iterate through all test cases, obtaining the available ink in each level for each printer and evaluating it to find a color that can be printed by all printers. To do the latter, first we need to find the cartridges that have the least amount of ink and use that value as a base for the ink that will be used in the other two printers. Then we can add up the values of each color until the sum is exactly $10^6$, if it exceeds $10^6$ we should subtract the excess of ink, if it falls short then we should indicate that there is no color that can be used to print a D in the three printers. The final step of the process is to display the results. 

1. Initialize a variable for the test case counter. 
1. For each test case, do the following: 
    1. Read the ink levels for each of the three printers. 
    1. Find the minimum ink level for each color (CMYK) across the three printers. 
    1. Calculate the total ink level by summing the minimum ink levels for each color. 
        1. Check if the total ink level is $10^6$: 
        1. If true, we have a valid result, and no further calculations are needed. 
        1. If false, adjust the ink levels as needed to ensure their sum is equal to $10^6$: 
            1. Calculate the difference between the total ink level and $10^6$. 
            1. Iterate through the minimum ink levels array and do the following: 
            1. If the difference is greater than the current color ink level, subtract the color ink level from the difference and set the color ink level to 0. 
            1. If the difference is less than the current color ink level, subtract the difference from the color ink level and break the loop. 
        1. If it falls short, print `IMPOSSIBLE`. 
    1. Print the adjusted ink levels for each color (CMYK). 

The solution ensures that we obtain a color that can be printed by all three printers using their available ink levels, or otherwise indicate that it is impossible to print such a color. 

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Alternative Solutions

Even if the algorithm implemented for all languages is basically the same, every code we built has slight differences in their approach to input reading, output formatting, and iteration. In this section we will analyze each step of the algorithm, comparing and contrasting all the solutions.  

1. Initialize a variable for the test case counter. 

    Dart, Kotlin, Python 1, and Python 2 initialize the counter implicitly by using a loop with a range. TypeScript explicitly initializes a count variable. 

1. Read the ink levels for each of the three printers. 

    Dart, Kotlin, and Python 1 handle reading input in the main loop, whereas Python 2 and TypeScript use separate input handling. Python 2 uses a function (find_color) that takes the number of test cases and printer data as arguments, while TypeScript reads printer data as strings and then converts them into arrays of numbers. 

1. Find the minimum ink level for each color (CMYK) across the three printers. 

    Dart, Kotlin, and TypeScript use a similar approach, calculating minimum ink levels for each color directly using a conditional or Math.min() function. 

    Python 1 uses NumPy's amin() function to find the minimum ink level for each color, while Python 2 uses nested loops in the find_color() function to find the minimum ink levels. 

1. Calculate the total ink level by summing the minimum ink levels for each color. 

    Dart, Kotlin, and Python 2 sum the ink levels directly within the main loop, while Python 1 uses NumPy's sum() function. TypeScript uses the reduce() function to calculate the total ink level. 

1. Check if the total ink level is 10^6 and adjust the ink levels as needed. 

    All implementations follow a similar approach, using conditional statements to check the total ink level and adjusting ink levels as needed. Python 2, however, separates this logic into the find_color() function. 

1. Print the adjusted ink levels for each color (CMYK). 

    Dart, Kotlin, Python 1, and Python 2 handle output formatting within the main loop, using different ways to concatenate strings (e.g., string interpolation in Dart and Kotlin). TypeScript defines a separate loop for printing the output. 

### Pros and cons of each alternative 


| Language | Pros | Cons |
|:-|:-|:-|
| Dart | | Does not use any data structure<br>optimizations, such as lists or<br>tuples, which could potentially<br>simplify the implementation and<br>improve code readability. |
| Kotlin | Reads input and prints output<br>within the main loop, simplifying<br>the code structure.  | Does not separate the logic for<br>finding the ink levels into a<br>separate function, which could<br>make the code less modular and<br>harder to reuse or refactor. |
| Python code 1 | Makes use of NumPy for array<br>operations, which can be efficient<br>for larger datasets.  | Uses multiple for-loops to handle<br>printer input, which could make<br>the code less readable and harder<br>to maintain, especially when<br>dealing with a larger number of<br>printers. |
| Python code 2 | Separates the logic for finding the<br>ink levels into a separate function,<br>making the code more modular<br>and reusable.  | Does not use any specialized<br>libraries or built-in functions to<br>optimize the calculation of the<br>minimum ink levels, which may<br>lead to a less efficient or slower<br>solution. Nested loops in the<br>`find_color()` function make<br>the code slightly less readable. |
| Typescript | | Uses hard-coded input values for<br>testing, making it less flexible for<br>testing with different input cases.  |
| Dart, Kotlin and<br>Typescript | Use of greedy algorithm for<br>adjusting ink levels, which leads<br>to more efficient code by<br>reducing the number of iterations. | |
| Kotlin and Dart | | Less efficient loop for adjusting<br>the ink levels, as it could require<br>additional iterations.  |
| Python code<br>1 and 2 | Use of list comprehension in<br>reading inputs results in concise<br>code. | |
{: width="10" height="1000" .w-75}
