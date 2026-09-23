---
name: app-architecture-skill
description: Padrão de arquitetura para sistemas de gestão CRUD (cadastros + registros ao longo do tempo + relatórios sobre eles) em React + Vite + TypeScript + Zustand, publicados na Vercel, com persistência por blob de estado único em arquivo local ou Supabase (inclusive começando direto pela nuvem). Use sempre que for estruturar um app de gestão do zero, decidir onde colocar lógica de negócio, desenhar o schema de dados/estado, escolher como persistir dados (arquivo local vs. backend na nuvem), ou definir convenções de CRUD por entidade — mesmo que o usuário não use a palavra "arquitetura" explicitamente, só descreva um novo sistema com cadastros e telas de listagem. É puramente sobre estrutura de dados, estado, persistência e lógica de aplicação — não cobre nada de visual/design de interface (cores, tipografia, layout, componentes visuais); para isso existe uma skill de design separada.
---

# Arquitetura de app de gestão (local-first + nuvem opcional)

Padrão arquitetural para qualquer sistema de gestão CRUD-pesado: cadastros, registros que se acumulam ao longo do tempo, relatórios/agregados sobre esses dados. Não é amarrado a nenhum domínio de negócio específico — o mesmo esqueleto serve para controle financeiro, agendamento, estoque, ou qualquer outro sistema de gestão.

A ideia central, que justifica quase toda decisão abaixo: **o app inteiro opera sobre um único blob de estado**, nunca sobre múltiplas fontes de verdade espalhadas. Isso barateia decisões que normalmente são caras — trocar de backend de persistência, gerar relatórios/export, sincronizar entre telas — porque tudo lê e escreve no mesmo lugar.

## Quando aplicar isto

Use este padrão quando: os dados cabem confortavelmente na memória do navegador (milhares a dezenas de milhares de registros, não milhões), o app é usado por poucos usuários por conta (não é algo com escrita concorrente pesada entre muitas pessoas ao mesmo tempo), e o valor de entregar rápido supera o valor de uma arquitetura de banco relacional normalizada desde o dia 1. Se o sistema crescer para precisar de relatórios cross-conta pesados, colaboração em tempo real entre várias pessoas na mesma conta, ou volumes muito grandes de dados, normalizar em tabelas relacionais vira uma migração futura — não um bloqueio para começar.

## Stack

- **React + Vite + TypeScript.** Sem framework com SSR — não há necessidade de renderização em servidor/rotas de servidor para um app que é essencialmente uma SPA autenticada.
- **Zustand** para o estado global. Prefira Zustand a Context/Redux aqui: a API de store única com `set()`/`get()` mapeia diretamente no padrão "um blob de estado" descrito abaixo, sem boilerplate de reducers/actions separados.
- **Nenhuma lib de roteamento.** Navegação entre telas é um campo `screenId` no store (a tela atual muda esse campo, cada tela lê ele para saber se deve renderizar), não um router. Um sistema de gestão com um conjunto fixo de telas — não URLs profundamente linkáveis/compartilháveis — não precisa dessa complexidade.
- **Vite, não Next.js — mesmo publicando na Vercel.** A Vercel detecta Vite sem configuração e serve o build como site estático. SSR, rotas por URL e Server Components não trazem ganho para uma SPA autenticada (não há SEO a indexar, a navegação é por `screenId`, e o backend na nuvem é acessado direto do navegador com segurança garantida por regras de acesso no banco). Se surgir necessidade real de código em servidor (webhook de pagamento, integração com API de terceiros, uso de chave secreta), usar **funções serverless numa pasta `/api`** do próprio projeto Vite — a Vercel as executa sem migrar de framework.
- **Estilização e visual** (Tailwind, fonte, ícones, tokens) são definidos pela skill de design, não aqui.

## Modelagem de dados: um único blob de estado

Todo o estado de negócio do app — todas as entidades, todas as configurações — é um único tipo TypeScript, por exemplo:

```ts
interface AppState {
  entidadeA: EntidadeA[];
  entidadeB: EntidadeB[];
  registros: Registro[];
  configuracoes: Configuracoes;
  meta: { lastModified: string };
}
```

Cada entidade é um array de objetos com `id: string` (gerado via um helper de id único, prefixado por tipo — facilita reconhecer de qual entidade é um id solto no meio de um log). Nunca objetos indexados por id como `Record<string, EntidadeA>` — arrays simples são mais fáceis de serializar, de iterar em telas de listagem, e de normalizar/migrar depois.

Três funções vivem junto da definição do schema e são o único ponto de entrada para criar ou carregar esse estado:

