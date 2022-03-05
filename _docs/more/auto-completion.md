---
title: Auto Completion
category: more
order: 3
---

Ufo supports bash auto-completion.  To set it up add the following to your `~/.profile` or `.bashrc`:

    eval $(ufo completion_script)

Remember to restart your shell or source your profile file.

Auto Completion examples:

    ufo [TAB]
    ufo ship [TAB]
    ufo ship --[TAB]
    ufo docker [TAB]
    ufo docker build [TAB]
