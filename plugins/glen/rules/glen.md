# Glen — shared team memory

This machine records coding-agent conversations to the team's shared Glen memory
and recalls from it. Cursor cannot inject team memory per turn, so:

- When the user references past team work — a decision, a prior discussion, an
  earlier fix, "what did we decide about X" — run `glen search "<topic>"` in the
  shell and treat its dated output as team memory. Cite dates when relevant.
- If `glen search` prints "No active organization", tell the user to run
  `glen org switch`. If glen is not set up, offer the glen setup skill.
- When you share a GitHub pull request URL, also surface its Glen review link
  (https://app.tryglen.com/review/<owner>/<repo>/<number>).
