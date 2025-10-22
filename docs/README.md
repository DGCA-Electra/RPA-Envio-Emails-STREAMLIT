# 🤖 RPA-Envio-Emails-STREAMLIT

## Automação Inteligente para Envio de Relatórios CCEE via E-mail com Streamlit e Microsoft Graph API

Este projeto oferece uma solução de **Automação de Processos Robóticos (RPA)** para otimizar o envio de relatórios da Câmara de Comercialização de Energia Elétrica (CCEE) a clientes. Desenvolvido com **Streamlit** e integrado à **API Microsoft Graph**, ele proporciona uma interface web segura e intuitiva para a geração automatizada de rascunhos de e-mails personalizados, diretamente na caixa de correio do usuário autenticado.

---

## ✨ Funcionalidades Principais

O sistema foi projetado para oferecer uma experiência robusta, segura e flexível:

-   **Criação de Rascunhos via Microsoft Graph API**: Gera rascunhos de e-mail diretamente na pasta "Rascunhos" da caixa de correio do **usuário autenticado** via Microsoft Azure AD, permitindo revisão antes do envio manual pelo usuário. (Substitui a automação local do Outlook).
-   **Autenticação Segura com Microsoft Azure AD**: Garante que apenas usuários autorizados da organização possam acessar a aplicação e criar e-mails em seu próprio nome, utilizando o fluxo de autenticação OAuth 2.0.
-   **Suporte a Múltiplos Relatórios CCEE**: Compatibilidade com diversos tipos de relatórios, incluindo GFN001, SUM001, LFN001, LFRES, LEMBRETE, LFRCAP e RCAP.
-   **Interface Web Intuitiva (Streamlit)**: Uma aplicação web amigável que simplifica a interação do usuário, guiando-o desde o login até a geração dos e-mails.
-   **Configuração Dinâmica**: Permite ajustar parâmetros dos relatórios (abas, colunas) e templates de e-mail (assunto, corpo HTML) via interface web ou arquivos JSON.
-   **Envio Multi-Analista**: Capacidade de qualquer usuário autenticado enviar relatórios em nome de qualquer analista listado, útil para cobrir férias ou delegar tarefas.
-   **Engine de Templates Jinja2**: Criação dinâmica de assuntos e corpos de e-mail altamente personalizados.
-   **Validação de Anexos**: Verificação da existência e limite de tamanho dos arquivos PDF a serem anexados.
-   **Tratamento de Erros e Logs**: Mecanismos robustos para lidar com falhas (leitura de arquivos, API, etc.) com registro detalhado em `logs/app.log`.

---

## 🛠️ Tecnologias Utilizadas

| Categoria             | Tecnologia         | Descrição                                                              |
| :-------------------- | :----------------- | :--------------------------------------------------------------------- |
| **Framework Web** | Streamlit          | Construção da interface de usuário interativa.     |
| **Dados & Excel** | Pandas, OpenPyXL   | Manipulação e leitura de dados de planilhas Excel.      |
| **Autenticação** | MSAL (Python)      | Biblioteca da Microsoft para autenticação com Azure AD. |
| **API E-mail** | Requests           | Para fazer chamadas à Microsoft Graph API (criar rascunhos). |
| **Templates** | Jinja2             | Motor de templates para renderização dinâmica de e-mails.              |
| **Segredos** | python-dotenv      | Carregamento seguro de credenciais a partir de arquivo `.env`. |
| **Segurança HTML** | Bleach             | Sanitização de HTML nos templates e pré-visualizações.      |
| **Utilitários** | Pathlib, logging   | Manipulação de caminhos e registro de logs. |
| **Serviços Externos** | Microsoft Azure AD | Para registro da aplicação e autenticação de usuários.             |
|                       | Microsoft Graph API| Para interagir com a caixa de correio do usuário (criar rascunhos). |

---

## 📦 Instalação e Configuração

Siga estes passos para configurar e executar o projeto:

### Pré-requisitos

