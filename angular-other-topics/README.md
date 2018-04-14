# Universal Rendering

---

# Problems with SPAs
- Initial load and rendering of data can be slow
- Search engine unfriendliness

---

# Slow Initial Load
- Even minor delay can have high impact on user acquisition ([Google 2009](http://googleresearch.blogspot.fi/2009/06/speed-matters.html))
- Even worse on mobile devices
- Bad user experience

---

# Search Engine Optimization with SPAs
Two main problems:
- No clear definition of ready
- Deep links are hard to get indexed

---

# No Clear Definition of Ready
- HTML rendered but no data available, yet
- Search engine is forced to guess when page is ready to be read
- Constantly changing content can cause even more difficulties

---

# Deep Links Are Hard to Get Indexed
- Not every browser supports HTML5 History API yet (IE9)
- Thus, HTML bookmark anchor (#) is used: `/home#section1`

-> Search engines don't detect these as separate pages

---

# Solution?
What if we could combine best of both worlds: *User experience of SPAs* and *SEO friendliness and high performance of vanilla HTML pages*?

---

# Universal Rendering
1. Render the app as plain HTML on server and return it
2. When the actual Angular app is ready on a client, change to use it

-> Best parts of both worlds!

---

# Angular Universal
- Separate project by Angular core team
- Provides functionality to render the app as plain HTML before being replaced by actual JavaScript app once loaded
- Can be used with multiple languages

---

# Preboot
- [Preboot](https://github.com/angular/universal/tree/master/modules/preboot) allows recording events before Angular takes over and replaying them afterwards
- Key features:
  - Record and play back events
  - Respond immediately to events
  - Maintain focus even page is re-rendered
  - Buffer client-side re-rendering for smoother transition
  - Freeze page until bootstrap complete if user clicks button
