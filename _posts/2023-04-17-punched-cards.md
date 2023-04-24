---
title: Punched Cards
categories: [Technical Logs, Individual]
tags: [typescript]
math: true

image:
    path: https://upload.wikimedia.org/wikipedia/commons/f/fe/Used_Punchcard_%285151286161%29.jpg
    alt: "A 12-row/80-column IBM punched card from the mid-twentieth century.
    By Pete Birkinshaw from Manchester, UK - Used Punchcard, CC BY 2.0, https://commons.wikimedia.org/w/index.php?curid=49758093"
---

> This page is still in WIP. A lot of information is missing or not reviewed.
{: .prompt-danger }

---

Try this problem by yourself first! Check the [Code Jam Page](https://codingcompetitions.withgoogle.com/codejam/round/0000000000876ff1/0000000000a4621b).

## Overview

We want to draw a Punched Card with ASCII art. We first receive an integer $T$. This will be the total number of test cases. Then each line will represent two integers: $R$ the number of rows of the Punched Card and $C$ the number of columns. Let's give an example.

| Sample Input | Sample Output |
|:-|:-|
| <code>2<br>2 2<br>3 4</code> | <code>Case #1:<br>..+-+<br>..|.|<br>+-+-+<br>|.|.|<br>+-+-+<br>Case #2:<br>..+-+-+-+<br>..|.|.|.|<br>+-+-+-+-+<br>|.|.|.|.|<br>+-+-+-+-+<br>|.|.|.|.|<br>+-+-+-+-+</code> |

For each Punched Card, we can notice four distinct patterns.

- **First line:** `..+-+-+-+`. String that starts with `..+` and has a repeating pattern of `-+` for a total of $C - 1$ times.
- **Second line:** `..|.|.|.|`. Starts with `.` and has a repeating pattern of `.|` for a total of $C$ times.
- **Rest of odd lines:** `+-+-+-+-+`. Starts with `+` and repeats `-+` $C$ times.
- **Rest of even lines:** `|.|.|.|.|`. *Prefix* is `|` and repeats `.|` $C$ times.

We can create a function that creates a string with a prefix and repeats a pattern $n$ times. Then we group all rows and print them. This will give us our Punched Card!

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Context

In very simple terms, you want to draw a [Punched Card](https://en.wikipedia.org/wiki/Punched_card) with [ASCII art](https://en.wikipedia.org/wiki/ASCII_art). Your input will be two integers $R, C$ representing the number of rows and the number of columns of your punched card, respectively. See the following example:

```
Input:
2 4

Output:
..+-+-+-+
..|.|.|.|
+-+-+-+-+
|.|.|.|.|
+-+-+-+-+
```

Notice the four dots at the top-left corner of the ASCII art. Those four dots will ALWAYS be on that same position on every Punched Card you draw.

### Searching for patterns

To make this task easier, we will search for patterns to help us automate this task. We can immediately see two distinct patterns:

| Top and Bottom: | In Between: |
|:-:|:-:|
| `+-+-+-+-+-+-+-+-+-+` | `|.|.|.|.|.|.|.|` |

We can then break this patterns into $$\underbrace{+}_{P}$$ and $$\underbrace{-+-+-+-+-+}_{Repeat}$$, where $P$ is our *prefix* `+` and $Repeat$ is the pattern `-+` that we'll repeat after the *prefix*. This also applies to the pattern `|.|.|.|.|.|`.

Then we can exploit the *prefix* part for the top four dots by using `..+` as our prefix and `-+` as our repeating pattern.

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Solution

As seen on the [Context section](#searching-for-patterns), we will automate the creation of every line using a prefix, a repeating pattern and the number of times the pattern will repeat. The code is as following:

```typescript
function createRow(prefix: string, middle: string, count: number): string {
    return [prefix, ...Array(count + 1).join(middle).split(' ')].join('') + "\n";
}
```

The code starts by creating an array with the `prefix` as the first element. The rest of the elements will be `count` copies of the `middle` repeating pattern. Finally we join all the strings and append a new line `\n`.

After this, we can start creating the punch card ASCII art. We will define 4 different rows in total:

1. `rowX`, which will serve as the `+-+` pattern with the first `..` dots.
1. `rowY` will define the `|.|` pattern with the first two dots.
1. `rowA` will be just the pattern of `+-+`.
1. `rowB` bin be the pattern `|.|`.

The code will be the following:

```typescript
function cardBuilder(row: number, col: number): string {
    const rowX = createRow("..+", "-+", col - 1);
    const rowY = createRow(".", ".|", col);
    const rowA = createRow("+", "-+", col);
    const rowB = createRow("|", ".|", col);

    return [
        rowX,   // <- First row.
        rowY,   // <- Second row.
        ...Array(row).join(rowA + rowB).split(' '),   // <- Rest of middle rows.
        rowA.substring(0, rowA.length - 1)   // <- Bottom row.
    ].join('');
}
```

Notice that we use a substring on the last `rowA` (line 11). This is because we get rid of the last `\n`, as the command `console.log` will print our last new line.

And this code will solve our problem. We just need to read the `R C` input and print the result from `cardBuilder(R, C)`.

### Reading the input

The best way that was found to read the input from stdin in Typescript was with first defining the following function:

```typescript
declare var require: any;
declare var process: any;

// We import the library `readline`.
const readline = require('readline');

function read_lines_from_stdin(callback: (input_lines: string[]) => void) {
    // We will create an array to store all inputs.
    const input_lines: string[] = [];
    // We tell `readline` to read from stdin.
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
        terminal: false
    });

    // Every time a new input line is given, push it to our array.
    rl.on('line', (input: string) => {
        input_lines.push(input);
    });

    // When we finish reading the input, "return" the array of inputs.
    rl.on('close', () => {
        callback(input_lines);
    });
}
```

Then we can use the function to retrieve all the input lines and work with them. Here's an example on how to use the function:

```typescript
read_lines_from_stdin((input_lines) => {
    let first_input = parseInt(input_lines[0]);
    console.log(`The first input was ${first_input}.`);

    for (var i: number = 1; i <= first_input; i++)
    {
        my_input = input_lines[i];
        console.log(`Input #${i} is ${my_input}.`)
    }
})
```

We could've used the module Promises, but because Code Jam compiled Typescript in the following way:[^1]

> - TypeScript:
>     - 3.3.3333 (package: node-typescript)
>     - `tsc Solution.ts --module system --outFile Solution.js`
>     - `nodejs Solution.js`

we couldn't really make the Promises module work. That's why we share how we managed to read the user input.

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Alternative Solutions

There's really no alternative solutions that we could think of.

---

## Footnotes

[^1]: See [Code Jam FAQ](https://codingcompetitions.withgoogle.com/codejam/faq)
    on section **Platform**.
