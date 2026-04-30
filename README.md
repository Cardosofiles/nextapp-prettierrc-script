# 🚀 Script criador de aplicação com NextJS, Shadcn UI com Prettier e ESLint com push para o Github Automatizado no WSL

Este script simplifica o fluxo de trabalho da criação de uma aplicação Next, com Shadcn UI iniciando e criando um repositório com o Git e Github.

O `create-next-app` exibe uma pergunta inicial sobre as configurações recomendadas:

```
Would you like to use the recommended Next.js defaults?
  ❯ Yes, use recommended defaults - TypeScript, ESLint, Tailwind CSS, App Router, AGENTS.md
    No, reuse previous settings
    No, customize settings - Choose your own preferences
```

Ao escolher **No, customize settings**, as perguntas são:

- What is your project named? **my-app**
- Would you like to use TypeScript? **Yes**
- Which linter would you like to use? **ESLint** *(ESLint / Biome / None)*
- Would you like to use React Compiler? **No** *(novo no Next.js 15+)*
- Would you like to use Tailwind CSS? **Yes**
- Would you like your code inside a `src/` directory? **Yes**
- Would you like to use App Router? (recommended) **Yes**
- Would you like to customize the import alias (`@/*` by default)? **No**
- Would you like to include AGENTS.md to guide coding agents? **No** *(novo no Next.js 15+)*

> ⚠️ A pergunta **"Would you like to use Turbopack for `next dev`?"** foi removida. O Turbopack agora é o bundler padrão.

---

## 📁 Estrutura

- Script: `next-shadcn-prettierrc`
- Local: `~/next-shadcn-prettierrc.sh`

---

## 📜 Conteúdo do Script

```bash
#!/bin/bash

clear
echo "🚀 Criador de projetos Next.js com pnpm + Tailwind + ESLint + Prettier + Shadcn + GitHub"

# 🧠 Pergunta o nome do projeto
read -p "📦 Qual o nome do projeto? " project_name

if [ -z "$project_name" ]; then
  echo "❌ Nome do projeto não pode estar vazio."
  exit 1
fi

# 🚧 Cria o projeto com as opções desejadas
pnpm create next-app@latest "$project_name" \
  --ts \
  --tailwind \
  --app \
  --src-dir \
  --eslint \
  --import-alias '@/*' \
  --turbopack \
  --no

cd "$project_name" || exit 1

echo "✅ Projeto criado com sucesso!"

# 💿 Instala dependências do Shadcn UI
echo "📦 Instalando dependências do Shadcn UI..."
pnpm add @shadcn/ui clsx tailwind-variants

# ⚙️ Inicializa Shadcn UI
echo "⚙️ Inicializando Shadcn UI..."
echo "🎨 Ao iniciar, selecione a cor base desejada usando as setas do teclado (⬆️⬇️) e pressione ENTER:"
echo "    → Neutral"
echo "    → Gray"
echo "    → Zinc"
echo "    → Stone"
echo "    → Slate"
pnpm dlx shadcn@latest init

# 🧹 Instala o Prettier + Plugin Tailwind
echo "🎯 Instalando Prettier e plugin Tailwind..."
pnpm add -D prettier prettier-plugin-tailwindcss

# 🛠️ Cria .prettierrc.json com configurações customizadas
cat > .prettierrc.json <<EOF
{
  "\$schema": "http://json.schemastore.org/prettierrc",
  "singleQuote": true,
  "jsxSingleQuote": false,
  "semi": false,
  "trailingComma": "es5",
  "arrowParens": "avoid",
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "bracketSameLine": false,
  "plugins": [ "prettier-plugin-tailwindcss" ]
}
EOF

# ⚙️ Cria configurações para VSCode em .vscode/settings.json
mkdir -p .vscode
cat > .vscode/settings.json <<EOF
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "eslint.workingDirectories": [
    {
      "mode": "auto"
    }
  ],
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "files.eol": "\\n",
  "files.insertFinalNewline": true,
  "prettier.enableDebugLogs": false,
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
EOF

# 🧬 Inicializa Git + GitHub
read -p "🔗 Deseja criar repositório Git local? (y/n): " git_init
if [[ "$git_init" == "y" ]]; then
  git init
  git add .
  git commit -m "🧱 initial commit"

  read -p "🌐 Deseja criar um repositório no GitHub e fazer push? (y/n): " gh_push
  if [[ "$gh_push" == "y" ]]; then
    read -p "🔐 Qual o nome do repositório no GitHub (ou ENTER para '$project_name')? " repo_name
    repo_name=${repo_name:-$project_name}

    gh repo create "$repo_name" --public --source=. --remote=origin --push
    echo "✅ Repositório enviado para o GitHub!"
  fi
fi

# ✅ Finalização
echo -e "\n🎉 Projeto '$project_name' criado e pronto para desenvolvimento!"
echo -e "\n👉 Rode com: \033[1mpnpm dev\033[0m"
```

