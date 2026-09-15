# Blizmo Website

**What it is:** a Next.js and Three.js marketing site for Blizmo, an early stage African 3D data infrastructure company. Their whole pitch is capturing real African environments, streets, buildings, everyday objects, as traceable, honestly labeled 3D data that studios and researchers can actually build on. I designed and built the site from planning through to a live, CMS powered deploy.

Worth noting up front: this is an early version of Blizmo's site. It is expected to keep evolving as the company and its dataset grow, so what is described here is a snapshot, not a finished, final product.

## My role

Frontend build, technical architecture decisions, and a real amount of cloud and DevOps work that grew out of the project rather than being planned from day one. Client facing throughout, working from feedback rounds rather than a fixed spec handed to me upfront.

The DevOps side ended up being a genuine part of this project, not an afterthought:

- Set up the full continuous deployment pipeline. Every push to GitHub triggers an automatic build and deploy through Vercel, no manual deploy step at any point.
- Handled environment variables and secrets properly. Public values and sensitive tokens are kept separate, sensitive credentials are marked as such, and nothing sensitive lives in the codebase itself.
- Configured production authentication for the CMS admin panel through TinaCloud, so publishing access is tied to a real authorized login, not a password sitting in a file somewhere.
- Ran a real security pass on the live application and fixed what was found.
- Built a repeatable performance workflow: measure with real tooling, fix the actual bottleneck, measure again, and keep a record of the before and after numbers.

## What I actually built

- **The homepage hero went through a real design evolution.** It started as a full scroll driven 3D experience, a continuous camera controlled journey through a virtual room, inspired by Cartier's Watches and Wonders microsite. After client feedback, that idea was set aside for something calmer and more aligned with the brand: what is live now is a hero banner built around an animated, looping water visual, paired with the site's core vision statement.
- **A pinned scroll and reveal Method section.** Five steps, Physical World, Capture, Structure, Research and Context, Train and Benchmark, each with a real 3D visual or media on one side and text on the other, advancing one step per scroll gesture on desktop. Had to specifically fix this so it does not try to do the same thing on mobile. Touch scrolling needs to just work normally, not get hijacked.
- **A full Markdown based content system for the Research and Findings section.** Posts as plain files with frontmatter (title, date, tags, author, cover image, SEO overrides, featured flag), automatically surfaced on the homepage and a dedicated hub page, with individual post pages generated per file.
- **Tina CMS wired on top of that same content system**, so publishing a new post does not need a developer touching code at all. A real visual editor commits straight back to the same Markdown files, which redeploy automatically through the pipeline above.
- **A real security pass, not a checklist tick.** Found and closed actual cross site scripting holes, tested with live hostile payloads rather than just code review, added proper security headers, and cleared a critical dependency vulnerability.
- **Performance work with real before and after numbers.** Diagnosed a 720ms Total Blocking Time issue down to the 3D bundle loading eagerly instead of only when scrolled into view, then fixed image handling, video loading strategy, and script loading order.

## What went sideways (and what I learned from it)

- The client rejected the entire first architectural direction after I had built a working version of it. That is the moment that actually taught me the most, not the rebuild itself, but learning to separate "this was good work" from "this was the wrong direction for what they actually wanted," and rebuilding without treating it as wasted effort.
- A dependency force fix (npm audit fix --force) silently upgraded React and Next past what the 3D library supported and broke the whole build with a cryptic runtime error. Real lesson: pinned versions exist for a reason, and force flags on a version sensitive stack are not a shortcut, they are a landmine.
- A video asset came back with content physically cropped out at export time. No CSS fix could recover pixels that were never encoded in the file. Learned to actually decode and measure the file before assuming it was a styling bug.
- Git merge conflict markers sat unresolved in .gitignore for a while, left over from an earlier session. Nothing broke immediately, which is exactly why it is dangerous. The kind of thing that only bites you later if you do not clean it up.

## Stack

Next.js, React Three Fiber and Three.js, Tailwind CSS, Framer Motion, Tina CMS, Vercel, GitHub.
