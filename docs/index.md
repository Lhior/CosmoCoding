# CosmoCoding

<div class="hero">
  <div class="hero__content">
    <p class="hero__eyebrow">Modern computing tools for cosmologists</p>
    <h1>Build a workflow that survives real pipelines, remote clusters, and long nights of debugging.</h1>
    <p class="hero__lede">
      CosmoCoding is a practical guide to setting up Visual Studio Code, using the debugger well,
      and adapting those habits to cosmology software such as RAIL, TXPipe, cobaya, cosmoSIS,
      and TreeCorr.
    </p>
    <div class="hero__actions">
      <a class="md-button md-button--primary" href="setup/vscode/">Set up VS Code</a>
      <a class="md-button" href="formatting/">Set up formatting</a>
      <a class="md-button" href="debugger/">Learn the debugger</a>
      <a class="md-button" href="https://github.com/Lhior/CosmoCoding-examples">Browse the examples repo</a>
    </div>
  </div>
</div>

## Start Here

<div class="section-grid">
  <a class="topic-card" href="setup/vscode/">
    <h3>1. Set up VS Code</h3>
    <p>Install the right extensions, select the right interpreter, configure Remote-SSH, and make cluster work feel local.</p>
  </a>
  <a class="topic-card" href="formatting/">
    <h3>2. Keep formatting consistent</h3>
    <p>Use Black, isort, and Flake8 so collaborators are not fighting over style in every pull request.</p>
  </a>
  <a class="topic-card" href="debugger/">
    <h3>3. Use the debugger well</h3>
    <p>Understand breakpoints, stepping, watches, exception handling, and how launch configurations control your runs.</p>
  </a>
  <a class="topic-card" href="software/rail/">
    <h3>4. Apply it to real tools</h3>
    <p>Each software page explains the usual entry point, the environment assumptions, and the debugger template to start from.</p>
  </a>
</div>

## Software Guides

<div class="section-grid">
  <a class="topic-card" href="software/rail/"><h3>RAIL</h3><p>Debug CLI-driven photometric redshift pipelines and config-heavy runs.</p></a>
  <a class="topic-card" href="software/txpipe/"><h3>TXPipe</h3><p>Debug individual stages before trying to reason about the full pipeline.</p></a>
  <a class="topic-card" href="software/cobaya/"><h3>cobaya</h3><p>Run samplers in serial first and inspect likelihood state before parallel scaling.</p></a>
  <a class="topic-card" href="software/cosmosis/"><h3>cosmoSIS</h3><p>Catch environment, plugin, and parameter-file failures at the module boundary.</p></a>
  <a class="topic-card" href="software/treecorr/"><h3>TreeCorr</h3><p>Debug catalog parsing and correlation setup on small inputs before large jobs.</p></a>
</div>

## General Tips

This section adapts the recommendations in the original workspace notes so the landing page doubles as a shareable starter guide.

### Security Practices

- Use a password manager so every account has a different password. If you can remember all of them, the passwords are probably not strong enough. Bitwarden is a strong free option.
- Turn on two-factor authentication wherever you can. App-based codes are usually better than SMS. Email is acceptable when app-based 2FA is not available.
- Use SSH keys for clusters and developer services. They are both more secure and much less tedious than typing passwords into login nodes forever.

### Organize Your Workflow

- Use a proper IDE instead of treating every project as a notebook-only workflow. Visual Studio Code and PyCharm are the most common choices in this space.
- Learn the debugger in your IDE. It lets you inspect program state at the point of failure instead of reconstructing everything with print statements.
- Use virtual desktops to separate research, messaging, development, and personal browsing.
- Use a browser with vertical tabs if you spend a lot of time in papers, docs, and arXiv. It makes high-tab-count work much easier to scan.
- Use a reference manager. Zotero is the default choice for most astronomy and cosmology students.
- Use git and GitHub for version control. Commits are better than filenames like `draftFinalFINAL_this_time_for_real.tex`.

### Keyboard Shortcuts Worth Memorizing

- `Ctrl+/` toggles comments on selected lines.
- `Tab` and `Shift+Tab` indent and unindent selected lines.
- `Ctrl+T`, `Ctrl+Shift+T`, `Ctrl+Tab`, and `Ctrl+Shift+Tab` are worth learning in the browser.
- `Ctrl+W` closes the current tab or window.
- `Win+Tab`, `Win+Arrow`, and `Win+Shift+Arrow` make multi-window work much less painful on large projects.
- A middle click often opens links in background tabs.
- `Win+L` locks your machine quickly when you need to walk away.

### What To Read Next

- [Set up VS Code](setup/vscode.md) for Remote-SSH, Copilot, interpreters, and practical cluster tips.
- [Code Formatting](formatting.md) for Black, isort, Flake8, and format-on-save recommendations.
- [Use the debugger](debugger/index.md) for breakpoints, watch expressions, and launch configuration anatomy.
- [Companion examples repository](https://github.com/Lhior/CosmoCoding-examples) for intentionally buggy examples and `debugging.json` templates.
