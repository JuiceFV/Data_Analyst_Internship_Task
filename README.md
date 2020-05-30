# Data Analyst Internship Task
Sup, Hi, Hello, Good (evening/morning/afternoon). I craeted this repository in purpose to study the [LaTeX](https://en.wikipedia.org/wiki/LaTeX) basis, due to the [Microsoft Word's](https://en.wikipedia.org/wiki/Microsoft_Word) tools are not enogh for me, and perhaps it will be helpful in my future education. However it was the tasks's solution I proud of, because I have been studing the math (entire math) in my own. The task has been presented by AAA Media Corporation [Rambler Group](https://ramblergroup.com/). I'd like to use this repository as a representation of me for the corporations and other people. Soooo, let's plunge into the tasks:)

## Table Of Contents
- [Data Analyst Internship Task](#data-analyst-internship-task)
  - [Table Of Contents](#table-of-contents)
  - [First Task](#first-task)
    - [The Condition](#the-condition)
    - [The Solution](#the-solution)
    - [Testing](#testing)
    - [Answer](#answer)
  - [Second Task](#second-task)
    - [The Condtition](#the-condtition)
    - [Solution for the first question](#solution-for-the-first-question)
      - [Combinatorics way](#combinatorics-way)
      - [Probability theory way](#probability-theory-way)
    - [Solution of the second part](#solution-of-the-second-part)
    - [Answer](#answer-1)
  - [The Tasks Descriptions (PDF-FILE to download)](#the-tasks-descriptions-pdf-file-to-download)
  - [The Tasks Solutions (PDF-FILE to download)](#the-tasks-solutions-pdf-file-to-download)

## First Task
This task checks whether how well you know the SQL, or how well you use Google, or both.  
### The Condition
Rambler Group's advertising campaign uses most fascinating and most memorable banners. Analytics have access to the databases containing data regard the banner' showing. The
**Shows_table** contains:

- *show_id* - an identier of a showing
- *day* - a day of a showig

| show_id | day        |
|---------|------------|
| 12367   | 2018-10-04 |
| 28736   | 2019-02-22 |
| 19862   | 2019-01-31 |

The **Click_table** contains:

- *click_id* - a show identifier clicked by an user
- *bounce* - an user dismissing from an advertising after click (0 - when an user relinked
to the site he keened in the information on the site. 1 - an user immediatly left the
site.)

| show_id | bounce |
|---------|--------|
| 12367   | 1      |
| 28735   | 0      |
| 15627   | 0      |

You need to get all users who clicked at a banner in February 2020, and they din't reject an
advertising.

### The Solution

1. First of all, I'd like to obtain all users, I mean the whole database of users, without any fillters. But I banned by the task, because it is clearly said that we need to fetch the "**users**", however I do not really aware what exactly is the "**users**". Ensuing of this I decided to derive the *show_id* (it also can be the *click_id*, result will be the same).
```SQL
SELECT show_id FROM Shows_table
```
2. The next scanty modification is to retrieve unique *show_id*. For instance, if an user clicked twice on the same banner, it shouldn't be represented twice, therefore distinct it.
```SQL
SELECT DISTINCT show_id FROM Shows_table
```
3. The next step is to detect those who clicked and remained on the advertising site. The INER JOIN the most appropriating command for this purpose.

> The INNER JOIN keyword selects records that have matching values in both tables

```SQL
SELECT DISTINCT show_id FROM Shows_table
INNER JOIN Clicks_table ON
show_id = click_id AND bounce ='0'
```
Let's deem this query on the example

<table>
<tr><td>

| show_id | day        |
|---------|------------|
| 12367   | 2018-10-04 |
| 15627   | 2020-02-22 |
| 28736   | 2019-01-31 |

</td><td>

| show_id | bounce |
|---------|--------|
| 12367   | 1      |
| 28736   | 0      |
| 15627   | 0      |

</td></tr>
</table>

In the above case the banners which ids are **15627** and **28735** are those banners that an user remained by clicking on. So, this request returns such list:
```SQL
SELECT DISTINCT show_id FROM Shows_table
```

|  12367      |
|-------------|
| **28736**   |
| **15627**   |

If be honest this one does the same as the previous one

```SQL
SELECT DISTINCT show_id FROM Shows_table
INNER JOIN Clicks_table ON
show_id = click_id
```
And the last thing in this subsection is to filter the case when an user remains on the advertising site. According the  task "*bounce - an user dismissing from an advertising after click (0 - when an user relinked to the site he keened in the information on the site. 1 - an user immediatly left the site.)*" This request perfectly does this, and returns such id's list.

| **28736**   |
|-------------|
| **15627**   |

4. The last what we should to do is to distill by date. The task says "*You need to get all users who clicked at a banner in February 2020, and they din't reject an advertising.*", it does pretty simple in SQL, by adding up this to the request.

```SQL
... day BETWEEN '2020-02-01' AND '2020-02-29'
``` 
5. The result looks like this:

```SQL
SELECT DISTINCT show_id FROM Shows_table
INNER JOIN Clicks_table ON
show_id = click_id AND bounce ='0' AND day BETWEEN '2020-02-01' AND '2020-02-29'
```
In our example this query has to return only the **15627**, because this is the only *click_id* within February 2020.

### Testing
In purpose to test my solution I was using the [SQLFiddle](http://sqlfiddle.com/) and PostgreSQL 9.6.
1. I created the test-tables, using DLL, by these queries:

```SQL
CREATE TABLE Shows_table (
    show_id INT ,
    day DATE
);
```
```SQL
CREATE TABLE Clicks_table (
    click_id INT ,
    bounce BOOLEAN
);
```
2. Fill them (tables) up, by using these requests
```SQL
INSERT INTO Shows_table ( show_id, day) VALUES (12367, '2018-10-04');
INSERT INTO Shows_table ( show_id, day) VALUES (28736, '2019-02-22');
INSERT INTO Shows_table ( show_id, day) VALUES (19862, '2019-01-31');
INSERT INTO Shows_table ( show_id, day) VALUES (11111, '2020-02-04');
INSERT INTO Shows_table ( show_id, day) VALUES (22222, '2020-02-01');
INSERT INTO Shows_table ( show_id, day) VALUES (33333, '2020-02-29');
INSERT INTO Clicks_table ( click_id, bounce ) VALUES (12367, '1');
INSERT INTO Clicks_table ( click_id, bounce ) VALUES (28736, '0');
INSERT INTO Clicks_table ( click_id, bounce ) VALUES (19862, '0');
INSERT INTO Clicks_table ( click_id, bounce ) VALUES (11111, '1');
INSERT INTO Clicks_table ( click_id, bounce ) VALUES (22222, '0');
INSERT INTO Clicks_table ( click_id, bounce ) VALUES (33333, '0')
```
And now the tables looks like these:

<table>
<tr><td>

| show_id | day        |
|---------|------------|
| 12367   | 2018-10-04 |
| 28736   | 2019-02-22 |
| 19862   | 2019-01-31 |
| 11111   | 2020-02-04 |
| 22222   | 2020-02-01 |
| 33333   | 2020-02-29 |

</td><td>

| show_id | bounce |
|---------|--------|
| 12367   | 1      |
| 28736   | 0      |
| 19862   | 0      |
| 11111   | 1      |
| 22222   | 0      |
| 33333   | 0      |


</td></tr>
</table>

3. For this example the query (my solution) returns the 22222 and 33333.
- **12367** - isn't suitable, because an user left the site and it was in October 2018
- **28736** - isn't suitable, because it was in February 2019.
- **19862** - isn't suitable, because it was in January 2019.
- **11111** - isn't suitable, because an user left the site.

### Answer
The answer for the First task
```SQL
SELECT DISTINCT show_id FROM Shows_table
INNER JOIN Clicks_table ON
show_id = click_id AND bounce ='0' AND day BETWEEN '2020-02-01' AND '2020-02-29'
```

## Second Task
This task checks your ability to use math.
### The Condtition
The friendly Rambler Group's community likes to play in the table football: At the odd days they play before lunch, at the even days the play after lunch. They are splitting at the *N* teams among each other, and every team plays with each another team. Because of the splitting onto the teams is randomly, the product of the games is random. Also I would note that there are no ties. Only win or lose.
1. Estimate the probability if one of the teams will finish the tournament without defeat.
2. How many times do you need to hold a tournament, so that with a probability of 98% at least once this happened?

### Solution for the first question
| What we have:|
|-------------|
|N - number of teams<br>P (win) = 1/2<br>P (lose) = 1/2 |

| Necessary to seek:|
|-------------|
|P (if one of the teams will finish the tournament without defeat) - ?|

There are two ways to solve it, the FIrst one is the combinatorics and the second is the probability theory. I will show the each one.

#### Combinatorics way
1. First of all we need to estimate how many games *N* teams plays. Because of each team plays with every another team, therefore the number of games is the number of combination *N* by 2. ("*by 2*" because two teams participate in a game.)

    >I'd like to recall that the factorial of a positive integer n, denoted by *n!*, is the product of all positive integers less than or equal to *n*.
For instance, the factorial of *N* is *1 × 2 × 3 × ... × (N − 2) × (N − 1) × N*.

    ![image](https://user-images.githubusercontent.com/35202460/83329291-4af8d300-a291-11ea-8d55-2ef658e2afc3.png)

2. Each team either wins nor loses, therefore there are two outcomes. Ensuing of this the amount of all possible outcomes in whole tournament is ![image](https://user-images.githubusercontent.com/35202460/83329575-f5bdc100-a292-11ea-9f87-f275a8030553.png)
3. Let's assume that the team *A* is won each game in the tournament. It means that from the *(N - 1)* games the team *A* is won *(N - 1)* games. Moreover, it means that the quantity of uncertain game's outcomes declines on *(N - 1)*. Therefore the overall number of outcomes, with the condition that team *A* wins all games is:
![image](https://user-images.githubusercontent.com/35202460/83329559-e5a5e180-a292-11ea-81d6-1e7b62cf48d9.png)
4. However, nobody knows which exactly team wins all games. It's not necessary that team *A* wins, as same as *A* it could be either team *B*, nor team *C*, nor team *D* and so on. If we will deem each case (for each team), then the overall amount of outcomes increases by *N* (number of teams) times.
![image](https://user-images.githubusercontent.com/35202460/83329646-53eaa400-a293-11ea-8f2c-272adc736315.png)
For *N* teams it (number of all possible outcomes, where one of the teams wins) equals to:
![image](https://user-images.githubusercontent.com/35202460/83329686-97dda900-a293-11ea-9f49-b3748b9a08de.png)
5. Recall that probability definition:

    > The probability of an event is a number indicating how likely that event will occur. And the probability of an event is: ![image](https://user-images.githubusercontent.com/35202460/83329742-db381780-a293-11ea-99c9-4b0e6c397f25.png) where m - the number of demanded outcomes and N - the number of all possible outcomes.

    Therefore to obtain the probability of a team will finish the tournament without defeat we need to divide the number of outcomes where one of the teams is won whole tournament ![image](https://user-images.githubusercontent.com/35202460/83329812-31a55600-a294-11ea-81d7-81376ddf1ca0.png) by number of all possible outcomes ![image](https://user-images.githubusercontent.com/35202460/83329829-471a8000-a294-11ea-806b-ed3c0dfd1644.png)
![image](https://user-images.githubusercontent.com/35202460/83329836-54376f00-a294-11ea-8670-bf2351f39566.png)

6. Here it is, the answer. It means that the probability of a team wins the whole tournament without lose is ![image](https://user-images.githubusercontent.com/35202460/83329863-73360100-a294-11ea-96e0-62cf1caa812f.png)

#### Probability theory way
If be honest this approach much easier than previous one.

1. Let's assume that the team *A* wins each game in the tournament, and the probability of this event is ![image](https://user-images.githubusercontent.com/35202460/83330185-22bfa300-a296-11ea-98ff-13dadab68f4c.png) - the probability of an outcome (whether lose nor victory) of one game. And *(N − 1)* the number of games which plays the team *A*.
![image](https://user-images.githubusercontent.com/35202460/83329966-fd7e6500-a294-11ea-99c7-f170e0f9eb22.png)

2. However the probability of that the **one** of the teams wins the tournament is
![image](https://user-images.githubusercontent.com/35202460/83329999-27378c00-a295-11ea-8cc2-761b21eaf911.png)

3. So, the probability that one of the teams wins the tournament without defeat is
![image](https://user-images.githubusercontent.com/35202460/83330017-3f0f1000-a295-11ea-8b42-d03ca32b7fca.png)

### Solution of the second part
How many times do you need to hold a tournament, so that with a probability of 98% at least once this happened (one of the teams wins the tournament without defeat)?

1. Let *x* be the quantity of tournaments required to befell to approximate the probability of the event (victory without defeat) to 98%.

2. Then,
![image](https://user-images.githubusercontent.com/35202460/83330086-a6c55b00-a295-11ea-877f-afe838fa978b.png)

    Let's explore the equation above. In the previous part we've figured out the probability that one of the teams wins the whole tournament without defeat is ![image](https://user-images.githubusercontent.com/35202460/83330160-06bc0180-a296-11ea-819d-da404a48b4ba.png) or ![image](https://user-images.githubusercontent.com/35202460/83330145-f86de580-a295-11ea-8800-010a885ce327.png). Of course the probability of that this event never happens is (*100% - probability of happens at least once*). Therefore, in our case it looks like ![image](https://user-images.githubusercontent.com/35202460/83330236-63b7b780-a296-11ea-9057-443d88d1820f.png), in other words this equation shows us the probability that there is no a team which wins a tournament without defeat. But this probability represents the only tournament, however we have *x* tournaments, and in the each one there is shouldn't be a team which wins at least 1 tournament without defeat.![image](https://user-images.githubusercontent.com/35202460/83330275-9b266400-a296-11ea-948b-1448cc62ca6b.png)


    And the last thing, when we subtracting from 100% the probability of the event when there is no a team which wins at least one tournament without defeat. This shows us the probability when a team which wins at least one tournament exsists.

3. And according the task's condition this probability equal to 98% or 0.98

    ![image](https://user-images.githubusercontent.com/35202460/83330384-34557a80-a297-11ea-9fcc-ba5e2363898e.png)

4. Substitute it under logarithm
    ![image](https://user-images.githubusercontent.com/35202460/83330419-6535af80-a297-11ea-82ed-86b96d29c0f5.png)

5. And use [log-power rule](https://www.rapidtables.com/math/algebra/logarithm/Logarithm_Rules.html#power%20rule)

    ![image](https://user-images.githubusercontent.com/35202460/83330441-91e9c700-a297-11ea-969a-0a36da9cae2d.png)

6. Derive the *x*

    ![image](https://user-images.githubusercontent.com/35202460/83330458-b9409400-a297-11ea-87d8-936fae22d3d1.png)

Here is it! The number of required tournaments is ![image](https://user-images.githubusercontent.com/35202460/83330480-e2612480-a297-11ea-9470-e09fb2b2f34b.png) Of course it depends on number of teams, for instance for two teams the only tournament enough.

### Answer
The answer for the Problem 1
1. The probability that one of the teams wins the tournament without defeat is ![image](https://user-images.githubusercontent.com/35202460/83330532-2eac6480-a298-11ea-8276-28b2ad5bea2a.png)
2. The number of required to befell tournaments to approximate the probability of the event (victory without defeat) to 98% is ![image](https://user-images.githubusercontent.com/35202460/83330558-5e5b6c80-a298-11ea-8f07-471652ea71f7.png)

## The Tasks Descriptions (PDF-FILE to download)
- [English Language Task Description](https://github.com/JuiceFV/Data_Analyst_Internship_Task/blob/master/Task_Description_ENG.pdf) ([Download](https://github.com/JuiceFV/Data_Analyst_Internship_Task/raw/master/Task_Description_ENG.pdf))
- [Russian Language Task Description](https://github.com/JuiceFV/Data_Analyst_Internship_Task/blob/master/Task_Descriptio_RUS.pdf) ([Download](https://github.com/JuiceFV/Data_Analyst_Internship_Task/raw/master/Task_Descriptio_RUS.pdf))

## The Tasks Solutions (PDF-FILE to download)
- [Russian Language Soluton](https://github.com/JuiceFV/Data_Analyst_Internship_Task/blob/master/Solution_RUS.pdf) ([Download](https://github.com/JuiceFV/Data_Analyst_Internship_Task/raw/master/Solution_RUS.pdf))
- [English Language Soluton](https://github.com/JuiceFV/Data_Analyst_Internship_Task/blob/master/Solution_ENG.pdf)([Download](https://github.com/JuiceFV/Data_Analyst_Internship_Task/raw/master/Solution_ENG.pdf))
