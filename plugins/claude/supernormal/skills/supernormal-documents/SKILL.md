---
name: supernormal-documents
description: Write, edit, rename and move documents in the member's Supernormal Rooms. Use when the member asks you to save, draft, write up or update something in a Room (a doc, a plan, notes, a wiki page), to add a decision or learning to a Room's memory, or to move a file or a meeting into another Room.
---

# Documents in Supernormal

A document is a text file in a Room, almost always markdown. Members open it in the app and may be
editing it while you work. You write as the signed-in member.

For anything with rows (a board, a tracker, a list to keep) use the supernormal-databases skill,
not a document.

## Write a new document

1. Find the Room (`rooms_list`). If the member did not say which Room and more than one fits, ask.
2. Check it does not exist already: `room_search` for its subject, or `room_list` on `/documents`.
   Update the existing document instead of writing a second one.
3. `room_write` with the path and the whole text as `content`. Leave `base` out.
4. Give the member the `href` the tool returned.

Where it goes:

- `/documents/<short-kebab-name>.md`, unless the member named a path.
- Start the file with a `# Title` line; the app shows it as the document's name.
- Use real line breaks in `content`, not `\n` written out.

## Change an existing document

`content` always replaces the whole file, so:

1. `room_read` the file.
2. Make your change in the text you read. Keep everything you were not asked to change.
3. `room_write` the full new text as `content`, and pass the text exactly as `room_read` returned
   it as `base`.

`base` is what keeps other people's work: the Room merges your change with edits members made after
your read. Without `base` your write replaces whatever is there, including a sentence someone typed
a minute ago. Always pass it when the file already exists.

Read the document's comment threads first when the change is more than a small fix
(`conversation_list` with `node_path`; see supernormal-context).

## Room memory

`/wiki/memory/decisions.md` and `/wiki/memory/learnings.md` hold what the Room has decided and
learned. The Room compiles them itself from chat and meetings.

- Add to them only when the member asks you to record something.
- They are append-only: `room_read` the page, add your bullet at the end, and write the page back
  with every existing line unchanged and `base` set. A write that drops or rewords a line is
  refused.
- One bullet per fact, in a plain sentence, with the date when it matters.
- Never write `/wiki/index.md` or `/wiki/memory/history.md`: the Room rewrites them.

## Paths the Room keeps itself

Goals, people, apps, databases, comment threads, the memory pages, `/tasks` and a meeting's
`shared-with.json` are stored as files, but they are not yours to write, move or transfer.
`room_write`, `room_move` and `room_transfer` refuse those paths, and a folder that holds any of
them, such as `/wiki`. Databases change through the `db_` tools; goals and comment threads are
read-only. A meeting shared into the Room from another Room stays where it is.

## Rename or move

- `room_move` renames or moves a file or folder inside one Room. Links to the old path stop
  working, so do it only when the member asks.
- `room_transfer` moves a file, or a whole meeting (`/meetings/<id>`), into another Room the member
  belongs to. The source keeps a redirect, so old links still open. A meeting keeps its
  `/meetings/<id>` path in the destination.
- Do not copy a file across Rooms with `room_read` and `room_write` when the member asked to move
  it: that leaves two copies that drift.
- Moving something into a shared Room shows it to everyone in that Room. If it comes from the home
  Room, confirm with the member that they mean to share it.

## Rules

- You cannot delete a file. If one should go, tell the member.
- Do not rewrite a document a teammate wrote beyond what the member asked for.
- Do not put secrets (passwords, keys, tokens) in a Room file.
- Tell the member the path of everything you wrote or moved.
