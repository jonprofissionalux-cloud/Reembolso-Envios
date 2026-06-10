# Reembolso Garantido — Olist Envios

> **Sistema de gestão de reembolsos para lojistas e administradores**

[![GitHub](https://img.shields.io/badge/GitHub-Repo-black?logo=github)](https://github.com/jonprofissionalux-cloud/Reembolso-Envios)
[![Pages](https://img.shields.io/badge/GitHub%20Pages-Live-blue)](https://jonprofissionalux-cloud.github.io/Reembolso-Envios/)
[![Figma](https://img.shields.io/badge/Figma-Design-F24E1E?logo=figma)](https://www.figma.com/design/UNY8LMXVndJBSirtTuTGbP/Reembolso-Garantido)

---

## Quick Links

| Link | Descrição |
|------|-----------|
| **Local** | `/Users/jonazzolini/.claude/skills/` |
| **GitHub Repo** | https://github.com/jonprofissionalux-cloud/Reembolso-Envios |
| **GitHub Pages** | https://jonprofissionalux-cloud.github.io/Reembolso-Envios/ |
| **Figma Design** | https://www.figma.com/design/UNY8LMXVndJBSirtTuTGbP/Reembolso-Garantido |
| **Figma node — Admin Detalhe** | node-id: `175-80793` |
| **Design System (ícones/tokens)** | https://github.com/pedrohenriquevalentim/olist-ds |
| **Branch** | `main` |

---

## Setup Local

```bash
cd /Users/jonazzolini/.claude/skills
ls -la
```

### Deploy após alterações

```bash
cd /Users/jonazzolini/.claude/skills
git add -A
git commit -m "tipo(escopo): descrição"
git push origin main
```

> GitHub Pages atualiza automaticamente após `git push`.

---

## Stack Técnico

| Tecnologia | Descrição |
|------------|-----------|
| **HTML/CSS/JS** | Puro — zero frameworks |
| **Armazenamento** | `localStorage` (chave: `rg_solicitations`) |
| **Versionamento** | Git + GitHub |
| **Deploy** | GitHub Pages |
| **Design System** | Olist DS — repo: `pedrohenriquevalentim/olist-ds` |

---

## Estrutura de Arquivos

```
~/.claude/skills/
├── index.html              # Landing inicial
├── lojista.html            # Dashboard lojista (3 views)
├── admin.html              # Dashboard admin (2 tabs + detail view)
├── hero-lojista.jpg        # Imagem hero
├── _sb_icons_patch.html    # Ícones do sidebar (SVGs do DS)
├── references/             # Docs do Design System (cores, tipografia, componentes...)
└── README.md               # Este arquivo
```

---

## Estado Atual Completo

### lojista.html — 3 Views

#### View 1: Landing Page (`#vlp`)
- Apresentação do serviço
- Botão "Ativar Reembolso Garantido"
- Ativa `rg_activated = '1'` + `rg_act_date`

#### View 2: Serviço Ativo (`#vact`) — 3 Tabs

**Tab 1: Abrir Solicitação**
- Barra de busca com dropdown de filtro
- Opções: "Número da Nota Fiscal" / "Código do Rastreio"
- Placeholder muda conforme seleção
- 5 mocks de busca (001–005)
- `overflow:visible` corrigido para dropdown aparecer

**Tab 2: Solicitações em Aberto**
- Cards horizontais: data, tempo de análise, destinatário + endereço, NF, valor, badge "Em Análise"
- Botão `···` → dropdown "Ver solicitação" / "Cancelar solicitação"
- Ao clicar em outro `···`, fecha o anterior
- Modal "Ver Solicitação": stepper 3 barras + resumo + informações adicionais + arquivos

**Tab 3: Finalizados**
- Cards horizontais: data, destinatário, NF, valor, resultado (badge + data + valor ou motivo)
- Dropdown variável por status:
  - Reembolsado: "Ver solicitação" + "Ver reembolso"
  - Parcial: "Ver solicitação" + "Ver reembolso"
  - Negado: "Ver solicitação" + "Reabrir solicitação"
- Modal unificado (`ml-final-solic`): resultado + resumo + informações + arquivos

#### View 3: Serviço Desativado (`#voff`)
- Banner de alerta
- Tabs com histórico somente leitura
- Ativa `rg_deactivated = '1'`

---

### admin.html — 2 Tabs + Detail View

#### Tab 1: Novas Solicitações
- Tabela: data, lojista, NF, reincidência, botão "analisar"
- SLA badge (azul / amarelo / vermelho por dias)

#### Tab 2: Finalizadas
- Tabela: data, lojista, NF, decisão, valor reembolsado, decisor

#### Detail View — Novas (ao clicar "analisar")
Comportamento: lista some com fade, detalhe aparece como nova página. Botão "Voltar" retorna à lista.

Layout fiel ao Figma (node `175-80793`):
- Breadcrumb: Inicio > Reembolso > Analisar
- Botão "Voltar" (isolado, acima do título)
- Header: Título (nome do lojista) + CNPJ + badge SLA à esquerda — 3 botões de ação à direita
- Card "Pedido": 2 colunas — esquerda (Valor, NF-e, Emissão) / direita (nome, endereço, email, tel)
- Card "Envio": transportadora + quantidade + tabela (Volume, Código de rastreio, Medidas e peso, Status, Valor)
- Card "Informações do lojista": fundo azul claro (`--primary-softer`), texto livre + anexos

#### Drawers de ação (Aprovar / Parcial / Negar)
- Overlay + slide-in da direita
- Aprovar: resumo financeiro + campo de justificativa opcional
- Parcial: campo de valor + justificativa obrigatória
- Negar: campo de motivo obrigatório

---

## Seed de Dados

| ID | Status | NF | Cliente | Valor | Motivo |
|----|--------|----|---------|----|--------|
| REE-00000001 | em_analise | 82630312 | Gustavo Moreira Farias Silva | R$ 226,24 | — |
| REE-00000002 | em_analise | 55566677788 | Carlos Eduardo Lima | R$ 350,00 | — |
| REE-00000010 | reembolsado | 11122233344 | Maria Santos Oliveira | R$ 180,00 | — |
| REE-00000011 | parcial | 99988877766 | Ana Paula Costa | R$ 420,00 | "O outro pacote foi entregue." |
| REE-00000012 | negado | 33344455566 | Roberto Silva Nunes | R$ 290,00 | "Pedido entregue conforme rastreio." |

**Chave localStorage**: `rg_solicitations` (array de objetos)

---

## localStorage Schema

```javascript
// Chave principal
rg_solicitations = [
  {
    id: "REE-00000001",
    status: "em_analise" | "reembolsado" | "parcial" | "negado",
    nf: "82630312",
    destinatario: "Gustavo Moreira Farias Silva",
    chamado: "CHM-2024-00847",
    valor: "R$ 226,24",
    valorReembolsado: null,   // preenchido se reembolsado ou parcial
    decisaoNota: null,         // motivo — preenchido se parcial ou negado
    timestamp: 1234567890000,
    data: "09/06/2026"
  }
];

// Flags de ativação (lojista.html)
rg_activated   = "1";         // presença = ativado
rg_act_date    = "2024-01-10";
rg_deactivated = "1";         // presença = desativado
```

---

## MCPs Disponíveis

| MCP | Uso |
|-----|-----|
| **filesystem** | Leitura/escrita em `~/.claude/skills/` |
| **figma** | Acesso ao Figma Desktop (get_design_context, etc.) |
| **Claude in Chrome** | Automação do navegador para testes |

---

## Regras de UI — IMPORTANTE

### Botões de ação
- Usar **sempre SVG inline** para ícones — nunca emojis
- Padrão dos 3 botões de ação no admin (negar / parcial / aprovar):
  - "Negar" e "Parcial": `background:#fff; border:1px solid #d1d1e3; color:var(--ns)`
  - "Aprovar": `background:var(--primary); color:#fff; border:none`
- Ícones dos botões: SVG `viewBox="0 0 24 24"` stroke, sem fill
  - Negar: circle + X (`M15 9L9 15M9 9l6 6`)
  - Parcial: circle + traço (`M8 12h8`)
  - Aprovar: circle + check (`M9 12l2 2 4-4`)
- Referência de ícones e tokens: `pedrohenriquevalentim/olist-ds`

### Cores (CSS vars)
```css
--primary: #0c29d0
--primary-base: #043fbe
--primary-softer: #f0f4fd   /* fundo card Informações lojista */
--ns: #312f4f                /* texto principal */
--nm: #5e5d5a                /* texto secundário */
--nb: #8f8d85                /* labels, placeholders */
--nsoft: #e8e5d9             /* bordas */
--bg: #fcfbf8                /* background geral */
--ok-soft: #c9ffee           /* badge verde fundo */
--ok-str: #054933            /* badge verde texto */
```

### Detail View — transição
- `list-view.hide` usa `position:absolute; visibility:hidden` para sair do fluxo
- `detail-view` começa `display:none`, só aparece ao chamar `showDetail()`
- Fade in/out de 200ms via `opacity` + `setTimeout`

---

## UI/UX — Correções Aplicadas

- `overflow:visible` no dropdown de filtro (lojista)
- Placeholder dinâmico conforme filtro selecionado
- Dropdown `···` fecha ao clicar em outro
- Modal stepper visual (3 etapas)
- Reset limpa `rg_deactivated` também
- Admin detail view: layout fiel ao Figma — espaço no topo removido, botões sem cor, card Pedido sem border-bottom fantasma
- Botões substituídos de emojis para SVG inline

---

## Checklist de Features

- [x] Landing page lojista (ativação)
- [x] Dashboard lojista (3 tabs)
- [x] Busca com filtro dropdown
- [x] Cards solicitações (em aberto + finalizados)
- [x] Modal "Ver solicitação"
- [x] Dropdown mais-opções (···)
- [x] Dashboard admin (2 tabs)
- [x] Admin detail view — nova página com fade (fiel ao Figma)
- [x] Drawers aprovar / parcial / negar
- [x] Seed de dados (5 itens)
- [x] localStorage persistence
- [ ] Validações avançadas
- [ ] Export de reembolsos (PDF/CSV)
- [ ] Notificações em tempo real
- [ ] Integração API Olist (futura)

---

## Próximos Passos

1. Sincronizar tokens do Figma com `pedrohenriquevalentim/olist-ds`
2. Substituir ícones SVG inline pelos componentes do DS
3. Validações de negócio
4. QA no navegador (Chrome MCP)
5. Integração com backend (quando disponível)

---

**Última atualização**: Junho 2026
**Mantido por**: Jonathan Azzolini
