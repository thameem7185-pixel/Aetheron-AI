# index11.html: single-file AI chat app

A self-contained AI chat interface in one HTML file (HTML, CSS and JavaScript, no build step, no dependencies). It is branded **Aetheron AI**. To rebrand it, change one constant.

Created by **Muhammad Thameem KT**.

## Features

- **Three modes** (sidebar): General, Code and Deep think. Each mode sends a different system prompt.
- **Typewriter replies** with a live ball at the tip of the text. The send button turns into a stop button while text is typing.
- **Markdown rendering**: headings, bold, italic, lists, quotes, links, inline code and fenced code blocks.
- **Code blocks** with Copy and Save buttons. Save downloads the snippet with the right file extension.
- **File attachments**: text and code files up to 2 MB are read in the browser and sent along with your message. Other files are sent by name only.
- **Voice input** through the browser Speech Recognition API. A finished phrase is sent automatically.
- **Voice replies** (on by default). Each reply can also be read aloud with its speaker button. Voice order:
  1. ElevenLabs, if you add an API key
  2. StreamElements free neural voice with a deep audio effect
  3. The device's built-in speech synthesis
- **Light and dark themes** with animated backgrounds: drifting colour glow in light mode, glowing stars in dark mode. The choice is remembered.
- **Chat tools**: new chat, export the conversation as a `.txt` file, copy any reply, and retry after an error.
- **Connection handling**: a "slow connection" note after 10 seconds, a 45 second timeout, offline and online notices.
- **Responsive layout**: the sidebar is fixed on wide screens (860px and up) and a slide-over drawer on phones.
- Respects `prefers-reduced-motion`.

## Quick start

1. Put `index11.html` on any static host, or open it locally.
2. Serve it over **HTTPS** (or `localhost`) if you want microphone input and clipboard copy to work reliably.
3. Make sure `WORKER_URL` points to a working backend (see below).

## Configuration

All settings are constants at the top of the `<script>` block.

| Constant | Purpose |
| --- | --- |
| `APP_NAME` | The assistant's name (set to `'Aetheron AI'`). It is used in the page title, brand labels, input placeholder, system prompts and downloaded file names. Change it only here. |
| `WORKER_URL` | The backend endpoint that receives chat requests. |
| `ELEVENLABS_API_KEY` | Optional. Leave as `"YOUR_ELEVENLABS_API_KEY"` to skip ElevenLabs. |
| `ELEVENLABS_VOICE_ID` | The ElevenLabs voice to use (default is George). |

The system prompts live in `getSystemPrompt()`. The welcome suggestion chips live in `WELCOME_CHIPS`.

### Backend contract

The app sends a `POST` request to `WORKER_URL` with JSON:

```json
{ "message": "<latest user message, including any attached file text>", "systemPrompt": "<prompt for the current mode>" }
```

It accepts a reply that is either a plain string or JSON with a `text` or `message` field. Error responses may include `message` or `error`.

## Saved in the browser (localStorage)

- `aetheron-theme`: `light` or `dark`
- `aetheron-voice`: `on` or `off`

If you rename the app, you may also want to rename these keys.

## Keyboard

- **Enter** sends the message on screens 860px or wider. **Shift+Enter** adds a new line. On phones, Enter adds a new line and the send button sends.
- **Esc** closes the sidebar on small screens.

## Things to know

- **No conversation memory is sent.** The `messages` array is kept in the page, but only the latest message goes to the backend, so the AI does not see earlier turns. To add memory, send the history from `messages` in the request and handle it in the backend.
- **The ElevenLabs key is exposed** to anyone who views the page source if you fill it in. Prefer proxying it through your backend.
- The free voice uses a third-party service (StreamElements), so it can be slow or unavailable. The device voice is the fallback.
- Binary files (images, PDFs) are not read. Only their names are sent.
- The file includes a `google-site-verification` meta tag. Remove or replace it if you host the page under a different site.

## File layout

Everything is inside `index11.html`:

1. `<style>`: theme variables, layout, chat, composer, sidebar, animations
2. HTML: sidebar, top bar, chat window, input bar
3. `<script>`: configuration, theme and background particles, modes and welcome screen, system prompts, file handling, voice input and output, typewriter, markdown renderer, send and retry logic, export

## Credits

Built by Muhammad Thameem KT.
