---
name: ds_design-system-base
description: Design system genérico para sistemas web de gestão (SaaS, painéis administrativos, CRMs, ERPs, agendas, financeiro, estoque etc.) em React + Tailwind v4. Define princípios visuais, tokens (cores, tipografia, espaçamento, raios, sombras), estrutura de layout (menu lateral flutuante + barra superior), componentes (botões, campos, formulários, tabelas, cards, badges, abas, modais, avisos), estados, ajuda contextual, responsividade, acessibilidade e checklist de revisão. Use ao iniciar o visual de qualquer sistema novo, criar ou revisar telas e componentes, ou quando o usuário pedir um visual "profissional", "limpo", "SaaS", "moderno" sem um design system próprio. Não define telas de negócio específicas: cada produto cria a sua skill de design derivada desta (com marca, paleta e telas próprias), e a derivada prevalece.
---

# Design System Base

Base visual reutilizável para sistemas de gestão. Não descreve telas de nenhum negócio: descreve **como** qualquer tela deve ser construída. Cada produto parte daqui e registra, numa skill própria, apenas o que é dele (marca, cor primária, fonte, telas, exceções).

## 0. Como usar em um sistema novo

1. **Definir a marca**: cor primária (uma só), fonte, logo (símbolo + horizontal + vertical, de preferência SVG).
2. **Gerar a escala da cor primária** (50 a 900) a partir da cor da marca, mantendo o 600 com contraste AA sobre branco para texto de botão.
3. **Criar a skill derivada** do produto (ex. `<produto>_design-system`) com: marca, paleta final, itens do menu, telas-chave e exceções. Tudo que não estiver lá segue esta base.
4. **Declarar os tokens uma única vez** no CSS de entrada (seção 10) antes de escrever qualquer tela.
5. **Guardar referências visuais** (prints, logo) em `design/referencias/` no repositório do produto.

Em caso de conflito: skill derivada do produto > esta base > outras skills de estética genéricas.

---

## 1. Princípios

- **Profissional antes de sofisticado.** A sofisticação vem de consistência, espaço, tipografia e hierarquia, não de enfeites.
- **Menos elementos, melhor hierarquia.** Na dúvida entre adicionar e remover, remover.
- **Uma ação principal evidente por tela.**
- **Densidade média**: telas de trabalho diário, nem galeria de arte nem cockpit.
- **Neutralidade**: o visual não deve assumir um público (gênero, idade, nicho) a menos que a marca peça.
- **Nenhum elemento decorativo sem função.** Nada de gradientes chamativos, brilhos, sombras fortes, ilustrações grandes, ícones gigantes.
- **Uma tela nova deve parecer do mesmo produto** mesmo para quem nunca viu as outras.

## 2. Cores

### 2.1 Estrutura

| Grupo | Uso |
|---|---|
| `primary-50…900` | Cor da marca. Ações principais, item ativo, foco, links. Usar com moderação. |
| `gray-50…900` | Neutros frios (slate). Textos, bordas, fundos. Uma única família de cinza no produto inteiro. |
| `success`, `warning`, `danger` (50, 100, 500, 600, 700) | Somente para comunicar estado. Nunca decoração. |
| `fundo` | Cor do fundo da página (token semântico, ver 2.3). |

Uma única cor de destaque (a primária). Não introduzir uma segunda cor de marca em uma seção isolada.

### 2.2 Paleta padrão (ponto de partida; trocar a primária pela da marca)

```css
/* Primária (exemplo: azul sóbrio) */
--color-primary-50:  #F0F6FC;  --color-primary-100: #E2EEF9;
--color-primary-200: #C9DFF2;  --color-primary-300: #A8CBE9;
--color-primary-400: #78AEDD;  --color-primary-500: #4F8FCC;
--color-primary-600: #3476B4;  --color-primary-700: #285E92;
--color-primary-800: #244E76;  --color-primary-900: #213F5F;

/* Neutros */
--color-gray-50:  #F8FAFC;  --color-gray-100: #F1F5F9;
--color-gray-200: #E2E8F0;  --color-gray-300: #CBD5E1;
--color-gray-400: #94A3B8;  --color-gray-500: #64748B;
--color-gray-600: #475569;  --color-gray-700: #334155;
--color-gray-800: #1E293B;  --color-gray-900: #0F172A;

/* Semânticas */
--color-success-50: #F0FDF4; --color-success-100: #DCFCE7; --color-success-500: #22C55E; --color-success-600: #16A34A; --color-success-700: #15803D;
--color-warning-50: #FFFBEB; --color-warning-100: #FEF3C7; --color-warning-500: #F59E0B; --color-warning-600: #D97706; --color-warning-700: #B45309;
--color-danger-50:  #FEF2F2; --color-danger-100:  #FEE2E2; --color-danger-500:  #EF4444; --color-danger-600:  #DC2626; --color-danger-700:  #B91C1C;
```

