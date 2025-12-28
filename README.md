# MandrackStudio 🇧🇷

---

Uma API de TTS (Text-to-Speech) auto-hospedada, **100% compatível com a OpenAI**, que utiliza as vozes neurais de alta qualidade e **gratuitas** da Microsoft Edge. Agora com **suporte a vozes multilinguais**, configuração simplificada e conversão real de formatos de áudio.

Este projeto foi criado por **Samuel de Sousa Santos** e é baseado no excelente trabalho de **travisvn/openai-edge-tts**.

---

## 🌟 Novidades da Versão 2.0.0

A versão 2.0 traz melhorias significativas focadas em flexibilidade e qualidade:

* **🗣️ Novas Vozes Multilinguais:** Use vozes capazes de falar múltiplos idiomas de forma fluida, incluindo Português, Inglês, Italiano e mais.
* **✍️ Mapeamento de Vozes Simplificado:** Personalize facilmente os apelidos de voz da OpenAI (`alloy`, `echo`, etc.) através de um ficheiro externo `voices.json`.
* **🎵 Conversão de Áudio Real:** Garantia de que o formato de áudio solicitado (`mp3`, `opus`, `flac`, etc.) é gerado corretamente usando `pydub` e `ffmpeg`.

Para ver todos os detalhes, consulte o nosso [**CHANGELOG.md**](CHANGELOG.md).

---

## 🚀 Principais Funcionalidades

* **✅ 100% Compatível com a API OpenAI:** Substitua o endpoint e comece a usar. Perfeito para n8n, Make, e outros sistemas.
* **⚡ Estabilidade Garantida:** Geração de áudio assíncrona para respostas rápidas e estáveis.
* **🧠 Filtro de Texto Inteligente:** Limpeza automática de emojis e Markdown para um áudio puro.
* **🔐 Segurança:** Proteção via chave de API (Bearer Token).
* **⚙️ Totalmente Configurável:** Altere portas, chaves, e vozes padrão via ficheiro `.env`.
* **🇧🇷 Feito no Brasil:** Todo o projeto e documentação em português.

---

## ✨ A Cereja do Bolo: Integração com o Painel MultiVozes

O **Multivozes BR Engine** foi projetado para ser o motor perfeito para o [Painel MultiVozes](https://multivozes.com/), transformando uma simples API em uma plataforma de produção de áudio completa e profissional.

| Funcionalidade | Usar a API Sozinha | Aliança MultiVozes + Engine |
| :--- | :---: | :---: |
| Vozes de Alta Qualidade | ✓ | ✓ |
| Geração Rápida e Gratuita | ✓ | ✓ |
| **Gestão de Múltiplos Usuários** | ❌ | ✓ |
| **Controle de Uso (Créditos)** | ❌ | ✓ |
| **Interface Visual para Diálogos**| ❌ | ✓ |
| **Histórico de Áudios Pessoal** | ❌ | ✓ |
| **Painel de Administração** | ❌ | ✓ |

---

## 📚 Documentação Completa

Para tirar o máximo proveito do projeto, consulte os nossos guias detalhados:

* [**Guia de Instalação para Iniciantes**](BEGINNERS_GUIDE.md)
* [**Guia de Uso da API e Exemplos**](API_USAGE_GUIDE.md)
* [**Guia de Integração com o n8n**](N8N-INTEGRATION.md)
* [**Guia de Vozes Disponíveis**](VOICES.md)
* [**Histórico de Versões (Changelog)**](CHANGELOG.md)
* [**Auxílio e Suporte**](SUPPORT.md)

---

## 📋 Pré-requisitos

* **Python 3.8 ou superior**
* **Git**
* **FFmpeg** (Essencial para a conversão de formatos de áudio)

---

## 🛠️ Guia de Instalação Rápida

1.  **Clone o repositório:**
    ```
    cd /opt
    git clone https://github.com/samucamg/multivozes_br_engine.git
    cd multivozes_br_engine
    ```

2.  **Crie e ative um ambiente virtual:**
    ```
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Instale as dependências:**
    ```
    pip install -r requirements.txt
    ```

4.  **Configure o `.env`:** Copie `.env.example` para `.env` e defina sua `API_KEY`.
    ```
    cp .env.example .env
    ```

5.  **(Novo!) Mapear vozes da Open ai:**  você pode editar de forma bem simples o mapeamentos de vozes para os padrões da openai.
    ```
    nano voices.json
    ```

---

## ▶️ Executando o Servidor

Para manter o servidor rodando, escolha a opção que melhor se adapta à sua necessidade:

### Opção 1: Teste Rápido (Terminal Aberto)

Ideal para testar se tudo está funcionando. Se fechar o terminal, o servidor para.

## ▶️ Executando o Servidor

Para manter o servidor rodando, escolha a opção que melhor se adapta à sua necessidade:

### Opção 1: Teste Rápido (Terminal Aberto)

Ideal para testar se tudo está funcionando. Se fechar o terminal, o servidor para.

```
# Ative o ambiente virtual (se ainda não estiver ativo)
source venv/bin/activate

# Inicie o servidor
python main.py
```

---

### Opção 2: Persistente com Tmux (Recomendado para Monitoramento)

O Tmux permite deixar o servidor rodando em "segundo plano", mas você pode "entrar" nele a qualquer momento para ver os logs em tempo real. Se fechar o SSH, ele continua rodando.

**Instale o Tmux:**

```
sudo apt update && sudo apt install tmux -y
```

**Crie uma sessão e inicie:**

```
tmux new -s multivozes

# Dentro da nova tela:
source venv/bin/activate
python main.py
```

**Para sair e deixar rodando (Detach):**

Pressione `Ctrl + B`, solte, e depois aperte `D`.

**Para voltar e ver os logs:**

```
tmux attach -t multivozes
```

**Nota:** Se o servidor reiniciar (reboot), o Tmux não inicia sozinho.

---

### Opção 3: Produção Automática (Systemd) 🏆

A forma mais profissional. O servidor inicia automaticamente no boot e reinicia sozinho se houver falhas.

**1. Comando de Instalação Automática:**

Copie e cole todo o bloco abaixo no terminal (estando dentro da pasta `multivozes_br_engine`):

```
sudo bash -c "cat > /etc/systemd/system/multivozes.service <<EOF
[Unit]
Description=Multivozes BR Engine Service
After=network.target

[Service]
User=$USER
WorkingDirectory=/opt/multivozes_br_engine
ExecStart=/opt/multivozes_br_engine/venv/bin/python main.py
Restart=always
RestartSec=5
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF" && sudo systemctl daemon-reload && sudo systemctl enable --now multivozes

```

**2. Gerenciando o Serviço:**

```
# Ver status
sudo systemctl status multivozes

# Ver logs em tempo real
sudo journalctl -u multivozes -f

# Parar
sudo systemctl stop multivozes

# Reiniciar
sudo systemctl restart multivozes
```

---

## 🌐 Acessando a API

O servidor iniciará na porta padrão **5050**.

Acesse a documentação interativa (Swagger UI) para testar a API em:

- **Local:** http://localhost:5050/docs
- **Remoto:** http://SEU_IP:5050/docs
```