- **`defaultState(): AppState`** — o estado vazio de uma conta nova, antes de qualquer seed.
- **`<entidade>SeedPadrao()`** — dados iniciais opcionais que fazem sentido existir desde o primeiro acesso. Extraia isso do fluxo de persistência para uma função pura junto do schema — assim todo caminho de carregamento usa exatamente o mesmo seed no primeiro acesso, sem duplicar a lista em dois lugares.
- **`normalizeState(raw: unknown): AppState`** — passa por AQUI todo dado carregado de qualquer fonte (arquivo local, registro de um backend na nuvem, backup importado). Preenche campos que não existiam em versões antigas do schema, corrige tipos, remove campos desconhecidos. **Este é o único lugar que precisa saber sobre evolução de schema** — quando você adicionar um campo novo a uma entidade no mês 6 do projeto, adiciona a migração aqui uma vez, e todo caminho de carregamento (arquivo antigo do usuário, conta criada há meses) automaticamente ganha o campo novo com um valor default sensato, sem quebrar.

## Estado da sessão vs. estado persistido

O store tem dois tipos de campo, e é importante não misturá-los:

1. **O blob de dados de negócio** (`data: AppState`) — é o que vai para o disco/banco.
2. **Campos de sessão**, soltos ao lado de `data` no mesmo store, mas explicitamente NÃO persistidos: se há uma fonte de dados conectada agora, qual backend está ativo, a tela atual, filtros/período visíveis na tela, se uma escrita está em andamento (liga um indicador visual), se um menu retrátil está aberto, toggles de exibição que só fazem sentido durante a sessão. Comente no código, ao lado de cada campo desses, que ele não é persistido — é fácil esquecer e tentar salvar um desses por engano.

Regra prática: se ao recarregar a página o valor DEVERIA sumir/resetar, é campo de sessão. Se deveria sobreviver a um refresh (porque é dado real do negócio), vai dentro do blob de dados.

## Lógica de negócio: funções puras, nunca globais

Todo cálculo derivado (agregados, totais, alertas, o que for) vive em arquivos separados por domínio, como **funções puras que recebem o estado (e outros parâmetros relevantes, como um período) explicitamente como argumento** — nunca lendo de uma variável de módulo ou de um singleton importado.

Por que isso importa tanto: qualquer tela, qualquer exportação (PDF, CSV), qualquer teste, pode chamar exatamente a mesma função e ter garantia de bater com o que a UI mostra. Se duas telas precisam do mesmo cálculo, as duas devem importar a mesma função — nunca reimplementar "na mão" uma segunda vez. Uma segunda cópia de uma conta que já existe em outro arquivo é exatamente onde bugs de divergência nascem: quando um dos dois lugares é corrigido depois e o outro não, o app passa a mostrar dois números diferentes para a mesma coisa dependendo da tela.

## Persistência: fachada única sobre local ou nuvem

Duas formas de guardar o estado, pensadas para coexistir (uma não substitui a outra):

- **Local**: uma API de acesso a arquivo do navegador (ex. File System Access API), abrindo/criando um arquivo no disco do usuário. Zero backend, zero conta, dado nunca sai da máquina. Bom default para um usuário único que não precisa acessar de outro dispositivo.
- **Nuvem**: autenticação + um banco simples, com uma linha por usuário guardando o estado inteiro serializado (ex. uma coluna `jsonb`/texto). **Não normalizar em tabelas relacionais desde o início** — guardar o blob inteiro como já está é o que torna essa opção barata de adicionar depois que o app já existe em modo local: nenhuma tela ou cálculo precisa mudar, só a camada de persistência.

O encaixe entre os dois: o store ganha um campo indicando qual backend está ativo, e a função que qualquer tela chama para salvar — uma única `persist()` — vira uma fachada pequena que olha esse campo e delega para a implementação local ou a de nuvem. Todas as telas continuam chamando só `persist()`, sem saber qual backend está ativo. Isso é o que faz adicionar um backend novo (ou trocar de fornecedor no futuro) custar uma tarde, não uma reescrita.

Login/conta na nuvem deve ser **opcional e gated por configuração de ambiente** — se as credenciais não estiverem configuradas nesse ambiente, o app cai graciosamente para só oferecer o modo local, sem quebrar nem mostrar um formulário de login inútil.

### Começar direto pela nuvem (Supabase)

Quando o produto já nasce precisando de acesso em mais de um dispositivo ou por mais de uma pessoa (ex. uma clínica com recepção e profissional), comece **direto com a nuvem** — o modo local passa a ser opcional/futuro, não o ponto de partida. A fachada `persist()` continua existindo desde o dia 1 (com uma implementação só), para que adicionar o modo local depois não mexa em nenhuma tela.

Padrão com Supabase:

