# Play Console — Respostas Conteúdo do App (TOCA)

Documento com as respostas para preencher a seção **"Conteúdo do app"** (App content) no Google Play Console para o app **TOCA** (`com.mironga.toca`). As mesmas respostas valem para o **Lar Nice** (`com.mironga.larnice`) trocando o nome onde aparecer.

Acesso: Play Console → Lar Nice/TOCA → sidebar **Política e programas** → **Conteúdo do app**.

---

## 1. Política de privacidade

**URL pública obrigatória.**

Após hospedar `docs/legal/toca/privacy-policy.html` (GitHub Pages, Vercel ou similar), cole a URL no campo "Política de privacidade". Exemplo:

```
https://[seu-usuario].github.io/toca-legal/privacy-policy.html
```

---

## 2. Acesso ao app (App access)

**Pergunta:** Todos ou apenas algumas partes do meu app estão restritas?

**Resposta:** `Todos ou algumas funcionalidades são restritas`

Justificativa: o app exige login (email/senha, Google ou Apple Sign-In) e novos usuários ficam com `approvalStatus: pending` até aprovação de um administrador. Sem aprovação, não há acesso ao conteúdo.

### Credenciais de teste a fornecer pro Google

Crie uma conta dedicada de revisão Google, **pré-aprovada no Firestore**, e forneça:

| Campo | Valor sugerido |
|---|---|
| Nome de usuário | `googleplay-review@mironga.app` (ou outro email que você crie) |
| Senha | (gerar senha forte — guardar em gerenciador) |
| Instruções de acesso | Abrir o app → tocar em "Já tenho conta" → digitar email e senha → entrar. A conta já está pré-aprovada para acesso direto sem fluxo de aprovação. |

**Como pré-aprovar a conta no Firestore (precisa fazer antes de fornecer ao Google):**

1. Criar a conta normalmente no app TOCA (registro com email/senha)
2. Firebase Console → projeto `mironga-14be8` → Firestore → `tenants/toca/users/{uid}/profile/main`
3. Editar campo `approvalStatus`: mudar de `pending` para `approved`
4. Adicionar role do tipo `medium` ou `membro` em `tenants/toca/accessControl/userRoles/items/{uid}` (atribuir ao ID do role default)

Sem essa pré-aprovação, o revisor da Google fica preso na tela "Aguardando aprovação" e a versão é rejeitada.

---

## 3. Anúncios (Ads)

**Pergunta:** Seu app contém anúncios?

**Resposta:** `Não, meu app não contém anúncios`

Justificativa: o app não exibe banners, intersticiais, rewarded ads, native ads nem qualquer outra forma de publicidade. Não usa AdMob, IronSource, AppLovin ou similar.

---

## 4. Classificação etária (IARC)

Questionário oficial do **International Age Rating Coalition**. As respostas abaixo levam a classificação **Livre (L) — Geral / 3+**.

| Categoria | Pergunta resumida | Resposta |
|---|---|---|
| **Violência** | Existe violência (real, animada, fantasia)? | **Não** |
| **Sangue ou sangue e gore** | Tem representação de sangue? | **Não** |
| **Conteúdo sexual** | Tem nudez, conteúdo sexual ou sugestivo? | **Não** |
| **Linguagem imprópria** | Tem palavrões ou linguagem grosseira? | **Não** |
| **Apostas e jogos de azar** | Permite ou simula apostas, dinheiro real? | **Não** (módulo financeiro registra mensalidades/estoque, não joga) |
| **Compras dentro do app** | Tem in-app purchases / IAP? | **Não** |
| **Conteúdo digital comprável** | Vende moedas/loot boxes/itens digitais? | **Não** |
| **Drogas, álcool, tabaco** | Faz referência ou mostra consumo? | **Não** |
| **Crime real** | Faz referência a crimes reais? | **Não** |
| **Discriminação e ódio** | Promove discriminação ou ódio? | **Não** |
| **Religião** | Tem temática religiosa? | **Sim** (Umbanda, Espiritismo e tradições religiosas brasileiras — religião lícita reconhecida) |
| **Política** | Tem temática política partidária? | **Não** |
| **Localização do usuário** | Compartilha localização com outros usuários? | **Não** (não usamos GPS nem partilha de localização) |
| **Conteúdo gerado por usuários** | Permite que usuários publiquem conteúdo visível a outros? | **Sim** (módulo Avisos — posts, comentários, imagens — apenas visíveis dentro da comunidade fechada; moderação por admins do terreiro) |
| **Compartilhamento de informações pessoais** | Permite compartilhamento livre de PII com estranhos? | **Não** (comunidade fechada, requer aprovação) |
| **Comunicação entre usuários** | Permite chat/mensagens entre usuários? | **Sim, limitado** (comentários em avisos — não há DM/chat direto) |

> **Atenção sobre a categoria Religião:** o questionário IARC pergunta sobre conteúdo religioso para garantir que não há proselitismo violento, ataque a outras religiões ou apologia a sectas. Conteúdo educativo e prático de tradições religiosas legais (Umbanda, Espiritismo) é classificado como **Livre**. Se o questionário tiver subcampos sobre "violência ritual" ou "sacrifício", responda **Não** — o aplicativo trata apenas de aspectos culturais, educativos e organizacionais.

**Resultado esperado:** Livre / Geral / 3+ (todas as faixas etárias).

---

## 5. Público-alvo e conteúdo (Target audience and content)

**Pergunta 1: A qual público-alvo seu app se destina?**

Marcar **somente**:
- ☑️ **18 anos ou mais**

