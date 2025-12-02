# Correção do Deploy no Vercel

## Problema Identificado

O projeto está sendo detectado incorretamente como **Next.js** no Vercel, mas na verdade é um projeto **Vite/React**.

## Solução: Configuração Manual no Painel do Vercel

Como o projeto foi criado manualmente, você precisa configurar manualmente no painel do Vercel:

### Passos para Corrigir:

1. **Acesse o painel do Vercel**: https://vercel.com/atendimentoliminars-projects/excalidraw-2

2. **Vá em Settings > General**

3. **Configure as seguintes opções**:

   - **Framework Preset**: Selecione `Other` ou `Static Site` (NÃO Next.js)
   - **Root Directory**: `.` (raiz do projeto)
   - **Build Command**: `yarn build:packages && yarn build`
   - **Output Directory**: `excalidraw-app/build`
   - **Install Command**: `yarn install`
   - **Node.js Version**: `18.x`

4. **Salve as configurações**

5. **Faça um novo deploy** (Redeploy ou push novo commit)

## Arquivos de Configuração

O `vercel.json` já está configurado corretamente com:
- `framework: null` - para evitar detecção automática
- `buildCommand` correto
- `outputDirectory` correto
- `nodeVersion: 18.x`

## Verificação

Após configurar, o deploy deve funcionar corretamente. O erro "No Next.js version detected" não deve mais aparecer.

