# Design System v0.1
## Plataforma de Gestão para Práticas Estéticas

**Status:** Draft / v0.1  
**Objetivo:** estabelecer uma linguagem visual consistente para todas as telas do produto.

---

# 1. Direção visual

A interface deve transmitir:

- Profissionalismo
- Confiança
- Organização
- Sofisticação
- Tecnologia
- Simplicidade
- Neutralidade de gênero

A plataforma atende profissionais e clientes de diferentes perfis. A identidade visual não deve assumir uma estética predominantemente feminina ou masculina.

### Princípio central

> **Menos elementos, melhor hierarquia.**

A interface deve priorizar informação relevante, espaço em branco e ações claras.

Evitar transformar cada informação em um card.

---

# 2. Referência estética

A linguagem visual deve seguir uma linha:

- SaaS premium
- Minimalista
- Leve
- Clean
- Profissional
- Contemporânea
- Alta legibilidade
- Baixa densidade visual

### Evitar

- Gradientes chamativos
- Sombras fortes
- Excesso de cores
- Rosa como cor predominante
- Roxo como cor predominante
- Elementos decorativos sem função
- Dashboards excessivamente carregados
- Cards dentro de cards sem necessidade
- Ícones excessivamente grandes
- Botões gigantes
- Textos excessivamente pequenos

---

# 3. Paleta de cores

## 3.1 Cores principais

```css
--color-primary-50:  #F0F6FC;
--color-primary-100: #E2EEF9;
--color-primary-200: #C9DFF2;
--color-primary-300: #A8CBE9;
--color-primary-400: #78AEDD;
--color-primary-500: #4F8FCC;
--color-primary-600: #3476B4;
--color-primary-700: #285E92;
--color-primary-800: #244E76;
--color-primary-900: #213F5F;
```

Uso:

- `primary-50/100`: backgrounds de destaque, seleção e estados suaves.
- `primary-500/600`: ações principais.
- `primary-700/800`: hover, elementos de maior contraste.
- `primary-900`: textos de maior hierarquia quando necessário.

A cor primária deve ser usada com moderação.

---

## 3.2 Neutros

```css
--color-white:    #FFFFFF;

--color-gray-50:  #F8FAFC;
--color-gray-100: #F1F5F9;
--color-gray-200: #E2E8F0;
--color-gray-300: #CBD5E1;
--color-gray-400: #94A3B8;
--color-gray-500: #64748B;
--color-gray-600: #475569;
--color-gray-700: #334155;
--color-gray-800: #1E293B;
--color-gray-900: #0F172A;
```

### Uso recomendado

- Background principal: `gray-50`
- Cards: `white`
- Bordas: `gray-200`
- Texto secundário: `gray-500/600`
- Texto principal: `gray-800/900`

---

# 4. Cores semânticas

## Sucesso

```css
--color-success-50:  #F0FDF4;
--color-success-100: #DCFCE7;
--color-success-500: #22C55E;
--color-success-600: #16A34A;
--color-success-700: #15803D;
```

Usar para:

- Cliente ativo
- Pagamento realizado
- Procedimento concluído
- Confirmações

---

## Atenção

```css
--color-warning-50:  #FFFBEB;
--color-warning-100: #FEF3C7;
--color-warning-500: #F59E0B;
--color-warning-600: #D97706;
```

Usar para:

- Pendências
- Informações que exigem atenção
- Documentos pendentes

---

## Erro

```css
--color-danger-50:  #FEF2F2;
--color-danger-100: #FEE2E2;
--color-danger-500: #EF4444;
--color-danger-600: #DC2626;
```

Usar para:

- Exclusão
- Erros
- Bloqueios
- Informações críticas

Nunca usar vermelho como elemento decorativo.

---

# 5. Tipografia

## Fonte

Prioridade:

```text
Inter
Segoe UI
system-ui
sans-serif
```

## Escala

```css
--font-xs:   12px;
--font-sm:   14px;
--font-md:   16px;
--font-lg:   18px;
--font-xl:   24px;
--font-2xl:  30px;
--font-3xl:  36px;
```

