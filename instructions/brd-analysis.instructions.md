---
description: "Use when reading BRD documents, ADO Stories, BA Documents, or HTML prototypes to plan development work. Covers how to extract requirements and map them to tasks."
---

# Document Analysis Standards

## Reading Order

1. **BRD / ADO Story** — establishes the business requirement and acceptance criteria.
2. **BA Document** — provides detailed functional specification and edge cases.
3. **HTML Prototype** — defines the visual and interaction target.

## Extraction Checklist

When given any document, extract and confirm:

- [ ] Business objective / user story ("As a... I want... So that...")
- [ ] Acceptance criteria (numbered list)
- [ ] UI elements and their behaviour
- [ ] API endpoints or data dependencies
- [ ] Validation rules
- [ ] Edge cases and error states
- [ ] Accessibility requirements

## Mapping to Code

- Each acceptance criterion → at least one component or function.
- Each UI section in the prototype → one React component.
- Each API call in the spec → one service function in `src/services/`.
- Each validation rule → one validator function in `src/utils/validators.ts`.

## What to Clarify Before Coding

- Missing acceptance criteria that cannot be inferred from context.
- Ambiguous data shapes (ask before inventing field names).
- Authentication / authorisation requirements not covered in the BRD.