---

## ✅ Como Instalar e Usar

### 1. 📥 Criar o script

```bash
nano ~/next-shadcn-prettierrc.sh
```

Cole o conteúdo acima e salve (`Ctrl+O`, `Enter`, `Ctrl+X`).

---

### 2. 🔓 Dar permissão de execução

```bash
chmod +x ~/next-shadcn-prettierrc.sh
```

---

### 3. ⚙️ Tornar o script global

Edite o arquivo `.zshrc` ou `.bashrc`:

```bash
nano ~/.zshrc
```

Adicione esta linha ao final:

```bash
alias next-shadcn-prettierrc="~/next-shadcn-prettierrc.sh"
```

Salve e recarregue o terminal:

```bash
source ~/.zshrc
```

---

### 4. 🚀 Usar o comando

Navegue até o diretório desejado e rode:

```bash
next-shadcn-prettierrc
```

---

## 🧠 Dica

Você pode personalizar este script para incluir validações, log de histórico, commits convencionais, entre outros.

---

## 📌 Requisitos

- Git instalado
- Node.js instalado **>= 20.9**
- pnpm instalado globalmente
- Ambiente Linux (Ubuntu, Kali, Arch, WSL, etc...)
- GitHub CLI configurado (opcional, para criar repositório no GitHub)

---

## 🛠 Exemplo de uso

```bash
$ next-shadcn-prettierrc
📦 Qual o nome do projeto?
```

---

## Observação

O script irá criar o arquivo `.prettierrc.json` com o conteúdo:

> ⚠️ **Tailwind CSS v4**: Não utiliza mais `tailwind.config.js`. A configuração de tema é feita via diretiva `@theme` no `globals.css`. O plugin `prettier-plugin-tailwindcss` v0.6+ detecta automaticamente o stylesheet.

```json
{
  "$schema": "http://json.schemastore.org/prettierrc",
  "singleQuote": true,
  "jsxSingleQuote": false,
  "semi": false,
  "trailingComma": "es5",
  "arrowParens": "avoid",
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "bracketSameLine": false,
  "plugins": [
    "prettier-plugin-tailwindcss"
  ]
}
```

Terá as configurações do projeto no diretório `.vscode`, com o conteúdo:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "eslint.workingDirectories": [
    {
      "mode": "auto"
    }
  ],
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "files.eol": "\n",
  "files.insertFinalNewline": true,
  "prettier.enableDebugLogs": false,
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

---

## Resumo das alterações (Next.js 15+ / Tailwind v4)

| Seção | Antes | Depois |
|---|---|---|
| Prompt ESLint | `Would you like to use ESLint? Yes` | `Which linter would you like to use? ESLint` |
| Prompt Turbopack | `Would you like to use Turbopack? Yes` | Removido — padrão no Next.js 15+ |
| Novos prompts | — | React Compiler (No) e AGENTS.md (No) |
| Node.js | `>= 18` | `>= 20.9` |
| Tailwind v4 | `tailwind.config.js` | Configuração via `@theme` em `globals.css` |

## 🙏 Agradecimentos

`Obrigado por usar este repositório!`

Se este script te ajudou a economizar tempo e agilizou o seu fluxo de trabalho,
considere deixar uma ⭐ no repositório — isso ajuda muito o projeto a crescer e
chegar a mais desenvolvedores.

Contribuições, sugestões e feedbacks são sempre bem-vindos.
Sinta-se à vontade para abrir uma _issue_ ou enviar um _pull request_. 🚀

> Feito pensando em produtividade e boas práticas de código.

