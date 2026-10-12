---
description: Advanced Conditional Logic
---

# Setting up advanced conditionals

Learn how to build powerful conditional workflows using AND/OR operators in Looply.

***

### Introduction

Conditional logic allows your workflows to make smart decisions based on multiple criteria. This guide will teach you how to combine conditions using AND and OR operators to create sophisticated business rules.

By the end of this tutorial, you'll understand:

* How AND and OR operators work together
* The concept of condition groups
* How to build complex business logic in the UI
* Workarounds for advanced scenarios

***

### Understanding the Basics

#### The Golden Rule

> **OR creates groups. AND combines conditions within a group.**

Think of it this way:

| Operator | What it does                                        |
| -------- | --------------------------------------------------- |
| **OR**   | Starts a new possibility                            |
| **AND**  | Adds another requirement to the current possibility |

When your workflow evaluates conditions, it checks if **any one group** is fully satisfied. Within each group, **all conditions** must be true.

***

### How Condition Groups Work

#### Building Groups Step by Step

When you add conditions in the Looply UI, they're processed in order:

1. Start with the first condition (this begins Group 1)
2. Each **AND** condition gets added to the current group
3. Each **OR** condition starts a brand new group
4. Continue until all conditions are added

#### Visual Example

Consider these five conditions:

| # | Condition       | Type   | Group   |
| - | --------------- | ------ | ------- |
| 1 | status = 201    | SIMPLE | Group 1 |
| 2 | subrc = 0       | AND    | Group 1 |
| 3 | var1 > 10       | OR     | Group 2 |
| 4 | var2 = "active" | AND    | Group 2 |
| 5 | var3 = 100      | OR     | Group 3 |

This creates three groups:

* **Group 1:** status = 201 AND subrc = 0
* **Group 2:** var1 > 10 AND var2 = "active"
* **Group 3:** var3 = 100

**Final evaluation:** Group 1 OR Group 2 OR Group 3

The workflow proceeds if **any one** of these groups evaluates to true.

***

### Practical Example: Order Processing

Let's build a real-world scenario. Your business wants to process an order if:

* Status is "approved" AND amount is less than £1,000, **OR**
* Priority is "high" AND customer is "premium" AND region is "US", **OR**
* Override flag is enabled

#### Building This in the UI

| Step | Condition      | Operator  | Value      | Type   |
| ---- | -------------- | --------- | ---------- | ------ |
| 1    | status         | equals    | "approved" | SIMPLE |
| 2    | amount         | less than | 1000       | AND    |
| 3    | priority       | equals    | "high"     | OR     |
| 4    | customer\_type | equals    | "premium"  | AND    |
| 5    | region         | equals    | "US"       | AND    |
| 6    | override\_flag | equals    | true       | OR     |

#### Resulting Groups

| Group   | Logic                                                              |
| ------- | ------------------------------------------------------------------ |
| Group 1 | status = "approved" AND amount < 1000                              |
| Group 2 | priority = "high" AND customer\_type = "premium" AND region = "US" |
| Group 3 | override\_flag = true                                              |

**Final Logic:** Group 1 OR Group 2 OR Group 3

#### Test Scenarios

| Test Case          | status   | amount | priority | customer\_type | region | override | Result           |
| ------------------ | -------- | ------ | -------- | -------------- | ------ | -------- | ---------------- |
| Standard approval  | approved | 500    | normal   | standard       | UK     | false    | ✅ Pass (Group 1) |
| Premium fast-track | pending  | 5000   | high     | premium        | US     | false    | ✅ Pass (Group 2) |
| Manager override   | rejected | 10000  | low      | standard       | UK     | true     | ✅ Pass (Group 3) |
| No match           | pending  | 2000   | normal   | standard       | UK     | false    | ❌ Fail           |

***

### Advanced Scenarios

#### What's Not Supported

The current system uses flat operator precedence where AND binds tighter than OR. This covers the vast majority of workflow use cases, but **nested parentheses are not directly supported**.

Examples that can't be built directly:

* `A AND (B OR C)` — nested OR within AND
* `(A OR B) AND (C OR D)` — multiple nested groups

#### The Solution: Distributive Law

You can rewrite nested expressions using mathematical equivalence. This is the same principle from Boolean algebra.

**Example 1: A AND (B OR C)**

**Equivalent form:** (A AND B) OR (A AND C)

Build it like this:

| Step | Condition | Value    | Type   |
| ---- | --------- | -------- | ------ |
| 1    | A         | value\_a | SIMPLE |
| 2    | B         | value\_b | AND    |
| 3    | A         | value\_a | OR     |
| 4    | C         | value\_c | AND    |

**Result:** Two groups that together equal the original nested logic.

**Example 2: (A OR B) AND (C OR D)**

**Equivalent form:** (A AND C) OR (A AND D) OR (B AND C) OR (B AND D)

Build it like this:

| Step | Condition | Value    | Type   |
| ---- | --------- | -------- | ------ |
| 1    | A         | value\_a | SIMPLE |
| 2    | C         | value\_c | AND    |
| 3    | A         | value\_a | OR     |
| 4    | D         | value\_d | AND    |
| 5    | B         | value\_b | OR     |
| 6    | C         | value\_c | AND    |
| 7    | B         | value\_b | OR     |
| 8    | D         | value\_d | AND    |

**Result:** Four groups that together equal the original nested logic.

\{% hint style="info" %\} **Pro tip:** While this expansion creates more conditions, the logic is mathematically identical. Your workflow will behave exactly as intended. \{% endhint %\}

***

### Quick Reference

#### Operator Behaviour Summary

| Operator | Creates New Group?    | Effect                |
| -------- | --------------------- | --------------------- |
| SIMPLE   | Yes (first condition) | Starts Group 1        |
| AND      | No                    | Adds to current group |
| OR       | Yes                   | Starts new group      |

#### Evaluation Rules

1. All conditions in a group must be TRUE for the group to pass
2. Only ONE group needs to pass for the overall condition to succeed
3. Groups are evaluated independently

#### Common Patterns

| Business Need               | Pattern                                |
| --------------------------- | -------------------------------------- |
| All criteria must match     | Use only AND operators                 |
| Any one criterion is enough | Use only OR operators                  |
| Multiple valid paths        | Mix AND within groups, OR between them |
| Fallback conditions         | Put fallback as final OR group         |

***

### Summary

* **OR** separates your conditions into independent groups
* **AND** chains conditions together within a group
* The workflow passes if **any single group** is fully satisfied
* For nested logic, use the distributive law to expand into equivalent flat conditions

