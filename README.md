# InsPyre

A step-by-step Python course that runs entirely in the browser. Students read short lesson cards, try examples in a built-in editor, and practice at three levels: Starter, Core and Challenge.

No installs, accounts or server are needed. It's a single HTML file.

## What's inside

| File | Purpose |
|------|---------|
| `index.html` | The whole app: lessons, editor, Python engine loader and styles |
| `.nojekyll` | Tells GitHub Pages to serve the file as-is |
| `README.md` | This file |

## Lessons

1. Variables
2. Variable types
3. Strings and methods
4. Boolean expressions
5. Conditional statements
6. Lists
7. For loops
8. Functions
9. Parameters and return values

Each lesson has an overview, one card per concept with a runnable example, a "Before you practice" card, and 7–9 practice problems across three difficulty tiers (65 in all). Problems are checked automatically, with progressive hints and plain-English explanations of Python errors.

Challenge problems are tougher multi-step tasks, and they are also re-run with different starting values, so a solution only passes if it truly works from the variables rather than hard-coded answers.

## Deploy on GitHub Pages

1. **Create a repository.** On GitHub, click **New repository**, name it (for example `inspyre`), make it **Public**, and create it.
2. **Upload the files.** On the new repository page, click **Add file → Upload files**. Drag in `index.html`, `README.md` and `.nojekyll`, then click **Commit changes**.
   - `.nojekyll` is a hidden file on Mac and Linux. On a Mac, press `Cmd + Shift + .` in Finder to show it. It's optional; the site works without it.
3. **Turn on Pages.** Go to **Settings → Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**, choose the `main` branch and the `/ (root)` folder, and click **Save**.
4. **Open the site.** After a minute or two, the Pages settings show your link, usually `https://YOUR-USERNAME.github.io/inspyre/`.

### Using git instead

```bash
git init
git add index.html README.md .nojekyll
git commit -m "Add InsPyre"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/inspyre.git
git push -u origin main
```

Then turn on Pages as in step 3.

## How it works

- **Python engine:** [Skulpt](https://skulpt.org), a Python interpreter written in JavaScript, loaded from the jsDelivr CDN. It covers everything the lessons use, but it isn't full CPython, so some advanced features may behave differently.
- **Fonts:** Figtree and DM Mono from Google Fonts, with system fallbacks.
- **Progress:** saved in each student's own browser with `localStorage`. Nothing is sent to a server. Clearing browser data, switching browsers or using a private window starts fresh.
- **Internet:** needed on first load to fetch the Python engine and fonts.

## Editing lessons

All lesson content lives in the `LESSONS` array near the top of the `<script>` in `index.html`. Each lesson has `sections` (concept cards), a `tip`, a `mistake`, a `deeper` note and `exercises`.

An exercise looks like this:

```js
{id:"p1", tier:"starter", title:"Square it",
 prompt:"<p>Finish <code>square(n)</code> so it returns n times itself.</p>",
 starter:`def square(n):\n    return \n`,
 hints:["Use the parameter n.", "return n * n"],
 solution:`def square(n):\n    return n * n`,
 tests:`assert square(3) == 9, "square(3) should be 9"`,
 expect:"9",                       // optional: exact printed output
 must:[[/def\s+square/, "Define a function named square."]],  // optional: required code patterns
 mustNot:[[/\*\*/, "Use * instead of ** here."]],          // optional: forbidden code patterns
 variants:[{set:{n:"5"}, tests:`assert ...`}]}               // optional: re-run with new starting values
```

- `tests` is Python that runs after the student's code. A failing `assert` shows its message as feedback.
- `expect` compares printed output, ignoring trailing spaces.
- `must` lists regular expressions the code has to contain, with a message for each.
- `variants` replace the first `name = ...` line for each variable in `set`, then check `tests` or `expect` again. Useful for problems that start from given variables.
- Give every exercise a unique `id`. Saved progress is keyed by it.
