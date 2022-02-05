# CS 462 Scoping Assignment -- Abstract

## Robert Detjens

---

# Pandoc As A Service

## Background

Pandoc makes writing and converting documents extremely easy. Documents can be
written as merely plain text as Markdown, and yet have powerful features such as
a full automatic citation system, LaTeX macros, and math rendering. This is made
possible through Pandoc's flavor of Markdown, and I am an unabashed shill for
it.

However, when working on a group project (such as Capstone), there exists a
problem in collaboration. If a group wants to work in real time on a Markdown
document *a la* Google Docs, there is no good way to achieve this. While
Markdown documents are extremely `git`-friendly due to being plain text, `git`
is not a real-time tool and simultaneous editing of the same file would likely
cause annoying merge conflicts when resolving changes.

There are tools that come close to this: Etherpad for plain text, Google Docs
for a full-fat document editor, or Overleaf for LaTeX; however none of these
integrate the features Pandoc Markdown provides. Etherpad does not support
Markdown rendering or export, Google Docs is very heavy and also does not
support Markdown rendering, and Overleaf requires LaTeX which is complicated.

## Proposal

My Capstone proposal is thus: creating a web application for real-time
collaborative Markdown editing, using Pandoc as the backend. This will provide
the best features from the platforms outlined above; namely the ease and
lightness of writing in Markdown with the powerful features of Google Docs and
Overleaf.

This will require several components, enough for a full 3-term cycle:

- Website frontend
  - Markdown editor & highlighting
  - Document preview
- Website backend
  - Real-time synchronization between open documents
  - Secure build environment for Pandoc

Preview and rendering could either be done on the server, or clientside by
compiling Pandoc to WASM and running it in the browser. This has (partially)
been done already ([link](https://github.com/y-taka-23/wasm-pandoc)).

Parts of Etherpad may be able to be reused, as Etherpad is open
source ([link](https://github.com/ether/etherpad-lite)).

\vfill

```md
#documents
#collaboration
#pandoc
#web
#wasm
```
