# Flash Courier — Site institucional (versão de produção)

Front-end **100% estático**: HTML + CSS + JavaScript vanilla. Sem frameworks,
sem build, sem dependências de npm. Basta servir os arquivos em qualquer
hospedagem (Nginx, Apache, S3+CloudFront, Vercel, etc.).

## Estrutura

```
site/
├── index.html           Página principal (landing institucional)
├── investidores.html    Página de Relações com Investidores
├── assets/              Imagens, logos e vídeo do hero (~25 MB)
└── README.md            Este arquivo
```

Única dependência externa: **Google Fonts** (Inter / JetBrains Mono) via
`fonts.googleapis.com`. Para eliminar a dependência, baixe as fontes e sirva
localmente com `@font-face`.

## Pontos de integração (backend a implementar)

### 1. Rastreamento de objetos — `index.html`, seção `#track`
Hoje a consulta é **simulada** no front (qualquer código com formato válido
mostra uma jornada de demonstração). Integrar no script da seção de rastreio:

- Input: `#track-input` · Botão: `#track-go`
- A função de consulta está no bloco `<script>` marcado com
  `// ===== Tracking =====` — substituir a simulação por `fetch()` à sua API
  e popular os elementos `#track-result` / `#track-notfound` com a resposta.
- Estados de UI já prontos: loading (`#track-loading`), não-encontrado
  (`#track-notfound`) e resultado completo (`#track-result`).

### 2. Login — link `Login` na navegação
Aponta para `#track` (placeholder). Trocar pela URL do portal real.

### 3. Contato — footer `#contact`
Hoje é informativo (sem formulário). Se houver formulário, fazer POST à sua
API ou serviço de e-mail.

### 4. Página de RI — `investidores.html`
Conteúdo estático com dados de exemplo. Atualizar números/documentos
conforme o material oficial aprovado.

## Vídeo do hero

`assets/Flash_Modelo1_didatico_compacto.mp4` (~15 MB). Recomendações:
- Servir com suporte a **HTTP Range requests** (padrão em qualquer servidor
  estático — necessário para streaming do `<video>`).
- Opcional: gerar uma variante WebM/AV1 menor e usar `<source>` múltiplo.
- O vídeo é `muted autoplay loop playsinline` — não portar informação
  essencial nele (acessibilidade).

## Cabeçalhos recomendados (servidor)

```
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'unsafe-inline';
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  font-src https://fonts.gstatic.com;
  img-src 'self' data:;
  media-src 'self';
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Cache-Control: assets/* → public, max-age=31536000, immutable
```

> Nota: os scripts e estilos são inline no HTML (por isso `'unsafe-inline'`).
> Para um CSP mais estrito, extraia-os para arquivos `.js`/`.css` próprios ou
> use nonces — mudança puramente mecânica, sem reescrever lógica.

## O que NÃO existe neste pacote (de propósito)

- Nenhum código do ambiente de design/aprovação (postMessage, iframes, etc.)
- Nenhum framework ou dependência de build
- Nenhum dado real de rastreamento — a simulação é claramente demarcada no código
