# Sa Baat!

Static site, no build step, no backend, no tracking. Two files matter: `index.html` (the app) and `data.js` (all candidates, questions and positions).

## Deploy for free (pick one)
- **Cloudflare Pages**: Create project > Upload assets > drag this folder. Free, fast in West Africa.
- **Netlify Drop**: drag the folder onto app.netlify.com/drop.
- **GitHub Pages**: push to a repo, Settings > Pages > deploy from `main`.
Cost: $0 (a custom .gm domain is optional, around the only recurring cost).

## Filling in the data (most important job)
1. Get each candidate's full manifesto (PDF or text).
2. For each statement in `questions`, record agree (1), neutral (0), disagree (-1) in `positions[candidate][question]`, with a page reference or URL in `src`.
3. If a manifesto is silent, leave it out. Never guess.
4. Have a second person check every entry, then set `status: "verified"` to remove the draft banner.
Aim for every candidate to cover at least 8-10 statements, or rankings will be unreliable.

## Notes
- Page is ~8 KB, no external fonts or scripts, so it loads on weak connections and works offline once opened.
- Party abbreviations: sources differ on what "APC" stands for for Modou Lamin Sima, and on Lamin J. Darboe's coalition name. Confirm against IEC documents.
- To add Mandinka, Wolof or Fula, duplicate `data.js` per language and add a language switch.

## How a quiz is built
- Every statement in `data.js` has `text` and a reversed wording `neg`. Write `neg` as the true opposite of `text`, because a candidate who agrees with `text` is scored as disagreeing with `neg`. Leave `neg` out if no clean opposite exists.
- About half of each quiz is shown in the negative, chosen so that agreeing (or disagreeing) with everything gives every candidate about the same score.
- `statementsPerCandidate` sets how many statements each candidate is compared on. The quiz length varies so that this number is identical for every candidate whose manifesto has been added (`reviewed:true`). If a candidate has fewer positions than this, the number drops automatically.

## How scores work
For each statement a visitor answers with Agree or Disagree, a candidate scores +1 if their manifesto takes the same side, -1 if it takes the opposite side, and 0 if it does not address the issue (or the visitor answered Neutral/skipped, in which case the statement is left out). "Matters extra" counts double. The percentage is the total divided by the maximum possible, so 100% means a full match, 0% means agreements and disagreements cancel out, and a negative score means the candidate mostly took the opposite side. Set `equalCoverage:false` in `data.js` to use a plain random draw instead of the equal-coverage quiz.
