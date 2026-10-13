# Deploy: Vercel (loja) + Render (API)

O frontend fica hospedado no Vercel. O Render executa apenas a API de checkout; o `vercel.json` encaminha `/api/*` para o servico Render `carol-presentes-api`.

## 1. Publicar a loja no Vercel

1. Envie este repositorio para um repositorio GitHub.
2. No Vercel, importe o repositorio e mantenha o **Root Directory** na raiz.
3. Nao e necessario framework, build command ou output directory; o Vercel publica `index.html` e seus arquivos estaticos.
4. Faça o deploy e copie a URL HTTPS final `.vercel.app`.

## 2. Publicar a API no Render

1. No Render, escolha **New + > Blueprint** e conecte o mesmo repositorio. O Render usara `render.yaml` para criar o servico `carol-presentes-api`.
2. Informe as variaveis solicitadas:
   - `MP_ACCESS_TOKEN`: Access Token da aplicacao Mercado Pago (nao use a Public Key).
   - `APP_URL`: URL HTTPS final da loja no Vercel, copiada no passo anterior.
3. Aguarde o deploy e confirme que `https://carol-presentes-api.onrender.com/healthz` retorna `{"status":"ok"}`.
4. O `vercel.json` encaminha as chamadas `/api/*` para o servico Render. Teste o carrinho e o checkout na URL do Vercel.

`MP_NOTIFICATION_URL` e opcional e so deve ser configurado se houver um webhook HTTPS publico implementado.

## Segredos e manutencao

- Nunca envie `.env` para o GitHub. As credenciais devem ficar apenas nas variaveis de ambiente do Render.
- Se um Access Token real ja foi compartilhado, enviado ao Git ou exposto em logs, revogue-o no painel do Mercado Pago e gere outro antes do deploy.
- O plano gratuito do Render pode suspender o servico por inatividade; a primeira chamada da API pode demorar enquanto ele inicia.
- Cada produto e preco tambem esta cadastrado no backend. Mantenha `server.js` e `js/app.js` sincronizados ao atualizar o catalogo.