-   **Python**: Versão 3.8 ou superior.
-   **Git**: Para clonar o repositório.
-   **Registro de Aplicativo no Microsoft Azure AD**: É **essencial** registrar a aplicação no Azure AD da sua organização para obter as credenciais e configurar as permissões.
    -   **Credenciais necessárias**: ID do Aplicativo (Cliente), ID do Diretório (Locatário), Segredo do Cliente.
    -   **Permissões de API (Delegadas)**: `User.Read` e `Mail.ReadWrite` (requer consentimento, possivelmente do administrador).
    -   **Plataforma de Autenticação**: Configurar uma plataforma **Web** com as URIs de Redirecionamento corretas (ex: `http://localhost:8501` para teste local, `https://SEU_IP_OU_DNS:8501` para acesso via rede). **Importante:** URIs de rede (não-localhost) devem usar **HTTPS**.
    -   **Fluxos de Cliente Público**: Certifique-se de que "Permitir fluxos de cliente público" esteja definido como **"Não"**.
-   **(Opcional, para HTTPS na rede local)** **OpenSSL**: Necessário para gerar certificados SSL/TLS autoassinados se você for expor a aplicação na rede local via HTTPS. Pode já estar incluído com o Git for Windows.

### Passos de Instalação

1.  **Clone o repositório**:
    ```bash
    git clone [https://github.com/malikribeiro/RPA-Envio-Emails-STREAMLIT.git](https://github.com/malikribeiro/RPA-Envio-Emails-STREAMLIT.git)
    cd RPA-Envio-Emails-STREAMLIT
    ```

2.  **Crie e Ative um Ambiente Virtual**:
    ```bash
    # Criar
    python -m venv venv
    # Ativar (Windows PowerShell)
    .\venv\Scripts\Activate.ps1
    # Ativar (Windows CMD)
    .\venv\Scripts\activate.bat
    # Ativar (Linux/macOS)
    # source venv/bin/activate
    ```

3.  **Instale as Dependências**:
    ```bash
    pip install -r requirements.txt
    ```

4.  **Configure o Arquivo `.env`**:
    * Crie um arquivo chamado `.env` na pasta raiz do projeto.
    * Adicione as credenciais obtidas do Azure AD (substitua pelos seus valores):
        ```dotenv
        # .env
        AZURE_CLIENT_ID="SEU_CLIENT_ID_DO_AZURE"
        AZURE_CLIENT_SECRET="SEU_SEGREDO_DE_CLIENTE_DO_AZURE"
        AZURE_TENANT_ID="SEU_TENANT_ID_DO_AZURE"
        # Use a URL que os usuários acessarão pela rede (HTTPS) ou http://localhost:8501 para teste local
        AZURE_REDIRECT_URI="https://SEU_IP_OU_DNS:8501"
        ```
    * **IMPORTANTE**: Este arquivo contém segredos. Certifique-se de que ele esteja listado no seu `.gitignore` e **NUNCA** seja enviado para o GitHub. Se um segredo vazar, **rotacione-o imediatamente** no Azure AD e limpe o histórico do Git.

### Estrutura de Arquivos Esperada (Relatórios e Contatos)

O sistema ainda espera a estrutura de diretórios baseada no login de rede do usuário para encontrar os arquivos Excel e os PDFs dos relatórios. Veja o `config.py` e `config_manager.py` para detalhes.

C:/Users/{login_usuario}/ └── ELECTRA COMERCIALIZADORA DE ENERGIA S.A/ └── GE - ECE/DGCA/... (conforme definido em config.py)


---

## 🚀 Execução da Aplicação