- **Tabela única do estado**: uma linha por conta, ex. `app_state (user_id uuid primary key references auth.users, data jsonb not null, updated_at timestamptz not null default now())`. Mesmo princípio de não normalizar desde o início.
- **RLS obrigatório** nessa tabela, com políticas de select/insert/update restritas a `auth.uid() = user_id`. É o RLS que torna seguro acessar o banco direto do navegador — sem ele, qualquer usuário logado leria o estado de todos.
- **Chaves no frontend**: só a URL do projeto e a chave publicável/anon, via `VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY` (em `.env.local`, que não vai para o git, e nas Environment Variables da Vercel). **Nunca** colocar a service role key em variável `VITE_*` — tudo com esse prefixo vai para o bundle público; se precisar dela, só dentro de uma função em `/api`.
- **Arquivos (fotos, documentos)** não entram no blob: vão para o Supabase Storage, com o blob guardando só o caminho/URL. O blob precisa continuar pequeno o suficiente para ser salvo inteiro a cada alteração.
- **Escrita**: `persist()` faz upsert do blob inteiro com debounce (agrupa alterações rápidas em uma escrita), e o campo de sessão "salvando..." indica escrita em andamento. Compare `meta.lastModified` ao carregar/salvar para detectar que outra sessão salvou algo mais novo, em vez de sobrescrever silenciosamente.

Ambos os caminhos de carregamento (abrir arquivo local, entrar numa conta pela primeira vez) usam o mesmo `defaultState()` + seeds + `normalizeState()` descritos acima — nunca duplicar a lógica de "o que é um estado inicial válido" em dois lugares.

## Convenção de ações CRUD por entidade

Cada entidade no store ganha um conjunto pequeno e uniforme de ações, não uma ação por campo:

- **`save<Entidade>(obj)`** — upsert por id: se já existe um item com esse id no array, substitui; senão, adiciona. Uma única ação de salvar serve tanto para criar quanto para editar — o formulário decide se está em modo "novo" ou "editando" olhando se recebeu um id, mas a ação do store é sempre a mesma.
- **`delete<Entidade>(id)`** — remove do array. Considere um soft-delete (uma flag de arquivado no lugar de remover) para entidades que têm histórico vinculado por outras entidades — nesse caso, uma ação de arquivar no lugar de excluir de fato.
- Ações mais específicas só quando a operação genuinely não é um simples upsert — por exemplo, quando precisa também atualizar outra entidade relacionada como efeito colateral.

## Fluxos assíncronos de UI como hooks que devolvem Promise

Confirmação de ação destrutiva, ou perguntar "aplicar esta mudança a este item só ou a um grupo relacionado?", são fluxos onde o código que chamou precisa esperar a resposta do usuário antes de continuar. O padrão que funciona bem: um hook (`useConfirm()`, `useScopeChoice()`, etc.) que devolve uma função que retorna uma `Promise` resolvida quando o usuário responde, mais o elemento a renderizar. Isso deixa o código de chamada linear (`const ok = await confirm(...); if (!ok) return;`) em vez de precisar de callbacks ou de estado espalhado para saber "estamos esperando confirmação de quê agora".

## Validação de formulário como estado, não como efeito colateral

Erros de campo obrigatório devem ser modelados como um objeto de estado local do formulário (`{ [nomeDoCampo]: mensagemDeErro }`), não como um efeito disparado direto na tela (tipo uma notificação temporária). Ao salvar: monta esse objeto checando cada campo, se algum campo tem erro guarda no estado e para; ao editar qualquer campo, limpa o erro correspondente àquele campo. Guardar uma referência (`ref`) por campo permite levar o foco/scroll até o primeiro campo inválido quando a validação falha. Essa é uma decisão de arquitetura de estado (onde mora o erro, quando ele é limpo), independente de como o erro é desenhado na tela.

## Versionamento

Uma constante de versão da aplicação (esquema tipo MAJOR.BUILD), num arquivo dedicado, mostrada em algum "Sobre o app", espelhada no `package.json`. Incrementar o BUILD uma vez por rodada de mudanças entregues (não por cada linha alterada dentro da rodada) — mantém o número significativo sem inflar.

## Erros para não repetir

- Uma tela que recalcula "na mão" algo que já existe como função pura em outro lugar (em vez de importar e chamar) é onde bugs de divergência acontecem — quando o cálculo original é corrigido depois, a cópia reimplementada continua com o bug antigo.
- Um formulário de criação que computa um valor default mas nunca de fato o aplica ao objeto salvo é um bug clássico de copiar-e-colar de uma versão anterior do formulário — revisar se toda variável computada é realmente usada.
- Uma ação de "editar todos os itens de um grupo relacionado de uma vez" que copia campos demais do item editado para os outros — incluindo campos que deveriam ser únicos por item (um índice de posição, uma data específica daquele item) — corrompe os outros itens do grupo. Ao editar em lote, ser explícito sobre exatamente quais campos são compartilhados vs. únicos por item.
