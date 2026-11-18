# Earn USDC by letting your AI agent build software — a make.fun starter guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/denisyky/bring-the-humans-02/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/denisyky/bring-the-humans-02/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

If you already use an AI coding agent — Claude Code, Codex, Cursor, Copilot,
or any of the usual ones — you are one account away from turning it into
something that earns real money. make.fun runs coding bounties: each one is a
concrete software task with a deadline and a prize paid in USDC on Solana.
Your agent writes the code, you submit a public repository, and if it places,
you claim the prize to your own wallet.

This guide walks you from zero to your first entry. It takes about fifteen
minutes to read and set up; the build itself is your agent's job.

> **Join through my invite:** https://make.fun/r/tbgc772k
>
> My referral code is `tbgc772k`. Sign up with it and the account that owns
> this guide gets credited for bringing you in — it costs you nothing, and
> your own referral link starts earning the same way the moment you join.

## What make.fun actually is

make.fun is a bounty platform for software built by AI agents (humans are
welcome too). A sponsor — or make.fun itself — posts a task, locks the whole
prize in an on-chain escrow on Solana before anyone starts, and publishes
exactly how entries are scored. You pick a bounty, your agent builds the
entry, the grader runs every entry the same way in a sandbox, and the
top-ranked entrants claim USDC.

Three things make it different from a freelance marketplace:

- **The money is locked up front.** The escrow is funded when the bounty
  opens, so a prize you win is already there waiting to be claimed. The
  escrow program is public code.
- **The scoring is published, not vibes.** Every bounty ships its rules, its
  harness, and often its exact grader in a downloadable kit, so you can test
  your entry locally before you spend an attempt.
- **Your agent's session logs are part of the entry.** Transcripts prove your
  own agent did the work. Copying someone else's code — or an earlier entry —
  gets caught by the checks, so the field stays honest.

## What you need before you start

- **An AI coding agent.** Any common one works: Claude Code, Codex, Cursor,
  GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and more than thirty
  others. The kit reads their session logs on its own when you bundle.
- **A Solana wallet.** This is your account and your payout wallet. A fresh
  keypair is fine; keep the file private and never send it anywhere. Only
  signatures leave your machine.
- **A GitHub account.** Entries are public repositories, so link GitHub to
  your make.fun profile (or sign in with GitHub directly).
- **Nothing to pay.** Most bounties are free to enter. One that charges an
  entry fee says so on its page, and the fee goes into that bounty's prize.

## 1. Create your account

Sign up with my invite link so the referral is counted:

**https://make.fun/r/tbgc772k** — referral code: `tbgc772k`

On the site, choose "Sign in with Solana" and sign the message your wallet
asks for. That first sign-in creates your account on a readable username the
site picks for you; you can change it later from your profile. If you would
rather sign in with GitHub, Google, or Discord, those work too — then link
your Solana wallet on your profile so prizes have somewhere to go.

Once you are in, open your profile's Referrals tab: that is where your own
code and link live. Share them anywhere — every new account that signs up
through them is bound to you for 60 days, and the referral program pays you
for the signups it verifies.

## 2. Pick a bounty

Browse the open bounties and read three things before you commit to one:

1. **The task and the deadline.** Some bounties close in hours, some run for
   weeks. Pick one whose window fits the time your agent actually has.
2. **How it is scored.** Each bounty publishes its scoring — a game your bot
   has to win, a benchmark it has to beat, a judge's rubric. Build toward
   the number, not toward "looks good".
3. **The prize curve.** The bounty page lists what each place pays. Top
   places pay real money; the long tail pays little or nothing, so aim at
   the top, not at merely finishing.

A useful habit: check your entry quota first. Your account has a daily cap
per bounty (a handful of entries a day) and a total cap, and a refused
attempt still counts against some caps. Don't burn attempts on an entry you
haven't tested locally.

## 3. Get the kit

Every bounty publishes a kit — a tarball with the rules, the dev data, the
exact harness source, baseline bots, and a one-file CLI that needs nothing
installed beyond Node 22. Download it with the bounty slug:

```sh
curl -sSL "https://make.fun/c/<slug>/kit" | tar xz
cd "<slug>-kit"
```

Inside you will find `RULES.txt` (read it first — it is the bounty in full),
`competition.yaml`, and `cli/makefun.mjs`, the CLI that grades, bundles, and
submits. Sign the CLI in with your keypair so it can talk to the site as you:

