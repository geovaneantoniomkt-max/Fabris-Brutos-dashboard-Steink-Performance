# Fabris Brutos — dashboard, estado atual

Dashboard de performance da **Fabris Brutos · Fábrica de Semijoias** (atacado, Limeira/SP), feito a
partir do modelo em `InVet Center`. O passo a passo genérico, com os erros conhecidos, está em
[`PLAYBOOK-NOVO-CLIENTE.md`](PLAYBOOK-NOVO-CLIENTE.md).

## Já está pronto

- **Google Ads coletado e funcionando.** Conta `100-734-5174` (*Fabris Brutos*), via MCC
  `295-287-1856`. Série diária de **16/05/2024 a 17/08/2025**: 2 campanhas de pesquisa,
  R$ 7.754 investidos e 1.657 leads de formulário.
- **Metas tiradas do histórico real**: R$ 4,00 por lead — mediana mensal de R$ 3,97 nos 13 meses
  com conversão medida (melhor mês R$ 2,00, pior R$ 12,27). Verba Google R$ 500/mês. A origem de
  cada número está em `_comentario_metas`, dentro de `public/config.json`.
- **Meta Ads zerado de propósito**, igual à InVet: `scripts/meta_zerado.py` gera `meta.json` e
  `organic.json` no formato do coletor real, tudo em zero.
- **Vocabulário adaptado**: o resultado é **contato/lead** de lojista ou revendedor, vindo do
  formulário do site (categoria `SUBMIT_LEAD_FORM` no Google Ads, ação "Enviar formulário de lead").
- **Receita e ROAS desligados** (`display.revenue = false`): o valor da conversão é o peso do
  formulário na conta, não faturamento.
- **Camada de senha**: 32/32 testes passando. Senha e chave de sessão em `SENHA-LOCAL.txt`.

## ⚠️ Dois achados na conta do Google Ads

1. **O rastreamento do formulário parou em jun/2025.** Junho a agosto/2025 tiveram R$ 1.330 de
   gasto e **zero** conversões. Vale conferir a tag de conversão no site antes de reativar.
2. **A conta não gasta nada desde 17/08/2025**, apesar da campanha principal estar ATIVA —
   provavelmente pagamento/faturamento. Por isso os atalhos "últimos 30/90 dias" aparecem zerados;
   use "Desde o início" para ver o histórico.

## Falta fazer

1. **Credenciais do Google localmente**: copie o `.env.google` de outro cliente (InVet/Nohotel)
   para esta pasta e troque `GOOGLE_ADS_CUSTOMER_ID=100-734-5174`. Depois `python scripts/segredos_github.py`.
2. ~~Trocar a logo~~ — feito: monograma "FB" dourado em `public/logo.jpg`.
3. **Criar o site no Netlify**: `npm run bundle` e arraste a pasta **`deploy-netlify`**
   (não a `public/`, senão o site sobe sem senha).
4. **Cadastrar a senha** no Netlify (*Environment variables*, All scopes, **sem** "Contains secret
   values"): `DASHBOARD_PASSWORD` e `SESSION_SECRET`, de `SENHA-LOCAL.txt`. Publique de novo depois.
5. **Secrets no GitHub** (*Settings → Secrets and variables → Actions*): os 4 do Google
   (`GOOGLE_ADS_DEVELOPER_TOKEN`, `GOOGLE_ADS_CLIENT_ID`, `GOOGLE_ADS_CLIENT_SECRET`,
   `GOOGLE_ADS_REFRESH_TOKEN`), `META_ADS_TOKEN`, `NETLIFY_AUTH_TOKEN` e `NETLIFY_SITE_ID`.
6. **Rodar o workflow** em *Actions → Atualizar dashboard → Run workflow*.

## Quando o Meta Ads entrar

O token do Meta ainda **não enxerga** nenhuma conta da Fabris Brutos. Dê acesso ao Usuário do
Sistema (conta de anúncios, página e Instagram — playbook §4), cadastre a **variável**
`META_AD_ACCOUNT_ID` no repositório (e `META_PAGE_ID`, `META_IG_USER_ID`) e o workflow passa a usar
`scripts/fetch_meta.py`. Defina uma meta própria de custo por lead para o Meta.

## Comandos

```bash
npm test                        # 32 verificações da camada de senha
python scripts/fetch_google.py  # recoleta o Google Ads (lê .env.google)
python scripts/meta_zerado.py   # regera meta.json e organic.json zerados
npm run bundle                  # monta deploy-netlify/
npm run dev                     # http://localhost:8790 (com a senha local)
```
