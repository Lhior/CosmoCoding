# arXiv

Keeping up with arXiv is easier when it becomes part of a workflow instead of a browser-tab habit. This page focuses on a practical setup built around Zotero, arXiv RSS feeds, and Overleaf.

## What a Reference Manager Does

A reference manager is a tool for storing, organizing, tagging, searching, and citing papers. In practice, it gives you one place to keep the papers you care about, rather than scattering them across browser bookmarks, PDFs in random folders, and half-finished BibTeX files.

For collaboration, that matters because a good reference manager helps with:

- collecting papers in a shared, searchable place,
- generating citations and bibliography data,
- tracking reading lists for a project or journal club,
- keeping notes, tags, and attachments tied to the paper instead of to your memory.

Zotero is the default recommendation here because it is widely used in academia, works well for both individual and group libraries, supports RSS feeds, and integrates cleanly with LaTeX-oriented workflows.

## Why Zotero Is a Good Default

Zotero works well for cosmology groups because it supports both personal libraries and shared group libraries.

That makes it useful for:

- your own reading queue,
- project-specific paper collections,
- literature review folders,
- journal-club style shared group libraries where multiple people add and discuss papers.

A shared library is especially useful for journal club because the group can maintain one curated collection of papers instead of relying on long email chains or chat messages to remember what was discussed.

## Zotero and Overleaf

Overleaf can link directly to Zotero. According to Overleaf's Zotero integration workflow, you link the accounts from your Overleaf account settings and authorize Overleaf through Zotero's web flow.

Once linked, Overleaf can use Zotero in two main ways:

1. It can import your Zotero `My Library` or an individual Zotero Group Library into a read-only `.bib` file in your Overleaf project.
2. It can import individual references via Overleaf's advanced reference search.

Useful details:

- Zotero Group Libraries work well for shared project bibliographies and journal clubs.
- Overleaf imports an entire library or group as a flat list rather than preserving individual folders within that library.
- If you update the references in Zotero, Overleaf can refresh the imported read-only `.bib` file.
- Overleaf's Zotero integration is a premium feature, so if you are not using a paid plan or institutional plan, you may need to export `.bib` files from Zotero manually instead.

## Shared Libraries for Journal Club

A clean journal-club workflow is:

1. Create a Zotero Group Library for the club.
2. Add papers there as they come up.
3. Use tags, notes, or folders to separate topics or weeks.
4. If you write notes or proceedings in Overleaf, link that same group library to the project bibliography.

This keeps discovery, curation, and writing connected instead of rebuilding the bibliography from scratch every time.

## What an RSS Feed Is

An RSS feed is a machine-readable stream of updates from a source such as a journal, website, or paper repository. Instead of repeatedly checking a page by hand, you subscribe once and your reader shows you new items as they appear.

For arXiv, RSS is a simple way to keep up with new submissions in the categories you care about.

## How arXiv Implements RSS

arXiv provides RSS and Atom feeds for active subject areas. The feeds update daily at midnight Eastern time.

The feed URLs are built from category names:

- whole archive or category: `https://rss.arxiv.org/rss/cs`
- specific subject class: `https://rss.arxiv.org/rss/math.QA`
- multiple categories together: `https://rss.arxiv.org/rss/cs.AI+q-bio.NC`

For astronomy and cosmology use cases, you would usually subscribe at the subject-class level, for example one of the `astro-ph.*` classes that matches your interests.

## Load an arXiv RSS Feed into Zotero

Zotero has built-in feed support, so you do not need a separate RSS reader for this workflow.

### Basic setup

1. Open Zotero.
2. Click the `Add Library` button above the left pane.
3. Choose `Add Feed` and then `From URL...`.
4. Paste the arXiv RSS URL.
5. Give the feed a recognizable title and save it.

An example URL is:

```text
https://rss.arxiv.org/rss/astro-ph.CO
```

You can subscribe to one category, one subject class, or a combined multi-category feed depending on how broad you want the stream to be.

### Why this is useful in Zotero

Zotero keeps track of which feed items you have looked at.

- new or unread feed items appear in bold,
- items you have already looked at appear in regular text.

This is one of the most useful parts of the workflow because Zotero effectively remembers where you left off in the feed. You do not need to manually remember whether a paper is new to you or whether you already checked it yesterday.

One caveat from Zotero's feed documentation: the read and unread state of feed items is not synced across computers, so that status is remembered per machine rather than globally through Zotero sync.

## Browsing Papers from the Feed

Once a paper shows up in the Zotero feed, going to the arXiv abstract page is straightforward.

- Select the paper in the feed.
- Double-click the paper title to open its URL, which for arXiv feed items is typically the abstract page in your browser.

That makes it easy to skim the abstract page before deciding whether the paper belongs in one of your actual libraries.

## Adding Papers from the Feed into Your Library

The feed is for discovery. Your Zotero libraries are for curation.

When you select a paper in the RSS feed, Zotero shows the abstract and metadata in the item pane. From there, adding the paper to a real library is simple:

- click `Save to My Library` to add it to your personal library,
- or use the dropdown on that button to save it directly into a Group Library.

This is the cleanest way to turn a paper from "I noticed this in today's feed" into "this belongs in our project bibliography" or "this should go into the journal club library".

In practice, the workflow is:

1. browse new boldface papers in the feed,
2. double-click interesting ones to inspect the arXiv abstract page,
3. if the paper is worth keeping, save it to your own library or to a shared group library.

## A Good Daily Workflow

One practical routine is:

1. open Zotero once a day,
2. check the arXiv feed entries that are still bold,
3. open the most relevant abstracts,
4. save strong candidates into the relevant project or journal-club library,
5. let Overleaf pull from that curated library when you start writing.

That is cleaner than using arXiv as a pile of browser tabs, and it scales much better when multiple collaborators are involved.