Evitar como cor de marca padrão: roxo/violeta "de IA", rosa predominante, neon, gradientes. Se a marca exigir, aplicar com a mesma contenção.

### 2.3 Aplicação

| Elemento | Token |
|---|---|
| Fundo da página | `fundo` = `#E9EEF4` (entre gray-100 e gray-200). **Nunca `gray-200`**: é a cor das bordas e os contornos dos cards somem |
| Cards, menu, painéis, campos | `white` |
| Superfícies internas neutras (cabeçalho de tabela, hover de linha, área de upload) | `gray-50` |
| Bordas | `gray-200`; bordas de campos `gray-300` |
| Texto principal | `gray-900` / `gray-800` |
| Texto secundário | `gray-600` / `gray-500` |
| Placeholder, desabilitado | `gray-400` |
| Ação principal | `primary-600`, hover `primary-700` |
| Seleção / item ativo | fundo `primary-50`, texto/ícone `primary-700`/`600` |
| Foco | anel `primary-500` / `primary-100` |

Exceções de cor (ex. verde do WhatsApp num ícone de canal) são permitidas quando a cor é parte da identidade de terceiros, ficam num token próprio e restritas àquele ícone. Registrar na skill derivada.

### 2.4 Modo escuro

Não é obrigatório na primeira versão. Para não bloquear o futuro: **toda cor sai de token**, nunca hex solto em componente. O modo escuro passa a ser um segundo conjunto de valores para os mesmos tokens.

## 3. Tipografia

- **Uma família sans para tudo** + a variante mono da mesma família para números em coluna. Padrão sugerido: **Geist + Geist Mono**. Alternativas: Satoshi, Outfit, Cabinet Grotesk. Evitar Inter como escolha automática e evitar serifas em sistemas.
- Fonte self-hosted (`@fontsource` ou pacote da fonte), `font-display: swap`. Não linkar Google Fonts em produção.
- Números que se comparam (valores em R$, quantidades, datas em tabela): `tabular-nums` / mono, alinhados à direita quando monetários.

| Papel | Tamanho | Peso | Cor |
|---|---|---|---|
| Título da página | 28–32px | 600 | gray-900 |
| Título de seção/card | 18–20px | 600 | gray-900 |
| Texto principal | 15–16px | 400/500 | gray-800/900 |
| Texto secundário | 13–14px | 400 | gray-500/600 |
| Label de campo | 14px | 500 | gray-700 |
| Mínimo absoluto | 12px | | |

Hierarquia por peso e cor, não por tamanhos gritantes. Nenhum texto abaixo de 12px.

## 4. Espaçamento, raios e sombras

- **Espaçamento em múltiplos de 4px** (escala padrão do Tailwind). 16px entre itens relacionados, 24px entre grupos, 32px entre seções, 40–48px em separações maiores.
- **Raios**: `sm 6px`, `md 8px` (botões, campos, itens de menu), `lg 12px`, `xl 16px` (cards, menu lateral, painéis, modais), `full` (avatares, badges, tags). Um sistema de raios só, aplicado igual em todo o produto.
- **Sombras quase imperceptíveis**, tingidas do cinza do produto, nunca pretas:
  - `sm: 0 1px 2px rgba(15,23,42,.04)`
  - `md: 0 4px 12px rgba(15,23,42,.06)` (menus suspensos, avisos)
  - `lg: 0 8px 24px rgba(15,23,42,.08)` (modais, painéis laterais)
- Preferir **borda + fundo** a sombra. Card funciona sem sombra.

## 5. Layout

```text
┌──────┐  ┌──────────────────────────────────────┐
│ Menu │  │ Busca                     ?   Perfil │  ← barra superior (sem caixa)
│ flu- │  ├──────────────────────────────────────┤
│ tuan-│  │                                      │
│ te   │  │           Conteúdo (cards)           │
│      │  │                                      │
└──────┘  └──────────────────────────────────────┘
```

### 5.1 Menu lateral flutuante

