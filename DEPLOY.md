# 🚀 Guia de Publicação do MangáHub

## Publicando no Render.com (Recomendado)

### Passo 1: Conectar GitHub ao Render

1. Acesse [render.com](https://render.com)
2. Faça login/crie uma conta
3. Clique em **"Connect GitHub"** e autorize a aplicação
4. Selecione este repositório

### Passo 2: Criar Web Service

1. Clique em **"New +"** → **"Web Service"**
2. Selecione o repositório `Mang-site`
3. Configure:
   - **Name**: `manga-site` (ou o nome que quiser)
   - **Runtime**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free (Grátis)

### Passo 3: Adicionar Variáveis de Ambiente

1. Em **"Environment"**, clique em **"Add Environment Variable"**
2. Adicione:
   - **Key**: `SESSION_SECRET`
   - **Value**: Gere um valor aleatório (ex: `openssl rand -hex 32` no terminal)
   
3. Adicione mais uma:
   - **Key**: `NODE_ENV`
   - **Value**: `production`

### Passo 4: Adicionar Disco Persistente (IMPORTANTE!)

**Sem isso, o banco de dados e as imagens serão perdidas!**

1. Em **"Disk"**, clique em **"Add Disk"**
2. Configure:
   - **Name**: `manga-data`
   - **Mount Path**: `/opt/render/project/src/data`
   - **Size**: 1 GB

3. Clique em **"Add Disk"** novamente para as uploads:
   - **Name**: `manga-uploads`
   - **Mount Path**: `/opt/render/project/src/public/uploads`
   - **Size**: 2-5 GB (conforme necessário)

### Passo 5: Deploy

1. Clique em **"Create Web Service"**
2. Render irá:
   - Clonar seu repositório
   - Executar `npm install`
   - Iniciar `npm start`
3. Aguarde 2-3 minutos até aparecer ✅ "Your service is live"

### Passo 6: Acessar seu site

- URL padrão: `https://manga-site.onrender.com`
- Você verá na dashboard do Render

---

## ✅ Checklist Antes de Publicar

- [ ] `.gitignore` criado (para não subir `node_modules` e banco de dados)
- [ ] `package.json` com todas as dependências
- [ ] `SESSION_SECRET` gerado (variável de ambiente)
- [ ] Discos persistentes configurados
- [ ] Teste local: `npm start` funciona em `http://localhost:3000`

---

## 🛠️ Troubleshooting

### Site fica offline/reinicia?
- Geralmente é por falta de disco persistente
- Verifique se os discos foram criados na aba "Disks"

### Banco de dados vazio após redeploy?
- Significa que o disco de dados não está persistindo
- Recrie o disco com o caminho correto

### Senha/SESSION_SECRET exposta?
- Use sempre variáveis de ambiente (não hardcode no código)
- Regenere `SESSION_SECRET` depois de publicar

### Imagens não aparecem?
- Verifique o caminho das uploads: `/public/uploads/`
- Garanta que o disco de uploads está montado

---

## 💡 Próximos Passos

### Domínio Próprio
Se quiser `seusite.com` ao invés de `manga-site.onrender.com`:
1. Compre domínio em Registro.br, Namecheap, etc.
2. No Render, vá para **"Settings"** → **"Custom Domain"**
3. Siga as instruções para apontar o DNS

### Backups
- Baixe o arquivo `data/manga.db` periodicamente
- Render permite acessar os arquivos via SSH (plano pago)

### Escalabilidade
Se o site crescer muito:
- Migre imagens para Cloudflare R2 ou AWS S3
- Considere plano pago do Render para melhor performance

---

**Pronto! Seu MangáHub estará no ar em minutos! 🎉**
