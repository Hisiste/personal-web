---
title: Double or One Thing
categories: [Technical Logs, Individual]
tags: [python]
math: true
---

> This page is still in WIP. A lot of information is missing or not reviewed.
{: .prompt-danger }

---

Try this problem by yourself first! Check the [Code Jam Page](https://codingcompetitions.withgoogle.com/codejam/round/0000000000877ba5/0000000000aa8e9c).

## Overview

We have a string of uppercase English characters. We can double or not every
character of the string. If we had the set of all possible results and sort them
alphabetically, which string would come first?

Our only control over the string is doubling or not every character. Sorting
alphabetically means comparing letters and choosing the one that appears before
in the alphabet. We would...

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Context

Let's summarize the problem. Take a string of uppercase English characters. We can highlight any of its letters, possibly all or none of them. Every highlighted letter will be duplicated, every non-highlighted letter will stay as is. We'll give an example with all of its highlighting possibilities.

| RIDE $\rightarrow$ RIDE | RI<span style="background-color: var(--mark-text);">D</span>E $\rightarrow$ RIDDE | RID<span style="background-color:var(--mark-text);">E</span> $\rightarrow$ RIDEE | RI<span style="background-color:var(--mark-text);">DE</span> $\rightarrow$ RIDDEE |
| <span style="background-color:var(--mark-text);">R</span>IDE $\rightarrow$ RRIDE | <span style="background-color:var(--mark-text);">R</span>I<span style="background-color:var(--mark-text);">D</span>E $\rightarrow$ RRIDDE | <span style="background-color:var(--mark-text);">R</span>ID<span style="background-color:var(--mark-text);">E</span> $\rightarrow$ RRIDEE | <span style="background-color:var(--mark-text);">R</span>I<span style="background-color:var(--mark-text);">DE</span> $\rightarrow$ RRIDDEE |
| R<span style="background-color:var(--mark-text);">I</span>DE $\rightarrow$ RIIDE | R<span style="background-color:var(--mark-text);">ID</span>E $\rightarrow$ RIIDDE | R<span style="background-color:var(--mark-text);">I</span>D<span style="background-color:var(--mark-text);">E</span> $\rightarrow$ RIIDEE | R<span style="background-color:var(--mark-text);">IDE</span> $\rightarrow$ RIIDDEE |
| <span style="background-color:var(--mark-text);">RI</span>DE $\rightarrow$ RRIIDE | <span style="background-color:var(--mark-text);">RID</span>E $\rightarrow$ RRIIDDE | <span style="background-color:var(--mark-text);">RI</span>D<span style="background-color:var(--mark-text);">E</span> $\rightarrow$ RRIIDEE | <span style="background-color:var(--mark-text);">RIDE</span> $\rightarrow$ RRIIDDEE |

Now, with all of our results, we must sort alphabetically all of them. We would obtain the following result:

| 1. RIDDE | 5. RIIDDE | 9. RRIDDE | 13. RRIIDDE |
| 2. RIDDEE | 6. RIIDDEE | 10. RRIDDEE | 14. RRIIDDEE |
| 3. RIDE | 7. RIIDE | 11. RRIDE | 15. RRIIDE |
| 4. RIDEE | 8. RIIDEE | 12. RRIDEE | 16. RRIIDEE |

As a result, **RIDDE** is the first word that appears. This is exactly the word we're looking for.

### How can we sort in alphabetical order?

Let's compare two words: *book* and *boat*. Alphabetically, which one comes first? And why? Let's make a procedure.

1. Compare each letter from left to right. Find the first ones that differ.

    | **B** ook - **B** oat | *b* **O** ok - *b* **O** at | *bo* **O** k - *bo* **A** t |
    | They're the same. (**b**) | They're the same. (**o**) | They differ. (**o** $\neq$ **a**) |

1. The letter that differs decides which word comes first.

    | Book > Boat |
    |:-:|
    | *Book* comes after *boat*, because *o* comes after *a* in the alphabet. |

1. What if one word is a subset of the other? The smallest word will come first, then.

    | **C** an - **C** annot | *c* **A** n - *c* **A** nnot | *ca* **N** - *ca* **N** not | *can* - *can* **N** ot |
    | Same letter. (**c**) | Same letter. (**a**) | Same letter. (**n**) | Differ. (**_** $\neq$ **n**) |

    | Can < Cannot |
    |:-:|
    | *Can* comes before *cannot*, because *can* is a subset of *cannot*. |

This procedure will be essential to understand the solution.

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Solution

The big picture of the solution is as following:

1. Walk through the character string letter by letter.
1. If the current letter comes before the next letter in the alphabet, **highlight it**.
1. If the current letter comes after the next letter in the alphabet, **don't highlight it**.
1. If the current letter is the same as the next letter, keep walking until you find a letter different from the current one. Highlighting all the letters or none of them will depend on if the current letter comes before or after the first different one, respectively.
1. The last letter **will never be highlighted**.

We illustrate two examples following the procedure.

#### RIDE

1. **R**I<span style="opacity: .2;">DE</span> $\rightarrow$ The *R* comes after the *I*. Hence, we won't highlight the *R*.
1. <span style="opacity: .2;">R</span>**I**D<span style="opacity: .3;">E</span> $\rightarrow$ The *I* comes after the *D*. Therefore, we won't highlight the *I*.
1. <span style="opacity: .2;">RI</span>**D**E $\rightarrow$ The *D* comes before the *E*. Now we will highlight the *D*.
1. <span style="opacity: .2;">RI</span><span style="background-color:var(--mark-text); opacity: .2;">D</span>**E** $\rightarrow$ We won't highlight the *E* because it is the last letter.

The resulting string is RI<span style="background-color:var(--mark-text); ">D</span>E. Just as we saw in the [context section](#context), this is the desired output.

#### FEEELING

1. **F**E<span style="opacity: .2;">EELING</span> $\rightarrow$ *F* > *E*. Therefore, *F* is not highlighted.
1. <span style="opacity: .2;">F</span>**E**E<span style="opacity: .2;">ELING</span> $\rightarrow$ *E* = *E*. We'll keep walking.
1. <span style="opacity: .2;">F</span>**E**EE<span style="opacity: .2;">LING</span> $\rightarrow$ *E* = *E*. Keep on walkiiiing.
1. <span style="opacity: .2;">F</span>**E**EEL<span style="opacity: .2;">ING</span> $\rightarrow$ *E* < *L*. Hence, we'll highlight all *E*'s.
1. <span style="opacity: .2;">F</span><span style="background-color:var(--mark-text); opacity: .2;">EEE</span>**L**I<span style="opacity: .2;">NG</span> $\rightarrow$ *L* > *I*. We won't highlight the *L*.
1. <span style="opacity: .2;">F</span><span style="background-color:var(--mark-text); opacity: .2;">EEE</span><span style="opacity: .2;">L</span>**I**N<span style="opacity: .2;">G</span> $\rightarrow$ *I* < *N*. The *I* must be highlighted.
1. <span style="opacity: .2;">F</span><span style="background-color:var(--mark-text); opacity: .2;">EEE</span><span style="opacity: .2;">L</span><span style="background-color:var(--mark-text); opacity: .2;">I</span>**N**G $\rightarrow$ *N* > *G*. You better not touch the *N*. Don't highlight it!
1. <span style="opacity: .2;">F</span><span style="background-color:var(--mark-text); opacity: .2;">EEE</span><span style="opacity: .2;">L</span><span style="background-color:var(--mark-text); opacity: .2;">I</span><span style="opacity: .2;">N</span>**G** $\rightarrow$ *G* > *_*. Last letter must never be highlighted. >:(

Therefore, the solution is F<span style="background-color:var(--mark-text)">EEE</span>L<span style="background-color:var(--mark-text); ">I</span>NG. Is this the right solution? Lets use the brute force way! This means: listing all highlighting possibilities and sorting them. We obtain:

| 1. F<span style="background-color:var(--mark-text);">EEE</span>L<span style="background-color:var(--mark-text);">I</span>NG $\rightarrow$ FEEEEEELIING | 5. F<span style="background-color:var(--mark-text);">EEE</span>LING $\rightarrow$ FEEEEEELING | ... | 253. <span style="background-color:var(--mark-text);">F</span>EEE<span style="background-color:var(--mark-text);">L</span>ING $\rightarrow$ FFEEELLING |
| 2. F<span style="background-color:var(--mark-text);">EEE</span>L<span style="background-color:var(--mark-text);">I</span>N<span style="background-color:var(--mark-text);">G</span> $\rightarrow$ FEEEEEELIINGG | 6. F<span style="background-color:var(--mark-text);">EEE</span>LIN<span style="background-color:var(--mark-text);">G</span> $\rightarrow$ FEEEEEELINGG | ... | 254. <span style="background-color:var(--mark-text);">F</span>EEE<span style="background-color:var(--mark-text);">L</span>IN<span style="background-color:var(--mark-text);">G</span> $\rightarrow$ FFEEELLINGG |
| 3. F<span style="background-color:var(--mark-text);">EEE</span>L<span style="background-color:var(--mark-text);">IN</span>G $\rightarrow$ FEEEEEELIINNG | 7. F<span style="background-color:var(--mark-text);">EEE</span>LI<span style="background-color:var(--mark-text);">N</span>G $\rightarrow$ FEEEEEELINNG | ... | 255. <span style="background-color:var(--mark-text);">F</span>EEE<span style="background-color:var(--mark-text);">L</span>I<span style="background-color:var(--mark-text);">N</span>G $\rightarrow$ FFEEELLINNG |
| 4. F<span style="background-color:var(--mark-text);">EEE</span>L<span style="background-color:var(--mark-text);">ING</span> $\rightarrow$ FEEEEEELIINNGG | 8. F<span style="background-color:var(--mark-text);">EEE</span>LI<span style="background-color:var(--mark-text);">NG</span> $\rightarrow$ FEEEEEELINNGG | ... | 256. <span style="background-color:var(--mark-text);">F</span>EEE<span style="background-color:var(--mark-text);">L</span>I<span style="background-color:var(--mark-text);">NG</span> $\rightarrow$ FFEEELLINNGG |

<div style="text-align: right; font-style: italic;">Source: Trust me, bro.</div>

That's the right solution! But wait... Why is it the right solution? Why does the procedure works?

### Why does it works?

In this work we will divide the explanation in two parts: with and without repeating consecutive letters. We will start with no repeating letters, as it is the easiest one.

#### Without repeating letters

In the procedure we walk through each character of the string. For each one we decide if we highlight it or not. Why does the procedure work this way? Let's review the example of RIDE more closely. We will start with the letter **R** and see what happens if we highlight it and what happens if we don't.

<div style="text-align: center; font-weight: bold;">R<span style="opacity: .3;">IDE</span></div>

| Highlighting? | After doubling | Comparing | Which one won? |
|:-|:-|:-|:-|
| Highlighted | <span style="background-color:var(--mark-text);">R</span>IDE $\rightarrow$ RRIDE | <span style="opacity: .3;">R</span> **R** IDE | Comes second. |
| Not highlighted | RIDE $\rightarrow$ RIDE | <span style="opacity: .3;">R</span> **I** DE | Comes **FIRST**! |

Because *R* comes after *I*, *RRIDE* comes after *RIDE*. We better **NOT** highlight the *R*.

<div style="text-align: center; font-weight: bold;"><span style="opacity: .3;">R</span>I<span style="opacity: .3;">DE</span></div>

| Highlighting? | After doubling | Comparing | Which one won? |
|:-|:-|:-|:-|
| Highlighted | R<span style="background-color:var(--mark-text);">I</span>DE $\rightarrow$ RIIDE | <span style="opacity: .3;">RI</span> **I** DE | Comes second. |
| Not highlighted | RIDE $\rightarrow$ RIDE | <span style="opacity: .3;">RI</span> **D** E | Comes **FIRST**! |

Because *I* comes after *D*, *RIIDE* comes after *RIDE*. We better **NOT** highlight the *I*.

<div style="text-align: center; font-weight: bold;"><span style="opacity: .3;">RI</span>D<span style="opacity: .3;">E</span></div>

| Highlighting? | After doubling | Comparing | Which one won? |
|:-|:-|:-|:-|
| Highlighted | RI<span style="background-color:var(--mark-text);">D</span>E $\rightarrow$ RIDDE | <span style="opacity: .3;">RID</span> **D** E | Comes **FIRST**! |
| Not highlighted | RIDE $\rightarrow$ RIDE | <span style="opacity: .3;">RID</span> **E** | Comes second. |

Because *D* comes before *E*, *RIDDE* comes before *RIDE*. We **MUST** highlight the *D*.

<div style="text-align: center; font-weight: bold;"><span style="opacity: .3;">RID</span>E</div>

| Highlighting? | After doubling | Comparing | Which one won? |
|:-|:-|:-|:-|
| Highlighted | RID<span style="background-color:var(--mark-text);">E</span> $\rightarrow$ RIDEE | <span style="opacity: .3;">RIDE</span> **E** | Comes second. |
| Not highlighted | RIDE $\rightarrow$ RIDE | <span style="opacity: .3;">RIDE</span> | Comes **FIRST**! |

Because *RIDE* is a subset of *RIDEE*, *RIDEE* comes after *RIDE*. We better **NOT** highlight the *E*.

This is exactly why we compare the current letter vs the next one. If the current letter of the string appears before in the alphabet with respect to the next letter of the string, we better have more of the current letter. If the current letter appears after than the next letter, we better jump to the next one as soon as we can.

On the other hand, highlighting the last character will only make it a superset of the non-highlighted one. In the alphabetical order, we prefer our word not to be the superset one.

All of this can be represented with the following code:

```python
def first_alphabetically(my_str: str) -> str:
    result = []
    current = 0
    while current < len(my_str) - 1:
        # If the current letter comes before the next letter, highlight it.
        if my_str[current] < my_str[current + 1]:
            result += my_str[current] * 2
            current += 1
        # If the current letter comes after the next letter, don’t highlight it.
        elif my_str[current] > my_str[current + 1]:
            result += my_str[current]
            current += 1
        # If they're the same letter... We will see it later.
        else:
            pass
    # The last letter will never be highlighted.
    result += my_str[-1]
    return ''.join(result)
```
{: file="solution.py"}

#### With repeating letters

<br>
<div style="text-align: center;">O &nbsp; &nbsp; O &nbsp; &nbsp; O</div>
<div style="text-align: center;">o o o</div>
<div style="text-align: center;">...</div>
<br>

## Alternative Solutions

Only one alternative solution came to my mind: **brute force**. As explained at
the beginning of the [solution section](#solution), it consists on:
1. Generating all possible highlightings.
1. Sorting the results.
1. Retrieving the first one that appeared.

Although the biggest pro in this solution is its simplicity, the biggest con is
what makes this solution inviable. Each letter can be highlighted or not,
independently of the other letters. Considering a string of $n$ characters, we
have a total number of $2^n$ possible highlightings. On a set of small strings,
this wont be much of a trouble. The problem begins when we have strings of at
most 100 characters (as seen in the *Test Set 2* on Google Code Jam). That is $$
2^{100} = 1\,267\,650\,600\,228\,229\,401\,496\,703\,205\,376 $$ possible
highlightings. AND THAT IS AN AWFUL LOT. We don't have that much memory nor
time! So this solution had to be discarted.
