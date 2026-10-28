# Indicadores Frota Legal — CEVA Logistics

Painel web (GitHub Pages) que substitui o `Ceva_dash.xlsx`. Um painel único com visão **Semana / Mês / Período**, filtros de tipo de veículo, transportador e checklist.

## Estrutura

```
index.html                         painel (contém a base fixa de placas e condutores)
dados/prontuario-equipamentos.xlsx export do Frota Legal — é o ÚNICO arquivo trocado toda semana
dados/historico_inspecoes.csv      histórico acumulado (gerado sozinho, não mexer)
scripts/atualizar_historico.py     script usado pela automação
.github/workflows/historico.yml    automação que acumula o histórico a cada upload
```

## Atualização semanal (≈ 2 min)

1. No Frota Legal (ambiente CEVA), exporte o **Prontuário de Veículos/Equipamentos** com situação **Ativo**.
2. Confira se o arquivo se chama exatamente `prontuario-equipamentos.xlsx` (renomeie se vier com “(1)” ou outro sufixo).
3. No GitHub, abra a pasta **dados** → **Add file → Upload files** → arraste o arquivo → **Commit changes**.
4. Em ~1–2 minutos o site já mostra os dados novos (atualize a página com Ctrl+F5 se precisar).

**Não é preciso filtrar nem apagar nada no Excel.** O painel já:
- ignora a inspeção de fumaça escura (considera só Inspeção Condutor e Inspeção de Segurança);
- abre na última semana completa (a semana em andamento aparece marcada como parcial);
- desconsidera placas fora da base fixa e agrupa responsáveis fora da lista de condutores em “Outros”.

## Quando é preciso gerar um index.html novo

Só quando mudar a **base fixa de placas/condutores** (BASE_SEMANAL_CEVA.xlsx) ou algo no layout/indicadores do painel. Nesse caso, substitua o `index.html` da raiz pelo novo.

## Histórico além de 60 dias

O prontuário só traz os últimos 60 dias. A cada upload, a automação (aba **Actions**) guarda as inspeções em `dados/historico_inspecoes.csv`, então semanas e meses antigos continuam no painel. Se a automação falhar, o painel continua funcionando com os 60 dias do arquivo.

## Primeira publicação

1. Crie o repositório (ex.: `painel-frotalegal-ceva`) e envie todos os arquivos desta pasta.
   - A pasta `.github` às vezes não vai pelo arrastar-e-soltar. Se não aparecer, use **Add file → Create new file**, digite o nome `.github/workflows/historico.yml` e cole o conteúdo do arquivo.
2. **Settings → Pages →** Source: *Deploy from a branch* · Branch: `main` / `(root)` → Save.
3. O endereço fica `https://<usuário>.github.io/painel-frotalegal-ceva/`.

## Dicas

- `?admin=1` no fim do endereço mostra as **notas técnicas** (placas inspecionadas fora da base, placas sem cadastro, duplicidades da base, responsáveis fora da lista).
- **Carregar arquivo local** (rodapé) pré-visualiza um prontuário sem publicar.
- Repositório público = o arquivo em `dados/` pode ser baixado por quem tiver o link. Não divulgue o endereço fora do cliente.
