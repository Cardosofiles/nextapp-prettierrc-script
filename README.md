# 🚀 Script de Criar uma aplicação com NextJS, Shadcn UI com Prettier e ESLint Automatizado no WSL

Este script simplifica o fluxo de trabalho da criação de uma aplicação Next, com Shadcn UI
iniciando e criando um repositório com o Git e Github.

- What is your project named? my-app (Pergunta no início do script)
- Would you like to use TypeScript? No / Yes (Yes para TypeScript)
- Would you like to use ESLint? No / Yes (Yes para ESLint)
- Would you like to use Tailwind CSS? No / Yes (Yes para TailwindCSS)
- Would you like your code inside a `src/` directory? No / Yes (Yes para src)
- Would you like to use App Router? (recommended) No / Yes (Yes para App Router)
- Would you like to use Turbopack for `next dev`? No / Yes (Yes para Turbopack)
- Would you like to customize the import alias (`@/*` by default)? No / Yes (Yes para alias)
- What import alias would you like configured? @/\* (No para configured)

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
  "plugins": [
    "prettier-plugin-tailwindcss"
  ],
  "tailwindConfig": "./tailwind.config.js"
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

Navegue até o diretório ou crie uma pasta para o seu projeto mkdir my-app e rode:

```bash
next-shadcn-prettierrc
```

---

## 🧠 Dica

Você pode personalizar este script para incluir validações, log de histórico, commits convencionais, entre outros.

---

## 📌 Requisitos

- Git instalado
- Node instalado >= 18
- Pnpm instalado globalmente
- Terminal WSL (Ubuntu)
- Github CLI configurado (opcional, para criar repositório no GitHub)

---

## 🛠 Exemplo de uso

```bash
$ create-next
📦 Aguarde a instalação, siga o passo a passo do script no terminal 🚀
```

## Observação

- O script irá criar o arquivo .prettierrc.json, com o conteúdo:

```
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
  ],
  "tailwindConfig": "./tailwind.config.js"
}
```

- Tera as configurações do projeto no diretório .vscode, com o conteúdo:

```
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
  "prettier.enableDebugLogs": false
}
```
