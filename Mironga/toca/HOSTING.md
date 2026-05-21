# Hospedar Política de Privacidade e Termos — TOCA

A Play Store e App Store exigem **URL pública** para Política de Privacidade. Três opções, da mais simples à mais profissional:

---

## Opção 1 — GitHub Pages (gratuito, ~2min)

A mais rápida. Bom para começar.

### 1.1) Criar repositório

```bash
cd ~/Documents  # ou onde preferir
mkdir toca-legal && cd toca-legal
git init -b main
```

### 1.2) Copiar os arquivos HTML

```bash
cp /Users/igorpereiradossantos/Igor/Umbanda/Mironga/docs/legal/toca/privacy-policy.html .
cp /Users/igorpereiradossantos/Igor/Umbanda/Mironga/docs/legal/toca/terms-of-use.html .
```

### 1.3) Criar index.html (opcional, redireciona pro principal)

```bash
cat > index.html <<'EOF'
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>TOCA — Documentos Legais</title>
</head>
<body>
  <h1>TOCA — Documentos Legais</h1>
  <ul>
    <li><a href="privacy-policy.html">Política de Privacidade</a></li>
    <li><a href="terms-of-use.html">Termos de Uso</a></li>
  </ul>
</body>
</html>
EOF
```

### 1.4) Commit e push

Crie um repo público no GitHub chamado `toca-legal`. Depois:

```bash
git add .
git commit -m "Initial: TOCA legal documents"
git remote add origin https://github.com/[SEU_USUARIO]/toca-legal.git
git push -u origin main
```

### 1.5) Ativar GitHub Pages

1. Acesse https://github.com/[SEU_USUARIO]/toca-legal/settings/pages
2. **Source:** Deploy from a branch
3. **Branch:** `main` / `(root)` → Save
4. Aguarde 1-2min e a URL ficará disponível em:

```
https://[SEU_USUARIO].github.io/toca-legal/privacy-policy.html
https://[SEU_USUARIO].github.io/toca-legal/terms-of-use.html
```

Cole a URL da privacy policy no Play Console.

---

## Opção 2 — Vercel (gratuito, ~3min, deploy automático em qualquer push)

Mais profissional e permite domínio customizado depois.

### 2.1) Repositório

Mesmos passos da Opção 1 até o `git push`.

### 2.2) Conectar ao Vercel

1. https://vercel.com/new
2. **Import Git Repository** → autorizar GitHub → selecionar `toca-legal`
3. **Framework Preset:** Other
4. **Root Directory:** ./
5. **Build Command:** (deixar vazio)
6. **Output Directory:** ./
7. **Deploy**

URL final:
```
https://toca-legal.vercel.app/privacy-policy.html
https://toca-legal.vercel.app/terms-of-use.html
```

Você pode também adicionar um domínio próprio (ex: `legal.toca.app`) em **Settings → Domains**.

---

## Opção 3 — Domínio próprio (mais profissional, ~R$ 40/ano)

Se você comprar um domínio (Registro.br, Namecheap, etc.):

1. Compre `toca.app` ou similar (R$ 40/ano em registro.br ou ~US$ 12/ano em namecheap)
2. Configure DNS apontando para Vercel (CNAME para `cname.vercel-dns.com`)
3. No Vercel → projeto → Settings → Domains → adicionar `toca.app` (ou subdomain como `legal.toca.app`)
4. URLs finais:
   ```
   https://toca.app/privacy-policy.html
   https://toca.app/terms-of-use.html
   ```

Essa é a configuração ideal para produção, mas pode esperar até o app estar pronto pra ir pra produção pública (não precisa pra Internal Testing).

---

## Recomendação para agora (TestFlight / Internal Testing)

Use **GitHub Pages** (Opção 1). É grátis, rápido e funciona perfeitamente para validação pelo Google/Apple. Depois você migra pra domínio próprio quando for lançar publicamente.

---

## Validar que o link funciona

Antes de colar no Play Console / App Store Connect, abre num navegador anônimo:

```
https://[SEU_USUARIO].github.io/toca-legal/privacy-policy.html
```

Tem que abrir sem login e sem 404. Se aparecer "404 Not Found", aguarde mais alguns minutos (GitHub Pages pode levar até 10min na primeira publicação).
