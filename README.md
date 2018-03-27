
Event-B Experiments by Fredrik Öhrström, developed using Roding 3.3 with SMT provers installed.

**Experiment 8:** max(all) = max(ran(to_week)∪ran(to_month))

The pattern matching rule that discharges max existance cannot understand anything else than
a keyword in place for the set. For example, calc/act_1/WD is proved automatically
because I name the variable `m=ran(to_week)∪ran(to_month)` together with `finite(m)`
then the existance predicate: `∃b·∀x·x∈m⇒b≥x` is automatically proved. If m is replaced with
ran(to_week)∪ran(to_month), then suddenly it cannot be proved! Also if the m is not explicitly
declared finite using `finite(m)`, the pattern matching proving does not match either. It does not help that
`finite(ran(to_week)∪ran(to_month))` is a hypothesis.

Using this information it is possible to prove inv_9/WD by naming an expression. I.e.
we have the goal `∃b·∀x·x∈ran(to_week)∪ran(to_month)⇒b≥x` then enter the expression
`ran(to_week)∪ran(to_month)` in the proof control, then click the ae (Add Expression) button.
The goal is transformed into `∃b·∀x·x∈ae⇒b≥x`. We still have to provide the hypothesis
`finite(ae)` and clicking ah, then two clicks on robot will automatically prove the supplied hyp and
also discharge the proof obligation.

**Experiment 7:** simple max of the union of two sets

Surprisingly difficult, as with all things max..... the problem is that the prover does not
understand that the union of two finite sets are also finite. Adding a theorem does not help.
Only adding a very explicit guard helps! If not using the guard, then the automatic prover fails.

The interactive prover runs ahead and destroys the goal for automatic proving. Thus
you must prune twice to get back to simpler goals.

calc/act1/WD: Prune at the root, then add the hypothesis `finite(m)`,
then prune again where the goal is `m≠∅ ∧ (∃ b · ∀ x · x∈m ⇒ b≥x)` and the hyps are:
```
to_week≠∅
to_month≠∅
m=ran(to_week)∪ran(to_month)
finite(m)
```
then run robot. Now it autoproves.

Odd that it can easily prove `finite(m)` but if max requires that for the WD proof,
why does it not attempt that proof automatically?

**Experiment 6:** count occurences of coins and count how many coins there are inside each block

Use an undefined function to_block to categorize the natural numbers into blocks.
This could for example be a blocks by 10, ie 0..9 maps to 0, 10..19 maps to 1, 20..29 maps to 2, etc.

Then accumulate natural numbers (`coins ⊆ ℕ`) keep a count of the number of occurences
(counts ∈ coins → ℕ) and how many coins in each block (bcounts ∈ to_block[coins] → ℕ)

addNewCoinAndNewBlock/inv_4/INV: goal `bcounts∈{b ↦ 1}∈to_block[coins∪{c}] → ℕ`
do not run robot, it will expand too much,
simply expand b into `to_block(c)` the goal `bcounts∈{to_block(c) ↦ 1}∈to_block[coins∪{c}] → ℕ`
can then be discharged using SMT.

addNewCoinUpdateBlock/inv_4/INV: again do not run robot (aka prune at
top) then replace b with `to_block(c)`, then prune AGAIN immediately
where the replacement took place, because the robot has already gone
to far. SMT is successfull immediately where the replacement took
place, but not if the robot is allowed to continue replacing.
(In fact it seems like it is the replace of `bn+1` into `bcounts(to_block(c))+1` that
makes the goal too complicated for the provers.)

**Experiment 5:** max of odd coins

When refining move a guard into an invariant. `@grd_3 odds = { x·x∈coins ∧ x mod 2 = 1 ∣ x }` becomes ` @inv1_3 collected_odds = { x·x∈coins ∧ x mod 2 = 1 ∣ x }`

