---
# templates/note.md
# ─────────────────────────────────────────────
# Default note template (use with Obsidian Templater plugin)
# Shortcut: bind to a hotkey in Templater settings
# ─────────────────────────────────────────────
title: <% tp.file.title %>
date: <% tp.date.now("YYYY-MM-DD") %>
modified: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - 
publish: false        # ← flip to true when ready to publish
draft: false
description: ""       # ← used for SEO meta description and RSS
---

# <% tp.file.title %>