Justificativa: o app gerencia uma comunidade religiosa/espiritual com cobrança de mensalidades, gestão financeira e dados sensíveis (CPF, RG). É dirigido a membros adultos do terreiro/centro. Embora seja seguro para todas as idades em conteúdo, o público-alvo prático são adultos responsáveis pela vida administrativa da comunidade.

**Pergunta 2: O app é projetado especificamente para crianças?**

**Resposta:** `Não`

**Pergunta 3: O app contém conteúdo que possa atrair crianças?**

**Resposta:** `Não`

Justificativa: interface utilitária/administrativa, sem mascotes, personagens cartoon, gamificação infantil ou elementos visuais voltados a crianças. Os jogos educativos (Quiz/Flashcards) têm visual adulto e temática religiosa/espiritual.

**Pergunta 4: Avisos / declarações**

Marcar:
- ☑️ Declaração de que o app **não** se destina a crianças e **não** está sujeito ao COPPA (US) nem ao GDPR Kids (EU)

---

## 6. Apps e jogos para a família (Designed for Families)

**Não se inscrever neste programa.** O TOCA não é direcionado a crianças.

---

## 7. Notícias

**Pergunta:** Seu app é um app de notícias?

**Resposta:** `Não`

---

## 8. COVID-19 / Contact Tracing

**Resposta:** `Não, o app não rastreia contatos nem fornece informações relacionadas a COVID-19`

---

## 9. Dados confidenciais e ações dispositivas

**Pergunta:** Seu app acessa SMS, ligações, ou requer acesso a dados sensíveis do Android?

**Resposta:** `Não`

Justificativa: o app pede permissões padrão (notificações, câmera para foto de perfil, galeria para upload de imagens) — nenhuma das permissões `SMS_*`, `CALL_*` ou de dados confidenciais.

---

## 10. Segurança de dados (Data safety)

Seção mais detalhada. Resumo das respostas:

**Coleta de dados?** `Sim`

**Compartilha dados com terceiros?** `Não` (Firebase é o provedor de infra; Google considera "processador", não "compartilhamento")

**Os dados são criptografados em trânsito?** `Sim` (HTTPS/TLS via Firebase)

**Usuários podem solicitar a exclusão dos dados?** `Sim` (botão "Excluir minha conta" no perfil)

### Tipos de dados coletados (marque os que se aplicam):

**Informações pessoais:**
- ☑️ Nome — obrigatório, conta de usuário, criptografado em trânsito, usuário pode excluir
- ☑️ Endereço de email — obrigatório, conta de usuário, criptografado em trânsito, usuário pode excluir
- ☑️ Número de telefone — opcional, conta de usuário, criptografado em trânsito, usuário pode excluir
- ☑️ Endereço — opcional, conta de usuário, criptografado em trânsito, usuário pode excluir
- ☑️ Documento (CPF/RG) — opcional, identificação no terreiro, criptografado em trânsito **e em repouso (AES-256-GCM)**, usuário pode excluir

**Fotos e vídeos:**
- ☑️ Fotos — opcional (foto de perfil, upload em avisos), conta de usuário e funcionalidades do app, criptografado em trânsito, usuário pode excluir

**Arquivos e documentos:**
- ☑️ Arquivos — funcionalidade do app (uploads de PDF/áudio nos materiais e avisos), criptografado em trânsito

**Atividade no app:**
- ☑️ Interações no app — funcionalidades do app (progresso nos jogos, RSVPs, comentários), criptografado em trânsito

**Identificadores de dispositivo ou de outras coisas:**
- ☑️ ID de instalação único — funcionalidades do app (token de push), criptografado em trânsito, usuário pode excluir

**Não marcar:**
- ❌ Localização (não coletamos GPS)
- ❌ Histórico de pesquisa na web
- ❌ Histórico de instalação de outros apps
- ❌ Dados financeiros bancários (mensalidades são registradas como info, não como pagamento processado pelo app)
- ❌ Áudio gravado pelo usuário
- ❌ Saúde e condicionamento físico

---

## Checklist final antes de publicar

- [ ] Política de Privacidade hospedada em URL pública e linkada
- [ ] Termos de Uso hospedados (opcional, mas recomendado)
- [ ] Conta de teste criada e pré-aprovada no Firestore
- [ ] Credenciais de teste informadas em "Acesso ao app"
- [ ] Anúncios: Não
- [ ] Classificação etária: questionário respondido → Livre (L)
- [ ] Público-alvo: 18+
- [ ] App para família: Não se inscreveu
- [ ] Notícias: Não
- [ ] Segurança de dados: questionário completo
- [ ] Detalhes da loja (descrição curta + longa, ícone 512×512, screenshots de pelo menos 2 dispositivos): apenas se for produção; opcional para Internal Testing

---

## Notas de adaptação para outros tenants

Para reusar este documento em outro tenant (Lar Nice, etc.):

1. **Substituir nomes:** "TOCA" → "Lar Nice"; "terreiro" → "centro" (ou outro `contextName` do tenant)
2. **Substituir bundle ID:** `com.mironga.toca` → `com.mironga.larnice`
3. **Substituir URL da política de privacidade** (cada tenant deve ter sua própria URL pública)
4. **Religião:** se o tenant não for de tradição afro-brasileira, ajustar a categoria do IARC (ainda assim respondendo "Sim" para religião — Espiritismo, católica, etc., todas levam a Livre)
5. **Compartilhamento de info:** se o tenant ativar funcionalidades como "lista pública de membros" ou similar, revisar a seção 10 (Segurança de dados)
