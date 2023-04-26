---
title: Punched Cards (Team)
categories: [Technical Logs, Team]
tags: []
math: true
date: 2023-04-24 11:02:03 -0600

authors: [karen, jose]

image:
    path: https://codejam.googleapis.com/dashboard/get_file/AQj_6U262jpm6Qd7uxxBv1jstjtSrZPrnp58urkNJJQ5Ol7y1bJepKIazfTkMpkRzBodMQB8RiruyLu_TJY37T67YQ/punched_card.png
    alt: ""
---

Try this problem by yourself first! Check the [Code Jam Page](https://codingcompetitions.withgoogle.com/codejam/round/0000000000876ff1/0000000000a4621b).

## Overview

The first problem of the team assignment was Punched Cards. The objective of the problem was, given a numerical value for rows and another for columns, to print an ascii representation of a punched card with the specified size. The algorithm essentially took the form "Print $X$ number of cells $Y$ number of times."

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Context

A secret team of programmers is plotting to disrupt the programming language landscape and bring punched cards back by introducing a new language called Punched Card Python that lets people code in Python using punched cards! Like good disrupters, they are going to launch a viral campaign to promote their new language before even having the design for a prototype. For the campaign, they want to draw punched cards of different sizes in ASCII art.

The ASCII art of a punched card they want to draw is similar to an $R \times C$ matrix without the top-left cell. That means, it has $(R \cdot C) - 1$ cells in total. Each cell is drawn in ASCII art as a period (`.`) surrounded by dashes (`-`) above and below, pipes (`|`) to the left and right, and plus signs (`+`) for each corner. Adjacent cells share the common characters in the border. Periods (`.`) are used to align the cells in the top row. 

For example, the following is a punched card with $R=3$ rows and $C=4$ columns:

```
..+-+-+-+
..|.|.|.|
+-+-+-+-+
|.|.|.|.|
+-+-+-+-+
|.|.|.|.|
+-+-+-+-+
```
{: file="Punched Card. R=3, C=4."}

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Solution

The final solution was to create two loops to start printing in the console a character, this character will depend in the next analysis.

What should we print in every box?

There are many characters in the output: `+`, `.`, `|` and `-`. We must see the matrix with numbers so we can see the pattern that is behind it. So, it would look something like this:

|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **0** | `.` | `.` | `+` | `-` | `+` | `-` | `+` | `-` | `+` |
| **1** | `.` | `.` | `|` | `.` | `|` | `.` | `|` | `.` | `|` |
| **2** | `+` | `-` | `+` | `-` | `+` | `-` | `+` | `-` | `+` |
| **3** | `|` | `.` | `|` | `.` | `|` | `.` | `|` | `.` | `|` |
| **4** | `+` | `-` | `+` | `-` | `+` | `-` | `+` | `-` | `+` |
| **5** | `|` | `.` | `|` | `.` | `|` | `.` | `|` | `.` | `|` |
| **6** | `+` | `-` | `+` | `-` | `+` | `-` | `+` | `-` | `+` |

Once this is done, we can start creating a pattern from this, this would be the pattern: 

1. **Character `+`**. The column indexes in the last example where it appears are 0, 2, 4, 6 and 8, and the row indexes where it appears are 0, 2, 4, 6. 
1. **Character `-`**. The column indexes where it appears are 1, 3, 5 and 7, and the row indexes where it appears are 0, 2, 4 and 6. 
1. **Character `|`**. The column indexes where it appears are 0, 2, 4, 6 and 8, and the row indexes where it appears are 1, 3 and 5. 
1. **Character `.`**. The column indexes where it appears are 1, 3, 5 and 7, and the row indexes where it appears are 1, 3, and 5. 
1. **Special Case**. The only `.` that are not where they are supposed to be, are the ones that are in the superior left corner, but this is the only special case in this problem.

In short, that means that we must ask if the column or row that we are analyzing is even or odd. 

1. If both (column and row) are even, we must print a `+`. 
1. If both (column and row) are odd, we must print a `.`. 
1. If the column is even and the row is odd, we must print a `|`. 
1. If the column is odd and the row is even, we must print a `-`. 
1. If the column is less than 2 and row is less than 2, we must print a `.`, only in this scenario. 

And with this analysis we can start coding the solution that prints the character only when needed and with a new line every time we finish the columns loop. 

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Alternative Solutions

### Box per cell

Another solution was to create boxes which have the functionality of every cell in the cards, in this way we could print three cell columns and four times to create the complete card. the problem with this solution was that the borders were going to be repeated many times and we will require to delete the borders that are repeated, and we will require more process and more time to get with the solution, that's why I didn't implement this solution. The next table illustrates how it would look without fixing: 


|   | 0 | 1 | 2 | 3 | 4 | 5 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **0** | `+` | `-` | `+` | `+` | `-` | `+` |
| **1** | `|` | `.` | `|` | `|` | `.` | `|` |
| **2** | `+` | `-` | `+` | `+` | `-` | `+` |
| **3** | `+` | `-` | `+` | `+` | `-` | `+` |
| **4** | `|` | `.` | `|` | `|` | `.` | `|` |
| **5** | `+` | `-` | `+` | `+` | `-` | `+` |

In the last table we can see that columns two and three are repeated and do not require one of both, so that's the kind of problem that this solution gives and also that we would require a matrix to save this data.

### Switch between elements

In this idea it was necessary to be switching between elements, for example the symbol `+`, after that the symbol `-` and so on. The problem with this solution was that we needed many `if`.
