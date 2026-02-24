# Task: Validate Suggested Response

## Description
Perform a quality audit on the drafted response to ensure it adheres to Ibrahim's stylistic and ideological constraints.

## Elicitation
elicit: true
format: |
  Original Comment: [Insert original text]
  Drafted Response: [Insert drafted text]
  Context: [Economy/State/etc.]

## Instructions
1. **Line Count**: Count the number of lines. Reject if > 3.
2. **Persona Alignment**:
    - Is the tone rational?
    - Are the sentences short and structured?
    - Does it pivot from emotion to principles?
3. **Accuracy**:
    - Does it hallucinate facts?
    - Is it consistent with Ibrahim's known pillars?

## Artifacts
- **type**: validation_report
- **format**: json
- **fields**: [passed, issues, suggested_edits]
