# Deploy no Render

Este é um guia para fazer deploy da aplicação Mang-site no Render.

## Pré-requisitos
- Conta no [Render.com](https://render.com)
- Repositório GitHub conectado à sua conta Render

## Passos para Deploy

### 1. Preparar Variáveis de Ambiente
Antes de fazer deploy, configure as variáveis de ambiente no painel do Render:

- `NODE_ENV`: `production`
- `PORT`: `3000`
- `SESSION_SECRET`: Gere uma string aleatória forte (recomendado: use um gerador online ou `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`)

### 2. Conectar ao Render

1. Acesse [dashboard.render.com](https://dashboard.render.com)
2. Clique em "New +" e selecione "Web Service"
3. Conecte seu repositório GitHub (brunopereirabatistabrunojonh-arch/Mang-site)
4. Configure:
   - **Name**: `manga-site` (ou seu preferido)
   - **Environment**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free (ou upgraded conforme necessário)

### 3. Adicionar Variáveis de Ambiente

No painel do Render, vá para "Environment" e adicione:

```
NODE_ENV=production
PORT=3000
SESSION_SECRET=<sua-string-aleatoria-aqui>
```

### 4. Deploy

O deploy acontecerá automaticamente após:
- Fazer push para o repositório
- Ou clicar em "Manual Deploy" no painel do Render

## Monitoramento

- Verifique os logs em "Logs" no dashboard do Render
- A URL da sua aplicação estará visível na página do serviço

## Observações Importantes

### Uploads de Arquivo
Atualmente, os uploads são salvos localmente em `public/uploads/`. 

⚠️ **IMPORTANTE**: No Render (e outras plataformas cloud), o sistema de arquivos é **ephemeral** (temporário). Arquivos salvos serão perdidos quando:
- A aplicação é reiniciada
- Um novo deploy é feito
- A instância é atualizada

**Solução recomendada**: Integrar um serviço de armazenamento em nuvem como:
- AWS S3
- Cloudinary
- Google Cloud Storage
- DigitalOcean Spaces

### Banco de Dados
Se o banco SQLite (`better-sqlite3`) for usado, ele será perdido nos reinícios.

**Solução recomendada**: Migrar para:
- PostgreSQL (com Render Database)
- MongoDB
- MySQL

## Troubleshooting

### Erro: "Cannot find module"
- Execute `npm install` localmente
- Verifique se o `package.json` tem todas as dependências

### Aplicação não inicia
- Verifique os logs no Render dashboard
- Confirme que `SESSION_SECRET` está configurado

### Uploads não aparecem após restart
- Este é comportamento esperado no Render (ephemeral filesystem)
- Considere usar um serviço de armazenamento em nuvem

## Próximos Passos

1. **Adicionar HTTPS**: Render fornece certificado SSL automático
2. **Custom Domain**: Configure um domínio personalizado nas configurações
3. **Backup de Dados**: Implemente backup automático do banco de dados
4. **Monitoramento**: Configure alertas para uptime e erros

---

Dúvidas? Consulte a [documentação do Render](https://render.com/docs)
