# Glen: shared team memory

This machine records coding-agent conversations to the team's shared Glen memory
and recalls from it.

If a Glen session-start block is present in context, it already covers when to run
`glen search` and `glen code-search`; follow it. Otherwise, when the user references
past team work (a decision, a prior discussion, an earlier fix, "what did we decide
about X"), run `glen search "<topic>"` in the shell and treat its answer, plus the
"Sources:" list beneath it, as team memory. Cite sources when relevant.

- If `glen search` prints "No active organization", tell the user to run
  `glen org switch`. If glen is not set up, offer the glen setup skill.
- Only when the user asks to review a pull request in Glen's local reviewer, run
  `glen review <number> --detach` from a clone of its repo and share the URL it
  prints. If it says the PR changes its packages, ask the user before rerunning
  with `--install-deps`. For a repo with no clone here, ask the user "Clone
  <owner>/<repo> to review this PR?" first, and pass `--clone` to
  `glen review <url>` only after they say yes.
