# Before / after

## Silent failure -> visible error (React)
Before:
```jsx
async function save() { await api.save(form); }
<button onClick={save}>Save</button>
```
After:
```jsx
const [status, setStatus] = useState("idle"); // idle | saving | saved | error
async function save() {
  setStatus("saving");
  try { await api.save(form); setStatus("saved"); }
  catch { setStatus("error"); }
}
<button onClick={save} disabled={status === "saving"}>
  {status === "saving" ? "Saving..." : "Save changes"}
</button>
{status === "error" && <p role="alert">We couldn't save your changes. Check your connection and try again.</p>}
{status === "saved" && <p role="status">Changes saved.</p>}
```

## Four states on a data screen
```jsx
if (query.isLoading) return <ListSkeleton />;
if (query.isError)   return <ErrorState message="We couldn't load your projects." onRetry={query.refetch} />;
if (!query.data.length) return <EmptyState title="No projects yet" hint="Create one to start tracking your work." action={<button onClick={create}>Create project</button>} />;
return <List items={query.data} />;
```

## Disabled submit -> explain and jump
Before: `<button disabled={!valid}>Submit</button>` (the user hunts for the problem).
After:
```jsx
<button type="submit">Create account</button>
// on submit
if (!valid) { setShowErrors(true); firstInvalidField.focus(); return; }
```
Each invalid field shows its message inline; focus lands on the first one.

## Bigger hit area without a bigger icon
```css
.icon-btn { position: relative; width: 24px; height: 24px; }
.icon-btn::after { content: ""; position: absolute; inset: -10px; } /* 44px target */
```

## Visible focus
```css
:focus-visible { outline: 2px solid currentColor; outline-offset: 2px; }
```

## Hover only with a pointer
```css
@media (hover: hover) { .btn:hover { background: var(--btn-hover); } }
```

## Optimistic toggle with rollback
```jsx
function toggleStar() {
  const previous = starred;
  const next = !previous;
  setStarred(next);                        // instant
  api.setStarred(id, next).catch(() => {
    setStarred(previous);                  // restore, never flip: a second click may have changed it
    toast("Couldn't update the star. Try again.");
  });
}
```

## Vague copy -> specific
- "OK" -> "Save changes" (or whatever the action is)
- "Are you sure?" -> "Delete 3 files? This can't be undone." [Delete 3 files] [Cancel]
- "Invalid email" -> "Enter an email like name@example.com"
- "No data" -> "No invoices yet. Create your first invoice." [New invoice]
