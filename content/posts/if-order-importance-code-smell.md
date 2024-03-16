+++
title = "Does the importance of the order of if statements indicate a \"code smell\"?"
description = ""
date = "2024-03-16"
author = "Filip Karlo Došilović"
tags = [
    "code-smell",
    "software-engineering",
    "programming",
]
enableComments = true
+++

Have you ever seen a code like this:

```python
def get_temperature_description(temperature: Celsius) -> TemperatureDescription:
  if temperature > 30:
    return TemperatureDescription.HOT
  if temperature > 25:
    return TemperatureDescription.WARM
  if temperature > 18:
    return TemperatureDescription.PLEASANT
  # ...
```

or this:

```python
def get_bmi_description(bmi: float) -> BMIDescription:
  if bmi >= 30:
    return BMIDescription.OBESE
  if bmi >= 25:
    return BMIDescription.OVERWEIGHT
  if bmi >= 18.5:
    return BMIDescription.NORMAL
  return BMIDescription.UNDERWEIGHT
```

The examples above share one important characteristic: **the order of if statements matter**. Changing that order would yield the wrong results. As such, we could ask **does the importance of the order of if statements indicate a _code smell_**.

## The Problem

**The main problem is that such code contains a silent, invisible requirement — the order of statements matters for the correct behavior**. If you are just reading the code, and have no intentions to change it, you are fine. But, if you are assigned a task to refactor or add additional code, without any comment or documentation that explicitly mentions that the order of if statements matters you could easily inadvertently disrupt the functionality by rearranging, modifying, or adding the statements in the wrong order. In this case, that invisible requirement is the main culprit that could cause all sorts of problems in the future.

Ok, but the order of statements matters in general, in every function or algorithm, that is how we get the desired behavior, you might think. And you would be correct. Indeed, the order of statements determines the flow and logic of functions and algorithms. However, the critical distinction lies in how this order is communicated and managed within the code.

In well-designed code, the importance of the order is either self-evident through clear and logical structuring, or it is explicitly documented. For example, in a well-structured loop, the sequence of operations follows a logical progression that is easy to understand. Similarly, in a complex algorithm, good practice dictates that comments or documentation should guide the reader through the sequence, making clear why certain steps follow others.

The issue arises when this order is not obvious or documented, especially in cases where the sequence might seem counterintuitive or where rearranging the code doesn’t immediately appear to break anything. Such scenarios create a trap for developers, particularly those who are new to the codebase or less experienced. They might make changes that seem harmless but disrupt the delicate balance of the program's logic.

Therefore, while it’s true that the order of statements is inherently important in coding, **the real problem occurs when this order becomes an unspoken rule, hidden within the code**. This lack of transparency can lead to bugs and errors that are difficult to trace and fix, ultimately impacting the maintainability and reliability of the software.

## Solutions

To fix the main problem — invisible requirement — we have to somehow make it visible, make it stand out. There are two possible ways to solve it.

The first solution is to document that the order of if statements matters. For instance:

```python
def get_bmi_description(bmi: float) -> BMIDescription:
  # NOTE: Order of if statements matters!
  if bmi >= 30:
    return BMIDescription.OBESE
  if bmi >= 25:
    return BMIDescription.OVERWEIGHT
  if bmi >= 18.5:
    return BMIDescription.NORMAL
  return BMIDescription.UNDERWEIGHT
```

While this is a perfectly valid approach, it is rather lazy and should rarely be used. In general, the programming wisdom says that documentation is the developer's best friend, but, in this case, there is a better solution.

A better solution would be to **write code in a way that eliminates the problem — the importance of order**. For instance, for the BMI example, we could explicitly write the boundaries of each range, both upper and lower bound:

```python
def get_bmi_description(bmi: float) -> BMIDescription:
    if bmi < 18.5:
        return BMIDescription.UNDERWEIGHT
    if 18.5 <= bmi < 25:
        return BMIDescription.NORMAL
    if 25 <= bmi < 30:
        return BMIDescription.OVERWEIGHT
    return BMIDescription.OBESE
```

Probably the first thing you notice is that the code does contain some order, specifically the BMI ranges follow a logical order. This is a typical programming convention, ordering the cases in a logical order, that makes the code easier to read and easier to reason about. But this is different from the main problem, ordering `if` statements to get the correct behavior, because now the order of `if` statements does not matter. **Statements are ordered for the sake of readability, not for the correct behavior.**

In the end, the problem is eliminated at its core, the code, thereby reducing the chances that the code and documentation would get out of sync, as was the case for the first proposed solution. Now, no matter how we order the `if` statements, the function will return the correct result for each input.

## Conclusion

In general, you should avoid writing `if` statements whose overall behavior depends on the order of execution, or, as a matter of fact, any other invisible requirements. If you find yourself writing such statements, try to refactor them in a way that eliminates those requirements, or, in the rarest of cases, document those requirements in the form of comments for that block of code.
