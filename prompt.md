# Ledger — System Context

## What It Is
Schema registry and data obligation manager. Owns field-level classifications and annotations that propagate into Pact, Arbiter, Baton, and Sentinel.

## How It Works
Schema YAML -> Registry (store verbatim) -> Annotations (field-level) -> Propagation Table (data-driven) -> Export adapters -> Migration gating.

## Key Constraints
- Field classification is first-class (38 constraints, C001-C038)
- Annotations propagate via data-driven table, never hardcoded
- Schema changes are append-only
- One backend per component (ownership exclusive)
- Return ALL violations, not just first
- Graceful degradation when Arbiter unavailable

## Architecture
10 components + config. Core: registry (schema store), migration (diff engine + blast radius), export (5 consumer adapters), mock (canary fingerprinting). Unified unit/unit_type abstraction across 12 backend types.

## Classification Tiers
PUBLIC, PII, FINANCIAL, AUTH, COMPLIANCE — each with distinct propagation rules and 8 built-in annotations.

## Exports
- Pact: component contracts with field annotations
- Arbiter: classification rules, canary fingerprints
- Baton: egress node config, field masks
- Sentinel: severity mappings per field

## Done Checklist
- [ ] Schema append-only invariant holds
- [ ] Annotation propagation is data-driven (no hardcoded rules)
- [ ] All validation returns ALL violations
- [ ] Export adapters produce valid consumer format
- [ ] Arbiter unavailability handled gracefully
