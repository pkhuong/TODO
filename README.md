TODO
====

Public TODO list. Cross between .finger braindump, all SBCL-related for now.

Priorities:

0. Hunt perf regressions in regalloc
1. Straighten jumps to jumps that appear late in the compilation process.
2. Switch/case: nearly working.

Code review for abarch's regalloc branch
=======

Clean up and commit. We can be smarter later.

Jump tables
=====
expose this as a function: (defun switch (n &rest thunks) (funcall (elt thunks n))).
https://github.com/pkhuong/sbcl/tree/switch-case

Live range splitting
====================


Short patches to review/triage
=======

* https://bugs.launchpad.net/sbcl/+bug/806398
* https://bugs.launchpad.net/sbcl/+bug/1204869
* https://bugs.launchpad.net/sbcl/+bug/1205148
* https://bugs.launchpad.net/sbcl/+bug/496726
* "stop useless looping" by SANO Masatoshi
* Bike's work on length
* 9e7a18990d8cfe726edca3450f84510f5676a3e1 robust pattern match with TYPEP
* "[PATCH] *print-length* for call-frame args in sb-debug:print-backt…" by Andreas Franke
* "[Sbcl-devel] Non-executable stack" by James Y Knight
* https://bugs.launchpad.net/sbcl/+bug/1234919
* "[Sbcl-devel] Tests don't terminate on NetBSD", by Aleksej Saushev
* https://bugs.launchpad.net/sbcl/+bug/1042405
* "Re: [Sbcl-devel] Major difficulties with SB-COVER", by Douglas Katzman
* "Re: [Sbcl-devel] Bug in CLHS regarding multiple-value-call?", by Douglas Katzman: style warn.
* https://bugs.launchpad.net/sbcl/+bug/800010
* "[Sbcl-devel] csv output of generation stats in gengcg.c", by Marco Baringer
* "[Sbcl-devel] Patch for Initial Move of GC code into package SB-GC", by Craig Lanning
* https://bugs.launchpad.net/sbcl/+bug/1177986
* https://bugs.launchpad.net/sbcl/+bug/1183982
* "[Sbcl-devel] This fix works (was Re: Can make (get-universal-time) to hang)", Max Mikhanosha
* "Re: [Sbcl-devel] Faster FIND and POSITION in unsafe code", Douglas Katzman

Documentation improvement
========
* https://bugs.launchpad.net/sbcl/+bug/1211414
* https://bugs.launchpad.net/sbcl/+bug/601576
* "Re: [Sbcl-devel] Building 1.1.12 from 1.0.23 doesn't work on Solaris 10 SPARC"
* "Re: [Sbcl-devel] Coding Style Questions", Jan Moringen

Doug's type VOP patches
=======

At least, apply the ones that work as is. Then see what can be improved once we select between branchful and
flagful VOPs.

Reply to posts
=======
*  SB-TRANSACTION
*  [Sbcl-devel] Windows sb-ext:run-program output issue
*  Re: [Sbcl-help] Questions about ftype declarations.
*  [Sbcl-devel] reader macro restarts not available when loading from a file
*  [Sbcl-devel] Progress report for automated testing of SBCL


Doug's assembler peephole patch
=======

Thoughts so far: doubt that performing flow analysis on assembly code is a good idea. How far can we go by only matching
suffixes of some VOP followed by a sequence of VOPs? That way we can exploit TN-level dataflow information that is correct
at a VOP granularity.

Also fixes
* <=/>= direct VOPs to avoid double comisd
* multiple-value cmov

Choose between flagful and branchful :predicate VOPs more smartly
======

If both are applicable with same cost, probably want to go for label-ful VOPs if the branch doesn't "look" like a cmov.
Otherwise, it's hard to optimise for both conditional branches and conditional moves.

IR2opt
======

jump-jump tensioning could use some love. IR1 block ordering doesn't always suffice, esp. with our funky insertion of
branches during IR2. Another reason IR1 block ordering isn't enough: some blocks compile to nothing.

SB-GMP
======

Add code to check for overflowed allocation, https://bugs.launchpad.net/sbcl/+bug/1206191

PPC threading patches
======
* Barriers in lock-free caches (e.g. memoised specifier-type)

* seqlock (frlock) in sb-concurrency get some barrier/read ordering wrong.

Not too hard SMOP
======
* "[Sbcl-devel] Feature request: unload all shared libraries", Teemu Likonen
* "[Sbcl-devel] Can the source transform for mapfoo make its initial cons dynamic-extent?", by Douglas Katzman
* Allocate TLS indices at load-time
     - subclass global-var for specials
     - add slot to annotate with either load-time handle (see l-t-v), or index
     - pass immediate or constant SC to VOPs (bind, read, set).
* data-vector-set-with-offset: return value ourself
* disassemble: emit preambule/prologue via :keyword argument
* no-consing declaration

Definitely longer-term patches
========
* https://bugs.launchpad.net/sbcl/+bug/327537
* https://bugs.launchpad.net/sbcl/+bug/1214610
* https://bugs.launchpad.net/sbcl/+bug/1028026
* "[Sbcl-devel] Globaldb screwyness because of purify, and a (huge) fix",
 "[Sbcl-devel] Nice compiler speedup",
 "[Sbcl-devel] Two more enhancements for globaldb" by Douglas Katzman
* "Re: [Sbcl-devel] Different treatment of repeated let* variables - special disagreement", by Douglas Katzman
* generalised brittleness in sb-concurrency tests (e.g. https://bugs.launchpad.net/sbcl/+bug/1188289)
* div by mul for signed/unsigned ceiling/floor/truncate
* real dead code elimination pass
* IR1 tree matching
* IR2 tree parsing
* combination-implementation-style for min/max of floats
* istm maybe-float-lvar-p should be more specific, like maybe-nan-lvar-p: if a float type
  has lower and upper bounds, it can't be a nan (or we're doing something seriously wrong).
* comparison with nan/infty: don't error out, return NIL (except for trapping nan).
* clang-based C++ FFI
* refcount at coarse granularity?
* mark/sweep for older generation
* Post RFC -> disable interrupts/gc during thread startup

Norec SpecTM
========

Sequence lock only protects commit/validate. EDSL for trivial read sequence id. Writes only during commit. 

Spinning on a single lock bit w/ proportional backoff seems to be comparable to a single CAS in terms of scaling.

Strangely, (preemptable) ticket lock + proportional backoff is much worse.

Just push something usable, we'll fiddle with the details later. It's already pretty good.
