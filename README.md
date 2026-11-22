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
For each statement a visitor answers with Agree or Disagree, a candidate scores +1 if their manifesto takes the same side, -1 if it takes the opposite side, and 0 if it does not address the issue. Neutral answers and skips are left out. Statements are weighted: ones that set the candidates apart (some address them, some do not, or they take different sides) count for more than ones every candidate promises, because agreeing with a statement everyone promises says nothing about who to vote for. "Matters extra" doubles a statement. The percentage is the weighted total divided by the maximum possible: 100% is a full match, 0% means agreements and disagreements cancel out, and a negative score means the candidate mostly took the opposite side. Set `weightByDistinctiveness:false` to give every statement the same weight, and `favourDistinctive:false` to stop quizzes favouring distinguishing statements. `equalCoverage:false` switches to a plain random draw.

## Quiz length with many candidates
`statementsPerCandidate` is set to 26 (an even number, so exactly half of each candidate's statements can be shown in the negative). The quiz length follows from it: about 38 statements with 4 candidates, and about 50 once all candidates are in. If an exact match cannot be found at 26, the builder tries 25, and never goes below 25. To shorten the quiz, lower `statementsPerCandidate` (for example to 16). Each candidate needs at least that many positions in `data.js`.

## Adding a candidate
Add their positions under `positions` (use existing statement ids), then set `reviewed:true`. Candidates who withdraw are simply deleted from `candidates`.
