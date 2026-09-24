---
name: ds_seguranca-lgpd
description: Segurança e LGPD para sistemas de gestão web (React/Vite + Supabase + Vercel) que guardam dados pessoais ou sensíveis de clientes (saúde, fotos, financeiro). Cobre controle de acesso com RLS por papel, chaves e variáveis de ambiente, arquivos privados, consentimentos, anonimização, auditoria, autenticação (e-mail, SMTP, MFA), publicação (domínio, alertas de navegador, cabeçalhos) e criptografia de campos. Use sempre que for criar ou alterar tabelas, políticas de acesso, upload de arquivos, fluxo de login/cadastro, textos legais, ou antes de publicar/colocar dados reais em produção — mesmo que o usuário não diga "segurança" ou "LGPD". Complementa a ds_app-architecture-skill (estrutura) e a ds_design-system (visual).
---

# Segurança e LGPD

Princípio: **a interface esconde por conveniência; o banco bloqueia por segurança.** Tudo que roda no navegador (código, chaves públicas, filtros, telas escondidas) pode ser lido e contornado por qualquer usuário logado. Nenhuma regra de acesso pode depender só do frontend.

Esta skill é um checklist. Antes de dar uma tarefa por concluída, percorra as seções que ela tocou.

## 1. Controle de acesso (Supabase / Postgres)

- **RLS ligado em toda tabela do schema `public`**, sem exceção. Tabela sem política = ninguém acessa (bom); tabela sem RLS = todo mundo acessa (grave).
- **Uma política por operação** (`select`, `insert`, `update`, `delete`). Não usar `for all` por conveniência: fica fácil liberar escrita onde só se queria leitura.
- Toda tabela de negócio tem `organizacao_id not null`, e toda política começa exigindo que o usuário seja membro ativo dessa organização.
- **RLS é por linha, não por coluna.** Se um papel pode ver a linha mas não alguns campos, esses campos vão para uma **tabela 1:1 separada** com política própria (ex.: `clientes` + `clientes_contato`). Não resolver com `select` de colunas no frontend nem com views sem `security_invoker`.
- Na inserção em tabelas filhas, a política confere que o registro pai pertence à mesma organização (`exists (select 1 from pai where pai.id = ... and pai.organizacao_id = ...)`), para impedir "pendurar" dados em registros de outro tenant.
- **Funções auxiliares de permissão** (`papel_na_org`, `tem_papel`, `pode_ver_...`) são `security definer`, `stable`, com `set search_path = ''` e ficam no **schema `private`**, não exposto pela API. Conceder `usage` no schema e `execute` apenas para `authenticated`.
- Funções chamadas pelo app via RPC (`criar_organizacao`, `aceitar_convite`, `anonimizar_...`) ficam em `public`, são `security definer` e **validam a permissão dentro do corpo** antes de agir. Revogar `execute` de `public` e `anon`.
- Operações irreversíveis (anonimizar, excluir definitivo) só por RPC com checagem de papel; nunca por `delete`/`update` liberado em política.
- Tabelas de histórico (auditoria, consentimentos) **não têm política de update/delete**: são somente inserção.

### Verificação obrigatória

1. Depois de toda migration: rodar os **advisors de segurança** do Supabase e resolver ou justificar cada aviso. Os avisos de "security definer executável" são aceitáveis apenas para as RPCs intencionais listadas acima.
2. **Testar cada papel** antes de liberar uma tela: um bloco `do $$ ... $$` que cria usuários falsos, troca `role`/`request.jwt.claims` com `set_config(..., true)`, executa leituras/escritas como cada papel e termina com `raise exception` contendo o resultado (a exceção desfaz tudo). Incluir sempre um "usuário de outra organização" que não pode ver nada.

## 2. Chaves e variáveis de ambiente

- No frontend, **somente** a URL do projeto e a chave **publicável/anon**. Ela é pública por natureza; a proteção vem do RLS.
- A **service role key nunca** vai para variável `VITE_*` (tudo com esse prefixo entra no bundle público). Se for necessária, só dentro de uma função serverless (`/api`) ou Edge Function.
- `.env.local` fora do git (`*.local` no `.gitignore`), com um `.env.example` sem valores versionado.
- Variável vazia conta como ausente (`valor?.trim() || undefined`) e o app mostra uma mensagem clara em vez de quebrar a tela inteira.
- Ao configurar na Vercel, conferir depois do deploy que o valor chegou (um deploy com variável vazia publica um site em branco).

## 3. Arquivos (fotos, documentos)

- Bucket **privado**, com `file_size_limit` e `allowed_mime_types` definidos.
- Caminho prefixado pela organização e pelo registro: `{organizacao_id}/{registro_id}/arquivo-{timestamp}.webp`. As políticas de `storage.objects` usam `storage.foldername(name)` para aplicar as mesmas regras da tabela correspondente (converter o trecho para uuid com uma função tolerante a erro).
- Acesso por **URL assinada** de validade curta (ex.: 1h), com cache no cliente. Nunca URL pública para dado de cliente.
- Imagem comprimida no navegador antes do upload (redimensionar + WebP): reduz custo e remove metadados EXIF (localização, aparelho).
- Nome de arquivo novo a cada troca (evita cache servindo a imagem antiga) e remoção do arquivo anterior.
- Fotos clínicas (antes/depois) em bucket separado das fotos de perfil, com políticas próprias (quem vê fotos clínicas é um grupo menor).

