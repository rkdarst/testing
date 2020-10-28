---
layout: episode
title: "Motivation"
teaching: 10
exercises: 0
questions:
  - "Why should we write tests?"
objectives:
  - "Discuss the advantages of writing and maintaining tests in research software."
---

## Why test?

How do you know that you don't mess up when
- you write some small code
- you write a large code

Anything sufficiently large *or* old *will* break eventually.  Do you
want to figure it out quickly or spend weeks debugging?



## Simulations and analysis with untested software do not constitute science

"Before relying on a new experimental device, an experimental scientist always
establishes its accuracy. A new detector is calibrated when the scientist
observes its responses to known input signals. The results of this
calibration are compared against the expected response. **An experimental
scientist would never conduct an experiment with uncalibrated detectors** - that
would be unscientific. So too, simulations and analysis with untested
software do not constitute science."
(copied from [Testing and Continuous Integration with Python](http://katyhuff.github.io/python-testing/),
created by Kathryn Huff, see also the Testing chapter in
[Effective Computation In Physics](http://physics.codes) by Anthony Scopatz and Kathryn Huff)

If you only calibrate once and never again, is that any better?

---

## Testing in a nutshell

In software tests, expected results are compared with observed results in order
to establish accuracy.  For example, put this in ``test_temp.py``:

```python
def fahrenheit_to_celsius(temp_f):
    """
    Converts temperature in Fahrenheit
    to Celsius.
    """
    temp_c = (temp_f - 32.0) * (5.0/9.0)
    return temp_c


def test_fahrenheit_to_celsius():
    temp_c = fahrenheit_to_celsius(temp_f=100.0)
    expected_result = 37.777777
    assert abs(temp_c - expected_result) < 1.0e-6
```

Run with ``pytest .`` - it finds all ``test_*.py`` files, finds all
tests inside, and runs them.  You can also run certain files and
certain tests.

**Exercise:** Why are we not comparing directly all digits with the expected result?

---

## Tests have *many* useful side-effects

- Functionality is preserved, even if you do big changes later
- Help users be confident in your code
- Help developers be confident to modify later (including you!)

---

## Tests make sure that expected functionality is preserved

- As projects grow, it becomes easier to break things without noticing immediately
- Testing helps detecting errors early
- Interpreted dynamically typed imperative languages need to be tested
- So do compiled languages but the compiler catches at least some errors
- Testing is essential for research software because we care about
  reproducibility of scientific results and because derivative research and
  programs depend on research software
- "Program testing can be used to show the presence of bugs, but never to show their absence!" (Edsger W. Dijkstra)

---

## Tests help users of your code

- Make it easier for others to verify whether the code has been correctly installed
- Users of your code publish papers based on results produced by your code
- Your peers need to be able to reproduce your (to be) published computational results

---

## Tests help developers of your code

- Tests make it possible to refactor the code
- Code is unsustainable without runnable tests and becomes legacy software
- Documentation which is up to date by definition
- Easier for external developers to contribute to the project without breaking your code

Suiting up to modify untested code:

<img src="{{ site.baseurl }}/img/suit.jpg" style="width: 400px;"/>

---

## Testable code tends to be better code

- Well structured code is easy to test
- Badly structured code is difficult to test automatically
- **Making code easy to test generally makes it better (more modular)**

#### Good code: pure and easy to test

```python
def fahrenheit_to_celsius(temp_f):
    temp_c = (temp_f - 32.0) * (5.0/9.0)
    return temp_c

temp_c = fahrenheit_to_celsius(temp_f=100.0)
print(temp_c)
```

#### Less good code: has side effects and is difficult to test

```python
f_to_c_offset = 32.0
f_to_c_factor = 0.555555555
temp_c = 0.0

def fahrenheit_to_celsius_bad(temp_f):
    global temp_c
    temp_c = (temp_f - f_to_c_offset) * f_to_c_factor

fahrenheit_to_celsius_bad(temp_f=100.0)
print(temp_c)
```

---

## Easier: defensive programming

``assert`` statements require a condition to be true, otherwise fail.
Example:

```python
def kelvin_to_celsius(temp_k):
    """
    Converts temperature in Kelvin
    to Celsius.
    """
    assert temp_k >= 0.0, "ERROR: negative T_K"
    temp_c = temp_k + 273.15
    return temp_c
```

It's a simple way to prevent you from the most dangerous person you
know: yourself.  But they don't substitute for tests.

---

## Types of testing

Different software has different scales of tests

- Unit tests: individual functions
- Integration tests: things fit together
- System tests: run the whole thing

Different use cases require different types of effort.  Don't be
afraid, do what helps *you*.

---

## Continuous integration

Imagine if you didn't have to think about tests: if they were always
automatically run.  It's possible and easy.

* Example: https://github.com/scipy/scipy/pulls (note green check and
  red x).

Suddenly, you don't have to be afraid about others contributing

---

## Would you trust a code ...

- ... when its tests do not pass
- ... if there are no tests at all
- ... if the tests are never run