- Painel branco com borda `gray-200`, `shadow-sm` e raio `xl`, **afastado 12–16px** do topo, da base e da esquerda da tela (não colado na lateral).
- Recolhido: 72–88px, só ícones centralizados, **tooltip** com o nome no hover/foco, **símbolo da marca** no topo.
- Expandido: ~240px, ícone + texto, **logo horizontal** no topo. Botão circular de expandir/recolher na borda direita. A preferência fica salva no navegador.
- Desktop: expandir **empurra** o conteúdo. Tablet/mobile: o menu abre sobreposto com fundo escurecido, a partir de um botão na barra superior.
- Item ativo: fundo `primary-50`, ícone `primary-600`. Ícones **sempre só contorno**, inclusive no item ativo: o destaque vem do fundo e da cor.
- Topo do menu: identidade de quem usa o sistema (logo + nome da empresa/cliente do produto; sem logo, iniciais sobre a cor primária). A marca do produto, quando o sistema é vendido para outras empresas, fica discreta no rodapé do menu.
- Parte inferior: somente ações secundárias (ex. Ajuda). **O perfil do usuário não fica no menu lateral.**
- Itens visíveis dependem da permissão do usuário. Módulo ainda não disponível aparece desabilitado com selo "Em breve" (expandido) ou tooltip "(em breve)" (recolhido), em vez de sumir.

### 5.2 Barra superior

- Mesmo fundo da página (token `fundo` com leve transparência e blur), fixa no topo, **sem borda nem caixa**.
- Ordem: busca global (campo branco, largo) → ajuda (`?`) → notificações (somente se existirem de verdade) → perfil (avatar + nome + papel/cargo, com menu "Meu perfil" e "Sair").
- Nada de métricas, banners ou saudações na barra.

### 5.3 Conteúdo

- Cada tela é composta por **cards brancos** (raio `xl`, borda `gray-200`) sobre o fundo. O card principal contém o cabeçalho da tela (título + descrição curta + ação principal à direita).
- Largura fluida com padding lateral de 16px (mobile) a 24px (desktop).
- Navegação interna por **abas** (texto 15px, indicador inferior de 3px na cor primária, sem caixas). Aba de recurso futuro aparece desabilitada com selo "Em breve" e links de volta ("← Voltar para …"). Evitar menus horizontais extras.

## 6. Componentes

### Botões

| Variante | Visual | Uso |
|---|---|---|
| Primário | fundo `primary-600`, texto branco | Uma ação principal por área ("Salvar", "Novo …") |
| Secundário | branco, borda `gray-300`, texto `gray-700` | Cancelar, voltar, ações alternativas |
| Ghost | sem fundo, texto `primary-600`, hover `primary-50` | Ações de baixa prioridade ("Ver todos", "Editar") |
| Perigo | fundo `danger-600`, texto branco | Somente ações destrutivas, com o verbo da ação |

Altura 44px (normal) ou 36px (compacto), raio `md`, ícone de 18px à esquerda opcional, rótulo em uma linha e com no máximo 3 palavras. Estado de carregamento troca o ícone por um indicador girando e desabilita o botão. `:active` com leve `scale(0.98)`. Botão só de ícone exige `aria-label` e tooltip.

### Campos

- Altura 44px, borda `gray-300`, raio `md`, fundo branco, texto 15px.
- **Label sempre acima** (nunca placeholder como label), obrigatórios com `*` em `danger-600`, **erro abaixo** do campo em `danger-600` 13px, ajuda abaixo em `gray-500`.
- Foco: borda `primary-500` + anel `primary-100`. Erro: borda `danger-500`.
- Máscaras para documentos, telefone e CEP; datas com o seletor nativo; selects com seta própria.
- Preencher automaticamente o que for possível (ex. endereço pelo CEP) e mover o foco para o próximo campo útil.

### Formulários

- Divididos em **seções** com título (18px) + descrição curta; campos sem relação não dividem a mesma seção.
- Grid responsivo: 1 coluna no mobile, 2–4 no desktop conforme o tamanho natural dos campos.
- Não pedir dado que pode ser derivado de outro.
- Ao salvar com erro: marcar todos os campos inválidos e **levar o foco ao primeiro**. Erros de formulário ficam no formulário, nunca em aviso flutuante.
- Criação de um registro mostra só o necessário para criá-lo. Abas de dados relacionados (histórico, anexos, financeiro) só existem depois que o registro foi salvo.
- Ações do formulário: "Cancelar" (secundário) + "Salvar …" (primário), no topo em telas de criação e ao final em edições.

