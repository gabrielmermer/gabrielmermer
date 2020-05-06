+++
showonlyimage = false
draft = true
image = "img/portfolio/Ankicli.png"
date = "2020-04-05"
title = "Anki card maker"
weight = 7
+++

create Anki cards from your terminal
<!--more-->


# Create Anki cards, the fast way.

[Anki](https://apps.ankiweb.net/) is a progam used for spaced repetition, and retaining knowledge for the long term.

The script I made in python makes it easier to add the new information you wish to remeber, by generating flashcards from user input and puting them into .csv table, which is easy to import into Anki.

It works on Windows, Mac and Linux, and can be used on android with [Termux](https://termux.com/).
Simply copy the script and choose your .csv file path.


Source code below:

```python
import csv
import os
os.system('cls' if os.name == 'nt' else 'clear')

tag = input("tag: ")

# press . to quit

card_count = 1
while True:

    os.system('cls' if os.name == 'nt' else 'clear')
    card_list = []

    print(card_count,'',end='')
    front_side = str(input("> "))
    if front_side == ".":
        break

    os.system('cls' if os.name == 'nt' else 'clear')

    back_side = str(input(">> "))

    os.system('cls' if os.name == 'nt' else 'clear')

    card_list.append(front_side)
    card_list.append(back_side)
    card_list.append(tag)

    current_row = [front_side,back_side,tag]

    # put your .csv file path below
    table = open('~/output.csv', 'a',encoding="utf-8")
    tablewriter = csv.writer(table, delimiter='|')
    tablewriter.writerow(current_row)
    table.close()


    card_count +=1
```
