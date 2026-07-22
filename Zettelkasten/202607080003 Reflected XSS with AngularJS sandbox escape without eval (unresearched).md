---
id: "202607080003"
title: Reflected XSS with AngularJS sandbox escape without eval (unresearched)
date: 2026-07-08
type: case
tags: [xss, reflected-xss, angularjs, stub]
source: "[[25_Lab25-Reflected XSS with AngularJS sandbox escape without strings]]"
publish: false
---

Lab requires an AngularJS sandbox-escape payload that works without `$eval` and without using any string literals at all. Marked on hold in the source note — no working payload or research notes captured yet. Flagged here as an open thread rather than left buried in a dated lab file: the AngularJS sandbox-escape technique family (constructor-chaining gadgets like `{}.toString.constructor('alert(1)')()` and its no-`$eval`/no-string variants) is a distinct, self-contained topic worth its own research pass rather than being solved incidentally.

## Links
- No solved concept notes yet — revisit and split out a proper concept note once this is actually worked through.