### Cards

- Agrupam informações relacionadas: título + descrição opcional + conteúdo.
- **Não criar um card por informação** e não aninhar cards sem necessidade.
- Indicadores (KPIs) em cards compactos só quando o número orienta uma decisão.

### Tabelas e listas

- Tabela quando há comparação entre muitos registros: cabeçalho discreto em `gray-50` com cantos arredondados, linhas com separador `gray-100`, altura confortável (~64px), hover `gray-50`, linha inteira clicável.
- Primeira coluna com avatar/identificador + nome + linha secundária (13px). Texto que não deve quebrar (nomes, datas, valores) com `nowrap`.
- Célula vazia mostra texto do vazio em `gray-400/500` ("Sem …"), nunca travessão.
- Status por badge; ações no fim da linha.
- Filtros acima da tabela: busca larga + selects.
- **Paginação padrão**: "Mostrando X a Y de Z" + seletor "Por página" (10, 30, 50, 100; padrão 10) à esquerda; primeira / anterior / números com reticências / próxima / última à direita. A escolha fica lembrada no navegador por tipo de lista, e mudar filtro/busca/quantidade volta para a página 1. Um único componente de paginação para o produto inteiro.
- **Abaixo de ~1024px a tabela vira lista** (avatar, nome, linha secundária, status).

### Badges, tags e status

- Formato pílula, 28px de altura, texto 13px 500, fundo `-50` + texto `-700` + anel `-100` da cor.
- Status sempre com texto (não só cor).
- Tags usam um conjunto fixo de tons (primary, success, warning, gray). Nada de cores livres.
- Pontos coloridos só quando representam uma categoria real.

### Avatar e imagens

- Circular em listas e menus; quadrado com raio `xl` em cabeçalhos de perfil. Sem foto: iniciais em `primary-700` sobre `primary-50`.
- Sem molduras decorativas. Imagens de pessoas/registros sempre com `alt` descritivo.
- **Envio de imagem com recorte**: escolher o arquivo abre um modal de ajuste (recorte, zoom, arrastar; "Cancelar" / "Usar imagem"). Máscara redonda para pessoas, quadrada para logos (permitindo afastar para caber inteiro com fundo transparente). Saída quadrada, comprimida (WebP) antes do envio.

### Modais e confirmações

- `<dialog>` nativo (foco preso, `Esc` fecha), largura 448–768px, raio `xl`, `shadow-lg`, fundo escurecido leve.
- Confirmação destrutiva: título, **consequência explicada** e botão de perigo com o verbo ("Excluir definitivamente"), nunca "OK".

### Avisos (toasts)

- Centralizados no rodapé, somem em ~4s, somente para **confirmar ação concluída** ou erro de ação sem formulário.

### Menus suspensos

- Painel branco, borda, `shadow-md`, raio `lg`, itens de 36–40px com ícone de 18px. Fecham com clique fora e `Esc`.

### Ícones

- **Uma única biblioteca** no produto. Padrão sugerido: **Phosphor** (`@phosphor-icons/react`), peso `regular` em tudo, inclusive estados ativos; `fill` só em ícones de marcas de terceiros.
- Tamanho 18–20px (22px na barra superior). Nunca desenhar SVG de ícone à mão. Ícones não competem com o texto.

## 7. Estados

Todo componente e toda tela consideram: padrão, hover, foco, ativo, desabilitado, **carregando**, **erro** e **vazio**. Nenhuma tela depende só do estado ideal com dados.

- **Carregando**: esqueletos com o formato do conteúdo final (não spinners genéricos, exceto dentro de botões e na carga inicial do app).
- **Erro**: mensagem clara em faixa `danger-50` com "Tentar novamente".
- **Vazio**: ícone pequeno em círculo `primary-50`, título, uma frase explicando como preencher e, quando fizer sentido, a ação principal. Distinguir "nada cadastrado" de "nada encontrado com esses filtros" (este último oferece "Limpar filtros").
- **Módulo futuro**: estado vazio explicando o que existirá ali, sem prometer datas.

## 8. Ajuda contextual

- Toda tela registra seu conteúdo de ajuda (perguntas e respostas curtas) num arquivo central; o botão `?` da barra superior abre um **painel lateral direito flutuante** com o conteúdo da tela atual, em blocos expansíveis.
- Rodapé do painel: "Falar com o suporte" (e-mail do produto) e a versão do app.
- **Tela nova sem ajuda registrada não está pronta.**

