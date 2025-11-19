# GitHub_Q
This repo contains all knowledge on GitHub &amp; the related automation scripts &amp; procedures.

### Here’s the timeline of what happens when you commit locally and then pull before pushing:
```
Remote Repo (GitHub)       Local Repo (Your PC)
------------------------------------------------
| Commit A | Commit B |    | Commit A |
------------------------------------------------
You make changes → Commit C locally
------------------------------------------------
Now:
Remote: A → B
Local:  A → C
------------------------------------------------
If you push now → Conflict risk because B exists remotely.
------------------------------------------------
Solution:
Pull first → Merge B into your local branch:
Local becomes: A → B → C (or merged)
------------------------------------------------
Then push → Everyone is in sync.
```
So pulling ensures you integrate others’ changes before pushing yours.
