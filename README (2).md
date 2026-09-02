# CVU · Levantamento de Exames — guia de instalação

Esse app funciona assim: o **GitHub Pages** hospeda a página (HTML/CSS/JS), e o
**Firebase** guarda os dados e sincroniza em tempo real entre todo mundo que estiver
logado — em qualquer computador, ao mesmo tempo.

Nenhum dado de paciente fica salvo no código do GitHub. Por isso o repositório pode
ficar **público e gratuito**; a segurança de verdade vem do login (e-mail/senha) e das
regras do Firestore configuradas abaixo.

---

## Parte 1 — Criar o projeto no Firebase (grátis)

1. Acesse **console.firebase.google.com** e clique em **Adicionar projeto**.
2. Dê um nome (ex: `cvu-levantamento`) e siga o assistente (pode desativar o Google
   Analytics, não é necessário).
3. No menu lateral, vá em **Build → Authentication → Get started**.
   - Na aba "Sign-in method", ative **E-mail/senha**.
4. No menu lateral, vá em **Build → Firestore Database → Criar banco de dados**.
   - Escolha **modo de produção** e a região mais próxima (ex: `southamerica-east1`).
5. Ainda em Firestore, vá na aba **Regras** e cole isto, substituindo o que já está lá:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /exames/{examId} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

   Isso garante que só quem estiver **logado** (com conta criada pelo app) consegue
   ler ou gravar dados. Clique em **Publicar**.

6. Volte pra **Visão geral do projeto** (ícone de casa), clique no ícone **`</>`**
   (Web) pra registrar um app da Web. Dê um nome e clique em registrar — **não**
   precisa do Firebase Hosting.
7. O Firebase vai te mostrar um bloco `firebaseConfig = {...}`. Copie esses valores.

---

## Parte 2 — Configurar os arquivos

1. Abra o arquivo **`firebase-config.js`** que veio junto com este projeto.
2. Cole os valores que você copiou no lugar de `"COLE_AQUI"`.

---

## Parte 3 — Subir pro GitHub

1. Crie um repositório novo em **github.com/new** (pode deixar **público**, sem
   custo). Ex: nome `cvu-levantamento`.
2. Envie estes 3 arquivos pra dentro do repositório (pela interface web do GitHub:
   "Add file → Upload files", ou por linha de comando):
   - `index.html`
   - `firebase-config.js` (já com suas chaves)
   - `data.json` (base antiga da planilha, pra importar depois)
3. Vá em **Settings → Pages** do repositório.
   - Em "Source", escolha **Deploy from a branch**, branch `main`, pasta `/root`.
   - Clique em **Save**. Em ~1 minuto o GitHub te dá um link tipo
     `https://seuusuario.github.io/cvu-levantamento/`.

---

## Parte 4 — Primeiro acesso

1. Abra o link do GitHub Pages.
2. Clique em **"Criar conta"**, digite o e-mail e uma senha (mínimo 6 caracteres) de
   cada funcionário que vai cadastrar exames. Cada um cria a própria conta uma vez;
   depois só faz login normalmente.
3. Na aba **Registros**, clique em **"Importar planilha antiga"** — isso só precisa
   ser feito **uma vez**, por qualquer pessoa, pra trazer os 119 registros que já
   existiam pro sistema novo.
4. Pronto: qualquer cadastro feito na aba **Cadastro**, por qualquer funcionário,
   aparece **na hora** pra todo mundo, inclusive na aba **Levantamento**
   (quantos raio-X de tórax, quantos ultrassons de abdômen, etc — agrupado
   automaticamente por exame, região, espécie e sistema afetado).

---

## Limites do plano grátis do Firebase (mais do que suficiente aqui)

- 50 mil leituras e 20 mil gravações de dados por dia.
- 1 GB de armazenamento.
- Pra uma clínica com algumas dezenas de cadastros por dia, isso não chega nem
  perto do limite.

## Se quiser editar o formulário depois

Todo o app está em um único arquivo, o `index.html` — os campos do formulário estão
na seção `<!-- CADASTRO -->`, e os agrupamentos do Levantamento estão na função
`renderLevantamento()` no final do arquivo. Dá pra pedir ajuda pra mim a qualquer
momento pra adicionar campos, editar registros existentes, ou exportar de volta pra
Excel.
