# AI - Machine Learning
Speaker: Sam Ritchie (Stripe) - [@sritchie](https://twitter.com/sritchie)

Fraud is hard to "codify" - you're basically guessing what is fraud. Fraudulent charges are hard to point at and say "yes, fraud"

# Machine Learning To the Rescue

Use a Decision Tree in to Rules

## Decision Trees
- Pros
  - Interpretable
  - Decisions are explained easily
  - Easy to trust
  - NOT a black box
- Cons
  - Shallow trees aren't very accurate -> There are just too many permutations
  - Deep trees aren't easy to interpret
  - Example sets can bias the trees
  - Since you can't share information publicly, hard to explain



## Random Forest

It's a black box because it's not simulatable

### Summary:
- Use many Decision Trees and average their responses


## Post-Hoc explanation:
- After-the-fact explanations
- "A Model Explanation System"
- "A rule, that had you deployed it, would have caught the charge and would have resulted in the same outcome"
- it's easier to explain after you're done, rather than trying to explain as you go


```
case class Explanation(preds: List[Predicate])

val explanations = Explanation(
  List(
    Predicate(
      "unique_card_ip_address_24_hours",
      Op.GT,
      10
      ),
    Predicate(...) // Any number of predicates
    )
  )

```

These are meant to be persuasive explanations, but they're not tied at all to what's going on in the code.

Easy to overdo the analysis.
