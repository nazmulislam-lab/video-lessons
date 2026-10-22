# Video Lessons

Give your coding agent a YouTube link and get a short course instead of a video to watch.

The skill pulls the transcript, plans one-concept lessons, and builds them one at a time as HTML pages. Each lesson has a quiz with instant feedback, fact-check notes where the video is wrong or out of date, and a small task to try for real. When you say "next", it first asks you a few recall questions in chat, then adapts the next lesson to what you missed. `review` runs spaced-repetition quizzes across everything you've learned.

It works with **Claude Code, Codex CLI, Gemini CLI**, and any agent that reads `SKILL.md` skill folders.

It is built on Matt Pocock's [`teach`](https://github.com/mattpocock/skills) skill and ships with **human-touch**, a writing skill that strips AI tells from the lesson text.

## What you need

| Requirement | Why | Required? |
|---|---|---|
| An agent that supports skills (Claude Code, Codex CLI, Gemini CLI, …) | Runs the skill | Yes |
| Python 3.8+ | Fetch-transcript and review scripts (standard library only, nothing to `pip install`) | Yes |
| **An Apify API token** | Fetches YouTube transcripts | Yes, see [below](#apify-token-the-only-key-you-need) |
| `yt-dlp` | Expands playlists and reads video lengths for cost estimates | Only for playlists |
| Web search in your agent | Fact-checking lessons | Recommended |

## Install

Clone the repo:

```bash
git clone https://github.com/nazmulislam-lab/video-lessons.git
cd video-lessons
```

### Option A: the install script (Linux, macOS, WSL)

```bash
./install.sh              # installs for every agent it finds
./install.sh --set-token  # stores your Apify token (input is hidden)
```

Install for one agent only with `--claude`, `--codex` or `--gemini`. For any other agent, use `--dir <its skills folder>`. `--link` symlinks instead of copying, so `git pull` updates the skills in place. `--uninstall` removes them.

If you already have a `human-touch` skill, the script leaves it alone unless you pass `--force`.

### Option B: Claude Code plugin

Inside Claude Code:

```
/plugin marketplace add nazmulislam-lab/video-lessons
/plugin install video-lessons@video-lessons
```

Plugin skills are namespaced, so the command is `/video-lessons:video-lessons`.

### Option C: `npx skills` (Cursor, OpenCode, Windsurf and 70+ other agents)

```bash
npx skills add nazmulislam-lab/video-lessons -g
```

It asks which agents to install to. It needs Node 22+.

### Option D: by hand (including Windows)

Copy both folders from `skills/` into your agent's skills folder:

| Agent | User-level skills folder |
|---|---|
| Claude Code | `~/.claude/skills/` (Windows: `%USERPROFILE%\.claude\skills\`) |
| Codex CLI | `~/.agents/skills/` |
| Gemini CLI | `~/.gemini/skills/` |

Keep `video-lessons` and `human-touch` side by side. The lesson skill looks for `../human-touch`.

Restart your agent after installing.

## Apify token: the only key you need

Transcripts come from [Apify](https://apify.com), using the `johnvc/YoutubeTranscripts` actor. You need your own Apify account and token. Nothing else needs a key: web search runs through your agent, and quizzes and review run locally.

**1. Get a token.** Sign up at [apify.com](https://apify.com) (the free plan includes monthly credit, which covers hundreds of videos). Then open **[Console → Settings → API & Integrations](https://console.apify.com/settings/integrations)** and copy your **Personal API token**.

**2. Put it in ONE of these places:**

| Where | How | Best for |
|---|---|---|
| **`~/.config/video-lessons/.env`** (recommended) | `./install.sh --set-token`, or create the file with one line `APIFY_TOKEN=apify_api_...` and run `chmod 600` on it. See [`.env.example`](.env.example). | Everyone. Works in every agent, survives restarts. |
| `APIFY_TOKEN` environment variable | `export APIFY_TOKEN=apify_api_...` in `~/.bashrc` / `~/.zshrc` (Windows: `setx APIFY_TOKEN apify_api_...`) | If you already manage keys as env vars |

**Don't paste the token into the chat** with your agent, and never commit it. The scripts read it from the file or the environment, send it only to `api.apify.com` in a request header, and never print it.

**Already have the Apify MCP server connected?** (e.g. `https://mcp.apify.com` in Claude Code.) The skill can fetch through it instead, with no token file. The token file is still the more reliable path, because it keeps long transcripts out of the agent's context.

### What it costs

| What | Price (Apify free tier, Oct 2026) |
|---|---|
| A video with captions (most of YouTube) | ~$0.003 per video |
| A video with **no** captions, transcribed with Whisper | ~$0.012 per minute (a 1-hour video ≈ $0.72) |

The skill **asks before** any Whisper run, and before fetching more than one video it shows the list and the estimated total. Each Apify call has a spending cap.

## Use it

| Claude Code | Codex CLI | Gemini CLI / others | What happens |
|---|---|---|---|
| `/video-lessons <youtube-url>` | `$video-lessons <url>` | "make video lessons from <url>" | Fetches the transcript, sets up a topic, plans the lessons, builds lesson 1 |
| `/video-lessons <playlist-url>` | same | same | Lists the videos and the cost, splits them into batches, and asks before fetching |
| `/video-lessons next` | `$video-lessons next` | "next lesson" | Recall check in chat, then the next lesson |
| `/video-lessons review` | `$video-lessons review` | "video-lessons review" | Spaced review of whatever is due, mixed across topics |

The first time you use a new topic, it asks for one sentence on **why** you want to learn it. Every lesson is written around that sentence.

## Where your lessons go

Everything you learn stays on your machine, outside this repo:

```
~/Learning/<topic>/            (set VIDEO_LESSONS_HOME to change ~/Learning)
  index.html                   course home: open this in a browser
  MISSION.md  RESOURCES.md  COMMUNITIES.md  GLOSSARY.md  NOTES.md
  review.json                  spaced-review schedule
  learning-records/            what you've shown you know
  reference/                   printable cheat sheets
  videos/0001-<slug>/
    PLAN.md  meta.json  transcripts/<video-id>.md
    lessons/01-<slug>.html ...
```

## What's in this repo

```
skills/video-lessons/   the skill: SKILL.md, formats/, templates/, scripts/
skills/human-touch/     the writing-cleanup skill used on lesson text (MIT, own LICENSE)
.claude-plugin/         Claude Code plugin + marketplace manifests
install.sh              installer for Claude Code, Codex, Gemini and others
.env.example            where the Apify token goes
```

## Privacy and security

- The only outbound calls the scripts make are to `api.apify.com` (with your token) and to YouTube via `yt-dlp` (no account needed). Your agent also makes web searches to fact-check lessons.
- Video URLs you submit go to Apify, which runs the scraper under your account.
- Transcripts, lessons and quiz history stay in `~/Learning`. Nothing is uploaded anywhere.
- The token file is created with `600` permissions in a `700` folder.
- Apify runs the transcript actors in its cloud under your account. The actors' authors (`johnvc`, `codepoetry`) never see your token; they only receive the video URLs.
- Transcripts and web pages are treated as untrusted input. The skill tells the agent never to follow instructions found in a video or web page, and to escape transcript text before it goes into lesson HTML.
- `fetch_transcript.py` accepts only `youtube.com` / `youtu.be` URLs and passes them to `yt-dlp` after `--`, so a crafted "URL" can't become a command-line option.
- `install.sh` never deletes a folder it didn't create unless you pass `--force`.

Found a security problem? See [SECURITY.md](SECURITY.md).

## Troubleshooting

| Problem | Fix |
|---|---|
| "No Apify token" | Run `./install.sh --set-token`, or check `~/.config/video-lessons/.env` contains `APIFY_TOKEN=...` |
| "Apify rejected the token (401)" | Copy the token again from the Apify console; tokens are revoked if you regenerate them |
| "Playlists need yt-dlp" | `pip install yt-dlp` (or `brew install yt-dlp`, `sudo apt install yt-dlp`) |
| The skill doesn't show up | Restart the agent. In Gemini CLI, run `/skills reload`; in Codex, `/skills` lists what it found |
| Lessons don't open | The agent prints the lesson's full path at the end of every lesson. Open that file, or `~/Learning/<topic>/index.html`, in your browser |

## Credits

- Teaching approach and file formats adapted from [`teach`](https://github.com/mattpocock/skills) by Matt Pocock (MIT).
- `human-touch` combines [stop-slop](https://github.com/hardikpandya/stop-slop) by Hardik Pandya and [humanizer](https://github.com/blader/humanizer) by Siqi Chen (both MIT).
- Transcripts via [Apify](https://apify.com) actors [`johnvc/YoutubeTranscripts`](https://apify.com/johnvc/YoutubeTranscripts) and [`codepoetry/youtube-transcript-ai-scraper`](https://apify.com/codepoetry/youtube-transcript-ai-scraper).

## License

MIT. See [LICENSE](LICENSE).
