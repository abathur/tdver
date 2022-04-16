tdver - test-driven versioning
=====

## Introduction
The backstory might belong on a blog, but I'm not blogging at the moment.
I'm doing early work on a Python BDD module, and pondering my initial
versioning. When both of these ideas shared the same headspace, I had a
fever-dream of a world in which software versioning is test-driven--a place
where machine-applied rules allow our ever-incrementing version numbers to carry
semantic freight they ironically can't often carry when individual developers
make decisions about when to increment.

## Goal
The purpose of this repository is to pretend the fever-dream was real, and
test its _plausibility_ by developing a specification for test-driven
versioning. If the specification can ever navigate the obvious obstacles and be
resolved into stable/sane logic for man and machine, the next steps are to build
tools implementing it, and see if a real-world project can survive test-driven
versioning.

This is a [call for dreamers][a]. Pull requests and issues appreciated. While I
am making an informal proposal, fresh takes are within scope.

## Informal proposal:
tdver proposes a quantitative *a.b.c[-d]* versioning system in which:
- *a* must (and may only) increment if any existing test is removed or modified
by a release
- *b* must (and may only) increment when a new non-bug-fix test is added by a
release AND *a* is not incremented
- *c* must (and may only) increment when a new bug-fix regression test is added
by a release AND *b* is not incremented.
- *d* must increment on any release not incrementing *a*, *b* or *c*


### Discussion/interpretation
One of the goals of tdver is that the version numbers convey meaning better than
human-selected versioning; what might be inferable?

#### Overall
- tdver shifts emphasis from releases the maintainer suggests will/won't
break depending code to whether or not the code is tested
- there is no time when the standard rules don't apply; the number of the first
release _follows_ the quantitative rules--it is either 0.1.0[-0] or 0.0.0-1
depending on whether it does or doesn't introduce tests (and this first version
already says a lot about the development philosophy behind the project)
- a simple git implementation focused on lines edited would require a separate
location for bug-fix tests.

#### A
- The test change/removal requirement of an *a* release encourages seeing tests
as a promise; as long as you are under the same *a* release, the coverage of
existing code should never regress
- An *a* release suggests something previously tested has been removed or
changed, though might be more specific cases:
	- something wasn't covered as promised in a way that wasn't fixable with a
	bug-fix or a new test
	- spring-cleaning or reorganization of old tests

#### B
- projects with no *b* version are obviously untested
- high *b* versions may imply a project is well structured, plans well
for the future, and is under swift, active development (as they are able to
introduce frequent new features without needing changes that disrupt old tests)
- in cases where the desire is minimal software with little feature creep, the
minimal use of *b* releases suggest a mature, stable project

#### C
- the history of a project's *c* releases should communicate how quickly the
maintainers push bug fixes
- this history probably also conveys how long (on average) the project takes to
stabilize following new major/minor releases

#### D
- projects with frequent *d* releases allow interested users to try new features
while they are being refined before formal release

### concerns/questions
- is MAJOR.MINOR.PATCH-DEV an acceptable nomenclature, or should tdver use new
terms that more tightly match what the positions mean?
- should tdver use a nonstandard prefix to communicate the versioning scheme?
(i.e., t0.0.0-1, tdv0.1.0); would it be compatible with dependency/provisioning
tools?
- can tdver tools keep track of metrics on ABCD releases in a way
that might let us graph development history usefully?
- should the final specification be so exact there's little room for a tool
implementation to differ from another, or should this be allowed as long as
general rules are satisfied? (consider the simple git implementation versus a
language-specific implementation which is intelligent enough not to iterate
for comment edits)
- tdver currently makes no provision for build metadata, pre-releases, or other
things that sometimes end up in version strings; can it live without them?
- tdver uses the -D indicator in a way that is consistent with how python's
setuptools computes and compares versions (i.e., 1.0.0-1 is GREATER than 1.0.0)
but in a way that would fall under how semver recognizes a pre-release string,
so it seems plausible that version comparisons in other languages/package
managers may be incompatible.

## Specification

## License
[Creative Commons - CC BY 4.0](http://creativecommons.org/licenses/by/4.0/)

[a]: http://en.wikisource.org/wiki/Ode_(O%27Shaughnessy) "Ode, by O'Shaughnessy"

