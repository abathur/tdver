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
tdver proposes a quantitative `x.y.z` versioning system in which:
- x must (and may only) increment if any existing test is removed or modified by
a release
- y must (and may only) increment when a new test is added by a release AND x is
not incremented
- z must increment on any release not incrementing x or y

tdver makes further qualitative but unenforced recommendations:
- z-incrementing releases should only be used to improve tested code within the
bounds of existing tests

### Discussion/extrapolation/interpretation
One of the goals of tdver is that the version numbers convey meaning better than
human-selected versioning:

- any project with a high `z` number will, by definition, have introduced
changes not accompanied by (new) tests.
	- High `z` numbers may indicate a loose coupling between the testing and
	development process. This implication will likely mean that the most
	test-driven projects avoid having many `z` versions.
	- However, it's also completely within scope to cleanly refactor tested code
	 in a `z` release.
- projects with high `y` versions, especially ones whose history rarely contains
a `z` version probably do a good job of releasing features and tests together
- projects with no `y` version are obviously untested
- the number of the first release follows the quantitative rules--it is either
0.1.0 or 0.0.1 depending on whether it does or doesn't introduce tests (and this
first version says a lot about the development philosophy behind the project)

### concerns/questions
- tdver's idiomatic use of the `z` number makes it fundamentally
incompatible with the assumptions semver users might make (i.e., it's perfectly
legal, if discouraged, to introduce breaking changes as long as you don't touch
the tests)
- incrementing the `x` number on the removal or editing of a test may discourage
maintainers from removing/editing tests that need it
- tdver shifts emphasis releases the maintainer suggests will/won't
break depending code to whether or not the code is tested
- a substantive portion of the "semantic freight" tdver may carry depends on
maintainers to avoid creating cynical or insufficient tests
- tdver's quantitative rules are incompatible with pre-release versions
- does major/minor/patch nomenclature fit?
- does tdver need to comment on the 'public api' in any way?
- should (can?) tdver use some sort of nonstandard version prefix to communicate
the versioning scheme? (i.e., t0.0.1, tdv0.0.1)
- tdver would require a y version increment to fix a bug if a test-case is
introduced, an x version update if a bugfix required modifying an existing test,
but would only require a z version increment if the bugfix ignores the tests; is
this a problem?

## Specification
I'm going to resist the urge to include my own thoughts to avoid
starting the discussion in too-small a box.

## License
[Creative Commons - CC BY 4.0](http://creativecommons.org/licenses/by/4.0/)

[a]: http://en.wikisource.org/wiki/Ode_(O%27Shaughnessy) "Ode, by O'Shaughnessy"
