
Event-B Experiments by Fredrik Öhrström

**Experiment 1:**

Split a single copy event into two separate copy events.

**Experiment 2:**

Add to set and get max, implement max using var, drop the actual set.

theorem @thm1_0 ∀p,s·p∈ℕ ∧ s⊆ℕ ∧ s≠∅ ∧ finite(s) ∧ p≤max(s) ⇒ max(s) = max(s∪{p})

For thm1_0/WD proof: robot, then for b in: ∃b·∀x·x∈s∪{p} ⇒ b≥x supply max(s).