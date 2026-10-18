# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives.** In an eval bundle, the repro report's setup lines,
and sometimes inside the pasted output where a command prints a
version. The issue context holds the target to compare against. In live
mode, my draft repro comment, the issue body, and the repo's setup
docs.

**What good looks like.** The report names anything that could change
the outcome: the operating system, the language version, the project
version or commit, and the version of any service the bug touches. If
one of those differs from what the issue targets, the report says so.
A version printed inside pasted output counts, as long as it is
readable.

## Steps

**Where it lives.** In an eval bundle, the repro report's account of
what was done, whether that is a list, a paragraph, or a shell
transcript. In live mode, my draft repro comment and any repo
documentation it points at.

**What good looks like.** Everything the trigger needs is in the
package: the command, the input file, the state the repo started in.
The report can send a reader to the repo's own setup docs. It cannot
send them to a file that only exists on my machine. The question to ask
is whether a reader holding the repo and the stated environment could
get to the same point without asking me anything.

## Behavior shown

**Where it lives.** In an eval bundle, the output blocks, logs, error
text, counts, or described screenshots in the repro report. The
behavior to compare them against is in the issue context. In live mode,
the code blocks in my draft, read against the issue body and any
expected-versus-actual section of its template.

**What good looks like.** When the report says it reproduced the
issue, the artifact shows the same symptom, in the same place, that the
issue describes. Things that look close but are not: an error thrown
during setup instead of in the code path the issue names, a different
exception, or the right component failing for a different reason. When
the report says it could not reproduce, the artifact has a different
job: it should show that the conditions the issue names were set up and
that the symptom was checked for, so a reader can see the attempt was
real. The comparison is against the issue's own wording. How the report
describes the issue does not count.

## Honesty

**Where it lives.** In both modes, the report's conclusion read against
the artifacts in that same report. Also the gap between what the issue
claims and the part of it the report confirmed.

**What good looks like.** Everything the report says happened is
visible in an artifact it shows, and anything it could not confirm is
named. A report saying it could not reproduce the issue, with the
commands it ran, is a good report. What fails: the word "reproduced"
over an artifact showing something else, a partial result written up as
a complete one, or a claim about the cause that no artifact backs up.

## Comms

**Where it lives.** In an eval bundle, the repo-facts block. It is the
only source of repo policy: contribution requirements, comment
templates, and any rule about disclosing AI assistance. Those get
checked against what the claim comment and repro report do. In live
mode, the repo's CONTRIBUTING file, README, issue and pull request
templates, any policy file, and the house rules in scope.md.

**What good looks like.** The comments meet every requirement the repo
states. Read each requirement as covering this package unless the repo
limits it to something the package does not contain. Where the repo
asks contributors to disclose AI assistance, the disclosure has to be
visible in the comment itself, so its absence is a failure rather than
missing evidence. The claim names details of this issue, such as the
symptom, the file involved, or what triggers it, so it could not be
pasted onto another issue unchanged. It commits only to investigating,
with no fix promised and no date given. A repo that states no
requirements is one every package satisfies.