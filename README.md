# Quiz ETEC — Kahoot com Firebase

Sistema de quiz multijogador em tempo real para uso em sala de aula.

---

## Arquivos

| Arquivo | Quem usa | Onde abrir |
|---|---|---|
| `host.html` | Professor | Computador/projetor |
| `player.html` | Alunos | Celular/notebook |

---

## Setup Firebase (5 minutos, uma vez só)

### 1. Criar o projeto
- Acesse https://console.firebase.google.com
- Clique em **Adicionar projeto** → dê um nome → desative Google Analytics → **Criar projeto**

### 2. Criar o Realtime Database
- No menu lateral: **Build → Realtime Database**
- Clique em **Criar banco de dados**
- Escolha região: **us-central1** (mais próxima disponível)
- Em "Regras de segurança": selecione **Modo de teste** → **Ativar**

### 3. Regras abertas (para testes em sala)
No console do Realtime Database, aba **Regras**, cole:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
Clique em **Publicar**.

> **Atenção:** Regras abertas são adequadas para uso em sala. Para produção permanente, implemente autenticação.

### 4. Obter as credenciais
- No menu lateral: **Visão geral do projeto** (ícone ⚙) → **Configurações do projeto**
- Role até **Seus apps** → clique em **</>** (Web) → dê um nome → **Registrar app**
- Copie o objeto `firebaseConfig` — você vai precisar dos campos:
  - `apiKey`
  - `authDomain`
  - `databaseURL` ← **importante: deve ter `.firebaseio.com`**
  - `projectId`
  - `storageBucket`
  - `messagingSenderId`
  - `appId`

### 5. Configurar o host.html
- Abra `host.html` no navegador
- Preencha os campos com as credenciais do passo 4
- Clique em **Salvar e continuar**
- As credenciais ficam salvas no `localStorage` — você não precisa inserir novamente

---

## Como usar em sala

### Professor (host.html)
1. Abra `host.html` no computador conectado ao projetor
2. Escolha o tema e configurações do quiz
3. Clique em **Gerar questões com IA e criar sala**
4. Projete o código de 4 dígitos e a URL para os alunos
5. Aguarde os alunos entrarem no lobby
6. Clique em **Iniciar quiz**
7. Para cada questão: aguarde o timer ou clique em **Revelar antecipadamente**
8. Após a última revelação: placar final + exportar CSV

### Alunos (player.html)
1. Abra `player.html` no celular (ou acesse a URL completa exibida no projetor)
2. Digite o código de 4 dígitos e seu nome
3. Aguarde o professor iniciar
4. Responda cada questão antes do timer zerar
5. Pontuação: até 1000 pts por questão (bônus por velocidade)

---

## Distribuir para os alunos

### Opção A — Arquivo local (mais simples)
Envie `player.html` por WhatsApp/email. O aluno abre no celular diretamente.
> O Firebase já está configurado via `localStorage` do host — **mas o player.html precisa do mesmo localStorage**, então prefira a opção B.

### Opção B — Servidor local (recomendado)
No terminal, na pasta do projeto:
```bash
# Python 3
python -m http.server 8080

# Node.js
npx serve .
```
Acesse: `http://SEU-IP-LOCAL:8080/player.html?sala=CODIGO`

Exiba o link com QR Code (use https://qr.io ou similar).

### Opção C — Hospedagem gratuita
Suba os dois arquivos no **GitHub Pages**, **Netlify Drop** (arraste a pasta) ou **Vercel**.
Os alunos acessam pela URL pública.

---

## Pontuação
- Questão correta: **500 a 1000 pts** (proporcional ao tempo restante)
- Questão errada ou tempo esgotado: **0 pts**
- Fórmula: `pts = 500 + (timeLeft / totalTime) * 500`

---

## Limpeza do banco
Após cada aula, salas antigas ficam no Firebase. Para limpar:
- Acesse o console do Firebase → Realtime Database → apague a chave `rooms`

---

## Requisitos
- Navegador moderno (Chrome, Safari, Firefox)
- Conexão com internet (Firebase é online)
- Chave de API Anthropic válida (geração de questões)