## 4. LGPD

- **Papéis legais:** o estabelecimento que usa o sistema é o **controlador** dos dados das clientes; o sistema é o **operador**. Isso aparece nos Termos de Uso.
- **Dados sensíveis** (saúde/anamnese, fotos do corpo, biometria) exigem consentimento específico (art. 11). Tratar como sensível por padrão.
- **Consentimentos** ficam numa tabela própria: tipo, concedido (sim/não), versão do termo, data, quem registrou. Revogar = **novo registro** com `concedido = false`; o estado atual é o registro mais recente. Nunca editar ou apagar consentimentos.
- **Textos legais versionados** no código (`TERMOS_VERSAO`, `CONSENTIMENTO_VERSAO`). Mudou o texto, muda a versão; a versão aceita fica gravada junto do aceite. Minutas precisam de revisão de advogado antes de uso real.
- Aceite dos termos: pela usuária no cadastro e pela responsável do estabelecimento ao criar a organização (gravando versão, data e quem aceitou).
- **Direito de exclusão:** preferir **anonimização** (apaga nome, contato, endereço, foto, observações; mantém o registro para histórico financeiro/estatístico) a apagar a linha. Irreversível, somente papel administrador, com confirmação explícita.
- **Arquivar ≠ excluir.** Arquivar só tira da lista principal.
- **Minimização:** não pedir dado que pode ser derivado (idade e aniversário saem da data de nascimento). CPF opcional quando não for obrigatório para a operação.

## 5. Auditoria

- Trigger genérico `after insert/update/delete` nas tabelas com dado pessoal, gravando organização, usuário, ação, entidade, id e **apenas os nomes dos campos alterados**. Nunca copiar os valores (a auditoria viraria uma segunda cópia do dado sensível).
- Visualização de dado sensível (ex.: abrir a anamnese) registrada por uma RPC `registrar_acesso`.
- Auditoria visível somente para o papel administrador.

## 6. Autenticação

- **Site URL e Redirect URLs** do Supabase Auth apontando para o endereço de produção (e `http://localhost:<porta>` para desenvolvimento). Sem isso, links de confirmação/recuperação levam para localhost.
- Em desenvolvimento, fixar a porta do Vite igual à Site URL (ex.: `server.port = 3000`, `strictPort`).
- **SMTP próprio** (Resend, Brevo...) antes de produção: o SMTP padrão do Supabase tem limite de poucos e-mails por hora. Exige domínio próprio verificado; enviar "em nome de" `@outlook`/`@gmail` por terceiros cai em spam ou é bloqueado.
- Troca de e-mail sempre com confirmação por link (a troca só vale depois de confirmada); mostrar o e-mail pendente na tela.
- Senha mínima de 8 caracteres; ativar a proteção contra **senhas vazadas** (plano pago do Supabase).
- **MFA** para o papel administrador quando houver dados sensíveis em produção.
- Convites de equipe por token com validade, vinculados ao e-mail: a conta que aceita precisa ter o mesmo e-mail do convite, e o token é de uso único.
- Logo após o login o token pode ser rejeitado por segundos ("JWT issued at future", diferença de relógio entre serviços). Repetir a chamada com espera curta nesse caso específico, em vez de mostrar erro.

## 7. Publicação

- **Domínio próprio** antes de uso real. Sites novos em subdomínios compartilhados (`*.vercel.app`) com tela de login são marcados como phishing com mais facilidade (Microsoft Defender/SmartScreen, Google Safe Browsing). Se acontecer: pedir revisão no portal de envio da Microsoft Security Intelligence e no Google Search Console.
- Cabeçalhos de segurança na Vercel (`vercel.json` → `headers`): `Content-Security-Policy` restringindo `connect-src` ao Supabase e APIs usadas, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` desligando o que não é usado, `frame-ancestors 'none'`.
- Contas e dados de teste criados diretamente no banco de produção devem ser **removidos antes de uso real** e listados no CLAUDE.md enquanto existirem.
- Planos gratuitos: sem backup automático e com pausa por inatividade (Supabase), e sem uso comercial (Vercel Hobby). Dados reais de clientes exigem plano com backup.

## 8. Criptografia de campos (roadmap)

- O Supabase já criptografa em repouso (disco) e em trânsito (HTTPS). Criptografia de campo é uma camada extra para que nem quem acessa o banco leia o conteúdo.
- Com dados compartilhados por uma equipe, a chave é **por organização**, não por usuário.
- Campo criptografado não permite busca/filtro no banco: para buscar (ex.: CPF), guardar também um **hash com segredo (blind index)** do valor normalizado.
- Candidatos: texto da anamnese, CPF. Chaves fora do banco de dados da aplicação (Vault/segredo do servidor), com rotação planejada.

## Checklist rápido antes de publicar

- [ ] Toda tabela nova com RLS e políticas por operação
- [ ] Advisors de segurança sem avisos não justificados
- [ ] Teste por papel executado (incluindo usuário de outra organização)
- [ ] Nenhuma chave secreta em `VITE_*` nem no git
- [ ] Buckets novos privados, com limites e políticas
- [ ] Auditoria nas tabelas com dado pessoal (sem copiar valores)
- [ ] Textos legais com versão atualizada quando alterados
- [ ] Site URL/Redirect URLs corretas para o ambiente
- [ ] Contas/dados de teste identificados para remoção
