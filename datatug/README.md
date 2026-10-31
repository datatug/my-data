# My DataTug Data

View your DataTug data with DataTug.

This project holds no data and no credentials. Its saved TugQL queries read the signed-in
user's own records in DataTug's Firestore database, through that user's own Firebase session and
the existing Firestore security rules:

- `users/{uid}` and `users/{uid}/ext/datatug`;
- `spaces/{personal space id}` and `spaces/{personal space id}/ext/datatug/projects/**`.

The queries are the same for everyone. DataTug fills in who you are when a query runs, through
two context variables:

| Variable | Value |
| --- | --- |
| `SneatUserID` | your Firebase Auth uid |
| `SneatUserPersonalSpaceID` | the key of the `type: "personal"` space in your Sneat user record |

`{name}` puts a value into a collection path; `@name` passes it as a bound value:

```
from spaces/{SneatUserPersonalSpaceID}/ext/datatug/projects
order by title asc
```

"Queries of a project" also asks for a `projectId`: take it from the `__key__` column of "My projects".

Open it in the DataTug web app and sign in to run the queries. DataTug Core and the CLI accept the
project in `datatug validate` but refuse to run these queries, because only the web app binds the
context variables.

Spec: [my-data-project](https://github.com/datatug/backstage/tree/main/spec/features/my-data-project).

## Boards

- **My DataTug Data** (`my-data`): your user record, your projects, the demo projects, your personal space and your settings.
- **DataTug project at Cloud** (`firestore-project`): one of your projects, by `projectId`: its record, queries and boards.
- **DataTug project at GitHub** (`github.com-project`): one demo project, by `projectId`: its record, queries and boards.

Each row of **My projects** and **Demo projects** opens the board for that project's `storage`, passing its `projectId`.

The demo projects come from the small inGitDB record set at this repository's root
(`demo_projects`), read straight from GitHub through `fixtures/ovdb/demo-projects.ovdb.yaml`.

The lists of each demo project's queries and boards (`demo_project_queries`, `demo_project_boards`) are copies, so update them when a demo project changes.
