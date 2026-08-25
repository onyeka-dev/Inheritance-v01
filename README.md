# Inheritance-v01
Solidifying my understanding of inheritance and it's relationship with cascade.

CSS Inheritance Lab

Why I Built This

I decided not to move on from CSS Inheritance immediately after studying it.

Instead, I built a small experiment designed to force myself to predict how inheritance, the cascade, and CSS-wide keywords would behave before checking the browser.

The goal wasn't to practice writing HTML or CSS. I already know the basics.

The goal was to find out whether I could actually reason about inheritance when multiple CSS concepts interact.

---

What I Wanted to Understand

At first, inheritance seemed simple:

«A child gets certain properties from its parent.»

But the deeper I went, the more questions appeared.

What happens when the child already has its own value?

What if I explicitly use "inherit"?

What does "initial" actually do?

How is "unset" different from "inherit"?

And then there was "revert".

Was "revert" related to the parent-child relationship, or was it related to the cascade?

Then I discovered "revert-layer", which introduced another layer of the cascade.

So I built the experiment to answer those questions instead of simply memorising definitions.

---

My Learning Process

My process was:

Learn
  ↓
Understand the concept
  ↓
Build an experiment
  ↓
Predict the result
  ↓
Test in the browser
  ↓
Compare prediction vs reality
  ↓
Explain the result
  ↓
Document what I learned

This was important because knowing the definition of inheritance is different from being able to predict what the browser will actually do.

---

What I Learned

1. Natural Inheritance

Some CSS properties naturally pass their values from a parent to a child.

For example:

.parent {
  color: red;
}

If the child doesn't establish its own "color", it can receive the parent's value.

Parent
  color: red
      ↓
   inheritance
      ↓
Child
  color: red

I also learned that not every CSS property inherits automatically.

Properties such as:

- "color"
- "font-family"
- "font-size"
- "line-height"

are normally inherited.

Properties such as:

- "margin"
- "padding"
- "border"
- "width"
- "height"

are normally not inherited.

One important lesson here was that I shouldn't guess whether a property inherits.

If I'm unsure, I should check the property's documentation.

---

2. A Child's Own Declaration

One of the first things I tested was what happens when the child already has its own value.

.card {
  color: red;
}

p {
  color: blue;
}

The paragraph stays blue.

The parent being red doesn't mean the child must become red.

The child already has its own declaration.

This also taught me something important about specificity:

«A parent's specificity does not directly compete with a child's declaration because they target different elements.»

The ".card" declaration applies to the parent.

The "p" declaration applies to the paragraph.

---

3. "inherit"

Then I tested the "inherit" keyword.

.parent {
  padding: 40px;
}

.child {
  padding: inherit;
}

Normally, "padding" doesn't inherit.

But "inherit" explicitly forces the child to take the value from its immediate parent.

Parent
  padding: 40px
       ↓
    inherit
       ↓
Child
  padding: 40px

This gave me one of my simplest mental models:

«"inherit" = take my immediate parent's value.»

---

4. "initial"

Next came "initial".

.parent {
  color: red;
}

.child {
  color: initial;
}

I initially had to separate this from inheritance.

"initial" does not look at the parent.

It tells CSS to use the property's CSS-defined initial value.

So:

inherit
→ look at parent

initial
→ look at property's initial value

My mental model became:

«"initial" = go back to the property's starting value.»

---

5. "unset"

"unset" was slightly more interesting because it behaves differently depending on the property's normal inheritance behavior.

If the property normally inherits:

unset
  ↓
inherit

If the property normally doesn't inherit:

unset
  ↓
initial

For example:

.parent {
  color: red;
}

.child {
  color: unset;
}

Since "color" normally inherits:

unset
 ↓
inherit
 ↓
red

But:

.parent {
  margin: 40px;
}

.child {
  margin: unset;
}

Since "margin" normally doesn't inherit:

unset
 ↓
initial
 ↓
0px

My mental model became:

«"unset" = follow the property's normal inheritance behavior.»

---

6. "revert"

"revert" initially confused me because I wasn't sure whether it had anything to do with inheritance.

It doesn't.

"revert" belongs to the cascade.

