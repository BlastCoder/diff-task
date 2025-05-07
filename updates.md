here is what I have added:

1. Decoupled diff listing from diff fetching
  The `/api/sample-diffs` endpoint now returns only PR metadata (ID, title, URL, and a `diffUrl`).  This lets the client fetch lists of merged PRs without immediately hitting GitHub’s rate limits.

2. On-demand diff retrieval
  When the user clicks a PR, the front-end posts its metadata to `/api/generate-notes`.  That route then:
  1. Authenticates 
  2. Sends a streaming chat request to OpenAI with a system prompt for “developer” vs. “marketing” sentences.
  3. Wraps OpenAI’s streamed response in an SSE so updates arrive in real time.

3, Other

  1. A new `useReleaseNotes` hook manages the POST, reads chunks from the SSE, parses JSON lines, and accumulates two arrays: one for developer-focused sentences, one for marketing-focused one
  2. Better loading and error states for both list-fetch and stream-fetch
