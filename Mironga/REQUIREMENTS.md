# Requisitos do Sistema — Mironga

**Versão:** 1.0
**Data:** 2026-04-17
**Produto:** Mironga — Plataforma Modular para Terreiros de Umbanda
**Plataformas:** iOS 13+, Android 5+, Web (PWA)

Este documento descreve os **Requisitos Funcionais (RF)** e **Requisitos Não Funcionais (RNF)** do Mironga. Ele é a fonte formal de rastreabilidade entre necessidades do produto, implementação técnica e critérios de aceitação.

**Convenções:**

- RF: Requisito Funcional, numerado por módulo (ex.: `RF-JOG-01`).
- RNF: Requisito Não Funcional, numerado por categoria (ex.: `RNF-SEG-01`).
- **Ator(es):** papéis que executam ou sofrem a ação (Usuário aprovado, Admin, Role customizado com permissão X, Sistema).
- **Permissão/Regra:** permissão do sistema ou regra do Firestore associada.

---

## Índice

- [Parte A — Requisitos Funcionais](#parte-a--requisitos-funcionais)
  - [RF-AUT — Autenticação](#rf-aut--autenticação)
  - [RF-ACL — Controle de Acesso](#rf-acl--controle-de-acesso)
  - [RF-PER — Perfil do Usuário](#rf-per--perfil-do-usuário)
  - [RF-TEN — Multi-Tenant](#rf-ten--multi-tenant)
  - [RF-JOG — Módulo Jogos](#rf-jog--módulo-jogos)
  - [RF-EVE — Módulo Eventos](#rf-eve--módulo-eventos)
  - [RF-MAT — Módulo Materiais](#rf-mat--módulo-materiais)
  - [RF-AVI — Módulo Avisos](#rf-avi--módulo-avisos)
  - [RF-EST — Módulo Estoque](#rf-est--módulo-estoque)
  - [RF-FIN — Módulo Financeiro](#rf-fin--módulo-financeiro)
  - [RF-MEN — Módulo Mensalidades](#rf-men--módulo-mensalidades)
  - [RF-NOT — Notificações](#rf-not--notificações)
  - [RF-LGP — LGPD](#rf-lgp--lgpd)
  - [RF-ADM — Administração](#rf-adm--administração)
- [Parte B — Requisitos Não Funcionais](#parte-b--requisitos-não-funcionais)

---

## Parte A — Requisitos Funcionais

### RF-AUT — Autenticação

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-AUT-01 | Cadastro por e-mail e senha | Visitante | Formulário de registro coleta nome, e-mail e senha; valida formato; cria conta no Firebase Auth e perfil no Firestore com `approvalStatus: 'pending'` (exceto se e-mail estiver em `adminEmails`). | Firebase Auth |
| RF-AUT-02 | Login por e-mail e senha | Visitante registrado | Após credenciais válidas, o usuário é redirecionado para o dashboard (se aprovado e perfil completo) ou para a tela de gate correspondente. | Firebase Auth |
| RF-AUT-03 | Login com Google | Visitante | Botão "Entrar com Google" utiliza `expo-auth-session` (web) ou `@react-native-google-signin/google-signin` (nativo); config OAuth por tenant. | Firebase Auth + OAuth por tenant |
| RF-AUT-04 | Login com Apple (iOS) | Visitante iOS | Botão "Entrar com Apple" aparece apenas quando `AppleAuthentication.isAvailableAsync()` retorna `true`; usa nonce SHA-256 e `OAuthProvider('apple.com')`. | Firebase Auth + entitlement `com.apple.developer.applesignin` |
| RF-AUT-05 | Esqueci minha senha | Visitante | Tela `forgot-password` envia e-mail de redefinição via `sendPasswordResetEmail`; mensagem genérica (não revela se e-mail existe). | Firebase Auth |
| RF-AUT-06 | Logout | Usuário autenticado | Ação de logout limpa estado local, marca tokens de push como `active: false` e redireciona para login. | — |
| RF-AUT-07 | Auto-aprovação de administradores | Sistema | Se o e-mail do novo usuário está em `adminEmails` do Firestore, `approvalStatus` é setado como `approved` automaticamente. | Seed do Firestore |
| RF-AUT-08 | Persistência de sessão | Sistema | Sessão persistida pelo Firebase Auth; usuário permanece logado entre aberturas do app. | AsyncStorage (Firebase SDK) |

### RF-ACL — Controle de Acesso

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-ACL-01 | Roles dinâmicos | Admin | Admin cria, edita e exclui roles; cada role possui um conjunto de permissões do sistema. | `admin.manageRoles` |
| RF-ACL-02 | Atribuir roles a usuários | Admin ou role com permissão | Associa um role a um usuário; afeta imediatamente as permissões do usuário (listener em tempo real). | `admin.assignRoles` |
| RF-ACL-03 | Excluir role com usuários vinculados | Admin | Oferece duas opções: reatribuir todos para outro role (bulk) ou atribuir individualmente (lista por usuário). Só então exclui o role. Roles `isDefault: true` não podem ser excluídos. | `admin.manageRoles` |
| RF-ACL-04 | Aprovação de usuário | Admin ou role com permissão | Novos usuários ficam `pending`; admin aprova ou rejeita; usuário recebe notificação push + in-app. | `admin.approveUsers` |
| RF-ACL-05 | Gate de autenticação | Sistema | Usuário não autenticado é redirecionado para `/login`; autenticado sem perfil completo, para `CompleteProfileScreen`; sem consentimento LGPD explícito, para `LgpdConsentScreen`. | `_layout.tsx` |
| RF-ACL-06 | Permissões granulares | Sistema | Cada feature do app é protegida por uma das 40 permissões do sistema (ver tabela `SYSTEM_PERMISSIONS` em `src/types/index.ts`). Verificação via `useHasPermission()` no client e Firestore Security Rules no servidor (check duplo). | 40 permissões |
| RF-ACL-07 | Run As User | Admin | Admin pode impersonar outro usuário (perfil, dados, progresso, permissões e identidade nas escritas). Banner `RunAsBanner` visível. Toda operação usa `effectiveUserId`. | Client-side; bypass em Firestore Rules |
| RF-ACL-08 | Run As Role | Admin | Admin pode simular permissões de outro role mantendo sua identidade. Apenas permissões mudam. | Client-side |
| RF-ACL-09 | Encerrar Run As | Admin | Botão no `RunAsBanner` encerra a impersonação e restaura identidade/permissões reais. | — |

### RF-PER — Perfil do Usuário

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-PER-01 | Visualizar perfil próprio | Usuário aprovado | Tela `(profile)/index.tsx` exibe dados do usuário logado, incluindo dados sensíveis descriptografados (CPF/RG). | `profile.view` |
| RF-PER-02 | Editar perfil próprio | Usuário aprovado | Tela `(profile)/edit.tsx` permite editar todos os campos do perfil; dados sensíveis são re-criptografados; cache de perfis é invalidado (`invalidateProfileCache`). | `profile.edit` |
| RF-PER-03 | Completar perfil (gate) | Usuário aprovado sem perfil completo | Tela `CompleteProfileScreen` é obrigatória até o perfil estar completo. Campos obrigatórios sempre: e-mail, nome completo. Demais: configuráveis por tenant. | Gate em `_layout.tsx` |
| RF-PER-04 | Campos obrigatórios por tenant | Admin | Admin configura, em `tenants/{tenantId}/config/requiredFields`, quais campos são obrigatórios (foto, data de nascimento, telefone, gênero, CEP, endereço, cidade, estado, CPF, RG). | `admin.manageTenant` |
| RF-PER-05 | Auto-completar endereço por CEP | Usuário | Campo CEP integra com API ViaCEP e preenche endereço, cidade e estado automaticamente. | — |
| RF-PER-06 | Upload de foto de perfil | Usuário | Foto é enviada para Firebase Storage (folder `profile-photos`) via `uploadService.ts`; NUNCA base64 no Firestore. | — |
| RF-PER-07 | Exportar dados (LGPD) | Usuário aprovado | Botão "Exportar meus dados" em Perfil → gera JSON estruturado e abre share sheet do sistema (iOS: Arquivos; Android: Downloads). | LGPD Art. 18 VI (portabilidade) |
| RF-PER-08 | Excluir conta (LGPD) | Usuário aprovado | Botão "Excluir minha conta" em Perfil → chama `requestAccountDeletion()` → Cloud Function `onAccountDeletionRequested` limpa dados em cascata. Dados financeiros mantidos (retenção fiscal 5 anos). | LGPD Art. 18 VI (eliminação) |
| RF-PER-09 | Dados sensíveis em subcollection isolada | Sistema | CPF e RG são armazenados em `users/{userId}/sensitive/main`, criptografados AES-256-GCM (`enc:v1:{base64}`); leitura apenas por owner e admin. | Firestore Rules + `fieldEncryption.ts` |

### RF-TEN — Multi-Tenant

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-TEN-01 | Seleção de tenant em build | Desenvolvedor | Variável `MIRONGA_TENANT` define qual arquivo JSON em `tenants/` é usado para gerar `app.config.js`. | Build-time |
| RF-TEN-02 | Tenant context em runtime | Sistema | `TenantContext` / `useTenant()` expõem config do tenant (id, nome, slug, Firebase config, OAuth, EAS, etc.). | Runtime |
| RF-TEN-03 | Isolamento de dados por tenant | Sistema | Todo documento do Firestore reside em `tenants/{tenantId}/...`. | Firestore Rules |
| RF-TEN-04 | Módulos habilitados por tenant | Admin | Campo `enabledModules?: string[]` no JSON do tenant restringe quais módulos estão disponíveis. Omitir = todos. | JSON do tenant |
| RF-TEN-05 | Bottom tabs configuráveis | Admin | Campo `bottomTabModules?: string[]` define ordem e seleção de módulos na barra inferior. Home é sempre o primeiro. | JSON do tenant |
| RF-TEN-06 | Sobrescrita de tema por tenant | Admin | Campo `themeOverrides` permite customizar cores, espaçamentos e tipografia. | JSON do tenant |
| RF-TEN-07 | Config de OAuth por tenant | Admin | `oauth.googleWebClientId`, `oauth.googleIosClientId`, `oauth.googleAndroidClientId` configurados por tenant para Google Sign-In. | `app.config.js` |
| RF-TEN-08 | Info do terreiro editável | Admin | Tela `tenant-edit.tsx` permite editar nome, descrição, logo (upload via Storage, folder `tenant-logos`). Visualização disponível para todos autenticados em `tenant-info.tsx`. | `admin.manageTenant` |

### RF-JOG — Módulo Jogos

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-JOG-01 | Hub de jogos | Usuário aprovado | Tela `jogos.tsx` lista os três jogos (Quiz Solo, Flashcards, Quiz Gerenciado), sujeito a permissões. | `jogos.play` |
| RF-JOG-02 | Criar pasta de assuntos | Role com permissão | Pasta com nome, descrição, ícone, cor, visibilidade (`visible`/`hidden`), `allowedRoles` e `order`. Pasta oculta esconde todos os assuntos dentro dela. | `jogos.manageFolders` |
| RF-JOG-03 | Criar/editar assunto | Role com permissão | Assunto com nome, descrição, ícone, cor, status (`draft`/`published`), `folderId`, `contentTemplate`, `allowedGames`. Status `draft` invisível para jogadores. | `jogos.manageSubjects` |
| RF-JOG-04 | Gerenciar perguntas do assunto | Role com permissão | Subcollection `questions/`: pergunta/afirmação, resposta/explicação, hint opcional. Suporta hierarquia: tópico principal (`isMainContent: true`, não jogável) → subconteúdos (`parentQuestionId`, `templateLabel`, `parentName`). | `jogos.manageQuestions` |
| RF-JOG-05 | Modelos pré-definidos de conteúdo | Role com permissão | CRUD de `ContentTemplatePreset` em `modules/jogos/contentTemplatePresets/`. Não permite editar/excluir preset associado a assuntos (oferece duplicação). | `jogos.manageTemplatePresets` |
| RF-JOG-06 | Remapeamento de modelo | Role com permissão | Ao trocar modelo de um assunto, tela de mapeamento de-para com dropdown por item antigo → novo; auto-map por nome; cria stubs para itens novos. | `jogos.manageSubjects` |
| RF-JOG-07 | Seleção de assuntos | Jogador | Primeiro passo em Quiz Solo e Flashcards: `SubjectSelector` com hero compacto, busca accent-insensitive, toggle "Selecionar todos", cards agrupados por pasta, filtrados por visibilidade/role/`allowedGames`. | `jogos.play` |
| RF-JOG-08 | Quiz Solo | Jogador | Múltipla escolha: 1 resposta correta + 3 erradas (de outras perguntas dos assuntos). Pontuação: +10 acerto, -3 erro (mínimo 0). Streaks: 5→+15, 10→+30, 15→+50, 20+→+75. | `jogos.play` |
| RF-JOG-09 | Flashcards | Jogador | Flip 3D (frente = pergunta, verso = resposta + hint) + swipe (sabia/não sabia) para classificação. | `jogos.play` |
| RF-JOG-10 | Repetição espaçada | Sistema | `QuestionLearningState` rastreia progresso por pergunta. Seleção priorizada: missed NEW (1000) > new (100) > beginner due (80) > intermediate due (60) > mastered due (40) > not-due (10–50). Intervalos: errou 2d, acertou 1x 5d, 2x 10d, dominada 30d. | `jogos.viewProgress` |
| RF-JOG-11 | Progresso de aprendizado | Jogador | Tela `progress.tsx` agrupa `QuestionLearningState` por assunto e exibe estatísticas (domínio, revisões pendentes, por pasta). | `jogos.viewProgress` |
| RF-JOG-12 | Criar sala de Quiz Gerenciado | Moderador (com permissão) | Fluxo em 3 etapas: (1) selecionar perguntas; (2) configurar tempo por pergunta (10–60s) + link "Alterar perguntas"; (3) tutorial; cria sala com código 4 dígitos (1000–9999). | `jogos.manageQuizRoom` |
| RF-JOG-13 | Entrar em sala | Jogador | Tela `managed-quiz.tsx` permite entrar com código de 4 dígitos. `joinRoom` é idempotente (fallback `updateDoc`). Protegido contra double-tap. | `jogos.play` |
| RF-JOG-14 | Jogar Quiz Gerenciado | Jogador | Timer sincronizado via RTDB (`getServerNow()`); pontuação: `(100 + 900 × tempoRestante/tempoTotal) × streakMultiplier` (1.2x–1.5x); erros = 0 pts. Real-time `onSnapshot`. | `jogos.play` |
| RF-JOG-15 | Moderar Quiz Gerenciado | Moderador | Tela `managed-quiz-moderator.tsx`: iniciar, avançar pergunta, ver ranking, encerrar sala. Moderador excluído do ranking. Cleanup no unmount finaliza sala automaticamente. | `jogos.manageQuizRoom` |
| RF-JOG-16 | Ranking do Quiz Gerenciado | Participante | Tela de resultados com ranking ordenado por pontuação total; exibe nome e foto resolvidos em tempo real (não desnormalizados). | — |
| RF-JOG-17 | Proteção de jogo ativo | Sistema | `ActiveGameContext` impede navegação acidental durante sessão (Alert de confirmação em tabs/drawer e botão voltar). | — |

### RF-EVE — Módulo Eventos

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-EVE-01 | Ver calendário de eventos | Usuário | Hub timeline `eventos.tsx` com seções "Próximos" e "Anteriores", busca (nome, local, tipo), `DateRangeFilter` e `FilterDropdown` (tipos + com materiais + com tarefas). | `eventos.view` |
| RF-EVE-02 | Criar tipo de evento | Role com permissão | CRUD de `EventType` (nome, ícone, cor) em `modules/eventos/eventTypes/`. | `eventos.manageEventTypes` |
| RF-EVE-03 | Criar evento | Role com permissão | Evento com nome, tipo, datas, local, descrição, observações, `allowedRoles`, `rsvpEnabled`, `doubleCheckEnabled`, `materialEditors`. | `eventos.create` |
| RF-EVE-04 | Editar/excluir evento | Criador ou role com permissão | Criador ou permissão `eventos.manage` pode editar/excluir. Exclusão dispara cleanup de notificações in-app. | `eventos.manage` |
| RF-EVE-05 | RSVP | Usuário | Botões de RSVP em `evento-details.tsx` (`confirmed`/`maybe`/`declined`). Subcollection `rsvps/{userId}`. Listagem só mostra indicador de status. | `eventos.view` |
| RF-EVE-06 | Check-in (double-check) | Role com permissão ou criador | Se `doubleCheckEnabled: true`, staff marca `checkedIn` em RSVPs. Pode ser ligado/desligado após criação. | `eventos.checkAttendance` |
| RF-EVE-07 | Material de evento | Criador, `materialEditors`, role com permissão | Subcollection `materials/`. Tipos `link` e `pdf`. `accessMode: 'all'` ou `'checked_in_only'` (só aparece se `doubleCheckEnabled`). Upload folder `event-materiais`. | `eventos.manage` |
| RF-EVE-08 | Tarefa de evento | Criador ou role com permissão | Subcollection `tasks/`. Título, descrição, `assigneeIds`, status (`pending`/`in_progress`/`done`), deadline. Assignees podem alterar status da própria tarefa. | `eventos.manageTasks` |
| RF-EVE-09 | Minhas tarefas | Usuário atribuído | Dashboard mostra até 3 tarefas pendentes (seção condicional). "Ver todas" → tela `/my-tasks` com busca fuzzy e filtros por status. | — |
| RF-EVE-10 | Contadores desnormalizados | Sistema | `materialCount` e `taskCount` atualizados via `increment()`. | Firestore Rules (AuditFields) |
| RF-EVE-11 | Visibilidade por role | Sistema | `allowedRoles` oculta eventos para usuários sem role compatível. Admins sempre veem. | Client + Firestore Rules |

### RF-MAT — Módulo Materiais

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-MAT-01 | Navegar por pastas aninhadas | Usuário | `materiais.tsx` com breadcrumb e navegação por estado interno (não empilha rotas). `parentFolderId: undefined` = raiz. | `materiais.view` |
| RF-MAT-02 | Criar pasta | Role com permissão | Pasta com nome, descrição, ícone, cor, `parentFolderId`, `visibility`, `allowedRoles`. Herança: pasta oculta/restrita oculta subpastas e itens. | `materiais.manageFolders` |
| RF-MAT-03 | Criar item | Role com permissão | `MaterialItem` com tipo (`link`, `pdf`, `audio`), `folderId` obrigatório. Upload via `uploadService.ts` folder `materiais`. | `materiais.manageItems` |
| RF-MAT-04 | Visualizar PDF restrito | Usuário | WebView + pdf.js. `restrictedView: true` → badge "Restrito", bloqueio de seleção, screenshot prevention (`expo-screen-capture`), sem export/print. | `materiais.view` |
| RF-MAT-05 | Visualizar PDF externo | Usuário | `restrictedView: false` → botão "Abrir externamente" no header obtém download URL via `getDownloadURL()` e abre com `Linking.openURL()`. | — |
| RF-MAT-06 | Busca e zoom no PDF | Usuário | Busca textual com highlight; pinch-to-zoom nativo do WebView; indicador de página e go-to-page. | — |
| RF-MAT-07 | Ouvir áudio | Usuário | `expo-audio`. Mini-player global flutuante (acima da tab bar) continua tocando ao navegar; tap abre player expandido (fullscreen) com seek bar, skip −15/+15, velocidade 0.5x–2x e loop. | — |
| RF-MAT-08 | Cache offline | Sistema | PDFs e áudios baixados no primeiro acesso e cacheados em `FileSystem.documentDirectory`. Indicadores de status cached. Abre sem internet se cacheado. | — |
| RF-MAT-09 | FAB expansível | Role com permissão | Botão "+" expande para "Nova pasta" e "Novo item" (quando dentro de pasta). Backdrop glassmorphism (blur iOS/Android). 1 opção = FAB direto. | — |
| RF-MAT-10 | Materiais de eventos na raiz | Usuário | Eventos com materiais aparecem como FolderCards na seção "Eventos" na raiz. Respeita `allowedRoles`, `accessMode`, `restrictedView`. Requer módulo eventos + `eventos.view`. | — |
| RF-MAT-11 | Busca global | Usuário | Digitar na raiz mostra direto: pastas (qualquer nível), arquivos (qualquer pasta acessível), materiais de eventos — tudo como ItemCard com `contextLabel`. | — |
| RF-MAT-12 | Material trancado | Usuário sem check-in | Quando `accessMode: 'checked_in_only'` e usuário não tem `checkedIn`, item aparece com ícone de cadeado e texto explicativo. | — |

### RF-AVI — Módulo Avisos

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-AVI-01 | Ver feed | Usuário | `avisos.tsx` com FlatList, pull-to-refresh, filtros (não lidos, pendentes de confirmação, fixados, com mídia) + `DateRangeFilter`. Fixados primeiro. | `avisos.view` |
| RF-AVI-02 | Criar aviso | Role com permissão | Formulário estilo post social: texto (obrigatório), título, imagens múltiplas, PDFs, visibilidade por `allowedRoles`, fixar, requer confirmação (`requiresAck`). Upload folder `avisos`. | `avisos.create` |
| RF-AVI-03 | Editar/excluir aviso | Criador ou role com permissão | Apenas criador e `avisos.manage` podem editar/excluir. Exclusão dispara cleanup de notificações. | `avisos.manage` |
| RF-AVI-04 | Curtir post | Usuário | Toggle coração. Subcollection `likes/{userId}`. `likeCount` via `increment()`. Long-press → bottom sheet `LikersList`. | `avisos.view` |
| RF-AVI-05 | Curtir comentário | Usuário | Toggle coração em comentários. Subcollection `comments/{commentId}/likes/{userId}`. Bottom sheet de curtidas. | `avisos.view` |
| RF-AVI-06 | Comentar | Usuário | Threading 1 nível. `parentCommentId`. Reply de reply fica sob o top-level original. Quote bubble estilo WhatsApp para replies de sub-comentários. | `avisos.comment` |
| RF-AVI-07 | Responder (swipe-to-reply) | Usuário | Gesture Handler: swipe para direita dispara reply com haptic. Banner de reply acima do input, auto-focus, scroll até o comentário respondido. | `avisos.comment` |
| RF-AVI-08 | Excluir comentário | Autor ou role com permissão | Excluir re-parenteia filhos diretos para o avô (atomic via `writeBatch`). Quote bubbles de excluídos mostram "Comentário excluído". | `avisos.comment` |
| RF-AVI-09 | Confirmar leitura automática | Sistema | Ao abrir detalhe: cria `reads/{userId}` + `increment(readCount)`. | — |
| RF-AVI-10 | Confirmação manual ("Li e estou ciente") | Usuário | Se `requiresAck: true`, botão grava `acknowledged: true` + `increment(ackCount)`. | `avisos.view` |
| RF-AVI-11 | Fixar aviso | Role com permissão | Toggle `pinned: true`. Fixados aparecem no topo. | `avisos.manage` |
| RF-AVI-12 | Visualizar "Quem leu" / curtidas | Usuário | Bottom sheets (`ReadersList`, `LikersList`) estilo Instagram com avatares carregados do perfil (resolvidos em tempo real). | — |
| RF-AVI-13 | Carrossel na Home | Usuário | `AvisosCarousel` no dashboard: até 2 posts, 2 linhas cada, avatares 32px, header uppercase, footer com contagem. Unread indicador: dot azul + nome bold. | — |

### RF-EST — Módulo Estoque

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-EST-01 | Ver estoque | Usuário | Hub `estoque.tsx` com tabs fixas; stats; busca; filtros. | `estoque.view` |
| RF-EST-02 | Cadastrar item | Role com permissão | Item com nome, descrição, categoria, unidade, preço e custo (em centavos), threshold de estoque baixo, imagens (até 5, primeira = capa). `resizeMode="contain"` em toda UI; sem `backgroundColor` nos containers. | `estoque.manageItems` |
| RF-EST-03 | Gerenciar categorias | Role com permissão | CRUD de `StockCategory` (nome, ícone, cor). | `estoque.manageCategories` |
| RF-EST-04 | Gerenciar unidades de medida | Role com permissão | CRUD de `UnitOfMeasurement`. Pre-seed: `un`, `kg`, `g`, `L`, `mL`. | `estoque.manageCategories` |
| RF-EST-05 | Gerenciar tipos de movimentação | Role com permissão | CRUD de `MovementType` com `behavior: increase/decrease/set`, `affectsFinance`, `financeType` (`revenue`/`expense`/`none`). Pre-seed: Entrada, Saída, Baixa, Ajuste. | `estoque.manageCategories` |
| RF-EST-06 | Gerenciar métodos de pagamento | Role com permissão | CRUD de `PaymentMethod`. Pre-seed: Pix, Cartão, Dinheiro. | `estoque.manageCategories` |
| RF-EST-07 | Registrar movimentação | Role com permissão | Transação atômica: atualiza quantidade do item conforme `behavior`; valida estoque negativo em `decrease`. Online: `runTransaction()`; offline: `writeBatch()`. | `estoque.manageMovements` |
| RF-EST-08 | Ponto de Venda (POS) | Role com permissão | Jornada via bottom sheets: chips horizontais de categorias → grid com busca → detalhe do produto → carrinho (stepper iFood) → pagamento → success overlay. Carrinho persiste em AsyncStorage. Preço de custo NÃO aparece no POS. | `estoque.sell` |
| RF-EST-09 | Venda normal | Vendedor | `Sale` com `saleType: 'normal'`, gera receita imediata + movimentações. | `estoque.sell` |
| RF-EST-10 | Venda interna (débito) | Vendedor | `Sale` com `saleType: 'internal'` + `debtorUserId`. Estoque é baixado, receita só entra com o pagamento do débito. Cria `UserDebt`. | `estoque.sell` + `financeiro.participant` no devedor |
| RF-EST-11 | Histórico de vendas | Role com permissão | Tela `estoque-sales.tsx`: busca, filtros, detalhe clicável. | `estoque.view` |
| RF-EST-12 | Detalhe de venda | Role com permissão | Tela `estoque-sale-details.tsx`: vendedor (resolvido em tempo real), itens clicáveis (navega para item), status, pagamento, notas. Permite cancelar venda. | `estoque.view` |
| RF-EST-13 | Cancelar venda | Role com permissão | Cria movimentações inversas para restaurar estoque. Em venda interna, cancela débito (pagamentos anteriores não revertidos). | `estoque.sell` |
| RF-EST-14 | Background sync (3 níveis) | Sistema | Nível 1: Firestore SDK (foreground). Nível 2: `expo-task-manager` + `expo-background-fetch` (app fechado, 15min, `stopOnTerminate: false`). Nível 3: NetInfo listener (sync imediato ao reconectar). Fila `@mironga/pendingSync`. Backoff: imediato → 1m → 5m → 15m. | — |
| RF-EST-15 | Valores em centavos | Sistema | Todos os valores monetários são inteiros em centavos. Formatação `R$` apenas na UI. | — |
| RF-EST-16 | Imagens via Storage | Sistema | Imagens de produto enviadas ao Firebase Storage (folder `estoque`). NUNCA base64 no Firestore. | — |

### RF-FIN — Módulo Financeiro

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-FIN-01 | Dashboard financeiro | Role com permissão | Seletor de período + 4 summary cards (receita, despesas, lucro, valor estoque). `AnimatedCounter` em todos os valores. `numberOfLines={1} adjustsFontSizeToFit` para evitar overflow. | `financeiro.view` |
| RF-FIN-02 | Gráficos customizáveis | Role com permissão | `react-native-gifted-charts`. Receita vs despesas, breakdown de despesas (donut), vendas por categoria (donut com legenda), estoque (custo + venda + margem), atividade. Customização salva em AsyncStorage. | `financeiro.view` |
| RF-FIN-03 | Top produtos (ranking) | Role com permissão | Ranking de cards (não gráfico de barras) com medalhas ouro/prata/bronze, barra proporcional, clicável para detalhes do item. Alimentado por TODAS as vendas completadas. | `financeiro.view` |
| RF-FIN-04 | Despesa fixa | Role com permissão | `FixedExpense` com categoria, valor (centavos), frequência de recorrência. | `financeiro.manageExpenses` |
| RF-FIN-05 | Despesa variável | Role com permissão | `VariableExpense` com categoria, valor (centavos), data. | `financeiro.manageExpenses` |
| RF-FIN-06 | Gerenciar categorias de despesa | Role com permissão | CRUD de `ExpenseCategory` (nome, ícone, cor). | `financeiro.manageExpenses` |
| RF-FIN-07 | Meu extrato individual | Participante (devedor) | Tela `financeiro-statement.tsx` com 2 abas (Resumo / Transações), saldo devedor, progresso, histórico mensal. Detalhe do débito em bottom sheet. | `financeiro.participant` |
| RF-FIN-08 | Pagar débito | Participante | Jornada estilo Nubank: bottom sheet dedicado com valor centralizado, quick amounts (25/50/75/Total), cards de método de pagamento, observação colapsável, review card, CTA. Suporta pagamento em lote (long-press). | `financeiro.participant` |
| RF-FIN-09 | Extratos (admin) | Role com permissão | Tela `financeiro-statements.tsx`: lista todos os usuários com débito, busca, filtros. Admin dá baixa em pagamento (`DebtPayment`). | `financeiro.viewStatements` |
| RF-FIN-10 | Contas a receber | Role com permissão | Dashboard financeiro exibe soma de débitos pendentes. | `financeiro.view` |
| RF-FIN-11 | Composição da receita | Role com permissão | Tela "Receita" detalha: vendas diretas + pagamentos de débitos + pendente. Seção "Produtos Mais Vendidos" com nota "X de vendas internas ainda não recebido" quando aplicável. | `financeiro.view` |

### RF-MEN — Módulo Mensalidades

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-MEN-01 | Configurar mensalidades | Admin | Tela `mensalidade-config.tsx`: `enabled`, `applicableRoles`, `pricingMode` (`global`/`per_role`), `paymentGroups` (dueDay 1–28), `gracePeriodDays`. | `mensalidade.manage` |
| RF-MEN-02 | Escolher grupo de pagamento | Usuário aplicável | Usuário escolhe `MensalidadeUserPref` (grupo com dueDay), o que define o dia de vencimento. | `mensalidade.view` |
| RF-MEN-03 | Geração automática | Sistema | Cloud Function `generateMensalidades` roda no dia 1, 06:00 (São Paulo). ID: `mens_YYYY_MM_userId`. | Scheduled |
| RF-MEN-04 | Geração manual | Admin | Admin pode acionar geração manualmente pela tela de admin. | `mensalidade.manage` |
| RF-MEN-05 | Informar pagamento | Usuário | Status muda para `under_review` (salva `previousStatus`). | `mensalidade.view` |
| RF-MEN-06 | Confirmar pagamento | Admin | Status muda para `paid` ou `partial`. Notifica usuário (push + in-app). | `mensalidade.confirmPayment` |
| RF-MEN-07 | Rejeitar pagamento | Admin | Reverte para `previousStatus` se não há outros pagamentos pendentes. Notifica usuário. | `mensalidade.confirmPayment` |
| RF-MEN-08 | Registro direto pelo admin | Admin | Admin registra pagamento sem passar por `under_review`. | `mensalidade.confirmPayment` |
| RF-MEN-09 | Marcar como atrasado | Sistema | Cloud Function `checkLateMensalidades` roda diariamente 09:00. Marca `late` após `gracePeriodDays`. | Scheduled |
| RF-MEN-10 | Enviar cobrança em lote | Admin | Tela `mensalidade-cobranca.tsx`: envio de push + in-app em lote. | `mensalidade.cobrar` |
| RF-MEN-11 | Painel do admin | Admin | Tela `mensalidade-admin.tsx` com tabs horizontais: Pendentes / Em análise / Atrasados / Pagos. | `mensalidade.manage` ou `mensalidade.confirmPayment` |
| RF-MEN-12 | Valores em centavos | Sistema | Todos os valores monetários das mensalidades são inteiros em centavos. | — |

### RF-NOT — Notificações

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-NOT-01 | Registro de device token | Sistema | No mount (autenticado + aprovado + `Device.isDevice`), registra Expo Push Token em `users/{userId}/pushTokens/{tokenHash}`. Logout: `active: false` (não deleta). | — |
| RF-NOT-02 | Resolução server-side | Sistema | `resolveRecipientsByRole()` em Cloud Functions: busca aprovados, filtra por `allowedRoles` + permissão do role no módulo, admin bypass, exclui autor. | — |
| RF-NOT-03 | 3 camadas de filtragem | Sistema | (1) Permissão OS; (2) acesso ao módulo + permissão; (3) preferências pessoais (opt-out). | — |
| RF-NOT-04 | Preferências opt-out | Usuário | Tela `notification-settings.tsx` com master switch + toggles por módulo e categoria. `undefined` = `true`. Seções dinâmicas por acesso. | — |
| RF-NOT-05 | Canais Android | Sistema | Canais: `avisos`, `eventos`, `tarefas`, `social`, `admin`, `financeiro`. | `notificationChannels.ts` |
| RF-NOT-06 | Notificações críticas | Sistema | "Conta aprovada" e "Conta rejeitada" ignoram master switch e preferências. | — |
| RF-NOT-07 | Centro de notificações (in-app) | Usuário | Tela `notifications.tsx` com SectionList (Novas / Anteriores), header "Marcar todas como lidas", empty state. Dual write (push + Firestore) em toda trigger. | — |
| RF-NOT-08 | Tab badges | Sistema | `useTabBadges` agrupa `channelId` → tab (avisos/social → Avisos; eventos/tarefas → Eventos; estoque → Estoque; admin/default → Home). | — |
| RF-NOT-09 | Item badges | Sistema | `useUnreadResourceIds` + `NotificationDot` (7px) no canto superior direito interno dos cards. | — |
| RF-NOT-10 | Auto mark-as-read | Sistema | Telas de detalhe chamam `markNotificationsReadByResource()` no mount → badge e bolinha somem em real-time. | — |
| RF-NOT-11 | Marcar tudo como lida | Usuário | Ação no centro de notificações zera todos os badges em cascata. | — |
| RF-NOT-12 | Cleanup automático | Sistema | `cleanupOldNotifications` (diário 04:00) deleta in-app > 30 dias. `cleanupExpiredTokens` (diário) remove tokens > 60 dias. | Scheduled |
| RF-NOT-13 | Cleanup por deleção de recurso | Sistema | Triggers `onResourceDeleted`, `onAvisoPostDeleted`, `onEventDeleted`, etc. limpam notificações órfãs em batch. | — |
| RF-NOT-14 | Modal de consentimento pré-prompt | Sistema | Antes do prompt OS, modal LGPD explica tipos de notificações e opt-out. Flag `@mironga/notificationConsentShown`. | — |
| RF-NOT-15 | Lembretes de evento | Sistema | `sendEventReminders` (cada 15min): lembretes 24h e 1h antes para RSVPs. Marca `remindersSent`. | Scheduled |
| RF-NOT-16 | Lembretes de tarefa | Sistema | `sendTaskDeadlineReminders` (cada 1h): deadline em 24h, status != done. | Scheduled |
| RF-NOT-17 | Rate limiting | Sistema | Collapse ID por post/evento (curtidas colapsam). Debounce 3+ comentários em 5min → agregação. Cooldown de 5 min por tipo/recurso. | `rateLimiting.ts` |

### RF-LGP — LGPD

| ID | Descrição | Ator(es) | Critério de aceitação | Referência legal |
|----|-----------|----------|-----------------------|------------------|
| RF-LGP-01 | Consentimento explícito no cadastro (e-mail) | Visitante | Checkbox obrigatório em `register.tsx`. `createUserProfile()` grava `lgpdConsentExplicit: true` + `lgpdConsentAt` + `lgpdConsentVersion`. | Art. 7º I |
| RF-LGP-02 | Consentimento explícito (Google/Apple) | Visitante social | Gate obrigatório `LgpdConsentScreen` após login social; usuário deve aceitar antes de acessar o app. | Art. 7º I |
| RF-LGP-03 | Política de privacidade acessível | Qualquer | Tela `privacy-policy.tsx` + modal inline `LgpdDocModal` (usado em gates e perfil). | Art. 9º |
| RF-LGP-04 | Termos de uso acessíveis | Qualquer | Tela `terms.tsx` + `LgpdDocModal`. | Art. 9º |
| RF-LGP-05 | Criptografia de dados sensíveis | Sistema | CPF e RG criptografados AES-256-GCM via `expo-crypto` (`enc:v1:{base64(iv+ciphertext+tag)}`). Chave em `tenants/{tenantId}/config/encryption.masterKey`. Cache em memória, nunca em disco. | Art. 46 |
| RF-LGP-06 | Dados sensíveis em subcollection isolada | Sistema | `users/{userId}/sensitive/main` com Firestore Rules que permitem apenas owner e admin. | Art. 46 |
| RF-LGP-07 | Direito de acesso | Usuário | Perfil visível em `(profile)/index.tsx`. | Art. 18 I |
| RF-LGP-08 | Direito de correção | Usuário | Edição em `(profile)/edit.tsx`. | Art. 18 III |
| RF-LGP-09 | Direito de eliminação | Usuário | Botão "Excluir minha conta" + Cloud Function `onAccountDeletionRequested` com limpeza em cascata. Dados financeiros retidos (Art. 16 II). | Art. 18 VI |
| RF-LGP-10 | Direito de portabilidade | Usuário | Botão "Exportar meus dados" gera JSON + share sheet. | Art. 18 V |
| RF-LGP-11 | Direito de revogação | Usuário | Excluir conta revoga consentimento automaticamente. | Art. 8º §5º |
| RF-LGP-12 | Direito de oposição | Usuário | Preferências granulares em `notification-settings.tsx`. | Art. 18 §2º |
| RF-LGP-13 | Retenção diferenciada | Sistema | Notificações 30 dias, tokens 60 dias, perfil enquanto conta ativa, dados financeiros até 5 anos após exclusão. | Art. 16 |
| RF-LGP-14 | Strings iOS em português | Sistema | `NSCameraUsageDescription`, `NSPhotoLibraryUsageDescription`, `NSMicrophoneUsageDescription` em pt-BR em `app.config.js`. `NSPrivacyTracking: false` em `PrivacyInfo.xcprivacy`. | — |

### RF-ADM — Administração

| ID | Descrição | Ator(es) | Critério de aceitação | Permissão/Regra |
|----|-----------|----------|-----------------------|-----------------|
| RF-ADM-01 | Dashboard admin | Admin ou role com permissão | Tela `(admin)/index.tsx` lista atalhos agrupados por módulo, usando `getModuleAdminRoutes()`. | Qualquer permissão `admin.*` |
| RF-ADM-02 | Aprovar usuário | Admin ou role com permissão | Tela `pending-users.tsx` lista pendentes; `approve-user.tsx` mostra detalhes antes de aprovar/rejeitar. Notifica o usuário. | `admin.approveUsers` |
| RF-ADM-03 | Listar usuários | Admin ou role com permissão | Tela `users.tsx`: busca (nome, e-mail, role), filtros por status e role (chips dinâmicos). Long-press: alterar role, visualizar como. | `admin.manageUsers` |
| RF-ADM-04 | Criar/editar role | Admin ou role com permissão | Tela `role-form.tsx`: nome, cor, permissões (multi-select). | `admin.manageRoles` |
| RF-ADM-05 | Excluir role | Admin | Tela `role-delete.tsx`: reatribuição bulk ou individual; exclusão após reatribuição. Roles `isDefault` protegidos. | `admin.manageRoles` |
| RF-ADM-06 | Atribuir role | Admin ou role com permissão | Tela `assign-role.tsx`. | `admin.assignRoles` |
| RF-ADM-07 | Editar info do tenant | Admin | Tela `tenant-edit.tsx`: nome, descrição, logo (upload). | `admin.manageTenant` |
| RF-ADM-08 | Configurar campos obrigatórios | Admin | Tela `required-fields.tsx`: toggles para cada campo configurável. Afeta `CompleteProfileScreen`. | `admin.manageTenant` |

---

## Parte B — Requisitos Não Funcionais

### RNF-DES — Desempenho

| ID | Descrição |
|----|-----------|
| RNF-DES-01 | Persistência offline do Firestore habilitada em todos os clients (cache local + listeners). |
| RNF-DES-02 | Cache module-level de perfis de usuário com TTL de 5 minutos (`useUserProfiles`); invalidação explícita via `invalidateProfileCache(userId)` após `updateUserProfile()`. |
| RNF-DES-03 | Loading states: skeleton preset compatível com o tipo de tela; `useDelayedLoading` (200ms) evita flash em carregamentos rápidos. |
| RNF-DES-04 | Animações entre 150–400ms usando Reanimated; `Easing.out(Easing.cubic)` para entradas. |
| RNF-DES-05 | Server time sync via RTDB (`/.info/serverTimeOffset`) garante consistência do timer do Quiz Gerenciado entre devices. |
| RNF-DES-06 | Listas longas usam FlatList com virtualização; PDFs renderizam páginas visíveis ± buffer (lazy rendering com pdf.js). |
| RNF-DES-07 | Imagens via `LoadingImage` com cache nativo (`cache: 'default'`) + timeout de segurança de 8s contra bugs de `onLoad` no Android. |

### RNF-SEG — Segurança

| ID | Descrição |
|----|-----------|
| RNF-SEG-01 | Todo documento do Firestore DEVE conter AuditFields (`created_at`, `updated_at`, `created_by`, `updated_by`). Regras validam presença e imutabilidade de `created_at` e `created_by`. |
| RNF-SEG-02 | Firestore Security Rules como enforcement real (check duplo com o client). |
| RNF-SEG-03 | Admin bypass em regras via estrutura de audit fields (não exige `created_by == request.auth.uid`) para permitir Run As User. |
| RNF-SEG-04 | Dados sensíveis (CPF, RG) criptografados em repouso com AES-256-GCM (`expo-crypto`). Chave de tenant em `tenants/{tenantId}/config/encryption`. |
| RNF-SEG-05 | Subcollection `users/{userId}/sensitive/main` — leitura apenas por owner e admin. |
| RNF-SEG-06 | `users/{userId}/pushTokens/{tokenId}` — owner-only write. |
| RNF-SEG-07 | Cloud Functions validam `tenantId` em toda operação (isolamento multi-tenant). |
| RNF-SEG-08 | Upload via signed URL (`getUploadUrl` Cloud Function) com expiração de 15 minutos. Content types permitidos: `application/pdf`, `image/jpeg`, `image/png`, `image/webp`, `audio/mpeg`, `audio/mp4`, `video/mp4`. |
| RNF-SEG-09 | Admins definidos por role `administrador` ou lista `adminEmails` — nunca hardcoded. |
| RNF-SEG-10 | Firestore Rules bloqueiam `created_at`/`created_by` mutáveis; `updated_at` monotônico (`>=` atual). |
| RNF-SEG-11 | Push notifications ignoram `effectiveUserId` — sempre enviam para o user real (não Run As). |

### RNF-PRI — Privacidade (LGPD)

| ID | Descrição |
|----|-----------|
| RNF-PRI-01 | Consentimento explícito obrigatório antes do acesso ao app (gate em `_layout.tsx`). |
| RNF-PRI-02 | 6 direitos do titular (acesso, correção, eliminação, portabilidade, revogação, oposição) implementados. |
| RNF-PRI-03 | Retenção diferenciada de dados: notificações 30d, tokens de push 60d, perfil enquanto ativo, dados financeiros até 5 anos após exclusão (Art. 16 II). |
| RNF-PRI-04 | NUNCA armazenar imagens como base64 no Firestore — sempre upload para Storage + download URL. |
| RNF-PRI-05 | Preferências de notificação respeitadas server-side (não apenas client-side). |
| RNF-PRI-06 | Documentação pública de privacidade em `app/(auth)/privacy-policy.tsx` e termos em `app/(auth)/terms.tsx` + modal inline para gates. |
| RNF-PRI-07 | Strings iOS em português (`NSCameraUsageDescription` etc.) explicando uso dos recursos. |

### RNF-USA — Usabilidade / UX

| ID | Descrição |
|----|-----------|
| RNF-USA-01 | 5 tipos de tela obrigatórios: Hub com Tabs, Detalhe, Formulário, Listagem/CRUD, POS/Fluxo Especial. Cada tipo tem padrão estruturado documentado. |
| RNF-USA-02 | Header e search/filtros SEMPRE fixos fora do ScrollView/FlatList; pull-to-refresh spinner aparece abaixo. Formulários e jogos são exceção (sem pull-to-refresh). |
| RNF-USA-03 | Header canônico: back button circular 40×40 com `arrow-back` 22px; título `fontSize.lg` + `fontWeight: '800'` + `letterSpacing: -0.5`; spacer 40px à direita. |
| RNF-USA-04 | Safe area gerenciada nos layouts (`_layout.tsx`), nunca em telas individuais. `View` com `flex: 1` como container raiz. |
| RNF-USA-05 | Navegação: tabs/drawer → `router.replace()`; cards → `safePush()`; botão voltar → `safeGoBack()`. Tab bar sempre visível. |
| RNF-USA-06 | FAB para toda ação de criação em telas de listagem (simples ou expansível com glassmorphism). Nunca botão no header. |
| RNF-USA-07 | Botões coloridos (primary, correct, wrong) → texto e ícones SEMPRE `#FFFFFF`. |
| RNF-USA-08 | Touch targets mínimo 48px de altura. |
| RNF-USA-09 | `AnimatedEntry` obrigatório em toda tela (fade + slide up, 400ms, delays escalonados). |
| RNF-USA-10 | `LoadingImage` obrigatório para toda imagem (rede ou local); shimmer + fade-in para HTTP; detecção automática de URIs locais. |
| RNF-USA-11 | Nomes async mostram `Skeleton.Text` durante loading — nunca string vazia, "Carregando..." ou "Desconhecido". |
| RNF-USA-12 | Formulários seguem design system: inputs sem `borderWidth` (usar shadow), `borderRadius.lg`, `letterSpacing: 0`, `keyboardShouldPersistTaps="handled"`. |
| RNF-USA-13 | Search input canônico: ícone `search` size 20, clear `close-circle` size 20, `fontSize.md`, shadow sutil. |
| RNF-USA-14 | Alerts via `AppModal` (`useModal`) — NUNCA `Alert` nativo. Listas em messages usando `•` ou `- ` viram cards estilizados automaticamente. |
| RNF-USA-15 | Bottom sheets via componente `BottomSheet` — NUNCA `Modal` com `animationType="slide"` (causa gap na home indicator). |

### RNF-ACE — Acessibilidade

| ID | Descrição |
|----|-----------|
| RNF-ACE-01 | `TouchableOpacity` com `activeOpacity={0.7}`. |
| RNF-ACE-02 | Haptic feedback em ações importantes (acerto/erro no quiz, swipe-to-reply, streak). |
| RNF-ACE-03 | `numberOfLines={1}` para nomes; `adjustsFontSizeToFit` em stats monetários compactos. |
| RNF-ACE-04 | Suporte a tema Light / Dark / System com persistência em AsyncStorage (`@mironga/themeMode`). |
| RNF-ACE-05 | Cores adaptativas via `useTheme()`; componentes NUNCA importam `colors` direto. |
| RNF-ACE-06 | Inputs com `autoCapitalize="none"` para e-mail e senha. |

### RNF-OFF — Offline-First

| ID | Descrição |
|----|-----------|
| RNF-OFF-01 | Firestore persistence local ativa em todos os clients; leituras e escritas suportadas offline. |
| RNF-OFF-02 | PDFs e áudios cacheados em `FileSystem.documentDirectory` no primeiro acesso (`materialCacheService.ts`). |
| RNF-OFF-03 | Estoque: background sync 3 níveis (Firestore SDK + expo-task-manager + NetInfo). Fila `@mironga/pendingSync` em AsyncStorage. |
| RNF-OFF-04 | Retry com backoff: imediato → 1m → 5m → 15m. |
| RNF-OFF-05 | Banner `OfflineBanner` indica estado offline. |

### RNF-MAN — Manutenibilidade

| ID | Descrição |
|----|-----------|
| RNF-MAN-01 | TypeScript strict mode. Sem `any`. Preferir `interface` sobre `type`. |
| RNF-MAN-02 | Nomenclatura: componentes PascalCase, serviços/hooks/utils camelCase, enums UPPER_SNAKE_CASE, tipos PascalCase. |
| RNF-MAN-03 | Código em inglês; conteúdo exibido ao usuário em pt-BR. |
| RNF-MAN-04 | AuditFields obrigatórios em todo write (`stampCreate`/`stampUpdate`). |
| RNF-MAN-05 | `stripUndefined()` antes de gravar no Firestore para campos opcionais. |
| RNF-MAN-06 | `effectiveUserId` nunca `user.uid` direto — para leituras e escritas. Exceções: `AccessControlContext` e `AuthContext`. |
| RNF-MAN-07 | Padrão anti-stale: referenciar por ID, resolver dados frescos via `useUserProfiles`/`useUserProfile`. Campos desnormalizados apenas como fallback. |
| RNF-MAN-08 | Escritas atômicas: `increment()` para contadores, `arrayUnion`/`arrayRemove` para arrays, `updateDoc` parcial para campos específicos — NUNCA read-then-write. |
| RNF-MAN-09 | Checklist de 9 seções para criação/alteração de módulo (ver CLAUDE.md). |
| RNF-MAN-10 | Regra de consistência sistêmica: mudanças de design aplicadas a TODAS as telas de uma vez. |
| RNF-MAN-11 | Permissões sincronizadas em 4 pontos: `SYSTEM_PERMISSIONS` + `SYSTEM_PERMISSIONS_INFO` + `PERMISSION_LABELS` + gate na tela. |

### RNF-ESC — Escalabilidade

| ID | Descrição |
|----|-----------|
| RNF-ESC-01 | Arquitetura multi-tenant com isolamento completo de dados por tenant. |
| RNF-ESC-02 | Cloud Functions na região `southamerica-east1` com `maxInstances: 10` (funções HTTP). |
| RNF-ESC-03 | Contadores desnormalizados via `increment()` para evitar custo de agregação (likeCount, commentCount, readCount, materialCount, taskCount etc.). |
| RNF-ESC-04 | Batch writes (chunked a cada 500) em cleanups e notificações in-app. |
| RNF-ESC-05 | `resolveRecipientsByRole()` em batch para envio de push. |
| RNF-ESC-06 | Lazy rendering de páginas de PDF (canvas pdf.js com buffer). |
| RNF-ESC-07 | Pagination implícita via `onSnapshot` + virtualização do FlatList. |

### RNF-PLA — Plataforma

| ID | Descrição |
|----|-----------|
| RNF-PLA-01 | iOS 13+ (requisito do Apple Sign-In). |
| RNF-PLA-02 | Android 5+ (API 21+). |
| RNF-PLA-03 | Web (PWA) com `PwaInstallGate` forçando instalação; Service Worker em `/sw.js`. |
| RNF-PLA-04 | Stack: Expo 55, React Native 0.83, React 19, Firebase JS SDK 12, TypeScript 5. |
| RNF-PLA-05 | Build nativo via `expo prebuild` + `expo run:ios`/`run:android` ou EAS Build. |
| RNF-PLA-06 | Development Build (não Expo Go). |

### RNF-CON — Conformidade

| ID | Descrição |
|----|-----------|
| RNF-CON-01 | Projeto Firebase `mironga-14be8` é o único deployável a partir deste repositório. TOCA é gerenciada separadamente. |
| RNF-CON-02 | APNs Key `.p8` obrigatória para push notifications no iOS (configurada via `npx eas credentials`). |
| RNF-CON-03 | Reversed client ID gerado automaticamente via `app.config.js` → `CFBundleURLTypes`. |
| RNF-CON-04 | Apple Sign-In requer Apple Developer Program ativo e capability habilitada por bundle ID. |
| RNF-CON-05 | Cloud Functions: `npm run build` obrigatório antes de `firebase deploy` (`lib/` é o que sobe, não `src/`). |
| RNF-CON-06 | `initializeApp()` DEVE preceder exports das notification triggers em `functions/src/index.ts`. |
| RNF-CON-07 | Após deploy de functions, remarcar "Permitir acesso público" no Cloud Run (`getuploadurl`). |

### RNF-OBS — Observabilidade

| ID | Descrição |
|----|-----------|
| RNF-OBS-01 | Logs com prefixo `[Mironga]` em todas as Cloud Functions. |
| RNF-OBS-02 | Scheduled functions: `generateMensalidades` (dia 1, 06:00), `checkLateMensalidades` (diário, 09:00), `sendEventReminders` (15min), `sendTaskDeadlineReminders` (1h), `cleanupOldNotifications` (diário, 04:00), `cleanupExpiredTokens` (diário). |
| RNF-OBS-03 | Firebase Crashlytics e Analytics podem ser habilitados via dependências (não obrigatório no build base). |

---

## Apêndice — Matriz de Permissões do Sistema

**Total: 40 permissões.**

| Módulo | Permissão | Descrição resumida |
|--------|-----------|--------------------|
| jogos | `jogos.play` | Jogar quiz e flashcards |
| jogos | `jogos.viewProgress` | Ver próprio progresso |
| jogos | `jogos.manageSubjects` | CRUD de assuntos + status |
| jogos | `jogos.manageQuestions` | CRUD de perguntas |
| jogos | `jogos.manageFolders` | CRUD de pastas |
| jogos | `jogos.manageQuizRoom` | Criar e moderar Quiz Gerenciado |
| jogos | `jogos.manageTemplatePresets` | CRUD de modelos de conteúdo |
| profile | `profile.view` | Ver próprio perfil |
| profile | `profile.edit` | Editar próprio perfil |
| admin | `admin.manageRoles` | CRUD de roles |
| admin | `admin.assignRoles` | Atribuir roles a usuários |
| admin | `admin.manageUsers` | Listar e gerenciar usuários |
| admin | `admin.approveUsers` | Aprovar/rejeitar usuários |
| admin | `admin.manageTenant` | Editar info e config do tenant |
| eventos | `eventos.view` | Ver eventos |
| eventos | `eventos.create` | Criar eventos |
| eventos | `eventos.manage` | Editar/excluir eventos e materiais |
| eventos | `eventos.manageEventTypes` | CRUD de tipos de evento |
| eventos | `eventos.checkAttendance` | Check-in (double-check) |
| eventos | `eventos.manageTasks` | CRUD de tarefas de evento |
| materiais | `materiais.view` | Ver materiais |
| materiais | `materiais.manageFolders` | CRUD de pastas |
| materiais | `materiais.manageItems` | CRUD de itens |
| avisos | `avisos.view` | Ver feed |
| avisos | `avisos.create` | Criar avisos |
| avisos | `avisos.manage` | Editar/excluir/fixar avisos |
| avisos | `avisos.comment` | Comentar e responder |
| estoque | `estoque.view` | Ver estoque |
| estoque | `estoque.manageItems` | CRUD de itens |
| estoque | `estoque.manageMovements` | Registrar movimentações |
| estoque | `estoque.manageCategories` | CRUD de categorias/unidades/tipos |
| estoque | `estoque.sell` | Usar POS e registrar vendas |
| financeiro | `financeiro.view` | Ver dashboard financeiro |
| financeiro | `financeiro.manageExpenses` | CRUD de despesas |
| financeiro | `financeiro.participant` | Ter débitos / meu extrato |
| financeiro | `financeiro.viewStatements` | Ver extratos de todos (admin) |
| mensalidade | `mensalidade.view` | Ver próprias mensalidades |
| mensalidade | `mensalidade.manage` | Configurar e gerenciar |
| mensalidade | `mensalidade.cobrar` | Enviar cobranças em lote |
| mensalidade | `mensalidade.confirmPayment` | Confirmar/rejeitar pagamentos |

---

## Apêndice — Inventário de Cloud Functions

**Triggers Firestore — Criação:**
`onAvisoCreated`, `onAvisoCommentCreated`, `onAvisoLikeCreated`, `onCommentLikeCreated`, `onEventCreated`, `onEventUpdated`, `onRsvpChanged`, `onRsvpCheckIn`, `onTaskCreated`, `onTaskStatusChanged`, `onEventMaterialCreated`, `onMaterialItemCreated`, `onSubjectPublished`, `onUserApproved`, `onUserPending`, `onStockMovementCreated`, `onMensalidadePaymentCreated`.

**Triggers Firestore — Atualização:**
`onMensalidadePaymentVerified`.

**Triggers Firestore — Deleção (cleanup):**
`onResourceDeleted` (genérica) + triggers específicos por recurso.

**Trigger Auth:**
`onAccountDeletionRequested` (LGPD).

**Scheduled:**
`generateMensalidades`, `checkLateMensalidades`, `sendEventReminders` (dentro de `scheduledReminders`), `sendTaskDeadlineReminders`, `cleanupOldNotifications`, `cleanupTokens`, `sendExpenseReminders`, `sendMensalidadeCobranca`.

**HTTP:**
`getUploadUrl` (signed URL para uploads).

**Utilities internas:**
`sendPush`, `writeInAppNotifications`, `resolveRecipients`, `rateLimiting`, `cleanupNotificationsForResource`.

---

**Fim do documento.** Para especificação técnica detalhada (tipos, interfaces, schemas, serviços), ver [SPECS.md](../SPECS.md). Para contexto de desenvolvimento e convenções, ver [CLAUDE.md](../CLAUDE.md). Para uso do produto, ver [MANUAL.md](./MANUAL.md).
