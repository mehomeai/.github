# Governance

Me Home AI currently uses maintainer-led governance. @Khagendra01 is the initial
organization owner and repository maintainer. This document describes the current
structure; it does not claim a foundation, elected council, or independent board.

## Decisions and contribution path

Discuss substantial design changes in public issues or community discussions.
Record the decision, rationale, alternatives, and migration effects before merging.
Maintainers make final technical decisions after considering contributor feedback.
Participants can progress from contributor to reviewer and maintainer through
sustained contributions, constructive reviews, and responsible handling of access.
New maintainers and governance changes should be proposed publicly.

## Repository access and reviews

The default branch requires a pull request, one approving code-owner review,
passing `Repository health` CI, and resolved conversations. New commits dismiss
stale approvals. Merge history is linear and pull requests use squash merging.
Separate rules prevent default-branch deletion and force pushes without a bypass.
Release tags matching `v*` cannot be deleted or rewritten.

Repository administrators can bypass PR review and CI requirements **only through
a pull request**. This exception allows the sole maintainer to merge their own
work, since GitHub does not permit self-approval. Use it deliberately and explain
the reason in the PR description. Revisit the exception when another maintainer
can review changes. It does not bypass the separate history protections.

CODEOWNERS initially names @Khagendra01. It must name people or teams with write
access and be updated as maintainers join. Add application build and test jobs
and require their checks when code is introduced; the bootstrap hygiene check
is not an application test suite.

## Releases and security

No stable release or support cadence has been promised. Use semantic versioning
when releases begin, describe breaking changes, and publish reproducible build
instructions. Do not publish packages or enable deployments until their access,
permissions, and validation are designed. Follow SECURITY.md for private reports.

## Ownership continuity

Add a trusted second owner and establish recovery arrangements as the team grows.
Enable strong account authentication and review organization access regularly.
Never share personal account credentials or commit deployment secrets.