### Hierarquia

**Título da página**

- 28–32px
- weight 600/700
- color gray-900

**Título de seção**

- 18–20px
- weight 600

**Texto principal**

- 14–16px
- weight 400/500

**Texto secundário**

- 13–14px
- gray-500/600

**Labels**

- 12–14px
- weight 500

Evitar textos menores que 12px.

---

# 6. Espaçamento

Usar escala baseada em múltiplos de 4:

```css
--space-1:  4px;
--space-2:  8px;
--space-3:  12px;
--space-4:  16px;
--space-5:  20px;
--space-6:  24px;
--space-8:  32px;
--space-10: 40px;
--space-12: 48px;
--space-16: 64px;
```

Preferir:

- 16px entre elementos relacionados
- 24px entre grupos
- 32px entre seções
- 40–48px em áreas de maior separação

---

# 7. Border radius

```css
--radius-sm:  6px;
--radius-md:  8px;
--radius-lg:  12px;
--radius-xl:  16px;
--radius-2xl: 20px;
--radius-full: 9999px;
```

Uso:

- Inputs: 8px
- Botões: 8px
- Cards: 12–16px
- Avatar: full
- Badges: full

Evitar arredondamento excessivo em todos os elementos.

---

# 8. Bordas

```css
--border-color: #E2E8F0;
--border-subtle: #EDF2F7;
```

Bordas devem ser discretas.

Preferir:

```css
border: 1px solid var(--border-color);
```

Evitar bordas escuras e pesadas.

---

# 9. Sombras

Usar sombras extremamente sutis.

```css
--shadow-sm:
  0 1px 2px rgba(15, 23, 42, 0.04);

--shadow-md:
  0 4px 12px rgba(15, 23, 42, 0.06);

--shadow-lg:
  0 8px 24px rgba(15, 23, 42, 0.08);
```

Cards normalmente devem funcionar também sem sombra.

Preferir borda + background.

---

# 10. Layout geral

Estrutura:

```text
┌──────────────────────────────────────────────┐
│ Sidebar │ Topbar                             │
│         ├────────────────────────────────────┤
│         │                                    │
│         │        Conteúdo principal           │
│         │                                    │
│         │                                    │
└──────────────────────────────────────────────┘
```

## Sidebar

A sidebar é compacta e predominantemente baseada em ícones.

Características:

- Fundo branco
- Largura aproximada: 72–88px quando recolhida
- Borda direita sutil
- Ícones centralizados
- Item ativo com background `primary-50/100`
- Ícone ativo em `primary-600`
- Tooltip ao passar o mouse
- Botão de expandir/recolher
- Separação entre navegação principal e ações secundárias
- Avatar do usuário na parte inferior

### Sidebar expandida

Quando expandida:

- Exibir ícone + texto
- Manter mesma hierarquia visual
- Não alterar a identidade dos itens

---

# 11. Topbar

A topbar deve conter somente elementos funcionais.

Prioridade:

1. Busca
2. Notificações
3. Perfil do usuário

Evitar colocar métricas, banners ou informações decorativas.

A busca global deve permitir localizar clientes e outros elementos relevantes do sistema.

---

# 12. Navegação interna

Usar:

- Tabs
- Breadcrumbs
- Links contextuais

Evitar menus horizontais excessivos.

### Tabs

Características:

- Texto 14–15px
- Espaçamento horizontal generoso
- Estado ativo através de cor + indicador inferior
- Sem caixas pesadas ao redor das tabs

---

# 13. Cards

Cards devem agrupar informações relacionadas.

Estrutura:

```text
Título
Descrição opcional
────────────────
Conteúdo
```

Características:

```css
background: white;
border: 1px solid var(--border-color);
border-radius: 12px;
```

Usar sombra apenas quando necessário.

### Regra

Não criar um card para cada informação.

Exemplo ruim:

```text
[Card]
Nome

[Card]
CPF

[Card]
Telefone

[Card]
E-mail
```

