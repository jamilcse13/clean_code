# Clean Code

## Why Writing Clean Code Matters
### Reasons of Writing Clean Code:
* Reading is harder than writing
* Technical debt is depressing
* You're lazy
* Sloppy = Slow
* Don't be a verb

## Clean Code Principles
1. Right tool for the job
   - Watch for boundaries
   - Stay native over framework or tools
   - One language per file
2. High signal-to-noise ratio
   - TED: Terse, Expressive, Does one thing
   - DRY
   - Watch for repeated patterns
3. Self-documenting
   - Clear intent
   - Layers of abstractions
   - Format for readability
   - Favor code over comments

* The **_Rule_** of 7 effects
  - Number of parameters per method
  - Number of methods per class
  - Number of variables currently in scope


* **_DRY_** Principle: Don’t Repeat Yourself


## Naming
### Why Naming Matters
* To maintain code readability
* To avoid variable confliction
* To understand the intent of the variable
```angular2html
// bad practice
List<decimal> p = new List<decimal>() {5.50, 1.48};
decimal t = 0;
foreach(var i in p)
{
    t += i;
}
return t;
```
```angular2html
// good practice
List<decimal> prices = new List<decimal>() {5.50, 1.48};
decimal total = 0;
foreach(var i in prices)
{
    total += i;
}
return total;
```