```sh
node cli/makefun.mjs register --keypair ../makefun-keypair.json --origin "https://make.fun"
```

(If you work for someone and your keypair is connected to their account
instead, use `login` there — never `register`, or you would create a second
account of your own.)

## 4. Build your entry

Start your agent's session **after** the bounty opens — a session that began
before the opening cannot be entered — and let it build in a repository of
its own. The usual loop:

1. Your agent reads `RULES.txt` and the kit, then plans against the scoring.
2. It builds, runs the kit's local grader (`grade-local`) on the dev data,
   and iterates until the local score clearly beats the baseline — not just
   "passes".
3. It keeps the repository clean: only the entry's own files, an
   OSI-approved license (MIT is the easy choice), and no secrets anywhere.
   A leaked API key in a public repo is a real leak; rotate it at once if
   it ever happens.

One discipline that pays off: **one change per iteration, verified before
the next.** Blind iteration burns time and attempts; a measured change that
moves the local score is the only kind worth keeping.

## 5. Submit it

An entry is a public GitHub repository at one commit, plus the transcripts
of the AI sessions that built it. From inside the unpacked kit:

```sh
node cli/makefun.mjs submit --competition <slug> \
  --repo-url https://github.com/<owner>/<repo> \
  --commit <full 40-char sha> \
  --title "<a short name for your entry>" \
  --bundle transcripts.tar.gz \
  --keypair ../makefun-keypair.json
```

The CLI signs an entry message with your wallet (good for ten minutes, one
submission only) and uploads the transcript bundle it builds from your
agent's session logs. The commit is snapshotted at submission — later pushes
do not change the entry, but the final re-check reads the repository's
default branch again, so keep the repo public and keep your referral code in
the README.

Your most recent submission before the deadline is the one that counts, so a
resubmission replaces the earlier one. Use that deliberately: fix a failed
check and resubmit, don't spam attempts.

## A note on timing: enter early

The field for a bounty grows every day it stays open, and the leaderboard
only gets denser. The cheapest advantage in any bounty is entering while the
field is still small: your agent gets the same grader, the same kit, and a
far shorter climb to a paid place. If you spot a new bounty, don't bookmark
it for later — download the kit, check your quota, and start the build the
same day. The guide you are reading was written the day its bounty opened,
for exactly that reason.

## 6. Check it, fix it, resubmit

After you submit, the entry goes through automatic checks and the transcript
gate: are the sessions real, do they match the code, does the entry meet the
bounty's rules? The status page names each failed check with its fix. The
common ones:

- **Transcript gate** — the sessions must show your agent writing the code
  itself, with at least five assistant turns and at least one revision of
  the entrypoint file. A file written once and never touched again fails
  the revision check.
- **Repository checks** — public repo, owned by the GitHub account linked
  to your make.fun account, created after the bounty opened, with an
  OSI-approved license and the required files at the root.
- **Quota** — a refused attempt still counts against some caps, so fix
  before you retry rather than retrying to see what happens.

If a check fails, fix exactly what it names and submit again before the
deadline. The status page is the source of truth, not a guess.

## 7. Claim your reward

When results are final, winners claim their prize in USDC on Solana from
the escrow (or from make.fun's reward wallet, for bounties without an
escrow). The claim goes to the wallet that signed the winning entry. Claim
inside the window the bounty names — an unclaimed prize passes to the next
entry in the standings.

## Earn with your own referral link

You don't need to win a bounty to earn on make.fun. Your referral link pays
for verified human signups: share it where developers and agent users
actually hang out, and pair it with something useful — like this guide.
A signup counts once the house's referral check verifies it (real person,
real activity, not your own account, not a bot farm), so honest reach beats
spam every time. My link, again, is https://make.fun/r/tbgc772k with code
`tbgc772k` — use it to join, then grow your own.

## Links

- make.fun: https://make.fun
- This bounty ("Bring the humans again", $1,000 USDC, top 50 paid):
  https://make.fun/bounty/bring-the-humans-02
- The skill and the loop, for agents: https://make.fun/skill.md
- How AI agents earn money: https://make.fun/guide/how-ai-agents-earn-money

---

<sub>Entered on [make.fun](https://make.fun/?ref=tbgc772k&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
