---
name: ds_design-system
description: Design System da plataforma de gestão para práticas estéticas (Anora) — paleta, tipografia Geist, Tailwind v4 com tokens via @theme, ícones Phosphor, layout com sidebar compacta, cards, formulários, tabelas, estados e checklist de revisão. Use sempre que for criar ou revisar qualquer tela, componente visual ou estilo desse produto. Complementa a ds_app-architecture-skill (que cobre dados/estado/persistência) e prevalece sobre a taste-skill em caso de conflito, exceto onde indicado.
---

# Design System v0.7
## Plataforma de Gestão para Práticas Estéticas

**Status:** Draft / v0.7 (v0.7: menu Profissionais, tela de Categorias. v0.6: v0.6: submenu Catálogo, campos numéricos, cadastro em abas com um salvar. v0.5.2: ícones só contorno também no ativo; logo Anora de volta ao topo do menu (identidade do estúdio fora do menu). v0.5.1: itens inativos todos em `gray-500`. v0.5: identidade do estúdio no topo do menu e assinatura Anora no rodapé, item Equipe, paginação padrão, recorte de imagem, aba "Em breve". v0.4.1: v0.4.1: fundo da página no token `fundo` #E9EEF4. v0.4: fundo `gray-100`, perfil fora do menu lateral, ajuda contextual, avisos e módulos futuros. v0.2: fonte Geist, ícones Phosphor, implementação em Tailwind v4, modo escuro no roadmap. v0.3: sidebar flutuante e itens do menu, logo, tags, cadastro sem abas, visibilidade por papel)  
**Objetivo:** estabelecer uma linguagem visual consistente para todas as telas do produto.

**Base:** esta é a skill derivada do Anora a partir de `ds_design-system-base` (genérica, para qualquer sistema). Aqui ficam marca, paleta, telas e exceções do Anora; em caso de conflito, esta prevalece sobre a base.

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

- Background principal da página: token `fundo` = `#E9EEF4` (entre `gray-100` e `gray-200`). Não usar `gray-200` no fundo: é a mesma cor das bordas e os contornos dos cards sumiriam
- Superfícies internas neutras (cabeçalho de tabela, hover de linha, área da foto no formulário): `gray-50`
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
Geist
Segoe UI
system-ui
sans-serif
```

- **Geist** para toda a interface. Não usar Inter (decisão alinhada com a taste-skill: Inter é o default genérico de IA).
- **Geist Mono** para valores numéricos que precisam alinhar em coluna (valores em R$, quantidades em tabelas, KPIs). Usar `tabular-nums` quando possível.
- Carregar a fonte self-hosted (pacote `geist` ou `@fontsource`), com `font-display: swap`. Não linkar Google Fonts via `<link>`.

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

A sidebar é compacta, predominantemente baseada em ícones, e **flutuante**: um painel com cantos arredondados, destacado das bordas da tela, não uma coluna colada à lateral.

Características:

- Fundo branco, borda `gray-200` e `shadow-sm` (a borda sozinha já deve bastar para separar do fundo `fundo`)
- **Flutuante:** afastada 12–16px do topo, da base e da esquerda da viewport; altura total disponível; `radius-xl` (16px)
- Largura aproximada: 72–88px quando recolhida
- Ícones centralizados
- Item ativo com background `primary-50/100`, raio `radius-md`
- Ícones **sempre só contorno** (peso `regular`), inclusive no item ativo: sem ícone preenchido/chapado. O ativo se destaca pelo fundo `primary-50` e pela cor `primary-600`
- Itens não selecionados: ícone e texto em `gray-500`, todos com a mesma cor (disponíveis ou "Em breve"); hover escurece para `gray-900` com fundo `gray-100`
- Tooltip ao passar o mouse (somente no modo recolhido)
- Botão de expandir/recolher (chevron discreto na borda direita do painel)
- Separação entre navegação principal e ações secundárias (notificações, ajuda)
- Na parte inferior: somente o atalho de **Ajuda**. O perfil do usuário **não** fica no menu lateral (fica na topbar)
- Topo: **logo Anora** (símbolo "A" recolhido; símbolo + "ANORA" expandido). A identidade do estúdio (logo + nome) **não** vai no topo do menu (testado e recusado); local definitivo em avaliação
- O estado expandido/recolhido é preferência do usuário (lembrar entre sessões)

Itens da navegação principal, nesta ordem (ícones Phosphor):

```text
Início          House
Agenda          CalendarBlank
Clientes        User
Procedimentos   FlowerLotus
Financeiro      Wallet
Profissionais   UsersThree   (somente gestão; antes chamado "Equipe")
Estoque         Package
Relatórios      ChartBar
Configurações   GearSix      (somente gestão)
```

Menu com **submenu (grupo)**: o grupo **Catálogo** (`Storefront`) reúne **Serviços**, **Produtos** ("Em breve") e **Categorias** (somente gestão). Filhos podem ter papéis próprios. Regras:
- Abre **recolhido por padrão**; abre sozinho apenas quando a tela atual pertence a ele (menu expandido).
- Menu expandido: acordeão com o filho indentado (~22px) e uma linha vertical `gray-200` à esquerda; seta que gira ao abrir.
- Menu recolhido: clicar no ícone abre uma janelinha à direita (`rounded-lg`, borda, `shadow-md`) com o título do grupo e os filhos; fecha ao clicar fora ou ao navegar.
- O item do grupo fica destacado (`primary-50`) só quando um filho está ativo e o grupo está fechado.

Itens exibidos dependem do papel do usuário (ex. recepção não vê Financeiro, Relatórios nem Configurações). Um módulo ainda não implementado aparece desabilitado com tooltip "Em breve", não some da lista.

### Sidebar expandida

Quando expandida:

- Exibir ícone + texto (~240px de largura)
- Logo horizontal: símbolo "A" + "ANORA"
- Manter mesma hierarquia visual
- Não alterar a identidade dos itens
- A expansão empurra o conteúdo (não sobrepõe) no desktop; no tablet/mobile pode sobrepor com overlay

### Logo

- Referência em `/design/referencias/logo-anora.png`. Cor oficial da marca ≈ `primary-900` (`#213F5F`); o logo usa o token, não um hex próprio.
- Versões: símbolo (menu recolhido, favicon), horizontal (menu expandido), vertical/painel da marca (tela de login).
- Preferir SVG. Enquanto não houver vetor oficial, usar o PNG; nunca redesenhar o logo à mão em SVG.

---

# 11. Topbar

A topbar deve conter somente elementos funcionais.

Prioridade:

1. Busca
2. Ajuda (ícone `Question`, abre o painel de ajuda da tela atual)
3. Notificações (somente quando houver notificações reais no produto)
4. Perfil do usuário: avatar + nome + papel, com menu "Meu perfil" e "Sair"

A topbar fica fixa no topo com o mesmo fundo da página (token `fundo` com leve transparência e blur), sem borda.

Evitar colocar métricas, banners ou informações decorativas.

## Ajuda contextual

- Toda tela registra seu conteúdo de ajuda (perguntas e respostas curtas) num arquivo central; o painel mostra o conteúdo da tela atual.
- Painel lateral à direita, flutuante (mesmo raio e borda da sidebar), perguntas em blocos expansíveis.
- Rodapé do painel: "Falar com o suporte" (e-mail) e a versão do app.
- Tela nova sem ajuda registrada não está pronta.

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

Preferência de profissional
Observações
```

Não colocar campos sem relação no mesmo grupo.

Regras:

- Não pedir dado que pode ser derivado de outro (ex. aniversário sai da data de nascimento; idade é calculada).
- CEP preenche endereço, bairro, cidade e estado automaticamente; os campos continuam editáveis.
- CPF, telefone e CEP com máscara e validação.
- Label acima do campo, erro abaixo do campo, obrigatórios marcados com `*`. Nunca placeholder no lugar de label.

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
João Santos      10/09/2026        Sem agendamento        R$ 1.200       Ativo
```

- Célula sem valor mostra o texto do vazio em `gray-500` ("Sem agendamento"), nunca um travessão (`—`).
- Valores monetários alinhados à direita, em Geist Mono / `tabular-nums`.
- Contato (WhatsApp, ligar, e-mail) como ícones clicáveis com tooltip e `aria-label`.

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

- Status sempre com texto (não só cor): fundo `-50/-100` + texto `-700` da cor semântica; inativo em `gray-100` / `gray-600`.
- Concordância de gênero do status ("Ativa"/"Ativo") segue o campo Sexo do cliente; sem essa informação, usar a forma "Ativo".

## Tags

Tags do cliente (ex. "Harmonização Facial", "Recorrente") usam um conjunto fixo de tons derivado dos tokens, nunca cores livres:

- `primary` (padrão), `success`, `warning` e `gray`, sempre na combinação fundo `-50` + texto `-700`.
- A tag recebe um desses tons ao ser criada; não criar tons fora dessa lista (nada de bege, rosa ou roxo).

## Marcadores de categoria

Pontos coloridos antes de itens (ex. lista de procedimentos) só quando representam uma categoria real com legenda/significado, usando os mesmos tons das tags. Sem categoria, sem ponto.

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

Referência visual: `/design/referencias/cliente-perfil-visao-geral.webp` (a lista está em `clientes-lista.webp`).

Abas, blocos e ações de contato respeitam o papel do usuário: uma aba ou bloco que o papel não pode ver **não aparece** (não mostrar desabilitado nem "sem permissão"). Ex.: atendente não vê WhatsApp/Ligar/E-mail nem CPF; recepção não vê Anamnese nem Fotos.

---

# 22. Página de cadastro

A tela de cadastro deve priorizar preenchimento rápido.

Estrutura:

```text
Novo cliente                    Cancelar   Salvar cliente

────────────────────────

Foto

Informações pessoais

Informações adicionais

Tags

────────────────────────

Cancelar              Salvar cliente
```

O cadastro deve utilizar as mesmas regras visuais do restante do produto.

- O cadastro de um cliente novo **não tem abas**: Anamnese, Procedimentos, Financeiro, Fotos, Documentos e Histórico não existem antes do cliente ser salvo. Após salvar, navegar para a página do cliente (seção 21), onde as abas passam a existir.
- Editar cliente reutiliza o mesmo formulário, dentro da aba "Dados Pessoais" do perfil.
- Referência visual: `/design/referencias/cliente-novo-dados-pessoais.webp` (desconsiderar a barra de abas do print e o nome "Belleza", que é placeholder).

---

# 23. Ícones

Utilizar uma única família de ícones.

Biblioteca oficial do produto: **Phosphor** (`@phosphor-icons/react`).

- Peso `regular` sempre, inclusive em itens ativos (decisão do produto: nada de ícone preenchido). `fill` apenas em ícones de marca de terceiros (ex. WhatsApp).
- Não misturar com Lucide, Tabler ou outra família.
- Nunca desenhar SVG de ícone à mão; se faltar um glifo, escolher o mais próximo da própria Phosphor.
- Exceção de cor documentada: o ícone do WhatsApp usa o verde do canal (token `--color-whatsapp`), somente nesse ícone.

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

# 24.0 Paginação e imagens

**Paginação** (toda lista paginada do produto usa o mesmo componente):
- Rodapé: "Mostrando X a Y de Z <itens>" + seletor "Por página" (10, 30, 50, 100; padrão **10**) à esquerda; primeira / anterior / números com reticências / próxima / última à direita. Página atual em `primary-600`.
- A quantidade escolhida fica lembrada no navegador, com uma chave por tipo de lista (todas as listas de clientes compartilham a mesma).
- Trocar filtro, busca ou quantidade por página volta para a página 1.

**Envio de imagem** (foto de cliente, avatar, foto da equipe, logo):
- Escolher o arquivo sempre abre o modal **"Ajustar …"** com área de recorte, zoom (controle deslizante) e arrastar para posicionar; botões "Cancelar" e "Usar imagem".
- Pessoas: máscara redonda. Logo: quadrado, podendo afastar além da borda (logos largos cabem inteiros, com fundo transparente).
- Saída sempre quadrada em WebP (800px fotos de clientes, 400px avatares/equipe, 512px logo).

---

# 24.1 Avisos e módulos futuros

- **Aviso transitório** (toast): centralizado no rodapé, some em ~4s, apenas para confirmar ações concluídas ("Cliente cadastrado") ou erros de ações sem formulário. Erro de formulário fica no próprio formulário, nunca em aviso.
- **Módulo ainda não disponível:** aparece no menu desabilitado com o selo "Em breve" (expandido) ou tooltip "(em breve)" (recolhido). Abas ainda não implementadas mostram empty state explicando o que existirá ali, sem prometer datas.
- **Confirmação de ação destrutiva:** diálogo modal com título, consequência explicada e botão `danger` com o verbo da ação ("Anonimizar definitivamente"), nunca "OK".

---

# 24.2 Campos numéricos (nunca texto livre)

- **Duração:** dois seletores, **horas** (0 a 8) e **minutos** (de 5 em 5, 00 a 55), guardando minutos totais; mínimo 5 min. Para intervalos, o mesmo componente com limite menor e permitindo zero. Mostrar em linguagem natural o efeito ("na agenda ocupa 1h 40min").
- **Dinheiro:** `R$` fixo à esquerda, alinhado à direita, aceita só dígitos e preenche pelos centavos (18000 vira 180,00). Guardar em **centavos**.
- **Percentual:** `%` fixo à direita; só dígitos e uma vírgula, até 2 casas, **máximo 100** (150 vira 100; colar "30%" ou letras limpa). Guardar em centésimos de % (30% = 3000).
- **Inteiro com unidade** (dias, sessões): unidade fixa dentro do campo, só dígitos, com máximo; vazio significa "sem valor" (ex.: "Sem validade").
- Componentes únicos em `src/ui/CamposNumericos.tsx`.

# 24.25 Gestão de categorias

- Tela própria dentro do grupo do catálogo ("Categorias"), com abas por tipo (Serviços; Produtos "Em breve"). Campo "Nova categoria" no topo (Enter cria), lista com contagem de uso, lápis (renomear no lugar, Enter salva, Esc cancela) e lixeira.
- Nome único ignorando maiúsculas; erro claro quando repetido.
- Excluir sempre com confirmação que **diz o impacto** (quantos itens ficam "Sem categoria" e quantas regras ligadas serão removidas). Nunca bloquear a exclusão por estar em uso.

# 24.3 Cadastro em abas com um único salvar

- Registro com várias seções (ex.: serviço) usa **abas** e um botão **Salvar** no cabeçalho que grava todas as abas juntas. Aba com campo inválido recebe um **ponto vermelho** e o salvar leva à primeira aba com erro.
- Cadastro **novo** mostra só o essencial (sem abas); as abas completas aparecem depois de salvar.
- Alterações não salvas: aviso discreto em faixa `warning` e confirmação ao sair ("Descartar alterações").
- Quem só pode consultar vê as mesmas abas em modo leitura (campos desabilitados, sem botões de ação) e uma etiqueta "Somente consulta"; abas sensíveis (custo, comissão) nem aparecem.
- Escolha única entre 2 a 3 opções curtas: controle **segmentado** (barra), não select.

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

  --background: #E9EEF4; /* token fundo */
  --surface: #FFFFFF;

  --text-primary: #0F172A;
  --text-secondary: #475569;
  --text-muted: #64748B;

  --border: #E2E8F0;

  --success: #16A34A;
  --warning: #D97706;
  --danger: #DC2626;

  /* Typography */
  --font-family: Geist, "Segoe UI", system-ui, sans-serif;
  --font-mono: "Geist Mono", ui-monospace, monospace;

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

## Implementação dos tokens (Tailwind v4)

A estilização é feita com **Tailwind CSS v4** (plugin `@tailwindcss/vite`). Os tokens acima são declarados **uma única vez** no CSS de entrada, via `@theme`, e passam a ser as únicas opções disponíveis nas classes:

```css
@import "tailwindcss";

@theme {
  --font-sans: Geist, "Segoe UI", system-ui, sans-serif;
  --font-mono: "Geist Mono", ui-monospace, monospace;

  --color-fundo: #E9EEF4; /* fundo da página */
  --color-primary-50: #F0F6FC;
  /* ... toda a escala primary, gray, success, warning, danger das seções 3 e 4 */

  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;

  --shadow-sm: 0 1px 2px rgba(15, 23, 42, 0.04);
  --shadow-md: 0 4px 12px rgba(15, 23, 42, 0.06);
  --shadow-lg: 0 8px 24px rgba(15, 23, 42, 0.08);
}
```

Regras:

- Usar as classes geradas pelos tokens (`bg-primary-600`, `text-gray-900`, `rounded-md`, `p-4`). A escala padrão de espaçamento do Tailwind (múltiplos de 4px) já equivale à seção 6.
- **Proibido valor arbitrário** de cor, espaçamento ou raio (`bg-[#3a7bd5]`, `p-[13px]`, `rounded-[10px]`). Se um valor não existe no `@theme`, ou ele não deveria ser usado ou precisa ser adicionado aqui na skill primeiro.
- Remover do `@theme` as paletas padrão do Tailwind que não fazem parte do produto, para que não possam ser usadas por engano.
- Padrões repetidos (botão, input, badge, card) viram componentes React reutilizáveis, não listas de classes copiadas entre telas.

---

# 29.1 Roadmap visual

Itens decididos, mas fora do escopo atual:

- **Modo escuro.** Não implementar agora. Para não bloquear o futuro: toda cor sai dos tokens (nunca hex solto nos componentes), de forma que o modo escuro seja apenas um segundo conjunto de valores para os mesmos tokens.

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

## Agenda (grade de horários)

- **Cartões por status**, sempre com cor + borda esquerda de 4px + texto (nunca só cor): agendado (branco, cinza), confirmado (primary-50), concluído (success-50), faltou (warning-50), cancelado (cinza tracejado e riscado), bloqueio (cinza-100).
- **Grade:** coluna de horas fixa à esquerda, cabeçalho da coluna com avatar + nome + cargo, linha vermelha (`danger-600`) na hora atual, faixas fora do horário de trabalho em `gray-50`. Clicar num horário vazio abre o formulário já preenchido. No celular, o dia mostra uma profissional por vez com seletor.
- **Alternância Dia / Semana / Mês** em controle segmentado; profissional em foco em `Select` na semana e no mês.
- **Formulário em modal:** tipo (Agendamento | Bloqueio) no topo, cliente, serviço, profissional, data, horário, duração e observação. Avisos (choque, folga, fora do horário) em caixa âmbar, sem impedir o salvar. Depois de salvar, um passo de sucesso oferece o botão de WhatsApp (verde oficial do canal, único uso do token `whatsapp`).
- **Menu lateral:** só itens que existem; sem selo "Em breve".
- **Configuração de negócio** (categorias, horário, mensagens) fica numa aba "Personalização" de Configurações, não em telas soltas no menu.

## Cadastro longo em uma página (seções recolhidas)

- No lugar de abas, o cadastro traz o essencial em cima (o que o caso comum exige) e, abaixo, **seções recolhíveis**: ícone em quadrado `primary-50`, título, resumo de uma linha do que está preenchido e seta. Um único botão Salvar no fim da página.
- Seção com erro de validação abre sozinha e mostra um ponto vermelho. Seções de dado sensível (custo, comissão) simplesmente não existem para quem não pode vê-las.
- **Campo de pesquisa para escolher entre muitos itens** (ex.: serviço no agendamento): caixa com lupa, lista filtrada sem acento embaixo (fluxo normal, não flutuante, para não ser cortada pela rolagem do modal), setas + Enter + Esc, e o escolhido vira um cartão com "Trocar".

## Foto: um controle só

Foto de pessoa, serviço, cliente ou logo usa o mesmo componente: a imagem (redonda para pessoas, quadrada para serviços e logos, com as iniciais quando não há foto), um **ícone de câmera no canto** para adicionar ou trocar e, quando há foto, um **ícone de lixeira ao lado direito** da imagem para remover (com rótulo "Remover foto"), com o mesmo recorte e as mesmas mensagens. Nunca criar um controle de foto novo por tela: cada variação vira um botão que falta em algum lugar (ex.: sem "Remover" no cadastro de profissional).

## Meu perfil (quem não gere o estúdio)

Campos que a pessoa não altera aparecem desabilitados (cinza), com um aviso curto explicando a quem pedir a correção. O que ela pode alterar (foto e nome social) fica no topo, editável. Para a gestão, o mesmo cartão vira totalmente editável, com um atalho para a ficha completa.

## Matriz de permissões

Tabela com uma coluna por função e um **interruptor** (`role="switch"`, com rótulo acessível "permissão: função") por linha; permissões agrupadas por assunto com título; descrição curta embaixo de cada uma; aviso em âmbar para dado pessoal; "alterado" em âmbar sob o interruptor que mudou; "Restaurar padrão" no cabeçalho de cada coluna; um único botão Salvar e o aviso "Alterações não salvas". Pré-requisito desligado deixa o interruptor dependente desabilitado com a explicação.

## Escolher muitos itens agrupados

Lista agrupada por categoria abre com os **grupos recolhidos**: cada grupo tem uma caixa que marca o grupo todo (estado parcial quando só alguns estão marcados), título com contagem ("3 serviços • 2 marcados") e seta para expandir. A **pesquisa** abre sozinha os grupos com resultado e filtra os itens; marcar o grupo com pesquisa ativa marca só o que está visível.

## Estoque nas listas

Situação do estoque com **ponto de cor + texto** (nunca só cor): verde normal, âmbar "baixo" (chegou ao mínimo), vermelho "Esgotado". Um aviso âmbar no topo da lista resume quantos produtos precisam de reposição e leva ao filtro. Custo e margem são colunas só da gestão, com a nota "aparecem apenas para a proprietária e as administradoras".
