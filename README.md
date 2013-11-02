TODO
====

Public TODO list. Cross between .finger braindump, all SBCL-related for now.

Priority:

1. Source-transformed TYPEP
2. abarch's regalloc
3. Short patches, in order


Handle source-transformed TYPEP during flow-sensitive constraint
=======

Status: working branch, "just" have to review and commit.

Prioritised because it causes recent regressions.

Code review for abarch's regalloc branch
=======

It's blocking her work toward a merge.

Short patches to review/triage
=======

* https://bugs.launchpad.net/sbcl/+bug/455381
* https://bugs.launchpad.net/sbcl/+bug/674372
* https://bugs.launchpad.net/sbcl/+bug/1038034, comments #17/#18
* https://bugs.launchpad.net/sbcl/+bug/1245697
* https://bugs.launchpad.net/sbcl/+bug/806398
* https://bugs.launchpad.net/sbcl/+bug/1204869
* https://bugs.launchpad.net/sbcl/+bug/1205148
* https://bugs.launchpad.net/sbcl/+bug/496726
* "stop useless looping" by SANO Masatoshi
* Bike's work on length
* 9e7a18990d8cfe726edca3450f84510f5676a3e1 robust pattern match with TYPEP
* "[PATCH] *print-length* for call-frame args in sb-debug:print-backt…" by Andreas Franke
* "[Sbcl-devel] Non-executable stack" by James Y Knight
* "[Sbcl-devel] Another slight SETF cleanup" by Douglas Katzman
* https://bugs.launchpad.net/sbcl/+bug/1234919
* "[Sbcl-devel] Tests don't terminate on NetBSD", by Aleksej Saushev
* https://bugs.launchpad.net/sbcl/+bug/1042405
* "Re: [Sbcl-devel] Major difficulties with SB-COVER", by Douglas Katzman
* "Re: [Sbcl-devel] Quickie: (devar *myvar* (bad-fn)) was never warning, with fix and testcase", by Douglas Katzman
* "Re: [Sbcl-devel] Bug in CLHS regarding multiple-value-call?", by Douglas Katzman: style warn.
* "[Sbcl-devel] FASLoader improvement", by Douglas Katzman
* "Re: [Sbcl-devel] Misleading error message- implication is I called COMPILE-FILE when I didn't", by Douglas Katzman
* "Re: [Sbcl-devel] [Didier Verna] multiple string reads/writes of SB-IMPL::BAKQ-COMMA", by Douglas Katzman
* https://bugs.launchpad.net/sbcl/+bug/800010
* "[Sbcl-devel] csv output of generation stats in gengcg.c", by Marco Baringer
* "[Sbcl-devel] Patch for Initial Move of GC code into package SB-GC", by Craig Lanning
* https://bugs.launchpad.net/sbcl/+bug/1177986
* https://bugs.launchpad.net/sbcl/+bug/1183982
* "[Sbcl-devel] This fix works (was Re: Can make (get-universal-time) to hang)", Max Mikhanosha
* "[Sbcl-devel] Building with SB-DYNCOUNT feature", Douglas Katzman
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

Choose between flagful and branchful :predicate VOPs more smartly
======

If both are applicable with same cost, probably want to go for label-ful VOPs if the branch doesn't "look" like a cmov.
Otherwise, it's hard to optimise for both conditional branches and conditional moves.

IR2opt
======

jump-jump tensioning could use some love. IR1 block ordering doesn't always suffice, esp. with our funky insertion of
branches during IR2.

SB-GMP
======

Add code to check for overflowed allocation, https://bugs.launchpad.net/sbcl/+bug/1206191

PPC threading patches
======
Barriers in lock-free caches (e.g. memoised specifier-type)

Not too hard SMOP
======
* "[Sbcl-devel] Feature request: unload all shared libraries", Teemu Likonen
* "[Sbcl-devel] Can the source transform for mapfoo make its initial cons dynamic-extent?", by Douglas Katzman
* "Re: [Sbcl-devel] full-eval bug in handling of LET* (and LAMBDA) bindings", Douglas & Christophe

Jump tables
=====
expose this as a function: (defun switch (n &rest thunks) (funcall (elt thunks n))).

Definitely longer-term patches
========
* https://bugs.launchpad.net/sbcl/+bug/327537
* https://bugs.launchpad.net/sbcl/+bug/1214610
* https://bugs.launchpad.net/sbcl/+bug/1028026
* "[Sbcl-devel] Globaldb screwyness because of purify, and a (huge) fix",
 "[Sbcl-devel] Nice compiler speedup",
 "[Sbcl-devel] Two more enhancements for globaldb" by Douglas Katzman

* "Re: [Sbcl-devel] cold-print / hexstr lossage", by Douglas Katzman
* "Re: [Sbcl-devel] Different treatment of repeated let* variables - special disagreement", by Douglas Katzman
* generalised brittleness in sb-concurrency tests (e.g. https://bugs.launchpad.net/sbcl/+bug/1188289)

Norec SpecTM
========

Sequence lock only protects commit/validate. EDSL for trivial read sequence id. Writes only during commit.
