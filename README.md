# Clube UP Mobility — sistema real

Arquitetura: Cloudflare Workers + D1 + frontend HTML existente.

## O que este projeto entrega
- Banco D1 versionado por migrations.
- API protegida por sessão assinada no servidor.
- Login separado por função: membro, parceiro e admin.
- Cadastro de membro salvo no banco.
- Cadastro/gestão de parceiros e benefícios.
- Registro de utilização.
- Dashboard com métricas calculadas no banco.
- Dados do parceiro limitados à própria parceria.
- Área do membro.
- Área administrativa.

## Configuração
1. Instale Node.js.
2. Rode `npm install`.
3. Faça login no Cloudflare com Wrangler.
4. Crie o D1:
   `npx wrangler d1 create clube-up-mobility`
5. Copie o `database_id` retornado para `wrangler.toml`.
6. Aplique a migration:
   `npm run db:migrate:remote`
7. Defina `SETUP_KEY` como secret:
   `npx wrangler secret put SETUP_KEY`
8. Rode o setup uma única vez com POST para `/api/setup` usando o header `x-setup-key`.
9. Publique com `npm run deploy`.

Nunca coloque `SETUP_KEY` no HTML ou no Git.

## Observação
O HTML original foi preservado como base visual. A integração real usa `/api/*`. O arquivo atual ainda contém a camada antiga de demonstração; o Worker intercepta as ações reais e a próxima etapa é substituir progressivamente os dados demonstrativos da interface por chamadas à API. A autenticação e autorização reais ficam no servidor.
