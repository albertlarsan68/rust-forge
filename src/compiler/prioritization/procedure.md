# Prioritization WG - Procedure

This document details the procedure the WG-prioritization follows to fill the agenda for the weekly meeting of `T-compiler`.
The working group focuses mainly on triaging `T-compiler` and `T-libs` bugs, deciding if bugs are critical (potential release blockers) or not and building the agenda for the most important things `T-compiler` needs to discuss.

## General issues review process

- Check the status of the issue
- Try moving it forward if possible (ex. stimulate further comments from the issue author or a reviewer)
- Ask for more info if it's needed
- Is there an MCVE for the issue already?
- Check if it's a regression and label it accordingly (`regression-*` labels)
- Figure out the area the issue belongs and label it accordingly (`A-*` labels)
- [Ping notify groups](https://rustc-dev-guide.rust-lang.org/notification-groups/about.html) or relevant teams
- Assign if possible
- Nominate the issue if it needs to be discussed

## The procedure in detail

High level overview:

- [Prepare agenda content](#prepare-agenda-content)
  - Add `T-compiler` and `T-libs` to unlabelled `T-compiler` and `T-libs` issues
  - Assign priority to unprioritized issues with `I-prioritize` label
  - Process MCPs/FCPs
- [Generate Agenda](#generate-agenda)
  - Run cli to generate agenda
  - Fill agenda announcements
  - Add performance logs
- [Notify the team about the meeting](#notify-the-team-about-the-meeting)
  - Figure out which WGs need to check-in
  - Notify @T-compiler/meeting about the meeting on Zulip
- [Add details to the Agenda](#add-details-to-the-agenda)
  - Summarize stable/beta nominations
  - Summarize PR's waiting on team
  - Summarize `P-critical` and unassigned `P-high` regressions
  - Summarize I-nominated issues
- [Final reviews](#final-reviews)
  - Nominate more issues if needed
  - Re-sync and check the agenda right before the meeting

Then, after the meeting (ideally the same day) there are are some [followup
tasks](#follow-ups-after-meeting):
  - Remove `I-nominated` tags of already discussed issues
  - Accept or decline pull requests nominated for backport (according to the
    meeting decisions)
  - Create the next meeting agenda using the weekly agenda template

### Follow ups after meeting

- Remove [`to-announce` MCPs](https://github.com/rust-lang/compiler-team/issues?q=is%3Aall+label%3Amajor-change+label%3Ato-announce), only the ones that we've added to the agenda and not the ones that may have showed up between we've built the agenda and we execute this step.
- Remove `to-announce` FCPs from [rust repo](https://github.com/rust-lang/rust/issues?q=is%3Aall+label%3Afinished-final-comment-period+label%3Ato-announce), [compiler-team repo](https://github.com/rust-lang/compiler-team/issues?q=is%3Aall+label%3Afinished-final-comment-period+label%3Ato-announce) and [forge repo](https://github.com/rust-lang/rust-forge/issues?q=is%3Aall+label%3Afinished-final-comment-period+label%3Ato-announce), only the ones that we've added to the agenda and not the ones that may have showed up between we've built the agenda and we execute this step.
- Accept [`beta nominated`](https://github.com/rust-lang/rust/issues?q=is%3Aall+label%3Abeta-nominated+-label%3Abeta-accepted) and [`stable nominated`](https://github.com/rust-lang/rust/issues?q=is%3Aall+label%3Astable-nominated+-label%3Astable-accepted) backports that have been accepted during the meeting. For more info check [T-release backporting docs](https://forge.rust-lang.org/release/backporting.html)
  - To accept, add `beta-accepted` and keep `beta-nominated` label for beta backports and add `stable-accepted` and keep `stable-nominated` label for stable backports.
- Decline [`beta nominated`](https://github.com/rust-lang/rust/issues?q=is%3Aall+label%3Abeta-nominated+-label%3Abeta-accepted) and [`stable nominated`](https://github.com/rust-lang/rust/issues?q=is%3Aall+label%3Astable-nominated+-label%3Astable-accepted) backports that have been declined during the meeting.
  - To decline, remove `beta-nominated` label for beta backports and remove `stable-nominated` label for stable backports.
- Remove [`I-nominated`](https://github.com/rust-lang/rust/issues?q=is%3Aopen+label%3AI-nominated+label%3AT-compiler) tags of already discussed issues. For that check previous week agenda and Zulip meeting
- Create an empty agenda using [our template](https://hackmd.io/WQW0yzDDS16YvtBNurmj6A), as soon as our Thursday's weekly meeting ends. After creating the meeting change document permissions to Write -> Owners.
- Copy content from last week's agenda that is still pending. The relevant sections are `PRs waiting on team`, `P-critical`, `P-high regressions` and `Nominated issues`.
- Notify WG leads about [next week's
  checkins](https://rust-lang.github.io/compiler-team/about/triage-meeting/)

### Prepare agenda content

#### Add `T-compiler` and `T-libs` labels

Add `T-compiler` and `T-libs` labels to corresponding issues that are missing these labels.

- [No team assigned unprioritized I-prioritize](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+-label%3AP-critical+-label%3AP-high+-label%3AP-medium+-label%3AP-low+label%3AI-prioritize+-label%3AT-compiler+-label%3AT-cargo+-label%3AT-core+-label%3AT-doc+-label%3AT-infra+-label%3AT-lang+-label%3AT-libs+-label%3AT-libs-api+-label%3AT-libs-api+-label%3AT-release+-label%3AT-rustdoc+-label%3AA-rustdoc+-label%3AA-rustdoc-ui)
- [No team assigned stable nominations](https://github.com/rust-lang/rust/issues?q=is%3Aall+label%3Astable-nominated+-label%3Astable-accepted+-label%3AT-compiler+-label%3AT-cargo+-label%3AT-core+-label%3AT-doc+-label%3AT-infra+-label%3AT-lang+-label%3AT-libs+-label%3AT-libs-api+-label%3AT-libs-api+-label%3AT-release+-label%3AT-rustdoc+-label%3AA-rustdoc+-label%3AA-rustdoc-ui)
- [No team assigned beta nominations](https://github.com/rust-lang/rust/issues?q=is%3Aall+label%3Abeta-nominated+-label%3Abeta-accepted+-label%3AT-compiler+-label%3AT-cargo+-label%3AT-core+-label%3AT-doc+-label%3AT-infra+-label%3AT-lang+-label%3AT-libs+-label%3AT-libs-api+-label%3AT-libs-api+-label%3AT-release+-label%3AT-rustdoc+-label%3AA-rustdoc+-label%3AA-rustdoc-ui)
- [No team assigned I-nominated](https://github.com/rust-lang/rust/issues?q=is%3Aopen+label%3AI-nominated+-label%3AT-compiler+-label%3AT-cargo+-label%3AT-core+-label%3AT-doc+-label%3AT-infra+-label%3AT-lang+-label%3AT-libs+-label%3AT-libs-api+-label%3AT-libs-api+-label%3AT-release+-label%3AT-rustdoc+-label%3AA-rustdoc+-label%3AA-rustdoc-ui)
- [No team assigned PR's waiting on team](https://github.com/rust-lang/rust/issues?q=is%3Aopen+label%3AS-waiting-on-team+-label%3AT-compiler+-label%3AT-cargo+-label%3AT-core+-label%3AT-doc+-label%3AT-infra+-label%3AT-lang+-label%3AT-libs+-label%3AT-libs-api+-label%3AT-libs-api+-label%3AT-release+-label%3AT-rustdoc+-label%3AA-rustdoc+-label%3AA-rustdoc-ui)

#### Assign priority to unprioritized issues with `I-prioritize` label

We need all [`I-prioritize T-compiler`](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AT-compiler+-label%3AP-critical+-label%3AP-high+-label%3AP-medium+-label%3AP-low+label%3AI-prioritize) and all [`I-prioritize T-libs`](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AT-libs+-label%3AP-critical+-label%3AP-high+-label%3AP-medium+-label%3AP-low+label%3AI-prioritize) to be actually prioritized. To do so, we add one of the `P-critical`, `P-high`, `P-medium` or `P-low` labels and remove `I-prioritize` and also add a text such as:

> Assigning `P-XXX` as [discussed as part of the Prioritization Working Group procedure](link_to_zulip_conversation) and removing `I-prioritize`.

The procedure here follows the [General issues review process](#general-issues-review-process).

Note: triagebot automatically creates a topic and notify @*WG-prioritization* members once an issue is labelled with `I-prioritize`

Note #2: These lists should typically be empty when we are close to the meeting.

Note #3: we should not have unprioritized regressions ([stable](https://github.com/rust-lang/rust/issues?q=is%3Aopen+label%3Aregression-from-stable-to-stable+-label%3AP-critical+-label%3AP-high+-label%3AP-medium+-label%3AP-low+-label%3AT-infra+-label%3AT-libs+-label%3AT-release+-label%3AT-rustdoc), [beta](https://github.com/rust-lang/rust/issues?q=is%3Aopen+label%3Aregression-from-stable-to-beta+-label%3AP-critical+-label%3AP-high+-label%3AP-medium+-label%3AP-low+-label%3AT-infra+-label%3AT-libs+-label%3AT-release+-label%3AT-rustdoc) and [nightly](https://github.com/rust-lang/rust/issues?q=is%3Aopen+label%3Aregression-from-stable-to-nightly+-label%3AP-critical+-label%3AP-high+-label%3AP-medium+-label%3AP-low+-label%3AT-infra+-label%3AT-libs+-label%3AT-release+-label%3AT-rustdoc)) and ideally regressions should have an assignee.

#### Accept MCPs

Accept all [MCPs that have been on `final-comment-period`](https://github.com/rust-lang/compiler-team/issues?q=is%3Aissue+is%3Aopen+label%3Amajor-change+label%3Afinal-comment-period) for 10 or more days. Basically check that `final-comment-period` label was added more than 10 days ago.
To accept, remove `final-comment-period`, add `major-change-accepted` and close the issue.

### Generate Agenda

#### Run cli to generate agenda

Run triagebot's prioritization cli to generate the agenda.
For that you need to clone https://github.com/rust-lang/triagebot if you haven't done so already.
You need to export your `GITHUB_API_TOKEN` on Linux that's typically done by adding

`export GITHUB_API_TOKEN=<your key>`

to your `~/.profile` file.

And then run:

```
$ cargo run --bin prioritization-agenda
```

Copy the content of the generated agenda into the agenda on HackMD.

#### Fill agenda announcements

Check the compiler calendar to see if there's an outstanding event to announce and add it to the agenda.

#### Add performance logs

Add [Triage Logs](https://github.com/rust-lang/rustc-perf/tree/master/triage#triage-logs) to the agenda.

### Notify the team about the meeting

Create `[weekly meeting] YYYY-MM-DD #54818` topic in `#t-compiler/meetings` Zulip's stream and send the following messages:

```text
Hi @*T-compiler/meeting*; the triage meeting will happen tomorrow at <time:YYYY-MM-DDT14:00:00+00:00>
*WG-prioritization* has done pre-triage in #**t-compiler/wg-prioritization/alerts**
@*WG-prioritization* has prepared the [meeting agenda](link_to_hackmd_agenda)
We will have checkins from *WG-X* and *WG-Y*
@**person1** do you have something you want to share about @*WG-X*?
@**person2** do you have something you want to share about @*WG-Y*?
```

### Add details to the Agenda

#### Summarize stable/beta nominations

- Add them to the agenda explaining:
  - Who the author of the PR is
  - Who the assignee is
  - Which issue fixes, it is a regression? what's the priority?
  - Why was it nominated
  - Add other important details

Note: triagebot automatically creates a topic and notify @*WG-prioritization* members requesting addition to the agenda.

#### Summarize PR's waiting on team

These are PRs waiting for some decision by our team (`T-compiler` or `T-libs`).

Try to follow the [General issues review process](#general-issues-review-process).

We should:

- Add them to the agenda explaining:
  - Who the author of the PR is
  - Who the assignee is
  - What is the issue waiting for
  - Add other important details
- Explicitly nominate any issue that can be *quickly* resolved in a triage meeting.

Note: triagebot automatically creates a topic and notify @*WG-prioritization* members requesting addition to the agenda.

#### Summarize `P-critical` and unassigned `P-high` regressions

Try to follow the [General issues review process](#general-issues-review-process).

We should:

- [Notify the appropriate groups](https://rustc-dev-guide.rust-lang.org/notification-groups/about.html)
- Push them forward, if possible
- Assign if possible

- Add `P-critical`s and unassigned `P-high`s to the agenda explaining:
  - If it's assigned or not and to whom
  - Does it needs MCVE and/or bisection?
  - Are there identified culprits?
  - Do already have a PR open that fixes the issue?
  - Add other important details

Note: triagebot automatically creates a topic and notify @*WG-prioritization* members requesting addition to the agenda.

#### Summarize I-nominated issues

Issues labeled with `I-nominated` are important issues that we decide deserve discussion during the weekly meeting.

Try to follow the [General issues review process](#general-issues-review-process).

We should:

- Check if these issues were already discussed and in that case remove `I-nominated` label
- Check if each issue is worth being discussed
- Add them to the agenda explaining:
  - Who the assignee is
  - Is this an issue or a PR: if an issue, does it have a PR that fixes it?
  - Why was it nominated
  - Add other important details

Note: triagebot automatically creates a topic and notify @*WG-prioritization* members requesting addition to the agenda.

### Final reviews

#### Nominate more issues if needed

Check how packed the agenda looks like and if there's room for more nominations.

- [Other team's P-critical](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AP-critical+-label%3AT-compiler+-label%3AT-libs)
- [T-compiler P-high](https://github.com/rust-lang/rust/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+label%3AT-compiler+label%3AP-high+)
- [T-libs P-high](https://github.com/rust-lang/rust/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+label%3AT-libs+label%3AP-high+)

#### Re-sync and check the agenda right before the meeting

Re-run the script and re-synchronize contents of the agenda with new information.
