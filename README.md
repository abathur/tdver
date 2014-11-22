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

This is a [call for dreamers][a]. Pull requests and issues appreciated.

## Informal proposal:
tdver proposes a quantitative *a.b.c[-d]* versioning system in which:
- *a* must (and may only) increment if any existing test is removed or modified
by a release
- *b* must (and may only) increment when a new non-bug-fix test is added by a
release AND *a* is not incremented
- *c* must (and may only) increment when a new bug-fix regression test is added
by a release AND *b* is not incremented.
- *d* must increment on any release not incrementing *a*, *b* or *c*
- only *d* may increment if the provisional tests directory contains tests

A simple git implmentation just looking for lines added and removed would require that projects using tdver:
- must declare a location for provisional tests (tests introduced or
changed here will not require a *b* release)
- must declare a separate location for storing bug-fix tests
- must declare a separate location for all other tests

### Discussion/interpretation
One of the goals of tdver is that the version numbers convey meaning better than
human-selected versioning; what might be inferable?

####Overall
- tdver shifts emphasis from releases the maintainer suggests will/won't
break depending code to whether or not the code is tested
- there is no time when the standard rules don't apply; the number of the first
release _follows_ the quantitative rules--it is either 0.1.0[-0] or 0.0.0-1
depending on whether it does or doesn't introduce tests (and this first version
already says a lot about the development philosophy behind the project)

####A
- The test change/removal requirement of an *a* release encourages seeing tests
as a promise; as long as you are under the same *a* release, the coverage of old
code should never regress
- An *a* release suggests something previously tested has been removed or
changed, though there are more specific cases:
	- something wasn't covered as promised in a way that wasn't fixable with a
	bug-fix or a new test
	- spring-cleaning or reorganization of old tests

####B
- projects with no *b* version are obviously untested
- high *b* versions may imply a project is well structured, plans well
for the future, and is under swift, active development (as they are able to
introduce frequent new features without needing changes that disrupt old tests)
- in cases where the desire is minimal software with little feature creep, the
minimal use of *b* releases suggest a mature, stable project

####C
- the history of a project's *c* releases may communicate how quickly the
maintainers push bug fixes
- this history probably also conveys how long (on average) the project takes to
stabilize following new major/minor releases

####D
- projects with frequent *d* releases allow interested users to try new features
while they are being refined before formal release

### concerns/questions
- is MAJOR.MINOR.PATCH-DEV an acceptable nomenclature, or should tdver use new
terms which aren't wedded to the history of other versioning schemes?
- should tdver use a nonstandard version prefix to communicate
the versioning scheme? (i.e., t0.0.0-1, tdv0.1.0); would it be compatible with
dependency/provisioning tools?
- can tools for tdver versioning keep track of metrics on ABCD releases in a way
that might let us graph development history usefully?
- more broadly applied by the item above, should the final specification go
beyond the simple versioning mechanics into dictating minimum behavior a
tdver tooling implementation must have before citing itself as
tdver-VERSION-compliant? Maybe this is more like tdver-1.0-minimal and
tdver-1.0-full; depends on how useful the enchancements are...
- should tdver's D version format include information about provisional tests?
	- users are already accepting risk by taking a development version, so the
	point isn't necessarily communicating risk to them
	- but it may show whether development follows tests or vice-versa;
	- ex: t1.2.0-9-27 might indicate it's the 9th development release and that
	there are 27 provisional tests. If the previous was t1.2.0-8-27, development
	is following tests; if it was t1.2.0-8-1...)
	- the hitch is that counting tests is hard; the best we can probably do is
	either counting files or lines in the provisional directory.
- tdver currently makes no provision for build metadata, pre-releases, or other
things that sometimes end up in version strings; does it need them?
- tdver uses the -D indicator in a way that is consistent with how python's
setuptools computes and compares versions (i.e., 1.0.0-1 is GREATER than 1.0.0)
but in a way that would fall under how semver recognizes a pre-release string,
so it seems plausible that version comparisons in other languages/package
managers may be incompatible.

## Specification

## License
[Creative Commons - CC BY 4.0](http://creativecommons.org/licenses/by/4.0/)

[a]: http://en.wikisource.org/wiki/Ode_(O%27Shaughnessy) "Ode, by O'Shaughnessy"

