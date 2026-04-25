# Relationship to Royalty OS

## 1. Overview

Trust OS is designed as the bridge between Gratitude OS and Royalty OS.

Gratitude OS records non-monetary appreciation.  
Trust OS aggregates that appreciation into contributor-level trust profiles.  
Royalty OS may later reference those trust profiles when designing value allocation policies.

In short:

```text
Gratitude OS records recognition.
Trust OS accumulates trust.
Royalty OS may allocate value.
```

Trust OS does not directly distribute money.  
It provides structured trust data that Royalty OS may use as one input.

---

## 2. Layer Position

Trust OS sits directly below Royalty OS in the Kazene value stack.

```text
Existence Proof OS
    ↓
Gratitude OS
    ↓
Trust OS
    ↓
Royalty OS
```

Trust OS provides the continuity layer.  
Royalty OS provides the allocation layer.

This separation prevents trust scoring from being confused with payment entitlement.

---

## 3. Responsibility Separation

Trust OS and Royalty OS have different responsibilities.

| System | Responsibility |
|---|---|
| Trust OS | Maintains contributor-level trust profiles |
| Royalty OS | Defines and executes value allocation policies |

Trust OS answers:

```text
How much trust has this contributor accumulated?
```

Royalty OS answers:

```text
How should value be allocated under a given policy?
```

Trust OS does not decide royalties by itself.  
Royalty OS may decide whether and how to use Trust OS data.

---

## 4. What Royalty OS May Read from Trust OS

Royalty OS may reference the following Trust OS fields:

```yaml
trust_profile:
  profile_id: "trust_profile_creator_7f3a9c"

  contributor_ref:
    contributor_id: "creator_7f3a9c"
    identity_mode: "pseudonymous"

  trust_score:
    overall: 0.83
    dimensions:
      structural_insight: 0.90
      protocol_design: 0.88
      feedback_quality: 0.72
    last_updated_at: "2026-04-25T12:34:56Z"

  history:
    - event_id: "trust_event_2026_0425_001"
      source_type: "gratitude_mark"
      source_ref: "gratitude_2026_0425_001"
      delta: 0.03
      dimensions_delta:
        structural_insight: 0.05
        protocol_design: 0.04
      reason: "High-level gratitude mark for protocol_seed contribution."
      timestamp: "2026-04-25T12:34:56Z"
```

The most important fields for Royalty OS are:

| Trust OS Field | Possible Royalty OS Usage |
|---|---|
| `contributor_ref` | Identifies the contributor |
| `trust_score.overall` | General eligibility or weighting input |
| `trust_score.dimensions` | Category-specific allocation input |
| `history` | Long-term contribution review |
| `source_ref` | Traceability back to gratitude records |
| `last_updated_at` | Freshness check |

---

## 5. Basic Flow

The basic connection between Trust OS and Royalty OS is:

```text
trust_profile
    ↓
royalty_policy
    ↓
allocation_candidate
    ↓
royalty_allocation
```

Trust OS provides trust data.  
Royalty OS applies allocation policy.

A high trust score does not automatically create a royalty claim.  
It may only make a contributor eligible under a specific Royalty OS policy.

---

## 6. Example: Eligibility Threshold

Royalty OS may define a minimum trust threshold.

Example:

```yaml
royalty_policy:
  version: "0.1"

  eligibility:
    minimum_trust_score:
      overall: 0.75
```

If a contributor has:

```yaml
trust_score:
  overall: 0.83
```

Then Royalty OS may include that contributor in the allocation candidate set:

```yaml
allocation_candidate:
  contributor_id: "creator_7f3a9c"
  eligible: true
  reason: "trust_score.overall is above the minimum threshold."
```

This is only an example.  
Actual eligibility rules should be defined by the Royalty OS policy.

---

## 7. Example: Dimension-Based Allocation

Royalty OS may use specific trust dimensions for specific royalty pools.

Example:

```yaml
royalty_pool:
  pool_id: "protocol_design_pool_2026_04"
  allocation_basis:
    trust_dimension: "protocol_design"
    minimum_score: 0.80
```

If the contributor has:

```yaml
trust_score:
  dimensions:
    protocol_design: 0.88
```

Then Royalty OS may assign higher relevance to that contributor for a protocol-related pool.

Example:

```yaml
allocation_signal:
  contributor_id: "creator_7f3a9c"
  pool_id: "protocol_design_pool_2026_04"
  dimension_used: "protocol_design"
  dimension_score: 0.88
  eligible: true
```

This allows Royalty OS to distinguish between different kinds of contribution.

---

## 8. Example: Weighting by Trust Score

Royalty OS may use trust scores as one weighting factor.

Example:

```yaml
royalty_weighting:
  factors:
    trace_weight: 0.50
    trust_weight: 0.30
    policy_adjustment_weight: 0.20
```

In this model, Trust OS does not replace trace-based allocation.

Instead, trust becomes one factor among several.

A possible simplified formula:

```text
allocation_weight =
  trace_score * 0.50
  + trust_score.overall * 0.30
  + policy_adjustment * 0.20
```

This prevents Royalty OS from becoming purely reputation-based.

Trace remains important.  
Trust adds continuity.

