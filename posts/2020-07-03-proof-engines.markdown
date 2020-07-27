---
title: "If Software Teams were Proof Engines"
class: prose
description: Richard's Software Blog
---

[<u>Internet</u>](https://www.uxmatters.com/mt/archives/2018/01/a-shift-from-engineering-driven-to-design-driven-business-models.php) [<u>ink</u>](https://twitter.com/sachinrekhi/status/1232412946434641920) [<u>spills</u>](https://www.productplan.com/product-culture-mistakes/) [<u>occasionally</u>](https://twitter.com/AustinTByrd/status/1104466068146335744?s=20) [<u>about</u>](https://twitter.com/tsunanet/status/1234866890670923776) a distinction between "product-driven" and "engineering-driven" software cultures. These terms are somewhat vague -- apparently companies like Google are "engineering-driven" and companies like Amazon or Apple are "product-driven," though I can't speak to that.

I'd summarize the two mentalities like this:

**Product-driven approach**

  1. ???
  2. Ship desirable feature
  3. Profit

**Engineering-driven approach**

  1. Write desirable code
  2. ???
  3. Profit

This reminds me a little bit of "backward chaining" and "forward chaining": the two basic "inference methods" that proof engines use when searching for proofs.

"Backward chaining" is when you start from the proposition you hope to prove, your "goal", and proceed by identifying "subgoals", the proof of which would amount to proving your "goal", and then identifying subgoals of those subgoals, and so forth, until you reach subgoals that can be proven directly.

For example, suppose you want to prove the proposition "Prince Andrew and Prince William are related" given the following facts and rules:

* **Fact 1.** Queen Elizabeth is a parent of Prince Andrew.
* **Fact 2.** Queen Elizabeth is a parent of Prince Charles.
* **Fact 3.** Prince Charles is a parent of Prince William.
* **Rule 4.** If X is a parent of Y, then X is an ancestor of Y
* **Rule 5.** If X is an ancestor of Y, and Y is an ancestor of Z, then X is an ancestor of Z.
* **Rule 6.** If X is an ancestor of Y, and X is an ancestor of Z, then Y and Z are related.

With backward chaining, you take your goal, look for a rule where the "consequent" (the part after the "then") matches your goal, and then the antecedent (the part before the "then" and after "if") becomes your "subgoal." Then you add variable substitution with backtracking, and the ability to split conjunctions ("and"), according to some rules that I won't lay out explicitly (and also am not that confident in, I am only very superficially acquainted with this subject matter). Still, I'll run through an example. 
It's lengthy and rather boring -- skip ahead once you've get the gist.

1. Start with the goal "Prince Andrew and Prince William are related".
2. Notice that this matches the consequent of rule 6 "Y and Z are related", with Y="Prince Andrew" and Z="Prince William".
3. Substitute into the antecedent of rule 6: "X is an ancestor of Prince Andrew and X is an ancestor of Prince William". This is our new subgoal. This makes sense -- Princes Andrew and William are related if they share X, a common ancestor. Now who might that common ancestor be?
4. Try X = Prince Charles. Of course, we know the common ancestor is not Charles. But we are pretending to be a proof engine, which isn't smart enough to know and must try every possibility. Now our new subgoal is "Prince Charles is an ancestor of Prince Andrew, and Prince Charles is an ancestor of Prince William."
5. Break up the conjunction. See if we can satisfy "Prince Charles is an ancestor of Prince Andrew".
6. Notice that it matches the consequent of rule 4, "X is an ancestor of Y" with X = Prince Charles and Y = Prince Andrew.
7. Substitute into the antecedent of rule 4 for "Prince Charles is a parent of Prince Andrew."
8. Uh oh, this cannot be satisfied and doesn't match the consequence of any rule.
9. Let's backtrack to 5 when we were trying to prove "Prince Charles is an ancestor of Prince Andrew". Rule 4 didn't pan out, but maybe it can be proven another way.
10. Notice that it matches the consequent of rule 5, "X is an ancestor of Z", with X = Prince Charles and Z = Prince Andrew.
11. Substitute into the antecedent of rule 5: "Prince Charles is an ancestor of Y and Y is an ancestor of Prince Andrew"
12. Try Y = Prince Andrew. Now we are considering "Prince Charles is an ancestor of Prince Andrew, and Prince Andrew is an ancestor of Prince Andrew.
13. Break up the conjunction. See if we can satisfy "Prince Charles is an ancestor of Prince Andrew".
14. Wait, we were already trying to prove this in #5. Therefore, this proof would be circular, and we must backtrack. Let's backtrack to #11, when we are trying to prove "Prince Charles is an ancestor of Y and Y is an ancestor of Prince Andrew".
15. Try Y = Queen Elizabeth. Now we need to prove "Prince Charles is an ancestor of Queen Elizabeth and Queen Elizabeth is an ancestor of Prince Andrew"
16. This will fail -- I'll omit the details (Prince Charles is not, in fact, an ancestor of Queen Elizabeth.) In fact, there is no Y such that "Prince Charles is an ancestor of Y and Y is an ancestor of Prince Andrew". We could backtrack all the way to #6, when we were trying to prove "Prince Charles is an ancestor of Prince Andrew", but applying rule 4 failed, and so did applying rule 5. There are no more rules to apply.
17. Therefore we should backtrack further to #4, when we were trying to prove "X is an ancestor of Prince Andrew and X is an ancestor of Prince William". This time, let's try a different X, X = Queen Elizabeth. So our goal is "Queen Elizabeth is an ancestor of Prince Andrew, and Queen Elizabeth is an ancestor of Prince William".
18. Break up the conjuction, first let's prove "Queen Elizabeth is an ancestor of Prince Andrew."
19. By rule 4, we need only to prove "Queen Elizabeth is a parent of Prince Andrew.
20. That is satisfied directly in fact #1!
20. Let's try the other half of the conjunction from #17: "Queen Elizabeth is an ancestor of Prince William."
21. By rule 4, we can pick a subgoal "Queen Elizabeth is a parent of Prince William."
22. But this is a dead end and matches no rules. Let's backtrack to 20, where we were trying to prove "Queen Elizabeth is an ancestor of Prince William" but try a rule other than than rule 4.
23. Notice this matches the consequent of rule 5 - "X is an ancestor of Z" with X = Queen Elizabeth, Z = Prince William.
24. Substitute into the antecedent of rule 5 for "Queen Elizabeth is an ancestor of Y, and Y is an ancestor of Prince William"
25. Let's try Y = Prince Andrew. Then we must prove
"Queen Elizabeth is an ancestor of Prince Andrew, and Prince Andrew is an ancestor of Prince William".
26. Break up the conjuction. First let's prove "Queen Elizabeth is an ancestor of Prince Andrew."
27. By rule 4, we need only prove that "Queen Elizabeth is a parent of Prince Andrew.
28. This is satisfied by fact #1.
29. Now let's try the other half of the conjunction in #25. "Prince Andrew is an ancestor of Prince William"
30. By rule 4, we need only prove that "Prince Andrew is a parent of Prince William".
31. But this is a dead end. Let's backtrack to #29 and try and prove "Prince Andrew is an ancestor of Prince William" by means other than rule 4.
32. How about rule 5, for "Prince Andrew is an ancestor of Y, and Y is an ancestor of Prince William"
33. This will fail... I'll omit the details. We need to backtrack further to 24, when we were trying to prove "Queen Elizabeth is an ancestor of Y, and Y is an ancestor of Prince William".
34. Before we tried Y = "Prince Andrew" and that failed. This time lets try Y = Prince Charles, so "Queen Elizabeth is an ancestor of Prince Charles, and Prince Charles is an ancestor of Prince William".
35. Break up the conjunction and aim for the first half: "Queen Elizabeth is an ancestor of Prince Charles"
36. By rule 4, aim for "Queen Elizabeth is a parent of Prince Charles"
37. This is satisfied by fact #2.
38. Now aim for the second half of the conjunction in 34, "Prince Charles is an ancestor of Prince William".
39. By rule 4, aim for "Prince Charles is a parent of Prince William"
40. This is satisfied by fact #3.
41. We're done! We've found a proof.

So the characteristic of "backward chaining" is that with this approach you explore a lot of subgoals that would be useful if you could prove them, but that may turn out to be impossible to prove.

So what is forward chaining? Unlike backward chaining, with forward chaining you don't start with a goal. You start with what you know. And then you use what you know to deduce new knowledge. And you use that new knowledge to deduce more new knowledge -- until hopefully one of the things you deduce is the goal.

So, given the same example.

1. Pick a rule at random. Let's say rule 6.
2. Take the antecedent of the rule "if X is an ancestor of Y, and X is an ancestor of Z," and try to apply it to a fact.
3. Hmm, nope. We actually haven't deduced that anybody is the ancestor of anybody else yet, so we struck out with rule 6. Let's try a different one.
4. What about the antecedent of rule 4? "If X is a parent of Y, then X is an ancestor of Y".
5. Well we know from fact #1 that Queen Elizabeth is a parent of Prince Andrew. Therefore we can deduce by rule 4 that Queen Elizabeth is an ancestor of Prince Andrew.
6. Furthermore, we know from fact #2 that Queen Elizabeth is a parent of Prince Charles. Therefore we can deduce by rule 4 that Queen Elizabeth is an ancestor of Prince Charles.
7. Furthermore, we know from fact #3 that Prince Charles is a parent of Prince William. Therefore we can deduce by rule 4 that Prince Charles is an ancestor of Prince William.
8. That's it for rule 4. Now let's try applying rule 6, the antecedent of which is "If X is an ancestor of Y, and X is an ancestor of Z".
9. What if we let X = Queen Elizabeth, Y = Prince Andrew, and Z = Prince William? We deduced in #5 than Queen Elizabeth is an ancestor of Prince Andrew. But we've never deduced that Queen Elizabeth is an ancestor of Prince William, so this is a no go.
10. What if we let X = Queen Elizabeth, Y = Prince Andrew, and Z = Prince Charles? We deduced in #6 than Queen Elizabeth is an ancestor of Prince Charles, and we deduced in #5 than Queen Elizabeth is an ancestor of Prince Andrew. Both conditions are satisfied. Therefore we can deduce by rule 6 that "Prince Andrew and Prince Charles are related." Unfortunately, that's not our goal.
11. What about rule 5 now, with the antecedent "if X is an ancestor of Y, and Y is an ancestor of Z."
12. Let's take X = Queen Elizabeth, Y = Prince Charles, and Z = Prince William. We deduced in #6 that Queen Elizabeth was ancestor to Charles, and in #7 that Charles was ancestor to William. Therefore by rule 5 we can deduce that Queen Elizabeth is ancestor to Prince William.
13. Now let's do again like we did in #8-9 -- use rule 6 with antecedent "If X is an ancestor of Y, and X is an ancestor of Z", and let X = Queen Elizabeth, Y = Prince Andrew, and Z = Prince William.
14. We deduced in #5 that Queen Elizabeth is an ancestor of Prince Andrew. We deduced in #12 that Queen Elizabeth is an ancestor of Prince William. Therefore we can conclude that Prince Andrew and Prince William are related.
15. That was our goal -- yay!

So the characteristic of "forward chaining" is that with this approach you explore a lot of deductions that are completely valid but do not lead in any way to your goal.

As I mentioned, the "engineering-driven" approach to software kind of reminds me of forward chaining. When you're being "engineering-driven" your ideas about what to do next are suggested by the code itself. You know what changes are easy, or natural, or interesting code-wise. Maybe you implemented some feature with a one-to-many relationship in the database, and it occurred to you "it would be interesting if it were many-to-many". Maybe you refactored the stylings of your frontend into a common module, and you know it would just be a little bit more effort to add dark mode. Recently, as part of implementing a project, I reified the shape of a system which had been implicit into a concrete data type. And I thought, "now that I have a data structure that represent this system, I bet it wouldn't be too hard to write a function that takes two of these data structures and produces a diff, and I can use that to generate a changelog". Which is something some colleagues had spoken of and thought might be useful, but wasn't made a priority because it didn't seem worth the effort. But this sort of behavior -- any thought like "now that I have X, I bet it wouldn't be too hard to..." is forward chaining. It's looking for opportunities to move forward based on the current state of the system, not based on a product goal.

Now the "product-driven" approach, like backward chaining, is goal-directed. Leadership has an idea that will revolutionize the industry if we can pull it off. That's a goal. Or maybe somebody put together a list of the most common feature requests from users. Those are goals. The implementation comes later. Heck, who even knows if the implementation is possible under reasonable time and quality constraints? The idea comes first, and then it's up to the engineers or the software architects to come up with an implementation plan and decide whether or not it is feasible. And hopefully the implementation plan is incremental, and the project is broken up into parts -- subgoals, if you will.

Good product-driven people, of course, have a strong sense of what is feasible, and take that into account when they are choosing goals. And good engineering-driven people have a sense too of what sort of properties are valuable in a system from a product perspective, and don't go off writing random code without considering how it might plausibly lead to a desirable goal. This is much as mature proof engines don't naively backward-chain or forward-chain. They use heuristics, and are clever to pick subgoals that are more likely to be feasible when backward chaining, or to pick deductions that are more likely to lead to the goal when forward chaining.

"Engineering-driven" forward chaining can go bad: refactoring code as an end in itself. Introducing fashionable new frameworks or software paradigms just because they are cool, without a compelling product justification. "Product-driven" backward chaining can go bad too -- in a "feature factory" where software teams chase shipping feature after feature, without a mind to code or product quality.

I'm afraid I don't really have much of a point beyond introducing this analogy, and hoping you'll find it interesting. I think product-drivenness and engineering-drivenness each have their place. I think my personal nature as a software developer tends toward being more "engineering-driven" than "product-driven" -- I tend to be more engaged when I'm writing cool code for a boring reason, over boring code for a cool reason, although I consciously try to correct this and behave more "product-driven" than I would otherwise.

But my feeling is that what seems to be the "default" software culture undervalues "engineering-drivenness" somewhat. I wrote in my [last post](2020-03-28-against-process.html) about a mentality bent on systematizing software teams, that sees software teams like priority queues, and the central problem in software as assigning the highest priorities to the tasks with the highest expected net value. In that sort of paradigm, the goals are known, and so "product-driven" backward chaining is favored.

A different mentality wants software teams to be more organic. Software isn't always the pursuit of known goals -- often it is highly exploratory in nature. In this paradigm, the goals are unknown, so "engineering-driven" forward chaining is more favored.

So that's my take on "product-driven" vs "engineering-driven" software cultures. What about you? Are you happiest when forward chaining or backward chaining? Were your team's biggest or most surprising successes the result of forward chaining or backward chaining? Something to think about in the next team planning session.
