# Painel — Miguel

## Sincronizar PC e celular (recomendado)

Sem isso, cada aparelho guarda os dados separado. Com isso, os dois mostram sempre a mesma coisa, atualizando sozinho em tempo real. Usamos o **Firebase** (Google) — nesse caso você faz **login com email e senha**, sem precisar mexer em token nenhum no dia a dia.

### Passo 1 — Criar o projeto Firebase (só uma vez)

1. Vá em https://console.firebase.google.com → **"Criar um projeto"** (ou "Add project") → dê um nome (ex: "painel-miguel") → pode desativar o Google Analytics → **Criar**.
2. Dentro do projeto, no menu esquerdo: **Build → Authentication → Get started** → aba **Sign-in method** → ativa **"Email/Password"** → Save.
3. Ainda no menu esquerdo: **Build → Firestore Database → Create database** → escolha modo **produção** → escolha uma localização (qualquer uma próxima, ex: `southamerica-east1`) → Enable.
4. Na aba **Rules** do Firestore, substitua o conteúdo por isto e clica em **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /paineis/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```
   (Isso garante que só você, logado, consegue ler ou escrever seus próprios dados.)
5. Volta pra visão geral do projeto (ícone de engrenagem → **Project settings**) → desce até **"Your apps"** → clica no ícone **</>** (Web) → dá um nome → **Register app**. Vai aparecer um bloco `firebaseConfig = {...}` — copie ele.

### Passo 2 — Colar a config no arquivo (só uma vez, pode subir pro GitHub)

Abra o `index.html` num editor de texto, procure por `FIREBASE_CONFIG` (perto do início do `<script>`) e preencha com os valores que você copiou:
```js
const FIREBASE_CONFIG = {
  apiKey: 'AIzaSy...',
  authDomain: 'painel-miguel-xxxx.firebaseapp.com',
  projectId: 'painel-miguel-xxxx',
  storageBucket: 'painel-miguel-xxxx.appspot.com',
  messagingSenderId: '123456789',
  appId: '1:123456789:web:abc123',
};
```
Esses valores **não são segredo** — o Firebase é feito pra funcionar assim, com essa config visível no código. A segurança de verdade são as Regras que você colou no passo 1.4 (só quem logar com sua conta acessa seus dados). Pode subir esse arquivo pro GitHub sem medo de bloqueio.

### Passo 3 — Login (uma vez por aparelho)

1. Suba o `index.html` (já com a config preenchida) pro GitHub Pages.
2. Abre o site → aba **⚙ Config** → **"Criar conta"** com um email e senha que você escolher.
3. Em qualquer outro aparelho (celular, outro PC), abre o mesmo site → aba ⚙ Config → **"Entrar"** com o mesmo email e senha.

Depois de logado uma vez, o navegador continua logado sozinho — não pede de novo. Os dados sincronizam automaticamente e em tempo real entre todos os aparelhos logados com a mesma conta.

## Como colocar no GitHub Pages (rápido, PC e celular)

1. Crie um repositório no GitHub (pode ser público ou privado).
2. Suba estes 5 arquivos na raiz do repositório:
   - index.html
   - manifest.json
   - service-worker.js
   - icon-192.png
   - icon-512.png
3. No repositório: Settings → Pages → Source → escolha a branch (geralmente `main`) e pasta `/ (root)` → Save.
4. Espere 1-2 minutos. O GitHub te dá um link tipo:
   `https://SEU-USUARIO.github.io/NOME-DO-REPO/`
5. Abra esse link no celular e no PC.

## Instalar como app

- **Celular (Android/Chrome):** abra o link → menu (⋮) → "Adicionar à tela inicial" ou vai aparecer um prompt automático de instalar.
- **iPhone (Safari):** abra o link → botão de compartilhar → "Adicionar à Tela de Início".
- **PC (Chrome/Edge):** abra o link → ícone de instalar na barra de endereço (ou menu → "Instalar app").

Depois de instalado, abre como um app de verdade, com ícone próprio, sem barra do navegador — e funciona offline (os dados continuam salvos no aparelho onde você usa).

## Importante

- **Se você configurou a sincronização (passo acima):** os dados ficam iguais no PC e no celular, atualizando quando você entra ou aperta "↻ Atualizar".
- **Se não configurou:** os dados ficam por navegador/aparelho, sem sincronizar.
- Qualquer atualização de conteúdo (mudar código) precisa subir um novo index.html pro GitHub.

