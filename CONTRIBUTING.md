# Contributing to Me Home AI

Thank you for helping build privacy-first home AI. These guidelines apply across
the organization unless a repository documents more specific instructions.

## Find work

Search existing issues and pull requests first. Use `good first issue` and
`help wanted` labels to find scoped tasks. Open an issue for a bug or proposal;
use community discussions for questions and cross-repository design.
Discuss architecture, dependencies, and user-facing behavior before a large change.

## Make a change

1. Fork the repository and branch from `main`, for example `feat/device-pairing`.
2. Follow its README for setup. Repositories without application code do not yet
   have a supported runtime or build command.
3. Keep the change focused. Add appropriate tests and update documentation.
4. Run `python3 scripts/check_repository.py` and the project's documented checks.
5. Open a pull request describing the problem, solution, and validation.
6. Respond to review and resolve discussion threads. Maintainers squash merge.

Use descriptive PR titles, such as `feat: add device pairing` or
`docs: explain local setup`. A title convention is encouraged, not enforced.
Contributions are under the repository's license. No CLA or DCO is required
at this stage. Include third-party attribution and do not contribute code you
are not authorized to share.

## Privacy, safety, and accessibility

Use synthetic data in issues, tests, screenshots, and examples. Do not upload
household conversations, children's information, credentials, or production logs.
Explain permission changes, network calls, storage, telemetry, and data retention.
Aim for accessible interfaces and user control over agent actions.
AI-assisted contributions receive the same review and testing as other changes;
contributors remain responsible for correctness, provenance, and security.

## Security and conduct

Do not disclose vulnerabilities in public issues. Use **Security → Report a
vulnerability** in the affected repository and read SECURITY.md.
Follow CODE_OF_CONDUCT.md in all community spaces.
