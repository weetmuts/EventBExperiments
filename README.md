
Event-B Experiments by Fredrik Öhrström

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
