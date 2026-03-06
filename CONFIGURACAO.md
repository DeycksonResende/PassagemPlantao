# 🏥 Plantão Digital — Guia de Configuração

## O que você vai precisar
- Conta Google (gratuita)
- 15 minutos

---

## PASSO 1 — Criar o projeto Firebase

1. Acesse **console.firebase.google.com**
2. Clique em **"Criar um projeto"**
3. Dê um nome (ex: `plantao-enfermagem`)
4. Desative o Google Analytics (não é necessário) → **Criar projeto**

---

## PASSO 2 — Ativar o Firestore

1. No menu lateral, clique em **Firestore Database**
2. Clique em **"Criar banco de dados"**
3. Escolha **"Iniciar no modo de teste"** (permite leitura/escrita por 30 dias)
4. Selecione a região `southamerica-east1 (São Paulo)` → **Ativar**

> ⚠️ Após 30 dias, vá em **Regras** e altere para:
> ```
> rules_version = '2';
> service cloud.firestore {
>   match /databases/{database}/documents {
>     match /{document=**} {
>       allow read, write: if true;
>     }
>   }
> }
> ```

---

## PASSO 3 — Registrar o app Web

1. Na página inicial do projeto, clique no ícone **`</>`** (Web)
2. Dê um apelido (ex: `plantao-web`)
3. **NÃO** marque "Firebase Hosting"
4. Clique em **"Registrar app"**
5. Você verá um bloco de código como este:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "plantao-enfermagem.firebaseapp.com",
  projectId: "plantao-enfermagem",
  storageBucket: "plantao-enfermagem.appspot.com",
  messagingSenderId: "12345...",
  appId: "1:12345...:web:abc..."
};
```

---

## PASSO 4 — Configurar o arquivo HTML

1. Abra o arquivo `plantao-digital.html` em qualquer editor de texto
   (Bloco de Notas, Notepad++, VS Code, etc.)
2. Encontre a seção **CONFIGURAÇÃO FIREBASE** (linha ~15)
3. Substitua os valores placeholder pelos do seu projeto:

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "AIzaSy...",       // ← seu valor
  authDomain:        "plantao-enfermagem.firebaseapp.com",
  projectId:         "plantao-enfermagem",
  storageBucket:     "plantao-enfermagem.appspot.com",
  messagingSenderId: "12345...",
  appId:             "1:12345...:web:abc..."
};
```

4. Opcionalmente, troque a senha admin padrão:
```javascript
const ADMIN_MASTER_PIN = "admin2024"; // ← troque por algo seguro
```

---

## PASSO 5 — Hospedar o app (gratuito)

### Opção A — Netlify (mais fácil, recomendado)
1. Acesse **netlify.com** e crie uma conta gratuita
2. Arraste e solte o arquivo `plantao-digital.html` na área indicada
3. O Netlify gera automaticamente um link como:
   `https://meu-plantao.netlify.app`
4. Compartilhe esse link no grupo do WhatsApp da equipe! ✅

### Opção B — Firebase Hosting (mantém tudo no Firebase)
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# Coloque o HTML na pasta public/
firebase deploy
```

### Opção C — Abrir localmente (sem internet)
- Simplesmente dê dois cliques no arquivo HTML
- **⚠️ Os dados ficam no Firebase (online), mas o app abre direto no navegador**
- Funciona bem se todos usarem a mesma máquina

---

## Como usar

### Primeiro acesso (técnico)
1. Abra o link
2. Digite seu nome completo
3. Escolha um PIN de 4 dígitos
4. Pronto! Conta criada automaticamente.

### Login admin
- Nome: **ADMIN**
- PIN: o que você configurou em `ADMIN_MASTER_PIN`

### O admin pode
- Ver todas as pendências de todos os leitos
- Criar pendências
- Gerenciar equipe (ativar/desativar usuários, promover a admin, resetar PIN)
- Ver o log de auditoria completo (quem criou, quem resolveu, quando)

---

## Estrutura dos dados no Firebase

```
pendencias/
  {id}/
    leito, item, obs, prioridade
    status: "pendente" | "resolvido"
    criadoPor, criadoEm
    resolvidoPor, resolvidoEm

usuarios/
  {id}/
    nome, pin, role: "tecnico" | "admin"
    ativo, criadoEm

auditLog/
  {id}/
    acao, usuario, detalhes, timestamp
```

---

## Dúvidas frequentes

**Q: Os dados somem se fechar o app?**
Não! Ficam salvos no Firebase indefinidamente.

**Q: Funciona no celular?**
Sim, o layout é totalmente responsivo e mobile-first.

**Q: Posso customizar os leitos?**
Sim, abra o HTML e edite o array `LEITOS` (linha ~60).

**Q: E as pendências do checklist?**
Edite o array `ITENS` logo abaixo de `LEITOS`.

**Q: Esqueci meu PIN — o que faço?**
O admin pode resetar seu PIN para `1234` no painel de Equipe.
