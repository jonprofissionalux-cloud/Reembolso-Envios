# Reembolso Garantido — Olist Envios

> **Sistema de gestão de reembolsos para lojistas e administradores**

[![GitHub](https://img.shields.io/badge/GitHub-Repo-black?logo=github)](https://github.com/jonprofissionalux-cloud/Reembolso-Envios)
[![Pages](https://img.shields.io/badge/GitHub%20Pages-Live-blue)](https://jonprofissionalux-cloud.github.io/Reembolso-Envios/)
[![Figma](https://img.shields.io/badge/Figma-Design-F24E1E?logo=figma)](https://www.figma.com/design/UNY8LMXVndJBSirtTuTGbP/Reembolso-Garantido)

---

## 📋 Quick Links

| Link | Descrição |
|------|-----------|
| **Local** | `/Users/jonazzolini/.claude/skills/` |
| **GitHub Repo** | https://github.com/jonprofissionalux-cloud/Reembolso-Envios |
| **GitHub Pages** | https://jonprofissionalux-cloud.github.io/Reembolso-Envios/ |
| **Figma Design** | https://www.figma.com/design/UNY8LMXVndJBSirtTuTGbP/Reembolso-Garantido |
| **Branch** | `main` |

---

## 🛠️ Setup Local

```bash
# Abrir pasta
cd /Users/jonazzolini/.claude/skills

# Listar arquivos
ls -la

# Editar um arquivo
open index.html
```

### Deploy após alterações

```bash
cd /Users/jonazzolini/.claude/skills
git add -A
git commit -m "descrição da alteração"
git push origin main
```

> **Nota**: GitHub Pages atualiza automaticamente após `git push`

---

## 🎯 Stack Técnico

| Tecnologia | Descrição |
|------------|-----------|
| **HTML/CSS/JS** | Puro — Zero frameworks |
| **Armazenamento** | `localStorage` (chave: `rg_solicitations`) |
| **Versionamento** | Git + GitHub |
| **Deploy** | GitHub Pages |
| **Design System** | Figma (WIP) |

---

## 📁 Estrutura de Arquivos

```
~/.claude/skills/
├── index.html          # Landing inicial
├── lojista.html        # Dashboard lojista (3 views)
├── admin.html          # Dashboard admin (2 tabs)
├── hero-lojista.jpg    # Imagem hero
└── README.md           # Este arquivo
```

---

## 🏗️ Estado Atual Completo

### **lojista.html** — 3 Views

#### 1️⃣ **Landing Page (`#vlp`)**
- Apresentação do serviço
- Botão "Ativar Reembolso Garantido"
- Ativa `rg_activated = '1'` + `rg_act_date`

#### 2️⃣ **Serviço Ativo (`#vact`)**
3 Tabs principais:

##### **Tab 1: Abrir Solicitação**
- Barra de busca com dropdown de filtro
- Opções de filtro: "Número da Nota Fiscal" / "Código do Rastreio"
- Placeholder muda conforme seleção
- 5 mocks de busca (001–005)
- `overflow:visible` corrigido para dropdown aparecer

##### **Tab 2: Solicitações em Aberto**
- Cards horizontais com:
  - Data + tempo de análise
  - Destinatário + endereço completo
  - NF (Nota Fiscal)
  - Valor
  - Badge "Em Análise"
  - Botão `···` (more-horizontal) → dropdown
- Dropdown options: "Ver solicitação" / "Cancelar solicitação"
- Ao clicar em outro `···`, fecha o anterior
- **Modal "Ver Solicitação"** (em aberto):
  - Stepper 3 barras: Solicitação criada → Em análise → Finalizada
  - Resumo: NF, Destinatário, Transportadora, Valor NF, Rastreio, Status
  - Informações adicionais
  - Arquivos anexados

##### **Tab 3: Finalizados**
- Cards horizontais com:
  - Data
  - Destinatário + endereço
  - NF
  - Valor
  - Coluna "Resultado" (badge + data + valor reembolso ou motivo)
  - Botão `···` (dropdown variável por status)
- **Dropdown por status**:
  - **Reembolsado**: "Ver solicitação" + "Ver reembolso"
  - **Parcial**: "Ver solicitação" + "Ver reembolso"
  - **Negado**: "Ver solicitação" + "Reabrir solicitação"
- **Modal unificado finalizados** (`ml-final-solic`):
  - Seção "Resultado na análise" (badge + dados por status + link ver extrato)
  - Resumo (2 etiquetas)
  - Informações adicionais
  - Arquivos

#### 3️⃣ **Serviço Desativado (`#voff`)**
- Banner de alerta
- Tabs com histórico somente leitura (sem interações)
- Ativa `rg_deactivated = '1'`

---

### **admin.html** — 2 Tabs

#### 1️⃣ **Novas Solicitações**
- Tabela com:
  - SLA badge
  - Reincidência indicator
- Detalhe (ao clicar em linha):
  - 3 Cards: Pedido, Envio, Informações
  - Drawers para ações:
    - Aprovar (reembolso total)
    - Parcial (com motivo)
    - Negar (com motivo)

#### 2️⃣ **Finalizadas**
- Tabela com status finalizados
- Detalhe (ao clicar):
  - Card de resultado colorido
  - Cards Pedido e Envio
  - Informações read-only

---

## 🗄️ Seed de Dados

Estrutura `localStorage` com 5 itens de teste:

| ID | Status | NF | Cliente | Valor | Motivo |
|------|--------|-------|---------|-------|--------|
| REE-00000001 | em_analise | 82630312 | Gustavo Moreira | — | — |
| REE-00000002 | em_analise | 55566677788 | Carlos Eduardo | — | — |
| REE-00000010 | reembolsado | (alguma) | Maria Santos | R$ 180,00 | — |
| REE-00000011 | parcial | (alguma) | Ana Paula | R$ 210,00 | "O outro pacote foi entregue." |
| REE-00000012 | negado | (alguma) | Roberto Silva | — | "Pedido entregue conforme rastreio." |

**Chave localStorage**: `rg_solicitations` (array de objetos)

---

## 🔧 MCPs Disponíveis

| MCP | Uso | Status |
|-----|-----|--------|
| **filesystem** | Leitura/escrita em `~/.claude/skills/` | ✅ Ativo |
| **figma** | Acesso ao Figma Desktop | ✅ Ativo |
| **Claude in Chrome** | Automação do navegador para testes | ✅ Ativo |

---

## 🔐 localStorage Schema

```javascript
// Chave principal
const rg_solicitations = [
  {
    id: "REE-00000001",
    status: "em_analise" | "reembolsado" | "parcial" | "negado",
    nf: "82630312",
    rastreio: "ABC123456",
    cliente: {
      nome: "Gustavo Moreira",
      endereco: "Rua X, 123, Apto 456 — São Paulo, SP"
    },
    valor: 150.00,
    transportadora: "Sedex",
    dataCriacao: "2024-01-15",
    dataAnalise: "2024-01-20",
    dataFinalizacao: null,
    motivo: null, // preenchido se parcial ou negado
    valorReembolso: null, // preenchido se reembolsado ou parcial
    arquivos: []
  }
];

// Flags de ativação
const rg_activated = "1"; // string (presença = ativado)
const rg_act_date = "2024-01-10"; // data de ativação
const rg_deactivated = "1"; // string (presença = desativado)
```

---

## 📱 UI/UX Notes

- ✅ Dropdown filtro com `overflow:visible` corrigido
- ✅ Placeholder dinâmico conforme filtro selecionado
- ✅ Dropdown more-horizontal fecha ao clicar em outro
- ✅ Modal stepper visual (3 etapas)
- ✅ Cards unificados para diferentes status
- ✅ Reset corrigido (limpa `rg_deactivated` também)

---

## 📋 Checklist de Features

- [x] Landing page lojista (ativação)
- [x] Dashboard lojista (3 tabs)
- [x] Busca com filtro dropdown
- [x] Cards solicitações (em aberto + finalizados)
- [x] Modal "Ver solicitação"
- [x] Dropdown mais-opções (···)
- [x] Dashboard admin (2 tabs)
- [x] Admin detalhe + drawers
- [x] Seed de dados (5 itens)
- [x] localStorage persistence
- [ ] Validações avançadas
- [ ] Export de reembolsos (PDF/CSV)
- [ ] Notificações em tempo real
- [ ] Integração API Olist (futura)

---

## 🚀 Próximos Passos

1. **Sincronizar Figma** → Exportar tokens/componentes
2. **Validações** → Adicionar regras de negócio
3. **Testes** → QA no navegador
4. **Polish UI** → Micro-interações, animations
5. **API** → Integração com backend (quando disponível)

---

## 📞 Referências Rápidas

**Figma Design File:**
```
https://www.figma.com/design/UNY8LMXVndJBSirtTuTGbP/Reembolso-Garantido
```

**GitHub Repo:**
```
https://github.com/jonprofissionalux-cloud/Reembolso-Envios
```

**Pages ao vivo:**
```
https://jonprofissionalux-cloud.github.io/Reembolso-Envios/
```

---

**Última atualização**: Junho 2026  
**Mantido por**: Jonathan Azzolini