findMax/inv_3/INV: Goal `max(odds)∈ℕ` Do not let the robot run! It will expand odds into full set comprehension!
Instead prune to top, then type `odds⊆ℕ` in proof control then click the ah button (AddHypothesis). Then robot. The subgoal has been proved but the robot has unfortunately again expanded odds into full set comprehenstion. Prune the latest proof node with `eh with odds={...}` and then click ML (not robot!). Proof complete.

findMax/act_1/WD: robot then for b in: `∃b·∀x·x∈coins ∧ x mod 2=1 ⇒ b≥x` type `max(coins)` into yellow box. Then click robot.

ignoreEven/inv1_3/INV: robot then click = for set equality rewrite, then robot, then dc `x=c`, then robot*2.

findMax/grd_1/GRD: robot, then expand `collected_odds` in hyp, then robot.

findMax/grd_2/GRD: no robot, just expand `collected_odds` in hyp and the proof will be completed automatically.

thm2_1/WD: robot, then supply `max(s)` as upper bound in yellow box.

thm2_2/WD: robot, then supply `p` as upper bound in yellow box, then EITHER: prove by case `x=p`, then ML. OR use SMT directly.

INITIALISATION/thm2_2/INV: robot then for goal `p=max(s∪{p})` run SMT. OR

addOdd/inv2_4/INV: robot then for goal `max(collected_odds∪{c})=c` search/add theorem `∀p,s·p∈ℕ∧s⊆ℕ∧¬s=∅∧finite(s)∧p>max(s)⇒p=max(s∪{p})` instantiate p=c and s=collected_odds in the yellow boxes. It is automatically discharged.

**Experiment 4:** max of subsets `{w·w∈weeks ∣ max({ x·x∈points_in_time ∧ w=to_week(x) ∣ x }) }`

prune/grd2/WD: robot then in goal: `∃b·∀x·x∈points_in_time∧to_week(p)=to_week(x) ⇒ b≥x` insert `max(points_in_time)` in yellow box.

prune/invpnt1/INV: robot then for goal: `max({x·x∈points_in_time∧to_week(p)=to_week(x) ∣ x}) ∈ ℕ` apply SMT prover.

prune/invpnt2/INV: robot then for goal: `finite({w·∃p·p∈points_in_time∧w=to_week(p) ∣ max({x·x∈points_in_time∧w=to_week(x) ∣ x})})` click finite and select finite set of non-negative numbers. Then insert max(points_in_time) for existential goal. Then after robot, drop hyp `finite(points_in_time)` and search/add hyp `points_in_time⊆ℕ` then run SMT to discharge final `max(....})∈ℕ`

**Experiment 3:** collect odd coins

ignoreSingleEvenCoin/inv1_13/INV: `∀x·x∈checked∪{c} ∧ x mod 2=1 ⇒ x∈collected` since this is an even coin
x can never be c (`c mod 2 = 0`). After robot, for goal `x∈collected` prove by case: `x=c`.
The case `x=c` will quickly generate a contraction which makes that branch true!
The case `¬x=c` will imply `x∈checked` and then the hyp `∀x·x∈checked∧x mod 2=1⇒x∈collected` is used to prove the goal.

You can also do robot, then for goal: `x∈collected` insert x into the yellow box for hyp
`∀x·x∈checked ∧ x mod 2=1 ⇒ x∈collected`, then the obligations is proved using robot. Strange looking proof.

collectOddCoins/inv1_8/INV: robot, then for goal: `x mod 2=1` supply prove by case: `x=c`

collectOddCoins/act_1/SIM: robot, then in: `collected={x·x∈coins∧x mod 2=1 ∣ x}` click = to prove ⊆ in both directions instead.

**Experiment 2:** maximum of set

Add to set and get max, implement max using var, drop the actual set.

Useful theorem: `@thm1_0 ∀p,s·p∈ℕ ∧ s⊆ℕ ∧ s≠∅ ∧ finite(s) ∧ p≤max(s) ⇒ max(s) = max(s∪{p})`

thm1_0/WD proof: robot, then for b in goal: `∃b·∀x·x∈s∪{p} ⇒ b≥x` supply max(s) in yellow box.

**Experiment 1:** split event

Split a single copy event into two separate copy events.
