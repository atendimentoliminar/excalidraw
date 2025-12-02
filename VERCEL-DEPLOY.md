# Guia de Deploy no Vercel

Este documento descreve como fazer o deploy do Excalidraw no Vercel.

## Pré-requisitos

- Conta no [Vercel](https://vercel.com)
- Repositório Git conectado (GitHub, GitLab ou Bitbucket)
- Node.js >= 18.0.0
- Yarn 1.22.22

## Configuração do Projeto

O projeto já está configurado com:

- ✅ `vercel.json` - Configuração do Vercel
- ✅ `.vercelignore` - Arquivos a ignorar no deploy
- ✅ Build command configurado para monorepo

## Variáveis de Ambiente

Configure as seguintes variáveis de ambiente no painel do Vercel (Settings > Environment Variables):

### Obrigatórias (para produção completa)

```bash
# Firebase Configuration (JSON string)
VITE_APP_FIREBASE_CONFIG={"apiKey":"...","authDomain":"...","projectId":"...","storageBucket":"...","messagingSenderId":"...","appId":"..."}

# Backend URLs
VITE_APP_BACKEND_V2_GET_URL=https://your-backend.com/api/v2
VITE_APP_BACKEND_V2_POST_URL=https://your-backend.com/api/v2

# WebSocket Server (para colaboração)
VITE_APP_WS_SERVER_URL=wss://your-ws-server.com

# Portal URL (para colaboração)
VITE_APP_PORTAL_URL=https://your-portal.com

# Excalidraw Plus URLs (opcional)
VITE_APP_PLUS_APP=https://plus.excalidraw.com
VITE_APP_PLUS_LP=https://plus.excalidraw.com
```

### Opcionais

```bash
# Sentry (para monitoramento de erros)
VITE_APP_DISABLE_SENTRY=false  # ou true para desabilitar

# Tracking
VITE_APP_ENABLE_TRACKING=true

# PWA
VITE_APP_ENABLE_PWA=true

# AI Backend (se usar recursos de IA)
VITE_APP_AI_BACKEND=https://your-ai-backend.com

# Git SHA (gerado automaticamente pelo Vercel)
# VITE_APP_GIT_SHA é preenchido automaticamente via $VERCEL_GIT_COMMIT_SHA
```

### Para Deploy Básico (sem colaboração)

Se você só quer fazer deploy básico sem recursos de colaboração, pode deixar as variáveis opcionais vazias ou usar valores padrão.

## Processo de Deploy

### 1. Conectar Repositório

1. Acesse [Vercel Dashboard](https://vercel.com/dashboard)
2. Clique em "Add New Project"
3. Importe seu repositório Git

### 2. Configurar Projeto

O Vercel detectará automaticamente:
- **Framework Preset**: Other
- **Root Directory**: `.` (raiz do projeto)
- **Build Command**: `yarn build:packages && yarn build`
- **Output Directory**: `excalidraw-app/build`
- **Install Command**: `yarn install`

### 3. Adicionar Variáveis de Ambiente

1. Vá em Settings > Environment Variables
2. Adicione as variáveis listadas acima
3. Selecione os ambientes (Production, Preview, Development)

### 4. Deploy

1. Clique em "Deploy"
2. O Vercel irá:
   - Instalar dependências (`yarn install`)
   - Buildar os pacotes (`yarn build:packages`)
   - Buildar a aplicação (`yarn build`)
   - Fazer deploy do diretório `excalidraw-app/build`

## Estrutura do Build

O processo de build:

1. **Instalação**: `yarn install` (instala todas as dependências do monorepo)
2. **Build dos Pacotes**: 
   - `@excalidraw/common`
   - `@excalidraw/math`
   - `@excalidraw/element`
   - `@excalidraw/excalidraw`
3. **Build da App**: Compila `excalidraw-app` usando Vite
4. **Output**: Arquivos estáticos em `excalidraw-app/build`

## Verificação Local

Antes de fazer deploy, você pode testar o build localmente:

```bash
# Instalar dependências
yarn install

# Buildar tudo
yarn build:packages && yarn build

# Verificar se o build foi criado
ls excalidraw-app/build
```

## Troubleshooting

### Erro: "Cannot find module @excalidraw/common"

**Solução**: Certifique-se de que o `buildCommand` inclui `yarn build:packages` antes de `yarn build`.

### Erro: "Build timeout"

**Solução**: O build pode demorar. Aumente o timeout no Vercel ou otimize o processo.

### Variáveis de ambiente não funcionam

**Solução**: 
- Verifique se as variáveis começam com `VITE_APP_`
- Certifique-se de que estão configuradas no ambiente correto (Production/Preview)
- Faça um novo deploy após adicionar variáveis

### Build funciona localmente mas falha no Vercel

**Solução**:
- Verifique a versão do Node.js (deve ser >= 18.0.0)
- Verifique se o Yarn está sendo usado (não npm)
- Verifique os logs de build no Vercel para erros específicos

## Recursos Adicionais

- [Documentação do Vercel](https://vercel.com/docs)
- [Vite Environment Variables](https://vitejs.dev/guide/env-and-mode.html)
- [Excalidraw Development Guide](https://docs.excalidraw.com/docs/introduction/development)

