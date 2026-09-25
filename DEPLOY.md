# Publicação do sistema

O projeto está pronto para ser conectado a uma conta Cloudflare, mas a criação do banco remoto e o deploy precisam acontecer na conta do proprietário do projeto.

## 1. Criar o banco

`npx wrangler login`

`npx wrangler d1 create clube-up-mobility`

Copie o `database_id` para `wrangler.toml` no lugar de `COLOQUE_O_DATABASE_ID_AQUI`.

## 2. Criar segredo de setup

`npx wrangler secret put SETUP_KEY`

Use uma chave aleatória e guarde-a apenas no Cloudflare.

Também defina um segredo para autenticação:

`npx wrangler secret put AUTH_SECRET`

## 3. Aplicar o banco

`npm run db:migrate:remote`

## 4. Fazer o primeiro setup

Depois de publicar o Worker, faça uma requisição POST para `/api/setup` com o header `x-setup-key` igual ao valor do segredo `SETUP_KEY`.

O setup cria contas de demonstração no banco:

- Parceiro: `parceiro@demo.up` / `123456`
- Motorista: `motorista@demo.up` / `123456`
- Admin: `admin@upmobility.com.br` / `TroqueEstaSenha!`

Troque a senha do administrador antes de uso real.

## 5. Publicar

`npm run deploy`

## Segurança

O frontend nunca recebe o `AUTH_SECRET` nem o `SETUP_KEY`. Senhas são armazenadas com PBKDF2 + salt no Worker. A autorização de parceiro/admin é verificada no backend, não apenas no JavaScript do navegador.
