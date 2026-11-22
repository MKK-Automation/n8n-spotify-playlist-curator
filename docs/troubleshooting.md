# Troubleshooting

## The tools say the workflow was not found or do nothing

Open each of the six tool nodes and select *this* workflow in the **Workflow** field. Imported workflows have this field empty on purpose. Then **publish** the workflow. The tools run the published version.

## "Credentials not set" or a warning icon on a node

Select your Spotify credential on every key node, and your AI credential on both Chat Model nodes. See [setup.md](setup.md).

## Spotify rejects the redirect URL when connecting

Spotify restricts which redirect URIs it accepts. On a local install, open n8n at `http://127.0.0.1:5678` instead of `localhost`, create the credential from that address, and copy the redirect URL it shows into your Spotify app. Check Spotify's current documentation if it is still rejected.

## 403 or "user not registered" errors from Spotify

A Spotify app in development mode only works for accounts listed under **User management** in the Spotify dashboard. Add your account there.

## The workflow fails at "Read Registry" (data table not found)

Create the `curator_playlists` data table in the same project as the workflow, with four string columns: `playlistId`, `role`, `rule`, `isDefault`. If your table has a different name, re-select it in the four Data table nodes. See [setup.md](setup.md).

## "Required at ..." error from the agent

A tool input was missing. This can happen after editing a tool node's inputs. Optional inputs need a default value in their `$fromAI(...)` expression.

## The sorter nodes never turn green

That is expected. Tool calls run the sorter as a separate execution. Open the **Executions** tab and look for runs of mode "integrated".

## Some songs ended up in "Unsorted songs"

The AI classifies from each song's title, artist, album and release year, so some songs are ambiguous. Ask the agent to "retry the unsorted songs". Each retry handles up to 100 songs, and the agent can repeat it.

## Songs appear twice in a playlist

Run only one sort at a time. If two runs overlap, both see the same songs as new. Retrying unsorted songs clears existing copies before adding, so it will clean up duplicates for the songs it processes.

## A sort takes a long time

Each batch of 50 songs takes roughly 20 seconds. A library of 1,500 songs takes several minutes. The agent waits for the run to finish and reports at the end.

## I edited the workflow but nothing changed

Publish again. If you also have the workflow open in another browser tab, close it or refresh without saving, because saving from an older tab replaces your newer changes.
