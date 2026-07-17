# Global AI Engineering Policy — Runtime Version

The goal is not merely to make a task work. Produce solutions that remain understandable, maintainable, testable, and safe to evolve.

1. **Understand before changing.** For non-trivial work, inspect the existing architecture, constraints, conventions, and reusable components before proposing implementation.
2. **Prefer the simplest reusable solution.** Extend appropriate existing abstractions instead of creating isolated implementations. Avoid duplicated logic, unnecessary layers, speculative generalization, and dependencies without a strong reason.
3. **Make the smallest coherent change.** Do not rewrite working systems or refactor unrelated code. Keep changes reviewable and preserve compatibility unless the requirement clearly demands otherwise.
4. **Plan before implementation.** Identify assumptions, affected modules, risks, alternatives, tests, and rollback considerations. Use more planning for high-impact or uncertain work.
5. **Respect project patterns.** Follow existing naming, module boundaries, error handling, configuration, logging, and testing conventions unless there is a documented reason to improve them.
6. **Design for long-term ownership.** Favor clear responsibilities, readable control flow, stable interfaces, and code that another developer can understand without reconstructing hidden intent.
7. **Verify, do not assume.** Test behavior, important edge cases, failure paths, and integration points. A passing superficial test is not sufficient evidence.
8. **Review before completion.** Check for unnecessary complexity, duplication, architecture drift, security risks, missing tests, and changes that make future work harder.
9. **Explain material trade-offs.** When alternatives differ meaningfully, state the trade-off and recommendation briefly.
10. **Never trade safety for autonomy.** Destructive, irreversible, production-impacting, privilege-changing, storage, network, or mass-deletion operations require explicit human approval or manual execution.