1.  **Ative o ambiente virtual** (se não estiver ativo).
2.  **Escolha o modo de execução:**

    * **Para Teste Local (HTTP):**
        * Certifique-se que `AZURE_REDIRECT_URI` no `.env` esteja como `http://localhost:8501`.
        * Execute:
            ```bash
            streamlit run app.py
            ```
        * Acesse: `http://localhost:8501`

    * **Para Acesso na Rede Local (HTTPS Obrigatório para Login):**
        * **Gere os Certificados (uma vez):**
            * Instale/configure o `openssl` (pode vir com o Git for Windows).
            * No terminal, na raiz do projeto, execute:
                ```bash
                openssl req -x509 -newkey rsa:4096 -nodes -keyout key.pem -out cert.pem -sha256 -days 365 -subj "/CN=SEU_IP_OU_DNS" -addext "subjectAltName=IP:SEU_IP_DA_REDE,DNS:localhost"
                ```
                (Substitua `SEU_IP_OU_DNS` e `SEU_IP_DA_REDE`. Incluir `DNS:localhost` pode ajudar no acesso local via HTTPS).
            * Adicione `key.pem` e `cert.pem` ao `.gitignore`.
        * **Configure o `.env`:** Certifique-se que `AZURE_REDIRECT_URI` esteja como `https://SEU_IP_OU_DNS:8501`.
        * **Registre a URI HTTPS no Azure AD** (Plataforma Web).
        * **Execute com SSL:**
            ```bash
            streamlit run app.py --server.sslCertFile=cert.pem --server.sslKeyFile=key.pem --server.port=8501
            ```
        * **Use o Script (Recomendado):** Execute o script `run_secure.ps1` (ajuste se necessário):
            ```powershell
            .\run_secure.ps1
            ```
        * **Acesse:** `https://SEU_IP_DA_REDE:8501`. Ignore o aviso de segurança do navegador (certificado autoassinado).

3.  **Login:** Autentique-se com sua conta Microsoft.
4.  **Use a Aplicação:** Selecione os parâmetros e gere os rascunhos.

---

## 🖥️ Visão Geral da Interface e Navegação

-   **Tela de Login**: Autenticação inicial via Microsoft Azure AD.
-   **Navegação Principal**: Na barra lateral, com "Envio de Relatórios" e "Configurações".
-   **Parâmetros de Envio**: Centralizados no painel principal.
-   **Pré-visualização**: Exibe o e-mail renderizado em HTML antes da criação do rascunho.
-   **Visualização de Dados**: Apresentação clara dos dados carregados.

---

## ⚙️ Configurações Avançadas

-   **Configuração de Relatórios**: Ajuste de abas, cabeçalhos e mapeamento de colunas via UI ("Configurações") ou `config_relatorios.json`.
-   **Templates de E-mail**: Edição de assunto, corpo HTML e lógica de variantes via UI ("Configurações") ou `email_templates.json`.
-   **Configuração do Azure AD**: Requer acesso ao Portal Azure para gerenciar o registro do aplicativo, permissões e URIs de redirecionamento.

---

## 🐛 Tratamento de Erros e Logs

-   Erros comuns incluem falhas na leitura de arquivos (verifique caminhos e permissões), erros de autenticação (verifique `.env`, registro no Azure AD, URIs), e falhas na API Graph (verifique permissões, payload do e-mail, logs).
-   Logs detalhados são gravados em `logs/app.log`. Mensagens de erro relevantes também são exibidas na interface do Streamlit.

---

## 🔒 Segurança

-   **Autenticação**: Realizada via Microsoft Azure AD (OAuth 2.0).
-   **Segredos**: Credenciais do Azure AD são gerenciadas via arquivo `.env` (não versionado). **É crucial rotacionar segredos se expostos.**
-   **HTTPS**: Necessário para acesso via rede devido às restrições de redirecionamento do Azure AD. Use certificados autoassinados para ambiente interno.
-   **Validação de Entrada**: Sanitização de HTML e validação de formatos.
-   **Auditoria**: Logs de acesso podem ser consultados no Azure AD (requer permissão). Logs da aplicação em `logs/app.log`.
-   Consulte `docs/SECURITY.md` e `docs/security_audit.md` para mais detalhes.

---

## 🤝 Contribuição

Contribuições são bem-vindas! Siga o processo padrão de fork, branch, commit e pull request.

---

## 📄 Licença

Este projeto é de uso interno da ELECTRA COMERCIALIZADORA DE ENERGIA S.A.

---

## 👥 Autores

-   **Desenvolvido para**: DGCA
-   **Mantido por**: Malik Ribeiro Mourad

---

**Versão**: 1.1.0 (Atualizado com Autenticação Azure AD e MS Graph API)
**Última atualização**: Outubro 2025