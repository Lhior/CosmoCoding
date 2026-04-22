# General Tips

This section adapts the original starter recommendations into a cleaner standalone guide, so the landing page can stay focused on navigation while this page stays focused on workflow habits.

## Security Practices

- Use a password manager so every account has a different password. If you can remember all of them, the passwords are probably not strong enough. Bitwarden is a strong free option.
- Turn on two-factor authentication wherever you can. App-based codes are usually better than SMS. Email is acceptable when app-based 2FA is not available.
- Use SSH keys for clusters and developer services. They are both more secure and much less tedious than typing passwords into login nodes forever.

## Organize Your Workflow

- Use a proper IDE instead of treating every project as a notebook-only workflow. Visual Studio Code and PyCharm are the most common choices in this space.
- Learn the debugger in your IDE. It lets you inspect program state at the point of failure instead of reconstructing everything with print statements.
- Use virtual desktops to separate research, messaging, development, and personal browsing.
- Use a browser with vertical tabs if you spend a lot of time in papers, docs, and arXiv. It makes high-tab-count work much easier to scan.
- Use a reference manager. Zotero is the default choice for most astronomy and cosmology students.
- Use git and GitHub for version control. Commits are better than filenames like `draftFinalFINAL_this_time_for_real.tex`.

## Keyboard Shortcuts Worth Memorizing

- `Ctrl+/` toggles comments on selected lines.
- `Tab` and `Shift+Tab` indent and unindent selected lines.
- `Ctrl+T`, `Ctrl+Shift+T`, `Ctrl+Tab`, and `Ctrl+Shift+Tab` are worth learning in the browser.
- `Ctrl+W` closes the current tab or window.
- `Win+Tab`, `Win+Arrow`, and `Win+Shift+Arrow` make multi-window work much less painful on large projects.
- A middle click often opens links in background tabs.
- `Win+L` locks your machine quickly when you need to walk away.

## What To Read Next

- [Set up VS Code](setup/vscode.md) for Remote-SSH, Copilot, interpreters, and practical cluster tips.
- [arXiv](arxiv.md) for Zotero, RSS feeds, shared libraries, and an arXiv-reading workflow that scales beyond browser tabs.
- [Code Formatting](formatting.md) for Black, isort, Flake8, and format-on-save recommendations.
- [Use the debugger](debugger/index.md) for breakpoints, watch expressions, and launch configuration anatomy.
- [Companion examples repository](https://github.com/Lhior/CosmoCoding-examples) for intentionally buggy examples and `debugging.json` templates.
