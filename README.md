# Spotify Playlist Curator for n8n

**Chat with an AI agent that sorts your Spotify Liked Songs into playlists, in one prompt.**

Tell the agent how you like your music organised ("house, techno and everything else") and it classifies your whole Liked Songs library into those playlists. Like new songs later, ask again, and only the new ones are processed.

## What it does

- **Sorts your whole library in one prompt.** In one test run it sorted about 1,400 liked songs in roughly six minutes.
- **Only handles new songs on later runs.** It compares your Liked Songs with the playlists it manages, so it is safe to run again whenever you like.
- **Rename playlists freely.** Categories are tracked by Spotify playlist ID in an n8n data table, not by name.
- **Never removes songs from your Liked Songs.** Songs are copied into playlists.
- **Handles songs the AI can't place.** They go to an "Unsorted songs" playlist, and you can ask the agent to retry them.
- **Reuses playlists you already have.** Ask the agent to adopt an existing playlist as a category. Its name and songs stay as they are.

## SAMPLE PROMPTS

- *"Create playlists for house, techno and everything else"*
- *"Use my playlist Gym as the workout category"*
- *"Sort all my liked songs"*
- *"Retry the unsorted songs"*

## Quick start

1. In n8n, **import** [`workflow/spotify-playlist-curator.json`](workflow/spotify-playlist-curator.json) (Workflows, then the menu, then **Import from file**).
2. Create a **Spotify OAuth2 API** credential and add it to the 12 nodes marked with a key.
3. Add an **OpenRouter** API key (or any chat model) to the two Chat Model nodes.
4. Create the `curator_playlists` **data table** (four string columns).
5. Open the six tool nodes and select *this* workflow in each.
6. **Publish** the workflow, click **Open chat**, and say what categories you want.

The canvas has numbered sections and sticky notes that walk through each step. See [docs/setup.md](docs/setup.md) for details.

## How it works

A chat trigger feeds an **AI agent** that picks one of six tools (list, create, adopt, sort, retry, re-check). Each tool runs a sorter sub-workflow in the same workflow, which reads your playlists and Liked Songs from Spotify, classifies new songs in batches of 50 with an LLM, and adds them to the right playlists.

Details, including a diagram: [docs/how-it-works.md](docs/how-it-works.md).

## Requirements

- An n8n instance with **Data tables** and the **AI Agent** nodes (recent n8n 2.x). Self-hosted or cloud.
- A **Spotify** account and a Spotify developer app (see setup).
- An API key for an LLM provider. The workflow uses **OpenRouter** with `deepseek/deepseek-chat` by default, but any chat model node can replace it.

## Privacy and security

- **Your music data goes to your LLM provider.** To classify songs, the workflow sends each song's title, artist, album and release year to the model you configure (OpenRouter by default). Check that provider's data policy. The workflow only talks to Spotify and your LLM provider. Your Spotify credentials are never sent to the model.
- **What it changes in Spotify.** It creates playlists and adds songs to them. When you retry or re-check songs, it removes the moved songs from the playlist they came from (for example *Unsorted songs*). It never deletes playlists, never changes your Liked Songs, and never renames or edits a playlist you adopt.
- **Keep the chat private.** The chat trigger is private by default. Do not turn on the public chat option or share its link: anyone with it could ask the agent to create and fill playlists on the connected Spotify account.
- **Your credentials stay in n8n.** They are not part of the workflow file. Never paste tokens, API keys or credential exports into issues or discussions.

## Limitations

- **Accuracy depends on the model.** Spotify no longer shares artist genres through its API, so the model classifies from the song title, artist, album and release year. Some songs will land in Unsorted. Retrying usually places most of them.
- **Large libraries take time.** Roughly 20 seconds per batch of 50 songs.
- **Run one sort at a time.** Two sorts at once can add songs twice.
- **Spotify apps in development mode** only work for accounts you add under *User management* in the Spotify dashboard.
- **Publish after editing.** The tools call the *published* version of the workflow.

More in [docs/troubleshooting.md](docs/troubleshooting.md).

## License

[MIT](LICENSE)

## Author

Built by [MKK Automation](https://github.com/MKK-Automation).

## Disclaimer

This is an independent project. It is not affiliated with, endorsed by or sponsored by Spotify, n8n or OpenRouter. All trademarks belong to their respective owners.
