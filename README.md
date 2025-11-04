# 🔐 Guia Completo: Assinatura Automática de Commits com GPG

![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)
![GPG](https://img.shields.io/badge/GPG-Verified-success?style=for-the-badge)

Este guia ensina como configurar commits assinados automaticamente com GPG no Windows, Linux e macOS.

---

## 📋 Índice

- [O que é GPG e Por que Usar?](#o-que-é-gpg-e-por-que-usar)
- [Instalação](#instalação)
  - [Windows](#windows)
  - [Linux](#linux)
  - [macOS](#macos)
- [Configuração](#configuração)
  - [1. Gerar Chave GPG](#1-gerar-chave-gpg)
  - [2. Listar e Obter ID da Chave](#2-listar-e-obter-id-da-chave)
  - [3. Configurar Git](#3-configurar-git)
  - [4. Exportar Chave Pública](#4-exportar-chave-pública)
  - [5. Adicionar no GitHub/GitLab](#5-adicionar-no-githubgitlab)
  - [6. Testar](#6-testar)
- [Configurações Avançadas](#configurações-avançadas)
- [Troubleshooting](#troubleshooting)
- [Comandos Úteis](#comandos-úteis)

---

## 🎯 O que é GPG e Por que Usar?

**GPG (GNU Privacy Guard)** é uma ferramenta de criptografia que permite assinar commits do Git, garantindo:

✅ **Autenticidade**: Prova que você é realmente o autor do commit  
✅ **Integridade**: Garante que o commit não foi alterado  
✅ **Confiança**: Commits verificados aparecem com badge "Verified" no GitHub  
✅ **Segurança**: Protege contra ataques de personificação  

### Badge "Verified" no GitHub:

```
✅ Verified
This commit was signed with a verified signature
```

---

## 🛠️ Instalação

### Windows

#### Opção 1: Via Chocolatey (Recomendado)

```powershell
# Instale o Chocolatey primeiro (se não tiver)
# https://chocolatey.org/install

# Instale o GPG4Win
choco install gpg4win
```

#### Opção 2: Download Manual

1. Baixe: https://www.gpg4win.org/download.html
2. Execute o instalador
3. Instale com as opções padrão
4. Reinicie o terminal

#### Verificar Instalação:

```powershell
gpg --version
```

---

### Linux

#### Ubuntu/Debian:

```bash
sudo apt-get update
sudo apt-get install gnupg
```

#### Fedora/RHEL/CentOS:

```bash
sudo dnf install gnupg2
```

#### Arch Linux:

```bash
sudo pacman -S gnupg
```

#### Verificar Instalação:

```bash
gpg --version
```

---

### macOS

#### Via Homebrew (Recomendado):

```bash
# Instale o Homebrew primeiro (se não tiver)
# https://brew.sh/

# Instale o GPG
brew install gnupg
```

#### Via MacPorts:

```bash
sudo port install gnupg2
```

#### Verificar Instalação:

```bash
gpg --version
```

---

## ⚙️ Configuração

### 1. Gerar Chave GPG

Execute o comando:

```bash
gpg --full-generate-key
```

#### Durante a geração, responda:

**1. Tipo de chave:**
```
Please select what kind of key you want:
   (1) RSA and RSA (default)
Your selection? 1
```

**2. Tamanho da chave:**
```
What keysize do you want? (3072) 4096
```
✅ Recomendado: **4096 bits**

**3. Validade:**
```
Key is valid for? (0) 0
```
- `0` = nunca expira
- `2y` = expira em 2 anos
- `365` = expira em 365 dias

**4. Confirmação:**
```
Is this correct? (y/N) y
```

**5. Informações pessoais:**
```
Real name: Pedro Vieira
Email address: seu-email@exemplo.com
Comment: GitHub GPG Key
```

⚠️ **IMPORTANTE**: Use o **mesmo email** configurado no Git!

**6. Senha:**

Crie uma senha forte para proteger sua chave.

---

### 2. Listar e Obter ID da Chave

```bash
gpg --list-secret-keys --keyid-format=long
```

**Exemplo de saída:**

```
sec   rsa4096/3AA5C34371567BD2 2025-01-15 [SC]
      ABCD1234EFGH5678IJKL9012MNOP3456QRST7890
uid                 [ultimate] Pedro Vieira <seu-email@exemplo.com>
ssb   rsa4096/4BB6D45482678CE3 2025-01-15 [E]
```

📌 **Seu ID da chave é:** `3AA5C34371567BD2` (depois de `rsa4096/`)

---

### 3. Configurar Git

Substitua `3AA5C34371567BD2` pelo **seu ID da chave**:

```bash
# Configure o ID da sua chave
git config --global user.signingkey 3AA5C34371567BD2

# Ative assinatura automática de commits
git config --global commit.gpgsign true

# Ative assinatura automática de tags
git config --global tag.gpgSign true
```

#### Configuração Específica para Windows:

Se estiver no Windows e tiver problemas, adicione:

```bash
# Windows: especifique o caminho do GPG
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"

# Ou se instalou via Chocolatey:
git config --global gpg.program gpg
```

---

### 4. Exportar Chave Pública

```bash
# Substitua pelo seu ID
gpg --armor --export 3AA5C34371567BD2
```

**Saída:**

```
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQINBGXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
...
-----END PGP PUBLIC KEY BLOCK-----
```

📋 **Copie TODO o conteúdo** (incluindo as linhas BEGIN e END)

---

### 5. Adicionar no GitHub/GitLab

#### GitHub:

1. Acesse: https://github.com/settings/keys
2. Clique em **"New GPG key"**
3. Cole a chave pública
4. Clique em **"Add GPG key"**

#### GitLab:

1. Acesse: https://gitlab.com/-/profile/gpg_keys
2. Cole a chave pública
3. Clique em **"Add key"**

#### Bitbucket:

1. Acesse: https://bitbucket.org/account/settings/gpg-keys/
2. Cole a chave pública
3. Clique em **"Add key"**

---

### 6. Testar

```bash
# Crie um commit de teste
echo "teste gpg" > teste.txt
git add teste.txt
git commit -m "Test GPG signed commit"

# Verifique a assinatura
git log --show-signature -1
```

**Saída esperada:**

```
commit abc123def456...
gpg: Signature made Mon Jan 15 10:30:00 2025 -03
gpg: using RSA key ABCD1234EFGH5678
gpg: Good signature from "Pedro Vieira <seu-email@exemplo.com>" [ultimate]

    Test GPG signed commit
```

✅ Faça push e veja o badge **"Verified"** no GitHub!

---

## 🚀 Configurações Avançadas

### Configurar GPG Agent (Evitar Digitar Senha Toda Vez)

O GPG Agent armazena sua senha em cache por um período determinado.

#### Windows:

```bash
# Crie ou edite o arquivo
echo default-cache-ttl 34560000 > %APPDATA%\gnupg\gpg-agent.conf
echo max-cache-ttl 34560000 >> %APPDATA%\gnupg\gpg-agent.conf

# Reinicie o agent
gpg-connect-agent reloadagent /bye
```

#### Linux/macOS:

```bash
# Crie o diretório se não existir
mkdir -p ~/.gnupg

# Configure o GPG Agent
cat > ~/.gnupg/gpg-agent.conf << 'EOF'
default-cache-ttl 34560000
max-cache-ttl 34560000
enable-ssh-support
EOF

# Defina permissões corretas
chmod 700 ~/.gnupg
chmod 600 ~/.gnupg/gpg-agent.conf

# Reinicie o agent
gpgconf --kill gpg-agent
gpg-agent --daemon
```

**Valores de cache:**
- `34560000` = 400 dias (~13 meses)
- `3600` = 1 hora
- `86400` = 1 dia

---

### Exportar Variável GPG_TTY (Linux/macOS)

Adicione ao seu `~/.bashrc`, `~/.zshrc` ou `~/.profile`:

```bash
export GPG_TTY=$(tty)
```

Depois execute:

```bash
source ~/.bashrc  # ou ~/.zshrc
```

---

### Backup da Chave Privada

⚠️ **IMPORTANTE**: Guarde em local seguro!

```bash
# Exportar chave privada
gpg --armor --export-secret-keys 3AA5C34371567BD2 > gpg-private-key-backup.asc

# Exportar chave pública
gpg --armor --export 3AA5C34371567BD2 > gpg-public-key-backup.asc

# Exportar certificado de revogação
gpg --gen-revoke 3AA5C34371567BD2 > gpg-revoke-cert.asc
```

**Guardar em:**
- 💾 Pendrive criptografado
- ☁️ Gerenciador de senhas
- 🔒 Cofre seguro

---

### Importar Chave em Outro Computador

```bash
# Importar chave privada
gpg --import gpg-private-key-backup.asc

# Confiar na chave
gpg --edit-key 3AA5C34371567BD2
gpg> trust
Your decision? 5  # (5 = I trust ultimately)
gpg> quit
```

---

## 🐛 Troubleshooting

### Problema 1: "gpg: signing failed: Inappropriate ioctl for device"

**Solução (Linux/macOS):**

```bash
export GPG_TTY=$(tty)

# Adicione ao ~/.bashrc ou ~/.zshrc
echo 'export GPG_TTY=$(tty)' >> ~/.bashrc
source ~/.bashrc
```

---

### Problema 2: "gpg: signing failed: No secret key"

**Causa:** ID da chave incorreto ou não configurado.

**Solução:**

```bash
# Liste suas chaves
gpg --list-secret-keys --keyid-format=long

# Configure a chave correta
git config --global user.signingkey SEU_ID_CORRETO
```

---

### Problema 3: "gpg: can't connect to the agent"

**Solução:**

```bash
# Mata o agent
gpgconf --kill gpg-agent

# Reinicia o agent
gpg-agent --daemon

# Ou reinicie o computador
```

---

### Problema 4: "gpg: skipped: No secret key"

**Causa:** Email do Git diferente do email da chave GPG.

**Solução:**

```bash
# Verifique o email do Git
git config --global user.email

# Verifique o email da chave
gpg --list-keys

# Configure o email correto
git config --global user.email "email-da-chave@exemplo.com"
```

---

### Problema 5: Windows - Pinentry não abre

**Solução 1:**

```bash
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
```

**Solução 2:**

Edite `%APPDATA%\gnupg\gpg-agent.conf`:

```
pinentry-program C:\Program Files (x86)\GnuPG\bin\pinentry-basic.exe
```

Reinicie o agent:

```bash
gpg-connect-agent reloadagent /bye
```

---

### Problema 6: Commits não aparecem como "Verified" no GitHub

**Possíveis causas:**

1. ❌ Email do Git ≠ Email da chave GPG
2. ❌ Chave pública não adicionada no GitHub
3. ❌ Chave expirada
4. ❌ Email não verificado no GitHub

**Solução:**

```bash
# 1. Verifique os emails
git config user.email
gpg --list-keys

# 2. Verifique se a chave está no GitHub
# https://github.com/settings/keys

# 3. Verifique se o email está verificado no GitHub
# https://github.com/settings/emails
```

---

## 📚 Comandos Úteis

### Gerenciamento de Chaves

```bash
# Listar todas as chaves públicas
gpg --list-keys

# Listar todas as chaves privadas
gpg --list-secret-keys --keyid-format=long

# Ver detalhes de uma chave
gpg --edit-key KEY_ID

# Deletar chave privada
gpg --delete-secret-keys KEY_ID

# Deletar chave pública
gpg --delete-keys KEY_ID

# Gerar certificado de revogação
gpg --gen-revoke KEY_ID > revoke.asc

# Revogar uma chave
gpg --import revoke.asc
```

---

### Verificação de Commits

```bash
# Ver assinatura do último commit
git log --show-signature -1

# Ver assinatura de commits específicos
git log --show-signature

# Verificar assinatura de um commit
git verify-commit COMMIT_HASH

# Verificar assinatura de uma tag
git verify-tag TAG_NAME

# Assinar manualmente (se auto-sign estiver desativado)
git commit -S -m "Mensagem do commit"

# Assinar tag manualmente
git tag -s v1.0.0 -m "Version 1.0.0"
```

---

### Configurações Git

```bash
# Ver todas as configurações GPG do Git
git config --global --list | grep gpg

# Ver ID da chave configurada
git config --global user.signingkey

# Desativar assinatura automática
git config --global commit.gpgsign false

# Ativar apenas para um repositório específico
cd seu-repositorio
git config commit.gpgsign true
```

---

## 🎯 Script de Configuração Automática

### Linux/macOS:

Salve como `setup-gpg.sh`:

```bash
#!/bin/bash

echo "🔐 Configuração Automática de GPG para Git"
echo "=========================================="
echo ""

# Verifica se GPG está instalado
if ! command -v gpg &> /dev/null; then
    echo "❌ GPG não está instalado!"
    echo "Instale com: sudo apt-get install gnupg (Ubuntu/Debian)"
    exit 1
fi

echo "✅ GPG instalado"
echo ""

# Lista chaves existentes
echo "Suas chaves GPG:"
gpg --list-secret-keys --keyid-format=long
echo ""

# Pergunta se quer criar nova chave
read -p "Deseja criar uma nova chave GPG? (s/n): " CREATE_KEY

if [ "$CREATE_KEY" = "s" ]; then
    echo "Gerando nova chave..."
    gpg --full-generate-key
fi

# Lista chaves novamente
echo ""
echo "Chaves disponíveis:"
gpg --list-secret-keys --keyid-format=long
echo ""

# Pede o ID da chave
read -p "Digite o ID da chave GPG (ex: 3AA5C34371567BD2): " KEY_ID

# Configura Git
echo ""
echo "Configurando Git..."
git config --global user.signingkey $KEY_ID
git config --global commit.gpgsign true
git config --global tag.gpgSign true

# Configura GPG Agent
echo "Configurando GPG Agent..."
mkdir -p ~/.gnupg
cat > ~/.gnupg/gpg-agent.conf << 'EOF'
default-cache-ttl 34560000
max-cache-ttl 34560000
enable-ssh-support
EOF

chmod 700 ~/.gnupg
chmod 600 ~/.gnupg/gpg-agent.conf

# Configura GPG_TTY
if ! grep -q "GPG_TTY" ~/.bashrc; then
    echo 'export GPG_TTY=$(tty)' >> ~/.bashrc
fi

gpgconf --kill gpg-agent

# Exporta chave pública
echo ""
echo "=========================================="
echo "📋 Chave Pública (adicione no GitHub):"
echo "=========================================="
gpg --armor --export $KEY_ID
echo ""

echo "✅ Configuração concluída!"
echo ""
echo "Próximos passos:"
echo "1. Copie a chave pública acima"
echo "2. Adicione em: https://github.com/settings/keys"
echo "3. Execute: source ~/.bashrc"
echo "4. Faça um commit de teste"
```

**Executar:**

```bash
chmod +x setup-gpg.sh
./setup-gpg.sh
```

---

### Windows (PowerShell):

Salve como `setup-gpg.ps1`:

```powershell
Write-Host "🔐 Configuração Automática de GPG para Git" -ForegroundColor Cyan
Write-Host "==========================================" -ForegroundColor Cyan
Write-Host ""

# Verifica se GPG está instalado
if (-not (Get-Command gpg -ErrorAction SilentlyContinue)) {
    Write-Host "❌ GPG não está instalado!" -ForegroundColor Red
    Write-Host "Instale com: choco install gpg4win"
    exit 1
}

Write-Host "✅ GPG instalado" -ForegroundColor Green
Write-Host ""

# Lista chaves existentes
Write-Host "Suas chaves GPG:"
gpg --list-secret-keys --keyid-format=long
Write-Host ""

# Pergunta se quer criar nova chave
$createKey = Read-Host "Deseja criar uma nova chave GPG? (s/n)"

if ($createKey -eq "s") {
    Write-Host "Gerando nova chave..."
    gpg --full-generate-key
}

# Lista chaves novamente
Write-Host ""
Write-Host "Chaves disponíveis:"
gpg --list-secret-keys --keyid-format=long
Write-Host ""

# Pede o ID da chave
$keyId = Read-Host "Digite o ID da chave GPG (ex: 3AA5C34371567BD2)"

# Configura Git
Write-Host ""
Write-Host "Configurando Git..." -ForegroundColor Yellow
git config --global user.signingkey $keyId
git config --global commit.gpgsign true
git config --global tag.gpgSign true
git config --global gpg.program gpg

# Configura GPG Agent
Write-Host "Configurando GPG Agent..." -ForegroundColor Yellow
$gnupgPath = "$env:APPDATA\gnupg"
if (-not (Test-Path $gnupgPath)) {
    New-Item -ItemType Directory -Path $gnupgPath
}

@"
default-cache-ttl 34560000
max-cache-ttl 34560000
"@ | Out-File -FilePath "$gnupgPath\gpg-agent.conf" -Encoding ASCII

gpg-connect-agent reloadagent /bye

# Exporta chave pública
Write-Host ""
Write-Host "==========================================" -ForegroundColor Cyan
Write-Host "📋 Chave Pública (adicione no GitHub):" -ForegroundColor Cyan
Write-Host "==========================================" -ForegroundColor Cyan
gpg --armor --export $keyId
Write-Host ""

Write-Host "✅ Configuração concluída!" -ForegroundColor Green
Write-Host ""
Write-Host "Próximos passos:"
Write-Host "1. Copie a chave pública acima"
Write-Host "2. Adicione em: https://github.com/settings/keys"
Write-Host "3. Faça um commit de teste"
```

**Executar:**

```powershell
powershell -ExecutionPolicy Bypass -File setup-gpg.ps1
```

---

## ✅ Checklist Final

- [ ] GPG instalado
- [ ] Chave GPG gerada (4096 bits)
- [ ] ID da chave obtido
- [ ] Git configurado (`user.signingkey`, `commit.gpgsign`)
- [ ] Chave pública exportada
- [ ] Chave adicionada no GitHub/GitLab
- [ ] GPG Agent configurado
- [ ] `GPG_TTY` exportado (Linux/macOS)
- [ ] Commit de teste realizado
- [ ] Badge "Verified" aparece no GitHub
- [ ] Backup da chave privada realizado

---

## 🔗 Links Úteis

- **GPG4Win (Windows)**: https://www.gpg4win.org/
- **GitHub - GPG Keys**: https://github.com/settings/keys
- **GitLab - GPG Keys**: https://gitlab.com/-/profile/gpg_keys
- **GPG Documentation**: https://www.gnupg.org/documentation/
- **Git Signing**: https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work

---

## 📝 Notas Importantes

⚠️ **Segurança da Chave Privada:**
- Nunca compartilhe sua chave privada
- Faça backup seguro
- Use senha forte
- Guarde o certificado de revogação

🔄 **Múltiplos Computadores:**
- Exporte e importe sua chave privada
- Configure em cada máquina
- Mesmo ID de chave = mesmas assinaturas

📧 **Email Verificado:**
- O email do Git deve ser o mesmo da chave GPG
- O email deve estar verificado no GitHub
- Senão, commits não aparecem como "Verified"

⏰ **Expiração:**
- Chaves podem ter data de expiração
- Renove antes de expirar
- Ou crie sem expiração (`0`)

---

## 👤 Autor

**Guia criado para facilitar a configuração de assinaturas GPG no Git**

---

## 📄 Licença

Este guia é de domínio público e pode ser usado livremente para fins educacionais.

---

**🎉 Parabéns! Agora seus commits são verificados e seguros!** 🔐✅