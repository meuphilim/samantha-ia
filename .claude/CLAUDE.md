# Samantha — Project Context

Projeto: Assistente pessoal de IA baseada em llamafile (Mozilla AI).

## Estrutura

- `llama.cpp/tools/ui/` — Interface web (Svelte 5 + Vite 7 + Tailwind 4)
- `llama.cpp/` — Engine C/C++ (submódulo do upstream llama.cpp)
- `llamafile/` — Llamafile runtime (Cosmopolitan Libc)

## Customizações Feitas

- Tema completo Samantha-IA em `llama.cpp/tools/ui/src/app.css`
- Aba "Sobre a Samantha" em settings (`SettingsChatAboutTab.svelte`)
- Sidebar com logo sakura + branding
- Greeting personalizado com sakura.webp
- Favicon package (ico, png, apple-touch)
- Interface em Português Brasileiro

## Dev Commands

```bash
cd llama.cpp/tools/ui
npm install
NODE_OPTIONS="--insecure-http-parser" npx vite dev --host 0.0.0.0
```

## Assets

- Imagens em `llama.cpp/tools/ui/static/` (servidas em `/` via SvelteKit)
- sakura.webp em `/sakura.webp`
- Favicons em `/favicon.ico`, `/favicon-32x32.png`, etc.

## Stack

| Camada | Tecnologia |
|--------|-----------|
| Framework | Svelte 5 (runes) |
| Build | Vite 7 |
| CSS | Tailwind CSS 4 (oklch) |
| Ícones | lucide-svelte |
| Engine | llamafile / llama.cpp |

## Links

- Upstream: https://github.com/mozilla-ai/llamafile
- Engine: https://github.com/ggml-org/llama.cpp
