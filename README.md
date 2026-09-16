# Haven

**Private AI. Your machine. Zero cloud.**

Haven is a single-file chatbot that runs a large language model **entirely in your browser** using [WebLLM](https://github.com/mlc-ai/web-llm) and WebGPU. There is no server, no account, and no telemetry. After the model downloads once, chats stay on this device — including when you go offline.

Use it as your own unlimited local assistant. Fork it. Host it. Give it to someone who wants AI without handing their words to a company.

## Why this exists

Cloud chatbots are convenient and leaky. Every prompt is someone else's training data, policy surface, and invoice.

Haven is the opposite:

- **Private** — inference happens on your GPU. Conversations are stored in IndexedDB in this browser only.
- **Unlimited** — no tokens, no rate limits, no subscription. You pay in electricity and patience.
- **Yours** — one HTML file. Change the name, the models, the system prompt. Ship it to a USB stick if you want.

## Quick start

### Option A — open the file

1. Download [`index.html`](./index.html)
2. Open it in **Chrome, Edge, or another Chromium browser** with WebGPU enabled
3. Wait for the first model download (~1.2 GB for Llama 3.2 1B)
4. Chat

Firefox WebGPU support is improving but Chromium is still the reliable path.

### Option B — serve it locally

Some browsers treat `file://` more strictly. From this folder:

```bash
python3 -m http.server 8080
```

Then visit [http://localhost:8080](http://localhost:8080).

### Option C — GitHub Pages

1. Fork this repo
2. Settings → Pages → Deploy from branch → `main` / `/ (root)`
3. Open `https://<you>.github.io/haven-chat/`

The model still downloads to **each visitor's machine**. Your Pages site is only the UI. You never see their chats.

## What you need

| Requirement | Notes |
|---|---|
| WebGPU | Chrome / Edge 113+, or recent Chromium |
| Disk | ~1–3 GB cached model weights |
| RAM / VRAM | 1B models are comfortable on most laptops. 3B+ wants a discrete GPU or a recent Apple Silicon Mac |
| Network | Only for the first model download (and to load the WebLLM library). After that, offline works |

## Included models

Pick one from the header. Smaller is faster and uses less memory.

| Model | Approx. download | Best for |
|---|---|---|
| Llama 3.2 1B Instruct | ~1.2 GB | Everyday chat, weak GPUs |
| Qwen2.5 1.5B Instruct | ~1.0 GB | Compact + multilingual |
| Llama 3.2 3B Instruct | ~2.0 GB | Better reasoning if you have the VRAM |
| Phi 3.5 Mini Instruct | ~2.4 GB | Strong small generalist |

Model IDs come from WebLLM's prebuilt list. If a model fails to load, the UI shows the error — pick a smaller one.

## Features

- Streaming replies
- Multiple conversations, named from the first message
- Delete / export a thread
- Optional system prompt and temperature
- Stop generation
- Everything persisted locally (IndexedDB)

Nothing is uploaded. There is no backend in this repo.

## Make it yours

All of the product lives in `index.html`.

- **Brand** — search for `Haven` and the tagline
- **Default model** — `DEFAULT_MODEL` near the top of the script
- **Model list** — `MODELS`
- **Colors** — the `:root` CSS variables
- **System prompt** — Settings panel, or hardcode `DEFAULT_SYSTEM_PROMPT`

That is the point. This is a template for *your* private AI, not a SaaS.

## Privacy model

| Data | Where it goes |
|---|---|
| Prompts & replies | IndexedDB in this origin only |
| Model weights | Browser cache / Cache Storage |
| Analytics | None |
| Account | None |

Clearing site data for this origin deletes chats and may re-download the model.

**Caveat:** the first load fetches WebLLM from a CDN (`esm.run`) and model weights from the MLC / Hugging Face mirrors WebLLM uses. After that, inference is local. If you need air-gapped use, vendor the library and host the weights yourself via WebLLM's custom `appConfig`.

## Credits

- Inference: [MLC-AI WebLLM](https://github.com/mlc-ai/web-llm)
- Models: Meta Llama, Microsoft Phi, Alibaba Qwen — used under their respective licenses. Check those licenses before commercial redistribution of weights.

## License

MIT for the Haven UI and glue code in this repository. Model weights are **not** MIT. They remain under their original licenses.
