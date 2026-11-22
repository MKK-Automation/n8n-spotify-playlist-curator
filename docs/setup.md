# Setup

About 10 minutes. The workflow canvas has the same checklist in a sticky note.

## 1. Import the workflow

In n8n: **Workflows**, open the menu next to the create button, **Import from file**, and choose [`workflow/spotify-playlist-curator.json`](../workflow/spotify-playlist-curator.json).

The file contains no credentials, so nodes that need one show a warning icon until you finish the steps below.

## 2. Spotify credential (12 nodes)

1. Open the [Spotify developer dashboard](https://developer.spotify.com/dashboard) and click **Create app**.
2. In n8n, open any node marked with a key (for example **Get Me**), then **Credential**, **Create new**, **Spotify OAuth2 API**.
3. Copy the **OAuth Redirect URL** that n8n shows into your Spotify app's **Redirect URIs** and save.
   - Spotify has rules about which redirect URIs it accepts (for example, it may reject `localhost`). On a local install, opening n8n at `http://127.0.0.1:5678` instead of `localhost` usually avoids this. Check Spotify's current documentation if the redirect is rejected.
4. Paste the app's **Client ID** and **Client Secret** into n8n and click **Connect my account**.
5. Select that credential on all 12 key nodes. n8n often fills in the others for you, but check every node for a warning icon.

If your Spotify app is in development mode, only accounts listed under the app's **User management** can use it.

The 12 Spotify nodes are: Get Me, Get Playlist Total, Fetch My Playlists, Create Playlist, Get Playlist Counts, Fetch Playlist Pages, Get Liked Total, Fetch Liked Pages, Add To Playlist (Batched), Add Reallocated Tracks, Remove Moved From Source, Clear Existing Copies.

## 3. AI credential (2 nodes)

Add an **OpenRouter** API key to **Agent Chat Model** and **Classifier Chat Model**. The default model is `deepseek/deepseek-chat`. You can replace both nodes with any other chat model node (OpenAI, Anthropic, and so on).

## 4. Data table

The workflow stores which playlists are categories in an n8n data table. Create it in the **same project** as the workflow.

1. In the left sidebar, open your project (for example **Personal**) and go to the **Data tables** tab.
2. Click **Create data table** and name it exactly `curator_playlists`.
3. Add four columns, all of type **String**: `playlistId`, `role`, `rule`, `isDefault`.
4. Leave the table empty. The workflow fills it in on its first run.

Names must match exactly. If you use another table name, re-select your table in the four Data table nodes: **Read Registry**, **Register Legacy**, **Register Created**, **Register Adopted**.

## 5. Point the tools at this workflow

Open the six tool nodes (**List categories**, **Create categories**, **Adopt playlist**, **Sort liked songs**, **Retry unsorted songs**, **Re-check songs**) and set the **Workflow** field to *this* workflow.

## 6. Publish

Click **Publish**. The tools run the *published* version, so publish again after every edit.

## 7. Chat

Click **Open chat** and tell the agent which categories you want, for example:

> Create playlists for house, techno and everything else

Then:

> Sort all my liked songs

## Options

The **Settings** node (section 4 on the canvas) holds a few options, such as whether new playlists are public or private (private by default) and the name of the catch-all playlist ("Unsorted songs").
