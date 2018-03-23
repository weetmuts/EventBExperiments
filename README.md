
Event-B Experiments by Fredrik Öhrström

**Experiment 5:** max of odd coins

When refining move a guard into an invariant. `@grd_3 odds = { x·x∈coins ∧ x mod 2 = 1 ∣ x }` becomes ` @inv1_3 collected_odds = { x·x∈coins ∧ x mod 2 = 1 ∣ x }`

findMax/inv_3/INV: Goal `max(odds)∈ℕ` Do not let the robot run! It will expand odds into full set comprehension!
Instead prune to top, then type `odds⊆ℕ` in proof control then click the ah button (AddHypothesis). Then robot. The subgoal has been proved but the robot has unfortunately again expanded odds into full set comprehenstion. Prune the latest proof node with `eh with odds={...}` and then click ML (not robot!). Proof complete.

findMax/act_1/WD: robot then for b in: `∃b·∀x·x∈coins ∧ x mod 2=1 ⇒ b≥x` type `max(coins)` into yellow box. Then click robot.

ignoreEven/inv1_3/INV: robot then click = for set equality rewrite, then robot, then dc `x=c`, then robot*2.

findMax/grd_1/GRD: robot, then expand `collected_odds` in hyp, then robot.

findMax/grd_2/GRD: no robot, just expand `collected_odds` in hyp and the proof will be completed automatically.

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
