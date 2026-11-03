# HFEDM-Hypothetical-Five-Layer-Emotion-Dynamics-Model
A mathematical specification for a five-layer emotional dynamics system designed for conversational agents.
Emotion Dynamics Model under Conversational and Task Contexts

## What is this
A mathematical model of emotion designed for use in conversational agents. It models emotion as a five-layer dynamical system (temperament anchoring → object sentiments → long-term background → proximal response → short-term mood + subjective fatigue), outputs an eleven-axis state (ten emotions + subjective mental fatigue), and explicitly distinguishes the ownership boundaries between the emotion module and the memory module.

This repository currently focuses on **specification and theoretical documents**, providing computable equations, interface contracts, and engineering mathematical initial values. It does not yet contain complete implementation code or human validity verification.

## Core Features
- **Five-layer division of labor**: L1 affective style (slow), L2 object sentiments, L3 long-term background \(m\), L4 proximal response \(e\), L5 short-term mood \(x\) + fatigue \(f\).
- **Moving constraint box**: The long-term background directly constrains the allowable region of the short-term mood \(\mathcal{C}(m)\), rather than relying solely on decay.
- **Separation of events and continuous inputs**: One-time event jumps + continuous sources + internal residual evolution.
- **Bounded personality adaptation**: Affective baseline \(b\) is adjusted slowly only through qualified experiences; rewriting of memory strategies is not authorized.
- **Explicit interface contracts**: Time sharing with the memory module, historical snapshot recording, replay quotas, and ownership boundaries are all formally specified.
- **Engineering deployability**: Provides complete engineering numerical initial values, quantization rules (three decimal places while preserving the constraint box), and numerical analysis conditions.

## Current Status
- Theoretical specification: HFEDM and its specification document (complete definition that can be used independently)
- Memory theory: Not yet fully integrated (this specification has defined the merge interfaces and conditions for validity)
- Human validity: Verification not completed (psychology literature serves only as construct basis; equations and coefficients are mostly self-created)
- Real-world clock: This stage uses abstract time and is not connected to a real clock
- Implementation code: Not yet included

## Important Boundaries (Please Read First)
- Does not model physiological arousal, hormones, sleep, or circadian rhythms.
- Personality evolution is limited to emotional tendencies and emotional expression; rewriting of memory content, retrieval rules, or strategies is not authorized.
- The eleven-axis output is not a fully recoverable state; pause/resume requires saving internal state.
- Engineering initial values are self-set and await calibration; no claim is made that they already match humans.
- Memory replay has independent quotas and dosages and is not automatically counted as new evidence or new personality experience.

## How to Use This Specification
1. First read the “Core Features” and “Important Boundaries” sections of this README.
2. Enter [`docs/theory.md`](docs/theory.md) and read the corresponding chapters as needed.
3. If implementing, prioritize referencing:
   - State and evolution equations
   - Event contracts and shared flows
   - Output quantization
   - Interface with memory
   - Engineering initial values

## Important Statement
- The theory is currently in a state with no database that the author is unable to establish, and therefore does not claim to have obtained human validity or correctness verification. It only provides a hypothetical equation theory as a basic reference for implementation.
- There will be **no** subsequent additions of new clauses, supplements, or refinements to this theory. If considering it as a research basis (extension) or project reference, please keep this statement in mind.
- Literature referenced by this theory will be annotated at the end of the theory document. When using it as a project foundation or for extended research, please ensure compliance with the respective academic regulations (regulations may not be consistent). Any infringement or issues arising from personal problems are unrelated to the author.
- When using this document, please add a usage statement at the end and clearly indicate the source as ChichiOfficial and the project name. If it has been helpful to you, it is my honor.