It backs out of the current cascade origin's styling and allows the cascade to fall back to what would have applied without that origin's contribution.

That means "revert" does NOT mean:

«Go to my parent.»

And it doesn't mean:

«Go to my grandparent.»

Instead:

«"revert" = back out of the current cascade origin.»

This gave me an important distinction:

inherit
→ DOM / parent-child relationship

revert
→ cascade / origin relationship

---

7. "revert-layer"

Then I encountered "revert-layer".

This is similar to "revert", but it operates at the cascade layer level.

It backs out of the current cascade layer and allows the cascade to determine what would apply without that layer's contribution.

It doesn't move through the DOM.

It doesn't mean "inherit from another parent."

It doesn't simply mean "use the initial value."

My mental model became:

«"revert-layer" = back out of the current cascade layer.»

So now I have:

inherit
→ parent

revert
→ cascade origin

revert-layer
→ cascade layer

---

The Biggest Thing I Learned

The biggest lesson from this experiment wasn't actually one of the keywords.

It was understanding the relationship between cascade and inheritance.

Before this experiment, I could easily mix them together.

Now I understand that they have different jobs.

Cascade asks:

«Which declaration wins?»

Inheritance asks:

«Where does the value come from when inheritance is involved?»

This distinction became extremely important when I tested:

.card {
  color: red;
}

.card p {
  color: blue;
}

.card p {
  color: inherit;
}

Both ".card p" declarations have the same specificity.

So the later declaration wins through source order:

.card p {
  color: inherit;
}

Only after that declaration wins does "inherit" get resolved.

It then looks at the immediate parent:

.card {
  color: red;
}

Therefore the final color becomes red.

This gave me my strongest mental model:

Declarations apply
      ↓
Cascade decides the winner
      ↓
Winning declaration is resolved
      ↓
If the value is `inherit`
      ↓
Look at immediate parent
      ↓
Use parent's value

---

A Mistake That Helped Me Understand It

One of the most useful mistakes I made was assuming that "inherit" somehow competes with other values by itself.

It doesn't.

For example:

.exp p {
  color: blue;
}

.exp p {
  color: inherit;
}

The two declarations have the same specificity.

So the cascade uses source order.

The second declaration wins.

Only then does "inherit" operate.

But if something more specific wins:

#special {
  color: blue;
}

.exp p {
  color: inherit;
}

The ID selector wins.

Therefore "inherit" never gets the opportunity to operate.

This reinforced another important rule:

«"inherit" only operates if its declaration wins the cascade.»

---

My Final Mental Model

After completing the experiments, I now think about CSS like this:

          CSS DECLARATIONS
                 ↓
        Which declarations apply?
                 ↓
              CASCADE
                 ↓
       Which declaration wins?
                 ↓
       What is the winning value?
                 ↓
      ┌──────────┴──────────┐
      ↓                     ↓
 normal value           CSS keyword
                              ↓
                  resolve what it means
                              ↓
                         final value

And the CSS-wide keywords:

inherit
→ Take my immediate parent's value.

initial
→ Use the property's CSS-defined initial value.

unset
→ If normally inheritable → behave like inherit.
  If normally non-inheritable → behave like initial.

revert
→ Back out of the current cascade origin.

revert-layer
→ Back out of the current cascade layer.

---

What Changed In My Understanding

Before studying inheritance, I mostly thought:

«"If the parent has a property, the child gets it."»

Now I understand that this is incomplete.

I have to ask:

1. Does the property normally inherit?
2. Does the child have its own applicable declaration?
3. Which declarations actually target the child?
4. Which declaration wins the cascade?
5. What value did the winning declaration provide?
6. If that value is a CSS-wide keyword, what does that keyword tell CSS to do?

That is a much more accurate mental model.

---

Summary

The sentence I'm taking away from this entire experiment is:

«Cascade chooses the winner. The winning value tells CSS what to do.»

And when that winning value is "inherit":

«Take the property's value from my immediate parent.»

This experiment also taught me something broader about learning CSS:

Knowing a definition isn't the same as being able to predict the browser.

Building the experiment, making predictions, being wrong, and then explaining the browser's behavior gave me a much stronger understanding than simply reading the documentation and moving on.

That is the real reason I built this lab.
