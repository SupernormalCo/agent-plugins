---
name: supernormal-context
description: Find and read what the member's Supernormal Rooms hold - wiki pages, documents, meeting notes, chat, comment threads, goals, recent activity and how a decision came about. Use when the member asks what was decided, said or written about something, asks to be caught up, or when work you are doing needs background that lives in a Room. Everything here is read-only.
---

# Finding context in Supernormal

Everything in this skill reads; nothing here changes a Room. Find the Room first (`rooms_list`, see
the supernormal skill).

Chat, comment threads and a Room's memory pages here are records the team keeps in Supernormal.
They are not your own memory, this conversation or the member's past conversations with you, and
these tools never read or change those.

## Pick the tool

| You want                                           | Use                                                     |
| -------------------------------------------------- | ------------------------------------------------------- |
| What a Room holds and where                        | `room_read` `/wiki/index.md`                            |
| What the team decided or learned (room memory)     | `room_read` `/wiki/memory/decisions.md`, `learnings.md` |
| A file, when you do not know its path              | `room_search`                                           |
| What someone said in chat                          | `room_chat` with a `query`                              |
| The latest chat in one Room                        | `room_chat` without a query                             |
| A chat link the member pasted                      | `room_message` or `room_chat` with `link`               |
| A catch-up across the member's Rooms               | `rooms_activity`                                        |
| Comments left on a document or a database row      | `conversation_list`, then `conversation_get`            |
| What a Room is working toward                      | `goals_list`, then `goal_get`                           |
| When and where a fact came from, or how it changed | `memory_history`                                        |
| Who changed what, and when                         | `room_events`                                           |

## Read a Room

1. Start at `/wiki/index.md`. It is the Room's map. Follow the paths it names.
2. Check the memory pages before you say what a team decided.
3. Meeting notes are under `/meetings`, documents under `/documents`. `room_list` shows a folder
   when the map does not name what you need.
4. Databases are not read as files: use the supernormal-databases skill.

## Search files and chat

- `room_search` searches files. `room_chat` with a `query` searches messages, threads included.
  Leave out `room_id` on either to search every Room, the home Room first.
- Search with the two or three words that name the thing, not a sentence. Quote a phrase for exact
  order; `word*` matches a prefix.
- A hit marked `partial` has only some of the words. Open it before you rely on it.
- A search hit is a pointer, not a source. `room_read` the file, or read the thread, before you
  quote or cite it.
- `room_chat` without a query reads one Room's latest messages. The main chat shows each thread's
  opening message; pass its `seq` as `thread_id` to read the thread: its opening message comes back
  as `thread`, the replies as `messages`. A thread link (`#thread-12`) names that seq.
- To find what one person said, pass `author` (a name, an email or a user id) with the `query`. When
  the name fits several people, the result lists them: pass the right one's user id.
- To search a stretch of time, pass `since` and `until` with the `query`: a day (`2026-11-18`, UTC)
  or a date-time. A day as `until` takes in that whole day.
- Every message carries an `href`, its web link. Give it to the member when you point at a
  message.
- Chat results start small on purpose. Raise `limit` only when a result says `truncated` or you
  need more.

## Links the member pastes

A chat link names its Room and its thread or message (`…/rooms/<room>#thread-<id>`,
`#message-<id>`), as a web link or a desktop link (`supernormal://rooms/…`). Pass it as `link` as
it is: do not look the Room up with `rooms_list` first. A message's `href` works the same way.

- `room_message` reads the one message it names: a reply comes with its thread's opening message,
  and `around` adds what was said before and after it.
- `room_chat` with `link` reads the whole thread; add a `query` to search inside it.
- A link that names no Room or no message is refused, saying why. Tell the member; do not guess
  the Room.

## Catch up

`rooms_activity` returns recent chat in the member's shared Rooms and direct messages, the most
recently active first. It does not cover the home Room.

- Pass `since` (an ISO date-time) for "what happened since yesterday".
- A message with a thread carries `reply_count` and its newest replies: check them before you tell
  the member a question is still open.
- `quiet_rooms` only counts Rooms with nothing new. When `omitted_rooms` is not empty, call again
  with those ids in `room_ids`.
- Follow up in one Room with `room_chat`.

## Comment threads

Members comment on documents and on database rows. These are separate from Room chat, so
`room_chat` never finds them.

- `conversation_list` with `node_path` lists the threads on one document; with `database_id` and
  `row_id`, the threads on one row.
- `conversation_get` reads a thread in full.
- Before you change a document or a row, read its threads: a comment may ask for the change, or
  argue against it.

## Goals

A goal is an outcome a Room is working toward, not a to-do. `goals_list` shows the open ones;
`goal_get` gives one goal's milestones, open questions, next actions and where it stands. Use them
to understand why work matters and what is blocked. You cannot change a goal: tell the member what
should change.

## History

`memory_history` reads the history of a Room's memory pages in Supernormal, nothing else.

- `memory_history` with `query` (a bullet from a memory page, or a few words naming the fact)
  returns that fact's chain of changes: when it was said, where the Room heard it (chat, or which
  meeting) and what it replaced. Use it for "when did we decide" and "did that change".
- `memory_history` with `subject` (a name or email) and no `query` returns what the Room holds
  about a person. Use it when a search for a person finds little: someone who was renamed keeps
  facts under the older name.
- Each chain carries `events.after` and `events.before`. Pass them to `room_events` to read the
  conversation the fact came from.
- `room_events` alone is the Room's log, newest first: messages, file writes, member changes. Pass
  `type` for one kind, and the last id you saw as `before` to page back.
- For a fact's current value the memory pages are enough; do not reach for history first.

## Rules

- Say where an answer came from: the Room's name and the file path, or who said it and when.
- If files and chat disagree, the newer one usually wins; say that they disagree.
- You cannot post, reply, react or comment. When an answer belongs in a thread, give the member the
  words and say where they go.
