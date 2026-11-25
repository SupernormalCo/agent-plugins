---
name: supernormal-databases
description: Read and keep the databases in the member's Supernormal Rooms - issue trackers, boards and any list with rows and columns. Use when the member mentions an issue by key (ENGIN-12), a board, tracker, backlog or list in a Room, asks you to file, update, triage or summarize issues, or asks you to set up a new board or add a column. Not for the member's task board, which is supernormal-tasks.
---

# Databases in Supernormal

A database is a table kept in a Room: rows (issues), columns (properties) and a page per row.
Members see it as a board or a list. Every database has a Title column and an Issue ID column whose
keys (`ENGIN-1`, `ENGIN-2`) the Room assigns.

Two things that look alike and are not:

- **The task board** is not changed here. A task is work one person hands another, with an assigner
  and an assignee: use the supernormal-tasks skill and the `task_` tools.
- **A document** has no rows. If the member wants prose, use supernormal-documents.

## Find and read

1. `db_list` lists the Room's databases with id and slug. Either works as `database_id`.
2. `db_get` returns the whole database as a markdown table with an id column.
3. `db_query` returns the same table filtered by `status` (an option name) or `assignee`
   (`member:<user_id>`, with the user id from `room_members`).
4. `db_get_row` returns one row: its page, each column's value, and the values each column accepts.
   `row_id` takes the row id or the issue key.

Do not `room_read` a database's file: the `db_` tools return it in the form you can write back.

## Change a row

1. `db_get_row` first. It tells you the column names and the option names you may write, and the
   page as it stands.
2. `db_update_row` with only what changes:
   - `properties` maps a column name to its new value.
   - `title` renames the issue.
   - `body` replaces the issue's page. Start from the page `db_get_row` returned, keep what should
     stay and add to it.

Values by column type:

| Column type       | Value                                             |
| ----------------- | ------------------------------------------------- |
| status, select    | An option name exactly as `db_get_row` shows it   |
| multi_select      | A list of option names                            |
| person            | The member's name, or user id from `room_members` |
| checkbox          | `yes` or `no`                                     |
| date              | `YYYY-MM-DD`                                      |
| text, url, number | The value itself                                  |

A column or option that does not exist is refused, and the refusal names what does. Correct the
call; do not add a column or an option just to make a write go through.

The plan, notes and result for an issue belong on its page (`body`), not in a separate document.

## Add rows

- One row: `db_create_row` with `title`, `body` for its page, and `properties` for the other
  columns.
- Two or more: show the member the rows you will add and wait for their yes, then `db_create_rows`
  once, not `db_create_row` in a loop.
- Before filing, `db_get` or `db_query` to check the issue is not already there.
- The Room assigns the Issue ID. Do not set it.
- Give the member the key and `href` of what you created.

## Set up a database

Create one only when the member asks for a board, a tracker or a list to keep.

- `db_create` with a `name`. With no `properties` you get Title, Status (Todo, In progress, Done),
  Assignee and Issue ID, which suits most trackers. Start there unless the member named columns.
- To choose columns, pass `properties`. select, status and multi_select take
  `options: [{ name, color }]`.
- `db_add_property` adds a column later. `db_update_property` renames one, changes its type or
  replaces its options; `key` is the column key `db_get_row` shows.
- Passing `options` to `db_update_property` replaces the whole list. Include every option you want
  to keep, or rows that use a dropped option lose their value.
- `prefix` on the Issue ID column changes the keys issued from now on; existing keys stay.

## Rules

- Changing a column changes it for everyone who uses the board. Do it only when the member asks.
- You cannot delete a row or a database. Move a row to its closed status, or tell the member.
- You cannot comment on a row. Read its comment threads with `conversation_list` (`database_id` and
  `row_id`) before a large change; put what you have to say on the row's page or tell the member.
- Do not move other people's issues between statuses or reassign them unless the member asked.
- When you summarize a board, say which database and Room it came from and how many rows you read.
