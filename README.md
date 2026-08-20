# Painel — Miguel

## Sincronizar PC e celular (recomendado)

Sem isso, cada aparelho guarda os dados separado. Com isso, os dois mostram sempre a mesma coisa. Usamos um **Gist secreto do GitHub** como "banco de dados" — já que você vai ter conta no GitHub de qualquer jeito pra hospedar o site.

1. Vá em https://github.com/settings/tokens → **"Generate new token (classic)"**.
2. Dê um nome (ex: "painel-sync"), marque **só** a caixinha **"gist"** → gere o token → **copie na hora** (ele some depois).
3. Vá em https://gist.github.com → crie um gist **secreto** (não público) com um arquivo chamado `data.json` e conteúdo:
   ```json
   {}
   ```
4. Copie o **ID do gist** (fica no final da URL, depois do seu usuário — ex: `github.com/SEU-USUARIO/a1b2c3d4e5f6...` → o ID é `a1b2c3d4e5f6...`).
5. Abra o `index.html` num editor de texto, procure por `SYNC_CONFIG` (bem no início do `<script>`) e preencha:
   ```js
   const SYNC_CONFIG = {
     token: 'COLE_SEU_TOKEN_AQUI',
     gistId: 'COLE_O_ID_DO_GIST_AQUI',
   };
   ```
6. Salve o arquivo e suba pro GitHub (passo abaixo).

Depois disso, o cabeçalho do painel mostra "sincronizado", e tem um botão **↻ Atualizar** pra puxar mudanças feitas no outro aparelho na hora.

**Importante:** esse token dá acesso aos seus gists — não compartilhe o código do site publicamente com o token dentro. Se for subir num repositório público, é melhor deixar o `SYNC_CONFIG` vazio no GitHub e preencher só localmente em cada aparelho antes de instalar (edita o arquivo, salva, reinstala — dá mais trabalho, mas não expõe o token).

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