Preferir:

```text
[ Informações pessoais ]

Nome
CPF
Telefone
E-mail
```

---

# 14. Metric Cards

Indicadores podem utilizar cards compactos.

Exemplo:

```text
┌──────────────────────────┐
│ Última visita            │
│                          │
│ 15/09/2026               │
│ Há 7 dias                │
└──────────────────────────┘
```

Um dashboard deve utilizar somente os indicadores realmente relevantes.

Evitar transformar qualquer número em KPI.

---

# 15. Botões

## Primário

```text
Salvar cliente
Novo cliente
Agendar procedimento
```

Características:

- Background `primary-600`
- Texto branco
- Radius 8px
- Altura 40–44px

## Secundário

Fundo branco.

```text
Cancelar
Voltar
```

## Ghost

Para ações de baixa prioridade.

```text
Ver todos
Editar
Mais
```

## Danger

Somente para ações destrutivas.

```text
Excluir cliente
```

---

# 16. Inputs

Inputs devem ser simples e consistentes.

```css
height: 40–44px;
border: 1px solid #CBD5E1;
border-radius: 8px;
background: white;
```

Estados:

- Default
- Hover
- Focus
- Error
- Disabled

### Focus

Utilizar outline/border com `primary-500`.

Não utilizar efeitos exagerados.

---

# 17. Formulários

Formulários devem ser divididos em seções.

Exemplo:

```text
Dados pessoais

Nome completo       Nome social
CPF                 Data de nascimento
Sexo                Telefone
E-mail

Endereço

CEP
Endereço            Número
Complemento
Bairro              Cidade
Estado

Informações adicionais

Aniversário
Preferência de profissional
Observações
```

Não colocar campos sem relação no mesmo grupo.

---

# 18. Tabelas

Tabelas devem ser utilizadas quando houver necessidade de comparação entre muitos registros.

Características:

- Cabeçalho discreto
- Linhas com separadores sutis
- Boa altura de linha
- Ações no final
- Status através de badges
- Hover discreto

Exemplo:

```text
Cliente          Última visita     Próximo agendamento    Total gasto    Status
Maria Silva      15/09/2026        28/09/2026             R$ 4.850       Ativa
João Santos      10/09/2026        —                      R$ 1.200       Ativo
```

---

# 19. Badges / Status

Badges devem ser discretos.

Exemplo:

```text
[ Ativa ]
[ Inativo ]
[ Pago ]
[ Pendente ]
[ Concluído ]
```

Não utilizar cores saturadas.

---

# 20. Avatar e foto

Fotos de clientes devem ter tratamento discreto.

Formatos:

- Avatar circular em listas
- Avatar maior no perfil
- Thumbnail em galerias

Não utilizar molduras decorativas.

---

# 21. Página de cliente

A página do cliente deve ter uma hierarquia clara.

Estrutura:

```text
Cliente

[ Foto ]  Maria Silva
          32 anos | CPF
          Tags

          WhatsApp | Ligar | E-mail | Mais

──────────────────────────────────────

Visão Geral | Dados Pessoais | Anamnese |
Procedimentos | Financeiro | Fotos |
Documentos | Histórico

──────────────────────────────────────

KPIs principais

Última visita
Próximo agendamento
Valor total gasto
Frequência média

──────────────────────────────────────

Últimos procedimentos
Últimos pagamentos
Últimas fotos

──────────────────────────────────────

Anamnese
```

Não adicionar gráficos ou widgets somente para preencher espaço.

---

# 22. Página de cadastro

A tela de cadastro deve priorizar preenchimento rápido.

Estrutura:

```text
Novo cliente

Dados pessoais
Anamnese
Procedimentos
Financeiro
Fotos
Documentos
Histórico

────────────────────────

Foto

Informações pessoais

Informações adicionais

Tags

────────────────────────

Cancelar              Salvar cliente
```

O cadastro deve utilizar as mesmas regras visuais do restante do produto.

---

