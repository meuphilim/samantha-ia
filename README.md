# 🌸 Samantha — Assistente Pessoal de IA

**Samantha** é uma assistente pessoal de inteligência artificial baseada em **[llamafile](https://github.com/mozilla-ai/llamafile)** (Mozilla AI), projetada para oferecer uma experiência **100% local**, privada e eficiente, com interface em **Português Brasileiro**.

![sakura](llama.cpp/tools/ui/static/sakura.webp)

> "Toda grande ideia começa com uma conversa. Algumas delas continuam evoluindo linha por linha de código."

---

## ✨ Visão Geral

| Característica | Detalhe |
|----------------|---------|
| **Engine** | [llamafile](https://github.com/mozilla-ai/llamafile) v0.10.3 |
| **Modelo** | Qwen3.5-4B (GGUF Q4_K_S) |
| **Runtime** | Llamafile (Cosmopolitan Libc) |
| **Interface** | Svelte 5 + Vite 7 + Tailwind CSS 4 |
| **Idioma** | Português Brasileiro (pt-BR) |
| **Licença** | Apache 2.0 |

### Filosofia

Samantha foi concebida com uma filosofia **Local First** — seus dados permanecem sob seu controle com processamento local, sem dependências de nuvem. O llamafile empacota modelo, runtime e infraestrutura em um único executável portátil.

---

## 🎨 Personalizações

### Tema Visual (Samantha-IA Brand)

| Token | Valor |
|-------|-------|
| `--samantha-primary` | `oklch(0.52 0.22 264)` — Índigo |
| `--samantha-accent` | `oklch(0.62 0.17 164)` — Esmeralda |
| `--samantha-gradient` | Índigo → Violeta (135°) |

- Paleta completa em **oklch** com matiz 264 (índigo)
- **Modo escuro** com deep dark azul-acinzentado
- **Scrollbars** visíveis apenas no hover, com cor derivada da marca
- **Fontes**: Inter (sans), JetBrains Mono (mono)
- Classes utilitárias: `.samantha-glow`, `.samantha-gradient`, `.samantha-text-gradient`

### Interface

| Componente | Customização |
|------------|-------------|
| **Sidebar** | Logo sakura + "Samantha" + "Assistente Pessoal" |
| **Greeting** | Sakura.webp com glow, saudação em português |
| **Settings** | Nova aba "Sobre a Samantha" com hero, métricas, capacidades, roadmap, créditos |
| **Favicon** | Pacote completo (ico, png 16/32, apple-touch-icon) |
| **Idioma** | Interface completa em Português Brasileiro |

### Arquivos Modificados

```
llama.cpp/tools/ui/
├── src/
│   ├── app.css                                    → Tema completo (índigo, dark mode, brand tokens)
│   ├── app.html                                   → Favicon links atualizados
│   ├── lib/
│   │   ├── components/app/
│   │   │   ├── chat/ChatScreen/ChatScreenGreeting.svelte  → Saudação personalizada
│   │   │   ├── navigation/SidebarNavigation/              → Header com logo + nome
│   │   │   └── settings/
│   │   │       ├── SettingsChat/SettingsChatAboutTab.svelte → ★ NOVO — Aba "Sobre"
│   │   │       ├── SettingsChat/SettingsChat.svelte        → Render condicional
│   │   │       └── index.ts                                → Export do AboutTab
│   │   └── constants/
│   │       ├── routes.ts                                    → Slug ABOUT
│   │       ├── settings-registry.ts                         → Seção + pt-BR
│   │       └── ...                                          → Tradução pt-BR
│   └── routes/
│       └── +error.svelte                            → Página de erro traduzida
└── static/
    ├── sakura.webp                                  → ★ NOVO — Logo da marca
    ├── favicon.ico                                  → ★ NOVO
    ├── favicon-16x16.png                            → ★ NOVO
    ├── favicon-32x32.png                            → ★ NOVO
    └── apple-touch-icon.png                         → ★ NOVO
```

---

## 🚀 Como Usar

### Pré-requisitos

- Node.js 20+
- npm ou pnpm
- cosmocc (Cosmopolitan Libc toolchain) — baixado automaticamente por `make setup`

### Compilação Completa (llamafile + UI customizada)

Ordem correta para compilar o binário completo com assets customizados:

```bash
# 1. Baixar toolchain cosmocc + aplicar patches da engine
make setup

# 2. Construir a UI com assets customizados (sakura.webp, favicons, pt-BR)
cd llama.cpp/tools/ui
npm install
npm run build
cd ../..

# 3. Compilar o llamafile
make -j$(nproc)

# 4. Executar
./llamafile -m modelo.gguf --server --host 0.0.0.0
```

> **Nota sobre patches:** `make setup` aplica patches da engine ao submódulo `llama.cpp`
> (ou manualmente: `bash llama.cpp.patches/apply-patches.sh`).
> Essas modificações NÃO devem ser commitadas no fork — a fonte de verdade está em
> `llama.cpp.patches/`. Para restaurar o estado limpo do submódulo:
> `git -C llama.cpp reset --hard && git -C llama.cpp clean -fdx`

### Desenvolvimento (UI com hot reload)

```bash
cd llama.cpp/tools/ui
npm install
NODE_OPTIONS="--insecure-http-parser" npx vite dev --host 0.0.0.0
```

Acesse `http://localhost:5173/` — o Vite faz proxy para `localhost:8080` (llama-server).
A UI compilada estática fica em `llama.cpp/tools/ui/dist/` e é servida pelo llamafile em
`http://localhost:8080/`.

# 3. Compilar o llamafile (cosmocc — cross-compiler)
./configure
make -j$(nproc)

# 4. Executar
./llamafile -m modelo.gguf --server --host 0.0.0.0
```

> **Nota:** O passo 1 (`apply-patches.sh`) modifica o submódulo `llama.cpp` com patches
> da engine. Essas modificações NÃO devem ser commitadas — a fonte de verdade
> está em `llama.cpp.patches/`. Use `git -C llama.cpp reset --hard && git -C llama.cpp clean -fdx`
> para restaurar o estado limpo do submódulo.

### Build apenas da UI (desenvolvimento com hot reload)

```bash
cd llama.cpp/tools/ui
npm install
NODE_OPTIONS="--insecure-http-parser" npx vite dev --host 0.0.0.0
```

Acesse `http://localhost:5173/` — o Vite faz proxy para `localhost:8080` (llama-server).

---

## 🏗️ Estrutura do Projeto

```
D:\Samantha-Agent\
├── llama.cpp/                 # Engine principal (submódulo)
│   ├── tools/ui/              # ★ Interface web personalizada (Svelte)
│   ├── src/                   # Engine C/C++
│   ├── common/                # Código compartilhado
│   ├── include/               # Headers
│   ├── ggml/                  # GGML tensor library
│   └── models/                # Modelos GGUF
├── llamafile/                 # Llamafile runtime (Cosmopolitan)
├── whisper.cpp/               # Reconhecimento de fala
├── stable-diffusion.cpp/      # Geração de imagens
├── tools/                     # Ferramentas auxiliares
├── docs/                      # Documentação técnica
└── third_party/               # Dependências
```

---

## 🧠 Capacidades

| Capacidade | Descrição |
|------------|-----------|
| 💬 **Conversação Natural** | Contexto, intenções e histórico |
| 💻 **Desenvolvimento** | React, Node.js, Python, PostgreSQL, Docker |
| 📄 **Produção de Conteúdo** | E-mails, contratos, propostas |
| 🎓 **Apoio Educacional** | Simplificação de conceitos técnicos |
| 🔒 **Privacidade** | 100% offline |
| ⚡ **Performance** | Modelo Q4_K_S otimizado |

---

## 🗺️ Roadmap

- [ ] Memória persistente (Cross-session)
- [ ] Integração com Obsidian
- [ ] Integração com WhatsApp
- [ ] Agentes especializados em background
- [ ] Controle de dispositivos locais
- [ ] RAG Local com embeddings
- [ ] Interface desktop dedicada

---

## 👥 Equipe

| Papel | Nome |
|-------|------|
| **Fundador e Desenvolvedor** | Celso (Meuphilim) |
| **Co-worker de IA** | Samantha (ChatGPT/Claude) |

---

## 📜 Licença

Este projeto é um fork personalizado do **[llamafile](https://github.com/mozilla-ai/llamafile)** da Mozilla AI, licenciado sob [Apache 2.0](./LICENSE).

> **Nota:** As modificações no `llama.cpp` (engine) mantêm a licença MIT original para compatibilidade upstream.  
> O README original do llamafile está disponível em [`README_LLAMAFILE.md`](./README_LLAMAFILE.md).

Copyright 2024-2026 — Celso (Meuphilim)  
Copyright 2023 — Mozilla Foundation
