# 🚀 PedidoÁgil — Gestão de pedidos com Kanban, rotas e entrada via WhatsApp (Google Apps Script + Google Sheets)
Um web app construído em Google Apps Script + HTML/CSS/JavaScript voltado para operações que precisam de...
- Centralizar pedidos, clientes e produtos em um único fluxo operacional
- Reduzir retrabalho na digitação e padronizar o registro de pedidos
- Acompanhar status em tempo real (separação → rota → entrega) com rastreabilidade
- Gerar relatórios e apoiar decisões comerciais (fiado/contas, histórico do cliente)

## 💼 Por que esse projeto importa (contexto de negócio)
Operações de delivery e comércio local (açougue, mercado, conveniência, etc.) normalmente sofrem com pedidos chegando por múltiplos canais (principalmente WhatsApp), sem padrão de unidades/quantidades, com alta taxa de erro humano e perda de histórico. Isso gera:
- Tempo operacional desperdiçado em “interpretação” e redigitação
- Inconsistência de dados (itens, unidades, endereços e observações)
- Dificuldade de cobrança (fiado) e auditoria de alterações
- Falta de visibilidade do funil operacional (novo → separação → entrega)

Este sistema resolve isso ao transformar o pedido em **um pipeline operacional com estado**, garantindo **padronização, rastreabilidade e velocidade**, com persistência simples e barata em Google Sheets (sem infraestrutura dedicada), e com um caminho claro para automação via IA na entrada do WhatsApp.

## ✅ Principais funcionalidades (o que o app entrega)
**🧾 Pedidos**
- Cadastro e salvamento de pedidos com itens, unidades, observações e total
- Edição de pedido com regras de bloqueio por status (ex.: impedir edição em rota/entregue)
- Impressão de ticket de separação e impressão por observação (quando pedido vem “em texto”)

**🧠 Entrada via WhatsApp (Assistida por IA + fallback)**
- Cola do texto original do WhatsApp e interpretação automática em itens (produto/qtd/unidade)
- Normalização de unidades e tolerância a formatos incompletos (ex.: “6 moída”, “kl/kg”, sem unidade)
- Revisão humana pós-interpretação antes de salvar
- Armazenamento do “antes/depois” para aprendizado futuro:
  - texto original do WhatsApp
  - interpretação inicial do modelo
  - interpretação final (após ajustes humanos)
  - modelo e timestamp de processamento

**🧩 Kanban Operacional**
- Quadro por status (NOVO, EM_SEPARACAO, SEPARADO, EM_ROTA)
- Mudança de status com ações contextuais (ex.: atribuir separador, atribuir entregador, marcar entregue)
- Atualização periódica (polling) e renderização otimizada para manter UI responsiva
- Exclusão controlada e edição com histórico (operador + data da última edição)

**🗺️ Rotas**
- Planejamento de rota com agrupamento/otimização e geração de links para Google Maps
- Persistência de origem padrão e filtros por data

**👥 Cadastros e Comercial**
- Clientes, produtos, entregadores e separadores
- Contas a pagar (fiado): criação, status e auditoria de cobrança
- Relatórios com filtros por status/entregador/separador e totalização

## 🧱 Arquitetura (como eu estruturei)
**Frontend**
- HTML/CSS/JavaScript puro com foco em operação (inputs rápidos, atalhos, feedback via toasts)
- Componentização funcional por seções: Kanban, pedidos, relatórios, avulso/WhatsApp, comercial
- Impressão via geração de HTML de ticket e `window.print()`

**Backend**
- Google Apps Script (V8) como camada de API (Web App)
- Funções server-side para:
  - criar/atualizar pedidos
  - consultar dados e relatórios
  - atualizar status no Kanban
  - persistir metadados de auditoria e IA
- Controle de concorrência com `LockService` nas operações críticas (ex.: criação/edição de pedido)

**Persistência / “Banco de dados”**
- Google Sheets como base operacional:
  - `Pedidos` (estado do pedido + metadados de auditoria e IA)
  - `ItensPedido` (itens normalizados: produto/qtd/unidade/preço/subtotal)
  - `Clientes`, `Produtos`, `Entregadores`, `Separadores`, `ContasPagar`
- Estratégia de “schema evolution”: funções garantem colunas novas sem quebrar planilhas existentes.

## 🧠 Lógica “core” (o que eu considero o coração do sistema)
1) **Pipeline de pedidos com estado + regras de negócio**
   - O pedido evolui por estados operacionais (NOVO → EM_SEPARACAO → SEPARADO → EM_ROTA → ENTREGUE)
   - Ações e permissões variam conforme o estado (ex.: travar edição em rota/entregue)

2) **Persistência consistente em planilha (concorrência + integridade)**
   - Geração de IDs sequenciais e gravação atômica com lock
   - Escrita em “Pedidos” + “ItensPedido” garantindo consistência do pedido completo
   - Auditoria de edição (operador + timestamp) para rastreabilidade

3) **Entrada por WhatsApp com “human-in-the-loop” e trilha de aprendizado**
   - Interpretação automática do texto (modelo com bom custo/benefício) + fallback local
   - Usuário revisa e corrige antes de salvar
   - Registro do texto original e das interpretações inicial/final para treinar prompts/modelos no futuro

## 🧑‍💼 Competências demonstradas (para recrutadores)
- Full-stack end-to-end (UI operacional + API + persistência)
- Modelagem de dados pragmática (planilhas como banco com evolução de schema)
- Design de fluxos críticos com controle de concorrência e integridade (LockService)
- UX orientada à operação (atalhos, feedback imediato, impressão, redução de cliques)
- Integração com serviços externos (IA via HTTP + gestão de chave/escopos)
- Observabilidade e governança de dados (auditoria de edição + trilha “antes/depois” para melhoria contínua)

## 🧰 Tech stack
- Google Apps Script (V8)
- Google Sheets (persistência)
- HTML5 / CSS3 / JavaScript (Vanilla)
- Web App deployment (Apps Script)
- Integração opcional com OpenAI (chat completions + JSON mode)

## 🎥 Screenshots / Demo
<img width="1467" height="749" alt="image" src="https://github.com/user-attachments/assets/6ec7426b-6ea1-49b9-b131-840354d35b58" />
<img width="1470" height="736" alt="image" src="https://github.com/user-attachments/assets/7ac883a5-9195-48a9-ba7c-5480e959ba46" />
<img width="1467" height="749" alt="image" src="https://github.com/user-attachments/assets/574f405c-ef6e-4062-93c1-4ea116778937" />
<img width="1156" height="707" alt="image" src="https://github.com/user-attachments/assets/60571dbd-3eb1-4340-8e65-d186abfb1bd9" />
<img width="1467" height="662" alt="image" src="https://github.com/user-attachments/assets/caab38c0-6319-4347-99ed-72b12d6d3243" />
<img width="1469" height="775" alt="image" src="https://github.com/user-attachments/assets/0cdc4faa-4397-4a76-9b0e-ed0547361ef5" />
<img width="437" height="723" alt="image" src="https://github.com/user-attachments/assets/1fc43d8a-3640-41d7-a633-ffef376dbcb5" />
<img width="747" height="477" alt="image" src="https://github.com/user-attachments/assets/86601d3e-6de2-42be-84bc-769c4ee7d8f1" />
<img width="787" height="620" alt="image" src="https://github.com/user-attachments/assets/d7d0335a-290d-4447-ac32-43124ac8f6a3" />

LinkedIn: https://www.linkedin.com/in/taylor-pinheiro-/?locale=pt_BR![Uploading image.png…]()


Portfólio: [link]