## 9. Responsividade e acessibilidade

**Prioridade**: desktop → tablet → mobile. No mobile: menu vira sobreposto, tabelas viram listas, formulários em 1 coluna, KPIs empilhados, ações secundárias em menu "Mais". Não simplesmente encolher tudo.

**Obrigatório**:
- Contraste WCAG AA (texto 4.5:1, textos grandes 3:1), inclusive placeholders e botões.
- Labels associados aos campos; `aria-invalid` e `aria-describedby` nos erros.
- Navegação completa por teclado e **foco visível** (`outline` 2px `primary-500`).
- Área de clique mínima de 40px.
- Status nunca comunicado só por cor.
- `alt` em imagens relevantes; botões de ícone com `aria-label`.
- Respeitar `prefers-reduced-motion` (animações e transições desligadas).
- Idioma da página definido (`lang`).

**Movimento**: somente transições curtas (150–200ms) de cor, largura do menu e opacidade. Nada de animações em loop, parallax ou efeitos de rolagem em sistemas de gestão.

## 10. Implementação (Tailwind v4)

Tokens declarados **uma única vez** no CSS de entrada, removendo as paletas padrão do Tailwind para que ninguém use cores fora do sistema:

```css
@import "tailwindcss";

@theme {
  --font-sans: "Geist Variable", "Segoe UI", system-ui, sans-serif;
  --font-mono: "Geist Mono Variable", ui-monospace, monospace;

  --color-*: initial;
  --color-white: #FFFFFF;
  --color-transparent: transparent;
  --color-current: currentColor;
  --color-fundo: #E9EEF4;
  /* primary, gray, success, warning, danger: escalas da seção 2.2 */

  --radius-*: initial;
  --radius-sm: 6px; --radius-md: 8px; --radius-lg: 12px; --radius-xl: 16px; --radius-2xl: 20px;

  --shadow-*: initial;
  --shadow-sm: 0 1px 2px rgba(15, 23, 42, 0.04);
  --shadow-md: 0 4px 12px rgba(15, 23, 42, 0.06);
  --shadow-lg: 0 8px 24px rgba(15, 23, 42, 0.08);
}

@layer base {
  body { @apply bg-fundo font-sans text-gray-900; }
  :focus-visible { @apply outline-2 outline-offset-2 outline-primary-500; }
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: .01ms !important; transition-duration: .01ms !important; }
}
```

Regras:
- **Proibido valor arbitrário** de cor, espaçamento ou raio (`bg-[#...]`, `p-[13px]`, `rounded-[10px]`). Se falta um valor, ele entra primeiro nos tokens (e na skill derivada).
- Padrões repetidos viram **componentes** (`Button`, `IconButton`, `Campo`/`Input`/`Select`, `Badge`, `Card`, `Avatar`, `Modal`, `Abas`, `EstadoVazio`, `Skeleton`, `Aviso`), nunca listas de classes copiadas entre telas.
- Uma biblioteca de ícones, uma família tipográfica, um sistema de raios.

## 11. Textos da interface

- Português claro e direto, no registro do usuário final. Verbos concretos nos botões ("Salvar cliente", "Criar convite").
- Sem travessão (`—`) em textos da interface; usar vírgula, ponto ou dois-pontos.
- Sem jargão técnico em mensagens de erro: dizer o que aconteceu e o que fazer.
- Dados de exemplo realistas e locais (nomes, telefones, CPFs válidos); nunca "Fulano", "Lorem ipsum", "Acme".

## 12. Checklist de revisão

- [ ] Usa somente tokens (cores, raios, sombras, espaçamento) e a biblioteca de ícones do produto?
- [ ] Hierarquia clara e **uma** ação principal evidente?
- [ ] Cards usados com moderação, sem card por informação nem cards aninhados?
- [ ] Algum elemento pode ser removido sem perder informação ou operação?
- [ ] Estados de carregando, vazio (sem dados / sem resultados) e erro implementados?
- [ ] Formulário: labels acima, erros abaixo, foco no primeiro erro, sem pedir dado derivável?
- [ ] Tabela vira lista no mobile? Textos importantes sem quebra indevida?
- [ ] Contraste AA, foco visível, navegação por teclado, `aria-label` em botões de ícone?
- [ ] Itens e ações respeitam as permissões do usuário (escondidos, não só desabilitados, quando não permitidos)?
- [ ] Ajuda da tela registrada?
- [ ] A tela parece pertencer ao mesmo produto que as demais?
