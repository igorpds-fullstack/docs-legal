# Manual do Sistema — Mironga

**Versão:** 1.0
**Data:** 2026-04-17
**Público:** frequentadores, médiuns, pais/mães de santo e administradores de terreiros.

Este manual é o guia prático de uso do **Mironga**. Ele cobre desde a criação da sua conta até a administração completa do terreiro. Se você busca a especificação técnica, consulte [SPECS.md](../SPECS.md) e [REQUIREMENTS.md](./REQUIREMENTS.md).

---

## Índice

1. [Introdução](#1-introdução)
2. [Primeiros Passos](#2-primeiros-passos)
3. [Navegando no App](#3-navegando-no-app)
4. [Módulo Jogos](#4-módulo-jogos)
5. [Módulo Eventos](#5-módulo-eventos)
6. [Módulo Materiais](#6-módulo-materiais)
7. [Módulo Avisos](#7-módulo-avisos)
8. [Módulo Estoque](#8-módulo-estoque)
9. [Módulo Financeiro](#9-módulo-financeiro)
10. [Módulo Mensalidades](#10-módulo-mensalidades)
11. [Notificações](#11-notificações)
12. [Administração](#12-administração)
13. [Seu Perfil e Privacidade (LGPD)](#13-seu-perfil-e-privacidade-lgpd)
14. [Temas](#14-temas)
15. [Instalação como PWA (Web)](#15-instalação-como-pwa-web)
16. [Perguntas Frequentes e Solução de Problemas](#16-perguntas-frequentes-e-solução-de-problemas)

---

## 1. Introdução

### 1.1 O que é o Mironga

O **Mironga** é uma plataforma digital para terreiros de Umbanda. Cada terreiro tem o seu próprio app — com sua identidade visual, dados isolados e configurações próprias — gerado a partir de uma mesma base de código. Por isso o Mironga que você instala pode ter um nome diferente ("Terreiro do Pai João", "Casa de Caridade Oxalá" etc.).

### 1.2 Módulos disponíveis

Dependendo de como o seu terreiro configurou o app, você pode ter acesso a alguns ou todos os módulos:

- **Jogos** — Quiz de múltipla escolha, Flashcards e Quiz em Grupo (multiplayer) para aprender ervas, fundamentos, liturgia e outros temas.
- **Eventos** — Calendário de atividades do terreiro com confirmação de presença e tarefas.
- **Materiais** — Biblioteca de PDFs, áudios e links organizados em pastas.
- **Avisos** — Feed de comunicados do terreiro, com curtidas e comentários.
- **Estoque** — Controle do inventário (ervas, velas, produtos) e Ponto de Venda (POS).
- **Financeiro** — Dashboard de receitas e despesas, extratos e contas a receber.
- **Mensalidades** — Cobrança mensal recorrente e controle de pagamentos.
- **Administração** — Gestão de usuários, roles e configurações do terreiro.

### 1.3 Plataformas

O Mironga está disponível em:

- **iOS 13 ou superior** (iPhone e iPad).
- **Android 5 ou superior** (celular e tablet).
- **Web (PWA)** — você pode usar o Mironga pelo navegador e instalá-lo como aplicativo (ver [seção 15](#15-instalação-como-pwa-web)).

---

## 2. Primeiros Passos

### 2.1 Criar sua conta

Na tela inicial, toque em **"Cadastrar"**. Você pode escolher entre três formas de criar conta:

1. **E-mail e senha** — preencha nome completo, e-mail e uma senha forte. Marque a caixa aceitando a **Política de Privacidade** e os **Termos de Uso** (obrigatório pela LGPD). Toque em **Cadastrar**.
2. **Entrar com Google** — escolha a conta Google que deseja usar.
3. **Entrar com Apple** (somente iOS 13+) — autorize com Face ID, Touch ID ou sua senha da Apple.

> **Dica:** a opção "Entrar com Apple" pode não aparecer em todos os dispositivos (Expo Go, iPads sem biometria, simuladores). Use e-mail ou Google nesses casos.

### 2.2 Aguardar aprovação do administrador

Depois de criar sua conta, você verá uma tela informando que está **aguardando aprovação**. O administrador do seu terreiro precisa aprovar sua entrada antes que você tenha acesso aos módulos.

Você receberá uma notificação quando for aprovado (ou rejeitado, se for o caso).

> **Usuários com e-mail previamente listado como admin** são aprovados automaticamente.

### 2.3 Completar seu perfil

Na primeira vez que entrar após a aprovação, você precisa **completar o perfil**. Os campos obrigatórios variam conforme a configuração do seu terreiro, mas sempre incluem:

- Nome completo.
- E-mail (preenchido automaticamente).

E podem incluir também:

- Foto de perfil.
- Data de nascimento (com opção de mostrar/ocultar o ano).
- Telefone.
- Gênero.
- CEP (preenche endereço automaticamente via ViaCEP), endereço, cidade, estado (UF).
- CPF e RG (armazenados de forma criptografada, visíveis apenas para você e administradores).

### 2.4 Aceitar a Política de Privacidade (LGPD)

Se você se cadastrou por Google ou Apple, verá uma tela dedicada ao **consentimento LGPD**. Leia os resumos, toque nos links para abrir a política completa e os termos, marque a caixa **"Li e concordo"** e toque em **Aceitar e continuar**.

> Se não quiser aceitar, toque em **"Não aceito — sair"**. Sem o consentimento, o app não pode ser usado (exigência legal).

### 2.5 Permitir notificações

O app vai exibir um modal explicativo antes de pedir permissão ao sistema operacional para enviar notificações. Você pode aceitar (recomendado) ou recusar. Mesmo recusando, poderá ativar depois em **Ajustes > Notificações** do celular e também em **Drawer > Notificações** dentro do app.

---

## 3. Navegando no App

### 3.1 Dashboard (Home)

A tela inicial é a sua central. Ela traz, de cima para baixo:

- **Top bar:** ícone de menu à esquerda (abre o drawer) e sininho à direita (centro de notificações).
- **Hero section:** seu avatar (com anel de progresso se você tem acesso a Jogos), saudação contextual ("Bom dia, Igor"), subtítulo inteligente (progresso, avisos, tarefas) e atalhos de Quiz e Flashcards.
- **Avisos:** carrossel com os últimos 2 avisos do terreiro (estilo Threads/Instagram).
- **Minhas tarefas:** até 3 tarefas pendentes com deadline; você pode marcar como concluída direto do dashboard.
- **Acesso rápido:** card único com linhas compactas para cada módulo que você tem acesso.

### 3.2 Barra inferior (Bottom Tab Bar)

A barra inferior mostra de 2 a 5 abas, dependendo da configuração do seu terreiro. A aba **Home** é sempre a primeira. As demais podem ser **Avisos, Eventos, Materiais, Jogos, Estoque** (em qualquer combinação).

Ícones sem preenchimento indicam aba inativa; com preenchimento, a aba ativa. Bolinhas vermelhas com número são **badges de notificações não lidas** agrupadas por módulo.

### 3.3 Menu lateral (Drawer)

Toque no ícone de menu (canto superior esquerdo) para abrir o drawer. Ele lista:

- Todos os módulos que você pode acessar (inclui os que não estão no bottom tab).
- **Sobre o Terreiro** — nome, descrição, logo do seu terreiro.
- **Notificações** — tela de preferências (o que você quer receber).
- **Tema** — Claro, Escuro ou Sistema.
- **Sair** — logout.

### 3.4 Sininho — Centro de notificações

O sininho no canto superior direito do dashboard mostra um badge vermelho com o total de notificações in-app não lidas. Toque para abrir a tela **Notificações**, que mostra as últimas 30 dias agrupadas em "Novas" e "Anteriores". Toque em uma para ir direto ao conteúdo. O botão **"Marcar todas como lidas"** zera todos os badges.

### 3.5 Bolinhas vermelhas (badges)

- **Tab badge** (número sobre o ícone da aba): total de notificações não lidas do grupo.
- **Item badge** (`NotificationDot` — bolinha 7px): no canto superior direito dos cards com notificações relacionadas.

As bolinhas somem automaticamente quando você abre o conteúdo referenciado.

---

## 4. Módulo Jogos

O Mironga tem três jogos para aprender conteúdos da Umbanda: **Quiz Solo**, **Flashcards** e **Quiz Gerenciado** (em grupo). Todos usam o mesmo pool de conteúdos (chamados de **Assuntos**).

### 4.1 Entendendo Assuntos e Pastas

- **Assunto:** um tema de estudo (ex.: "Ervas de Proteção", "Orixás Femininos", "Liturgia de Gira"). Cada assunto contém um conjunto de **perguntas/respostas** autoradas.
- **Pasta:** agrupa assuntos (ex.: "Fundamentos", "Ervas", "Liturgia"). Pastas podem estar visíveis para todos ou restritas a determinados roles.

### 4.2 Quiz Solo

1. No hub **Jogos**, toque em **Quiz Solo**.
2. **Selecione os assuntos** que quer jogar (toque para marcar; use "Selecionar todos" para escolher tudo; a busca funciona sem acentos).
3. Escolha o **modo** (ex.: "Rápido 10", "Completo 30").
4. O quiz começa: leia a pergunta, toque em uma das 4 alternativas.
5. **Pontuação:** +10 por acerto, −3 por erro (nunca fica negativo). Sequências de acertos ("streaks") dão bônus: 5 seguidos → +15; 10 → +30; 15 → +50; 20 em diante → +75 a cada 5 acertos.
6. Ao final, veja o resultado e seu progresso atualizado.

> **Proteção:** se tentar sair ou navegar durante o jogo, o app pede confirmação.

### 4.3 Flashcards

1. No hub **Jogos**, toque em **Flashcards**.
2. Selecione os assuntos.
3. Escolha o modo.
4. Veja o **verso** (pergunta) do cartão; toque para virar (flip 3D) e ver a resposta.
5. **Swipe para direita** se você sabia; **para esquerda** se não sabia. Isso alimenta a repetição espaçada.

### 4.4 Quiz Gerenciado (multiplayer)

Ideal para aulas em grupo e giras de estudo.

**Como moderador (quem tem permissão `jogos.manageQuizRoom`):**

1. No hub **Jogos**, toque em **Quiz Gerenciado** → **Criar sala**.
2. **Etapa 1 — Selecionar perguntas:** escolha os assuntos e as perguntas específicas.
3. **Etapa 2 — Configurar:** defina o tempo por pergunta (10, 20, 30, 45, 60 s).
4. **Etapa 3 — Tutorial:** leia o passo-a-passo do moderador.
5. A sala é criada com um **código de 4 dígitos**. Compartilhe com os jogadores.
6. No lobby, aguarde os jogadores entrarem. Toque em **Iniciar** quando estiver pronto.
7. A cada pergunta, veja o ranking ao vivo. Avance para a próxima.
8. Ao final, veja o ranking consolidado. Botão **"Encerrar sala"** finaliza para todos.

**Como jogador:**

1. No hub **Jogos**, toque em **Quiz Gerenciado** → **Entrar com código**.
2. Digite o código de 4 dígitos e seu nome.
3. Espere no lobby; quando o moderador iniciar, você verá as perguntas uma a uma.
4. **Pontuação:** quanto mais rápido você responde, mais pontos — com bônus por sequência de acertos (1.2x a 1.5x). Erros não subtraem pontos.
5. Ao final, veja o ranking (o moderador é excluído do ranking).

> O timer é sincronizado entre todos os dispositivos, mesmo com relógios diferentes.

### 4.5 Seu progresso

Na tela **Jogos > Progresso**, veja quantas perguntas você **dominou**, revisões pendentes e estatísticas por assunto e pasta. O sistema usa **repetição espaçada**:

- Errou? Próxima revisão em 2 dias.
- Acertou 1x? 5 dias.
- Acertou 2x? 10 dias.
- Dominada? 30 dias.

### 4.6 Gerenciar Assuntos (para quem tem permissão)

Se você tem permissões administrativas de jogos, acesse **Jogos > Gestão > Assuntos** para:

- Criar e editar **pastas** (`jogos.manageFolders`).
- Criar e editar **assuntos** (nome, descrição, ícone, cor, modelo de conteúdo, jogos permitidos) — `jogos.manageSubjects`.
- Adicionar e editar **perguntas** dentro de cada assunto, com suporte a hierarquia (tópico principal + subconteúdos) — `jogos.manageQuestions`.
- Criar **modelos pré-definidos** de estrutura de conteúdo (`jogos.manageTemplatePresets`).

> **Assuntos em rascunho** não aparecem para jogadores. Mude para **Publicado** quando estiver pronto.
>
> **Regra espiritual fundamental:** ao cadastrar ervas, use linguagem de associação ("comumente associada a") — nunca "pertence a" ou "é de".

---

## 5. Módulo Eventos

### 5.1 Ver eventos

Na aba/módulo **Eventos**, você vê duas seções: **Próximos** e **Anteriores**. Use:

- **Barra de busca** (nome, local, tipo).
- **Filtro de data** (ícone de calendário) — presets ("Este mês", "Últimos 30 dias"), grid de meses ou intervalo customizado.
- **Filtros** (ícone de opções) — tipos de evento, "Com materiais", "Com tarefas".

### 5.2 Confirmar presença (RSVP)

Toque em um evento para ver os detalhes. Na tela de detalhes, toque em **Confirmar**, **Talvez** ou **Não posso**. Seu RSVP aparece para os demais membros. Você pode mudar a qualquer momento.

### 5.3 Detalhes do evento

A tela mostra: tipo, data, local, descrição, observações, lista de RSVPs (agrupados por status), materiais do evento e tarefas.

### 5.4 Check-in (presença confirmada)

Se o evento tem **double-check** ativo, alguém com permissão (`eventos.checkAttendance`) ou o criador do evento pode marcar você como **presente** no dia. Essa confirmação libera, por exemplo, materiais com `accessMode: 'checked_in_only'`.

### 5.5 Minhas tarefas

Tarefas de evento atribuídas a você aparecem:

- No **dashboard**, seção "Minhas tarefas" (até 3).
- Na tela dedicada **Minhas Tarefas** (acesso via "Ver todas"). Busca fuzzy + filtros por status (Pendentes, Em andamento, Concluídas).
- No **detalhe do evento** correspondente.

Você pode mudar o status da sua tarefa (`pending` → `in_progress` → `done`) direto do card.

### 5.6 Criar um evento

Com permissão `eventos.create`:

1. Na tela **Eventos**, toque no **FAB (+)**.
2. Preencha: nome, tipo, datas, local, descrição, observações.
3. **Visibilidade:** deixe `allowedRoles` vazio para todos verem, ou selecione roles específicos.
4. **RSVP:** ative ou desative.
5. **Double-check:** ative se quer controlar presença no dia.
6. **Editores de material:** adicione UIDs de quem pode subir materiais para este evento.
7. Salve.

Depois, no detalhe do evento, adicione **materiais** (`link` ou PDF) e **tarefas** (com responsáveis e deadline).

### 5.7 Gerenciar tipos de evento

Com permissão `eventos.manageEventTypes`, acesse **Eventos > Gestão > Tipos** para criar nomes, ícones e cores personalizados (ex.: "Gira de Caboclo", "Desenvolvimento", "Estudo Teórico").

---

## 6. Módulo Materiais

### 6.1 Navegar por pastas

Na aba/módulo **Materiais**, você vê as pastas e arquivos da raiz. Toque em uma pasta para entrar. O **breadcrumb** no topo mostra onde você está; toque em qualquer nível para voltar.

### 6.2 Visualizar um PDF

Toque em um arquivo PDF para abrir o visualizador in-app:

- **Zoom:** pinch-to-zoom.
- **Busca:** ícone de lupa no topo → digite o termo; o texto é destacado.
- **Paginação:** toolbar flutuante com página atual e ir para página.
- **Modo restrito:** se o PDF tem `restrictedView: true`, você verá o badge "Restrito". Nesse modo:
  - Não é possível copiar texto.
  - Screenshots são bloqueados (iOS/Android).
  - Não há opção de exportar ou compartilhar.
- **Modo externo:** em PDFs não restritos, o botão **"Abrir externamente"** no header permite baixar e abrir com outro app.

### 6.3 Ouvir um áudio

Toque em um arquivo de áudio. O player abre. Recursos:

- **Play/Pause** e seek bar arrastável.
- **Skip −15s / +15s**.
- **Velocidade** de reprodução (0.5x a 2x).
- **Loop** (repetir).

O áudio **continua tocando ao navegar** — aparece um **mini-player flutuante** acima da barra inferior em todas as telas. Toque nele para reabrir o player expandido.

### 6.4 Cache offline

No primeiro acesso, PDFs e áudios são baixados e cacheados no seu dispositivo. Indicadores nos cards mostram se o arquivo já está disponível offline. Sem internet, você pode abrir qualquer material já cacheado.

### 6.5 Busca global

Na raiz de Materiais, digite na barra de busca. O app mostra **diretamente** como resultados:

- Pastas que batem com o nome (em qualquer nível).
- Arquivos (em qualquer pasta acessível) com `contextLabel` (nome da pasta pai).
- Materiais de eventos relevantes.

### 6.6 Materiais de eventos

Na raiz de Materiais, se o seu terreiro usa o módulo Eventos, você verá uma seção **"Eventos"** com cards de eventos que têm materiais. Ao entrar, os materiais do evento aparecem como arquivos normais — sujeitos a todas as regras do evento (role, check-in, restrição de PDF).

Arquivos trancados por falta de check-in aparecem com **ícone de cadeado** e texto "Disponível após confirmação de presença".

### 6.7 Criar pastas e itens

Com permissão `materiais.manageFolders` ou `materiais.manageItems`:

1. Dentro de uma pasta (ou na raiz), toque no **FAB (+)**.
2. Escolha **"Nova pasta"** ou **"Novo item"**.
3. Para **pastas:** nome, descrição, ícone, cor, visibilidade, roles permitidos.
4. Para **itens:** tipo (link, PDF ou áudio), nome, descrição, arquivo (upload). Em PDFs: escolha "Restrito" ou "Externo".

> Uma pasta oculta ou restrita **esconde tudo o que está dentro dela**, mesmo que os filhos estejam com configurações diferentes.

---

## 7. Módulo Avisos

### 7.1 Feed social

Abra **Avisos** para ver o feed do terreiro, estilo rede social:

- Avisos **fixados** aparecem primeiro.
- **Pull-to-refresh** para atualizar.
- **Busca** por título/conteúdo.
- **Filtro de data** (mesmo controle dos Eventos).
- **Filtros:** "Não lidos", "Pendentes de confirmação", "Fixados", "Com mídia".

Cada post mostra: autor (foto + nome resolvidos em tempo real), data, título (opcional), texto, imagens/PDFs, e a barra de ações com **coração**, **olho** (quem leu) e **balão** (comentários).

### 7.2 Curtir, comentar, responder

- **Curtir** (tap no coração): toggle. Long-press abre a lista "Quem curtiu" (estilo Instagram).
- **Ler:** ao abrir o detalhe, sua leitura é automaticamente registrada.
- **Comentar:** tap no ícone de balão abre o detalhe com comentários. Digite no input no rodapé.
- **Responder a um comentário:** **arraste o comentário para a direita** (swipe-to-reply) ou long-press → "Responder". Aparece um banner de reply acima do input.
- **Curtir comentário:** toggle coração à direita do comentário.
- **Excluir comentário:** long-press → "Excluir" (somente o autor ou quem tem permissão). Filhos do comentário são re-parenteados para manter o fio.

### 7.3 Confirmar leitura

Alguns avisos exigem confirmação explícita (`requiresAck: true`). Na tela de detalhes, toque em **"Li e estou ciente"**. O administrador vê quantos confirmaram.

### 7.4 Fixar ou editar (com permissão)

Com permissão `avisos.manage`, um aviso pode ser:

- **Fixado** (aparece no topo do feed).
- **Editado** (texto, imagens, visibilidade).
- **Excluído** (cleanup de notificações em cascata).

### 7.5 Criar um aviso

Com permissão `avisos.create`:

1. No feed, toque no **FAB (+)**.
2. Formulário estilo criação de post:
   - Avatar do autor (você).
   - Campo de título (opcional).
   - Campo de texto (obrigatório).
   - Barra de mídia: adicionar imagens e PDFs.
   - Opções: **Fixar**, **Exigir confirmação**, **Visibilidade** (roles permitidos).
3. Toque em **Publicar**.

Todos os usuários que têm permissão de ver recebem push + notificação in-app.

---

## 8. Módulo Estoque

### 8.1 Ver estoque

Na aba/módulo **Estoque**, você vê abas horizontais: **Estoque**, **Vendas**, **Financeiro** (dependendo das suas permissões). Na aba **Estoque**:

- Stats no topo (total de itens, valor em estoque).
- Busca por nome.
- Filtros por categoria (chips horizontais).
- Alertas de **estoque baixo** (item com quantidade <= threshold).

### 8.2 Detalhe de um item

Toque em um item para ver: imagens (deslize para ver todas), nome, descrição, categoria, unidade, preço de venda e custo, estoque atual, histórico de movimentações.

### 8.3 Cadastrar um item (permissão `estoque.manageItems`)

1. No hub Estoque, **FAB (+)**.
2. Preencha: nome, descrição, categoria, unidade de medida, preços (em reais — o app converte para centavos internamente), estoque inicial, threshold de estoque baixo.
3. **Adicione imagens** (até 5). A primeira é a capa. Todas são mostradas com `contain` (nunca cortadas).
4. Salve.

### 8.4 Registrar uma movimentação (permissão `estoque.manageMovements`)

Use para entradas (compras, doações), saídas (uso interno, vendas externas), baixas (perdas, vencimentos) e ajustes:

1. No item, toque em **"Registrar movimentação"**.
2. Escolha o **tipo** (Entrada, Saída, Baixa, Ajuste — cada um com comportamento dinâmico).
3. Informe quantidade e valor (quando aplicável).
4. Adicione observação.
5. Salve.

Se o tipo afeta finanças (`affectsFinance: true`), a movimentação aparece automaticamente no módulo Financeiro (como receita ou despesa).

### 8.5 Ponto de Venda (POS) (permissão `estoque.sell`)

1. No hub Estoque, toque em **POS** (ou na aba correspondente).
2. **Filtre por categoria** usando os chips horizontais. Use a busca para ir direto a um produto.
3. **Tap em um produto** → bottom sheet com imagem grande, nome, descrição, preço e estoque. Ajuste a quantidade (stepper) e **Adicionar ao carrinho**.
4. O **cart bar flutuante** mostra quantos itens e o total. Tap para abrir o carrinho.
5. No carrinho: ajuste quantidades (stepper iFood — lixeira quando qty=1), escolha **método de pagamento**, adicione observações.
6. Confirme. **Success overlay** "Continuar vendendo" permite fechar mais vendas sem sair da tela.

> **Preço de custo não aparece no POS** — apenas na tela de estoque e no financeiro.

### 8.6 Venda interna (cobrança via extrato)

Se o comprador é membro do terreiro e você quer cobrá-lo depois:

1. No carrinho, marque a opção **"Venda interna"** e selecione o devedor.
2. O estoque é baixado normalmente, mas a **receita só entra quando o débito é pago**.
3. Um `UserDebt` é criado e aparece no extrato do devedor.

### 8.7 Histórico de vendas

Na aba **Vendas**: busca, filtros, cards de vendas com status (concluída, cancelada, pendente). Tap em uma venda abre o detalhe com:

- Vendedor (nome e foto resolvidos em tempo real).
- Itens da venda (clique para ir ao item).
- Status, método de pagamento, notas.
- Botão **Cancelar venda** (se ainda possível): reverte as movimentações.

### 8.8 Gerenciar categorias, unidades, tipos e métodos (permissão `estoque.manageCategories`)

No hub Estoque, acesse:

- **Categorias de produto** (cor, ícone).
- **Unidades de medida** (un, kg, g, L, mL — já vêm pré-cadastradas).
- **Tipos de movimentação** (define comportamento: aumenta, diminui ou seta estoque; afeta finanças ou não).
- **Métodos de pagamento** (Pix, Cartão, Dinheiro etc. — pré-cadastrados).

Cada uma tem lista + formulário dedicado.

---

## 9. Módulo Financeiro

### 9.1 Dashboard

Acesse **Financeiro** (ou a aba correspondente). O dashboard mostra:

- **Seletor de período** (navegue entre meses).
- **4 summary cards** animados (contagem crescente): Receita, Despesas, Lucro, Valor do Estoque (custo, com subtítulo do valor de venda).
- **Alerta de low stock** (se houver).
- **Gráficos customizáveis** (próxima seção).

### 9.2 Gráficos customizáveis

O dashboard exibe gráficos construídos com `react-native-gifted-charts`:

- **Receita vs despesas** (barras ou linha).
- **Breakdown de despesas** (donut por categoria).
- **Vendas por categoria** (donut com legenda — ícone e cor da categoria, receita, quantidade, percentual).
- **Top produtos** — **ranking de cards** (não gráfico de barras): medalha ouro/prata/bronze, barra proporcional, qtd vendida, receita. Tap no card abre o item.
- **Valor do estoque** (custo + venda + margem potencial).
- **Atividade** (faturamento mensal).

Você pode **esconder ou reordenar** gráficos; a configuração fica salva localmente.

### 9.3 Cadastrar despesas

Com permissão `financeiro.manageExpenses`:

- **Despesas fixas** (recorrentes): aluguel, conta de luz. Nome, categoria, valor, frequência.
- **Despesas variáveis** (pontuais): compra de material, conserto. Nome, categoria, valor, data.
- **Categorias de despesa**: CRUD próprio (nome, ícone, cor).

### 9.4 Extratos

- **Meu extrato** (permissão `financeiro.participant`): acessível pelo drawer → "Meu Extrato". Duas abas:
  - **Resumo**: saldo devedor grande, progresso de pagamento, stats, atividade recente, histórico mensal.
  - **Transações**: total em aberto, busca, filtros, cards por mês.
- **Detalhe do débito**: bottom sheet com total, valor pago/restante, lista de produtos (carregada da venda vinculada), link "Ver venda vinculada", histórico de pagamentos.
- **Pagar débito**: tap em "Pagar" abre bottom sheet estilo Nubank — valor centralizado, quick amounts (25/50/75/Total), método de pagamento, observação opcional, review card, CTA.
- **Pagamento em lote**: long-press selecione múltiplos débitos → barra flutuante → pagar tudo junto.

- **Extratos admin** (permissão `financeiro.viewStatements`): tela `financeiro-statements.tsx` lista todos os usuários com débito, permite dar baixa em pagamentos informados e ver o extrato individual de qualquer pessoa.

### 9.5 Contas a receber

No dashboard financeiro, um card agrega todos os débitos pendentes em aberto (soma e contagem).

---

## 10. Módulo Mensalidades

### 10.1 Minhas mensalidades (usuário)

Se seu terreiro cobra mensalidade e seu role se aplica, você verá **Mensalidades** no drawer. A tela **Minhas Mensalidades** mostra:

- Mensalidade do mês atual com status (Pendente, Em análise, Pago, Parcial, Atrasado).
- Histórico dos meses anteriores.

### 10.2 Informar pagamento

1. Toque na mensalidade pendente.
2. Toque em **"Informar pagamento"**.
3. Preencha valor, método e comprovante (se solicitado).
4. Status muda para **"Em análise"**. Aguarde a confirmação do administrador.

### 10.3 Aguardar confirmação

Você recebe uma notificação push + in-app quando o administrador **confirma** (status → Pago/Parcial) ou **rejeita** (volta ao status anterior). Nas duas telas (sua e do admin), abrir a tela marca todas as notificações relacionadas como lidas.

### 10.4 Painel do administrador

Com permissão `mensalidade.manage` ou `mensalidade.confirmPayment`, acesse **Mensalidades > Admin**. A tela tem abas horizontais:

- **Pendentes** (sem pagamento informado).
- **Em análise** (pagamentos informados aguardando verificação).
- **Atrasados** (após o grace period).
- **Pagos**.

Toque em uma mensalidade para ver detalhes e **Confirmar**, **Rejeitar** ou **Registrar pagamento direto** (sem passar por "em análise").

### 10.5 Enviar cobrança em lote (permissão `mensalidade.cobrar`)

Na tela **Mensalidade > Cobrança**, selecione usuários (filtros disponíveis) e dispare envio em lote. Cada destinatário recebe push + notificação in-app.

### 10.6 Configurar (permissão `mensalidade.manage`)

Em **Mensalidade > Configuração**:

- **Ativar** mensalidades para o terreiro.
- **Roles aplicáveis** (só esses pagam).
- **Modelo de preço**: `global` (mesmo valor para todos) ou `per_role` (cada role tem seu valor).
- **Grupos de pagamento**: cada grupo define um **dia de vencimento** (1–28). Usuários escolhem o grupo ao qual pertencem.
- **Grace period** (dias após o vencimento antes de marcar como atrasado).

A geração das mensalidades de cada mês acontece **automaticamente no dia 1 às 06:00** (horário de São Paulo). Também pode ser disparada manualmente.

---

## 11. Notificações

### 11.1 Centro de notificações

Acesse pelo **sininho** no dashboard ou pelo drawer. A lista inclui tudo dos últimos 30 dias:

- Novos avisos, comentários, respostas, curtidas.
- Eventos criados/alterados, RSVPs recebidos (para criador), lembretes (24h, 1h).
- Tarefas atribuídas, deadline amanhã, tarefas concluídas.
- Novos materiais (com respeito aos `allowedRoles`).
- Assuntos publicados (para jogadores).
- Pendências de aprovação (para admins).
- Confirmação/rejeição de pagamento de mensalidade.
- Status da sua conta (aprovada/rejeitada).

Tap em uma notificação abre o conteúdo e marca como lida.

### 11.2 Preferências

Drawer > **Notificações**. A tela é dinâmica — só mostra seções dos módulos que você tem acesso. Você encontra:

- **Master switch** (desliga tudo exceto críticas).
- Toggles por módulo e categoria (ex.: "Novos avisos", "Curtidas", "Lembretes de evento", "Tarefas atribuídas").

Desativar aqui **não remove a notificação in-app** — ela ainda aparece no sininho. O que muda é que você não recebe o **push** no celular.

### 11.3 Notificações críticas

Algumas notificações ignoram preferências e master switch:

- **Conta aprovada**.
- **Conta rejeitada**.

São enviadas sempre, independentemente das configurações.

---

## 12. Administração

Esta seção é para quem tem roles com permissões `admin.*`. A maior parte dos links está agrupada em **Administração** no drawer e no dashboard.

### 12.1 Dashboard admin

A tela reúne atalhos para:

- **Usuários pendentes** (se houver).
- **Todos os usuários**.
- **Roles**.
- **Atribuir roles**.
- **Campos obrigatórios** do tenant.
- **Info do terreiro**.
- Atalhos específicos dos módulos (ex.: "Configurar mensalidades", "Gerenciar tipos de evento").

### 12.2 Aprovar usuários pendentes (`admin.approveUsers`)

Tela **Usuários pendentes** lista quem aguarda aprovação. Toque em um card para ver nome, e-mail, data de cadastro. Toque em **Aprovar** ou **Rejeitar**. Uma notificação é enviada ao usuário.

### 12.3 Gerenciar usuários (`admin.manageUsers`)

Tela **Usuários** lista todos os usuários do terreiro:

- **Busca** por nome, e-mail ou role.
- **Filtros por status** (Todos, Aprovados, Pendentes) — só aparecem se houver pendentes.
- **Filtros por role** (chips dinâmicos) — só aparecem se houver 2+ roles.
- **Long-press** em um usuário: alterar role, visualizar como (Run As User).

### 12.4 Gerenciar roles (`admin.manageRoles`)

- **Criar role**: nome, cor, permissões (multi-select dentre as 40 do sistema).
- **Editar role**.
- **Excluir role**:
  - Roles `isDefault` (padrão) não podem ser excluídos.
  - Sem usuários vinculados → exclusão direta.
  - Com usuários → tela dedicada com duas opções: **"Atribuir todos para outro role"** (bulk) ou **"Atribuir individualmente"** (select por usuário). Depois exclui.

### 12.5 Atribuir role (`admin.assignRoles`)

Tela **Atribuir** permite associar um role a qualquer usuário. Muda imediatamente o que ele vê no app.

### 12.6 Campos obrigatórios (`admin.manageTenant`)

Tela **Campos obrigatórios do terreiro** permite marcar quais campos do perfil são obrigatórios (foto, data de nascimento, telefone, gênero, endereço, CPF, RG). Se você adicionar um novo campo obrigatório, **usuários já aprovados são bloqueados** até completar o perfil.

### 12.7 Info do terreiro (`admin.manageTenant`)

Tela **Editar terreiro** permite alterar nome, descrição e logo do terreiro. A visualização está disponível para todos os usuários autenticados.

### 12.8 Run As User / Run As Role

Recurso poderoso para admins validarem o que outros usuários veem.

- **Run As User**: em Usuários, long-press → "Visualizar como". Assume **totalmente** a identidade do usuário (perfil, dados, progresso, permissões). Toda escrita usa `effectiveUserId`, então é como se o próprio usuário estivesse usando o app.
- **Run As Role**: em Roles, long-press → "Visualizar como". Assume **apenas as permissões** do role. Identidade do admin preservada.

Um banner vermelho (`RunAsBanner`) fica visível o tempo todo durante a impersonação. Botão **"Encerrar"** volta ao normal.

> Notificações push ignoram Run As — sempre vão para o seu user real.

---

## 13. Seu Perfil e Privacidade (LGPD)

### 13.1 Editar perfil

Drawer > seu avatar → **Editar perfil**. Você pode alterar:

- Nome, foto, data de nascimento, telefone, gênero, endereço, CPF, RG.
- O sistema re-criptografa dados sensíveis automaticamente.
- Muda o nome ou foto? Todo lugar que exibe seu nome/foto no app atualiza em tempo real (sem stale data).

### 13.2 Exportar seus dados

Em Perfil → seção **Privacidade e Dados** → **"Exportar meus dados"**:

1. O app compila todos os seus dados em JSON estruturado.
2. Abre o share sheet do sistema.
3. **iOS:** toque em "Salvar em Arquivos".
4. **Android:** toque em "Downloads" ou compartilhe por e-mail.

### 13.3 Excluir sua conta

Em Perfil → seção **Privacidade e Dados** → **"Excluir minha conta"**:

1. Confirme a ação (ela é irreversível).
2. O app registra `lgpdDeletionRequestedAt`.
3. Uma Cloud Function limpa seus dados em cascata: sensíveis, tokens de push, notificações in-app, progresso, roles, RSVPs, likes, comentários, materiais criados pelo usuário, participações em quiz rooms.
4. **Dados financeiros (mensalidades, pagamentos, débitos, vendas) são retidos por até 5 anos** — exigência fiscal (LGPD Art. 16 II).
5. Sua conta Firebase Auth é deletada.

Não há reativação. Para voltar, você cria uma conta nova do zero (novo uid, precisa de nova aprovação).

### 13.4 Política de privacidade e termos

Sempre acessíveis:

- **Login/cadastro:** links diretos.
- **Consentimento LGPD:** modal inline durante gates.
- **Perfil:** seção "Privacidade e Dados".

---

## 14. Temas

Drawer > **Tema**. Três opções:

- **Claro** (padrão).
- **Escuro**.
- **Sistema** (acompanha o tema do celular/computador).

A preferência é salva localmente (`@mironga/themeMode`). O app adapta todas as cores automaticamente — cada terreiro pode ainda ter `themeOverrides` específicas definidas pelo seu administrador.

---

## 15. Instalação como PWA (Web)

Se você acessa o Mironga pelo navegador (Safari iOS, Chrome Android/desktop), o app detecta e exibe um **gate de instalação** antes de permitir o acesso completo.

### No iPhone/iPad (Safari)

1. Toque no botão de compartilhar (ícone de seta saindo do quadrado).
2. Role e escolha **"Adicionar à Tela Inicial"**.
3. Dê um nome e confirme.

### No Android (Chrome)

1. Toque no menu (três pontos no canto superior direito).
2. Escolha **"Instalar app"** ou **"Adicionar à tela inicial"**.

### No desktop (Chrome, Edge)

1. Ícone de instalação aparece na barra de endereços (monitor com seta para baixo).
2. Clique e confirme.

Após instalar, abra o ícone como se fosse um app nativo.

---

## 16. Perguntas Frequentes e Solução de Problemas

### O botão "Entrar com Apple" não aparece

- Requer iOS 13+ em um dispositivo físico.
- Não funciona em Expo Go — precisa de Development Build.
- Se o build é compartilhado entre tenants, cada tenant precisa ter a capability **"Sign In with Apple"** habilitada no Apple Developer Portal com seu bundle ID.

### O login com Google falha

- Verifique conexão com a internet.
- Nas versões nativas, seu terreiro precisa ter `oauth.googleIosClientId` / `googleAndroidClientId` configurado.
- Se o problema persistir, peça suporte ao administrador.

### Esqueci minha senha

- Na tela de login, toque em **"Esqueci minha senha"**.
- Digite seu e-mail.
- Você receberá um link de redefinição. Por segurança, a mensagem é genérica (não revela se o e-mail existe).

### Não recebo notificações push

- Verifique se o seu sistema operacional permite notificações para o Mironga (Ajustes > Notificações > [Nome do App]).
- Verifique as preferências internas: Drawer > **Notificações**.
- iOS: push só funciona em dispositivo físico, não em simulador.
- Android: confirme que os canais de notificação não foram desativados.

### Um PDF não abre ou dá erro

- Confirme que você tem conexão (ou que o arquivo já está cacheado).
- Se for um PDF restrito, lembre-se: não é possível exportar nem fazer screenshot.
- Feche e abra o app novamente.

### O áudio não toca

- Verifique se o modo silencioso do celular não está ativo.
- Em iOS, certifique-se de que o app tem permissão de áudio.
- Ao reabrir, o áudio continua de onde parou.

### Consigo usar offline?

- **Sim.** O Mironga é offline-first. Funcionam offline: leitura do dashboard, avisos lidos anteriormente, PDFs/áudios cacheados, registros do Estoque (ficam em fila e sincronizam quando voltar à internet).
- **Exige internet:** login inicial, criar sala de Quiz Gerenciado, upload de imagens/arquivos.

### Meu nome/foto no app não está atualizado

- Quando você edita o perfil, o cache é invalidado automaticamente e o app reflete o novo nome/foto em todos os lugares.
- Se persistir, feche e reabra o app.

### O sininho mostra "Tudo em dia!"

- Parabéns — você leu todas as notificações. Nenhuma pendência.

### Preciso mudar de tema mais rápido

- Drawer > Tema. A mudança é instantânea.

### Fui "visualizado como" por um admin

- É normal e seguro. Admins podem simular sua conta para diagnosticar problemas ou dar suporte. Todas as ações ficam registradas nos AuditFields como feitas por você — há visibilidade e rastreabilidade, não intrusão silenciosa.

### Excluir minha conta é reversível?

- **Não.** A exclusão é definitiva. Dados não-financeiros são removidos em cascata; dados financeiros ficam retidos por até 5 anos (exigência fiscal). Para voltar ao Mironga do seu terreiro, você precisa criar uma conta nova e ser aprovado novamente.

### Como reportar um bug ou pedir uma feature

- Fale com o administrador do seu terreiro. Ele pode escalar para o time técnico do Mironga.

---

**Fim do manual.** Dúvidas técnicas de desenvolvimento → [CLAUDE.md](../CLAUDE.md) e [SPECS.md](../SPECS.md). Requisitos formais → [REQUIREMENTS.md](./REQUIREMENTS.md). Visão de produto → [PRD.md](../PRD.md).
