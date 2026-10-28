# localmind

**A browser-based AI workspace that runs text models on your device.** Choose a supported model, chat, work with text files, make a simple chart, generate an experimental image, and create ZIP files. No AI inference server is used by this project.

> **Early prototype:** model and browser compatibility varies. Local inference can be slow, and AI responses may be wrong. Read the privacy and model notes below before using it.

## Features

- In-browser text generation with [WebLLM](https://github.com/mlc-ai/web-llm) and [Transformers.js](https://github.com/huggingface/transformers.js).
- Model picker grouped by approximate size: Small, Medium, Large, and less restricted community fine-tunes.
- Streaming WebLLM responses and worker-based model execution to keep the interface more responsive.
- Saved conversations and username in browser local storage.
- LocalMind Help answers app usage questions without loading a model.
- New chat commands: enter “new chat: your message” or “create a new chat and ask AI to …” to start a fresh chat and send the remaining prompt.
- JSON chat backup and restore.
- Text file attachments, simple numeric chart previews, ZIP creation, and experimental image generation using a separate SD-Turbo WebGPU model.
- Responsive layout with a mobile navigation drawer.

## Run locally

The page uses JavaScript modules and Web Workers. Open it from a local web server instead of double-clicking `index.html` (which gives it a `file://` URL and blocks workers in many browsers).

1. Download or clone this repository.
2. Start a static web server in the project directory. For example, with Python:

   ```sh
   python -m http.server 8000
   ```

3. Open [http://localhost:8000](http://localhost:8000) in a supported browser.
4. Choose a model and select the download arrow. The first download may take several minutes. Keep the tab open until loading finishes.

You can also use a static host that serves the files over HTTPS. No backend server is required by the app; a static host only serves the page and its assets.

## Models and device requirements

Model availability is limited to the IDs registered by WebLLM’s prebuilt configuration, plus the Transformers.js model listed in the picker. The community fine-tune group includes Hermes, OpenHermes, and NeuralHermes variants. These are often described as less restricted, but behavior depends on the specific model and prompt; “uncensored” is not a guarantee.

Small models are the best starting point for limited hardware. Medium and especially Large models require considerably more GPU memory and storage. The model itself may download hundreds of megabytes or several gigabytes. A compatible browser with WebGPU support is required for text and image models; some older phones and computers will not work. Loading or generating on unsupported hardware may fail. The Transformers.js option also uses WebGPU in a worker to avoid blocking the page.

Image generation uses a separate SD-Turbo model via the worker-first web-txt2img library; it does not use or depend on the selected text LLM. It downloads its own model assets and needs WebGPU. Image generation is experimental and requires additional GPU memory. See [web-txt2img](https://github.com/lacerbi/web-txt2img) for the image runtime. ZIP creation and JSON backup happen locally in the browser.

## Privacy and storage

- Prompts are sent to the local model running in the browser; the app does not send them to an AI inference API.
- The app downloads its JavaScript libraries and model weights from external providers (including esm.run and Hugging Face) when needed. Those providers receive ordinary asset requests; this page does not upload chat prompts to them for inference.
- Chat history and the username are saved in this browser’s `localStorage`. Model files are cached by the browser. The in-app **Clear cache & chats** button removes saved conversations and this app’s known WebLLM, Transformers.js, and image-model caches after confirmation. **Data is stored locally**, so clearing site data can remove chats, the username, and cached model files.
- Use **Backup chats** to download a JSON backup and **Restore** to import it later. Keep the backup somewhere safe; it is not included in model downloads.
- This app does not include a server-side account or cross-device chat sync.

## Project files

```text
index.html   The standalone app (HTML, CSS, and JavaScript)
README.md    This guide
```

## Included assets

- index.html — the interactive app (HTML, CSS, and JavaScript)
- README.md — this guide
- favicon.svg — scalable browser tab icon
- app-icon.svg — scalable home screen/app icon
- manifest.webmanifest — app name, install display mode, and icon metadata
- LocalMind-preview.svg — static vector illustration of the app UI; it is not an executable replacement for index.html

## Troubleshooting

- **“WebGPU unavailable”** — update your browser and graphics drivers, then check that the device/browser supports WebGPU. Try a smaller model.
- **Page says `file://`** — start a local static web server as shown above and use `http://localhost:8000`.
- **Model download fails** — check your connection, available storage, browser cache, and whether the selected model fits your device. Some networks block model hosting sites.
- **Generation is slow** — try a Small model and a shorter prompt. Model size and device GPU have a large effect on speed.
- **Chats disappeared** — restore the JSON backup if you exported one. Browser local storage cannot recover chats after it has been cleared.

## License

No project license is included yet. Before publishing, choose and add a license that fits your project. The model weights and third-party libraries have their own licenses and terms; check each model’s model card and each library’s license before redistribution or commercial use.