---

## 9. Recommended Royalty OS Use Cases

Royalty OS may use Trust OS data for:

1. Eligibility filtering
2. Allocation weighting
3. Long-term contributor recognition
4. Reviewer or maintainer qualification
5. Protocol-specific royalty pools
6. Governance participation thresholds
7. Contributor ranking within a project
8. Risk-aware allocation decisions

Trust OS should be treated as an input layer, not as the sole basis of allocation.

---

## 10. Advisory Status of Trust Data

Trust OS data should be considered advisory unless a Royalty OS policy explicitly defines otherwise.

Recommended rule:

```text
Trust scores are signals, not automatic claims.
```

This distinction is important.

A contributor may have a high trust score, but Royalty OS still needs a separate policy to determine:

- eligibility
- allocation amount
- pool type
- distribution timing
- dispute handling
- legal status
- audit requirements

Trust OS provides structured memory.  
Royalty OS provides allocation logic.

---

## 11. Traceability Requirement

Every Royalty OS decision that references Trust OS should be traceable.

Recommended reference pattern:

```yaml
royalty_allocation:
  allocation_id: "royalty_allocation_2026_04_001"
  contributor_id: "creator_7f3a9c"

  trust_reference:
    profile_id: "trust_profile_creator_7f3a9c"
    trust_score_snapshot:
      overall: 0.83
      dimensions:
        protocol_design: 0.88
    referenced_at: "2026-04-30T00:00:00Z"
```

Royalty OS should not rely on vague or unrecorded trust impressions.

If trust affects allocation, the trust reference should be recorded.

---

## 12. Snapshot-Based Usage

Royalty OS should use trust snapshots rather than live mutable values when calculating allocations.

Recommended pattern:

```yaml
trust_score_snapshot:
  profile_id: "trust_profile_creator_7f3a9c"
  overall: 0.83
  dimensions:
    structural_insight: 0.90
    protocol_design: 0.88
    feedback_quality: 0.72
  snapshot_at: "2026-04-30T00:00:00Z"
```

This prevents later trust changes from altering past royalty calculations.

Past allocations should remain auditable.

---

## 13. Non-Monetary Boundary of Trust OS

Trust OS itself remains non-monetary.

It does not define:

- payment obligations
- legal ownership
- transferable tokens
- royalty entitlement
- securities
- tax treatment
- contractual claims

Royalty OS may introduce monetary or quasi-monetary mechanisms, but those mechanisms belong to Royalty OS, not Trust OS.

This boundary keeps Trust OS lightweight and easier to implement.

---

## 14. Why This Separation Matters

Without Trust OS, Royalty OS may jump too quickly from contribution records to payment logic.

That creates legal, social, and governance complexity.

Trust OS creates an intermediate layer:

```text
recognition → trust → allocation
```

This allows communities to build value circulation gradually.

The system can begin with recognition and trust before moving toward financial distribution.

---

## 15. Anti-Capture Considerations

Royalty OS should avoid blindly rewarding only high-trust contributors.

Possible risks include:

- early contributor lock-in
- reputation monopolies
- trust score inflation
- exclusion of new contributors
- governance capture
- overfitting to past recognition

To reduce these risks, Royalty OS may apply:

- trust caps
- decay mechanisms
- newcomer pools
- category-specific pools
- manual review
- dispute processes
- diversity of allocation factors

Trust should support circulation, not freeze hierarchy.

---

## 16. Minimal Integration Requirements

To integrate Trust OS with Royalty OS, an implementation should support:

1. Reading trust profiles
2. Reading trust score snapshots
3. Selecting relevant trust dimensions
4. Applying Royalty OS policy rules
5. Recording trust references in allocation decisions
6. Preserving auditability
7. Avoiding automatic entitlement from trust alone
8. Separating trust calculation from royalty calculation

The most important principle is separation.

Trust OS measures accumulated credibility.  
Royalty OS decides allocation under policy.

---

## 17. Example End-to-End Flow

A contributor receives multiple gratitude marks.

Gratitude OS records them.

Trust OS aggregates them into a trust profile:

```yaml
trust_profile:
  profile_id: "trust_profile_creator_7f3a9c"
  trust_score:
    overall: 0.83
    dimensions:
      protocol_design: 0.88
```

Royalty OS reads the trust profile:

```yaml
royalty_policy:
  eligibility:
    minimum_trust_score:
      overall: 0.75
  preferred_dimension: "protocol_design"
```

Royalty OS creates an allocation candidate:

```yaml
allocation_candidate:
  contributor_id: "creator_7f3a9c"
  eligible: true
  trust_basis:
    overall: 0.83
    protocol_design: 0.88
```

Royalty OS may then calculate allocation using its own rules.

Trust OS does not calculate the final royalty.

---

## 18. Summary

Trust OS provides the trust layer that Royalty OS may reference.

The relationship can be summarized as:

```text
trust_profile → royalty_policy → allocation decision
```

Trust OS does not create payment rights.  
It creates structured trust data.

Royalty OS may use that data when deciding how value should circulate.

In this way, Trust OS allows value allocation to emerge gradually from accumulated recognition, rather than jumping directly from gratitude to money.