# 23. Ícones

Utilizar uma única família de ícones.

Preferência:

- Lucide
- Tabler
- Phosphor

Escolher apenas uma biblioteca para o produto.

Ícones devem:

- possuir espessura consistente
- ter tamanho geralmente entre 18–20px
- nunca competir visualmente com o texto
- possuir função clara

Evitar ícones decorativos.

---

# 24. Estados

Todos os componentes devem considerar:

```text
Default
Hover
Focus
Active
Disabled
Loading
Error
Empty
```

Nenhuma tela deve depender exclusivamente do estado ideal com dados preenchidos.

---

# 25. Empty states

Quando não houver dados:

```text
Nenhum procedimento encontrado

Quando o cliente realizar o primeiro procedimento,
ele aparecerá aqui.

[ Agendar procedimento ]
```

Evitar ilustrações grandes e decorativas.

---

# 26. Responsividade

Prioridade:

1. Desktop
2. Tablet
3. Mobile

No mobile:

- Sidebar vira navegação compacta
- Tabelas podem virar listas/cards
- Formulários passam para uma coluna
- KPIs podem ser empilhados
- Ações secundárias podem ficar em menu

Não simplesmente reduzir todos os elementos.

---

# 27. Acessibilidade

Obrigatório:

- Contraste adequado
- Labels associados aos inputs
- Navegação por teclado
- Estados de foco visíveis
- Área de clique adequada
- Não depender somente de cor para comunicar status
- Textos alternativos para imagens relevantes

---

# 28. Regras para implementação com IA

Antes de criar uma nova tela:

1. Verificar componentes existentes.
2. Verificar referências visuais em `/design/referencias`.
3. Reutilizar padrões existentes.
4. Não criar nova paleta.
5. Não criar nova escala de espaçamento.
6. Não criar novo estilo de botão sem necessidade.
7. Não adicionar elementos decorativos sem função.
8. Manter a mesma densidade visual das telas existentes.

### Regra de consistência

> Uma nova tela deve parecer pertencer ao mesmo produto mesmo sem o usuário conhecer o restante da aplicação.

### Regra de contenção

Se houver dúvida entre adicionar ou remover um elemento:

> Preferir a solução mais simples que preserve a informação e a operação.

---

# 29. Tokens resumidos

```css
:root {
  /* Colors */
  --primary: #4F8FCC;
  --primary-hover: #3476B4;
  --primary-light: #F0F6FC;

  --background: #F8FAFC;
  --surface: #FFFFFF;

  --text-primary: #0F172A;
  --text-secondary: #475569;
  --text-muted: #64748B;

  --border: #E2E8F0;

  --success: #16A34A;
  --warning: #D97706;
  --danger: #DC2626;

  /* Typography */
  --font-family: Inter, "Segoe UI", system-ui, sans-serif;

  /* Radius */
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;
  --radius-full: 9999px;

  /* Spacing */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-12: 48px;
  --space-16: 64px;
}
```

---

# 30. Checklist de revisão visual

Antes de considerar uma tela pronta:

- [ ] A tela utiliza a paleta oficial?
- [ ] A tipografia está consistente?
- [ ] O espaçamento segue a escala?
- [ ] A sidebar está consistente?
- [ ] Os botões seguem os padrões?
- [ ] Os inputs seguem os padrões?
- [ ] Os cards estão sendo utilizados com moderação?
- [ ] Existe excesso de elementos?
- [ ] Existe informação que poderia ser removida?
- [ ] A hierarquia visual está clara?
- [ ] A ação principal está evidente?
- [ ] Estados de loading/empty/error foram considerados?
- [ ] A tela parece pertencer ao mesmo produto?
- [ ] Nenhum elemento foi adicionado apenas por decoração?

---

# 31. Princípio final

A plataforma deve parecer:

**profissional antes de parecer sofisticada.**

A sofisticação deve surgir da consistência, espaçamento, tipografia,
hierarquia e qualidade dos componentes — e não da quantidade de elementos
visuais.
