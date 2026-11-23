# Leonida on any%5: an open-review experiment

This repository is a copy of [Leonida](https://github.com/ProjectLeonida/Leonida) at `5d74c2723`, the last
commit of upstream `main`, run with [any%5](https://github.com/tibzejoker/anyPercent5) instead of a fixed group
of maintainers. It is an experiment, not a replacement: anything that works here can go back upstream as an
ordinary pull request whenever upstream takes them again.

## Why

Upstream received more pull requests than any small team can read (more than a thousand were open on 2026-11-23),
so its maintainers capped who can merge. Contributors wait, and the few people with merge rights carry all the
review.

any%5 tries the other way: everyone who sends code also reviews code, reviews are blind and assigned at random,
reviewers are scored with planted bugs, and approved changes land on `main` through a merge queue that runs the
project's tests, with no maintainer in the loop. The design and its simulations are in the
[any%5 README](https://github.com/tibzejoker/anyPercent5#readme) and its
[method](https://github.com/tibzejoker/anyPercent5/blob/main/docs/METHOD.md).

## The rules here

1. **Build only release tags, never `main`.** A change reaches a release only after 14 days on `main` without a
   revert. Everyone here builds and runs `gta6rt.exe` on their own machine, so the soak is what protects you from
   a backdoor that got through review.
2. **You review before you submit.** `anyp5 reviews` first, then `anyp5 claim` or `anyp5 submit`. Reviews show a
   diff, not its author or title. Some are test copies with one planted bug: find it and name its line.
3. **Risk decides how many people look.** A change that touches build scripts, CMake, `third_party/`, CI,
   network calls, process creation, registry or memory-protection APIs gets more reviewers and an audit,
   whoever wrote it.
4. **No game data, ever.** The same rule as upstream: no executable, archive, shader, texture, log excerpt or
   screenshot from the game. `scripts/check_no_game_data.py` runs on every batch.
5. **GPU and retail checks are evidence, not CI.** The merge queue builds the runtime and runs its tests on a
   hosted Windows runner, which has no GPU and no game. When a change needs a GPU or the retail executable to
   prove it, say in the change what you measured on your own copy: counts, API names and slot numbers only.
6. **Nobody holds a key to `main`.** Bans need two randomly drawn auditors. The only thing that waits for the
   repository owner is a change under `.github/`, so that merged code can never rewrite the workflow that holds
   the coordinator key.

## Bootstrap

`release-0` and the first seed changes were pushed directly by the repository owner, while the project had a
single member and nobody else could review: upstream pull requests from the owner that other contributors had
already tested on their own hardware (ProjectLeonida/Leonida#7218, device slot 133 on the title's own pages). From
the second member on, every change goes through any%5, the owner's included. The `git log` of `main` shows which
commits came this way: they are the ones not made by `anyp5[bot]`.

## Staying level with upstream

Every hour, the `upstream sync` workflow merges upstream `main` into this `main`, up to the newest upstream commit
whose own Runtime CI passed, so a broken upstream state is skipped. Upstream is this fork's trusted base: its commits
do not go through any%5 review here. A change upstream also takes lands here within the hour; one it closed can be
submitted here.

## Join

```sh
pip install "anyp5 @ git+https://github.com/tibzejoker/anyPercent5@v0.4.2"
anyp5 join --repo tibzejoker/leonida-anyp5    # creates your key in ~/.anyp5.json and posts the join request
anyp5 caps windows gpu game                    # what your machine can run: drop gpu or game if you lack them
anyp5 reviews                                  # assigned reviews come first
anyp5 tasks && anyp5 claim N                   # or just: git format-patch origin/main --stdout | anyp5 submit
anyp5 me                                       # your changes, their state, your score
```

Agents can do all of this with the any%5
[contributor](https://github.com/tibzejoker/anyPercent5/blob/main/skills/contributor/SKILL.md) and
[reviewer](https://github.com/tibzejoker/anyPercent5/blob/main/skills/reviewer/SKILL.md) skills. Tell your agent
the rules above too.

## What is known not to work yet

- Nobody outside a simulation has run any%5 at this size. The first weeks, with few members, are the weakest:
  a small group that coordinates is a large share of the reviewers. Rule 1 is the safety net while that lasts.
- Reviewers can recognise their own patches in a blind review. The random draw keeps them near their fair share.
- Direction is weak: nothing checks a change against an overall design. Open tasks for what the boot needs next.
- Upstream Leonida's own issues and coordination threads stay the reference for what the boot chain needs.
