# Plataforma de Automação de Relatórios CCEE com Streamlit

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)

Uma aplicação web moderna e robusta desenvolvida para automatizar por completo o processo de geração e envio de relatórios financeiros da CCEE, substituindo um fluxo de trabalho legado baseado em macros VBA.

<!-- Insira aqui o GIF/Imagem de demonstração do seu projeto. É a parte mais importante! -->
<!-- Exemplo: <img src="caminho/para/seu/demo.gif" alt="Demonstração da Aplicação"> -->
<!-- Dica: Use ferramentas como ScreenToGif para gravar sua tela de forma fácil. -->

## 📖 Sobre o Projeto

Este projeto nasceu da necessidade de otimizar um processo crítico de negócio que era manual, demorado e suscetível a erros. O fluxo anterior, dependente de macros complexas em VBA, foi reimaginado e reconstruído como uma aplicação web moderna, intuitiva e escalável.

A plataforma centraliza a lógica de múltiplos relatórios (`GFN001`, `SUM001`, `LFN001`, etc.), permitindo que analistas gerem e enviem dezenas de e-mails personalizados com os anexos corretos em questão de segundos, transformando horas de trabalho em apenas alguns cliques.

## ✨ Principais Funcionalidades

-   **Interface Web Intuitiva:** Construída com **Streamlit**, oferece uma experiência de usuário limpa e direta, com navegação em abas para Envio e Configurações.
-   **Configuração Centralizada:** Um painel de configurações completo permite que os usuários gerenciem todos os parâmetros (caminhos de arquivos, nomes de abas, linha de cabeçalho e mapeamento de colunas) para cada tipo de relatório, sem a necessidade de qualquer alteração no código.
-   **Motor de Dados com Pandas:** Utiliza a biblioteca **Pandas** para ler, limpar, validar e consolidar dados de diversas planilhas Excel, lidando com estruturas complexas e mapeamentos dinâmicos.
-   **Lógica de Negócio Modular:** Cada relatório possui um "handler" dedicado que encapsula suas regras de negócio específicas — templates de e-mail, condições de envio (`valor > 0`), e a lógica para construção dos nomes de anexos.
-   **Automação de E-mail com Outlook:** Integração nativa com o Microsoft Outlook via **pywin32** para gerar rascunhos de e-mail, preenchidos com corpo em HTML, destinatários e os PDFs corretos, prontos para a revisão final do analista.

## 🛠️ Stack de Tecnologias

| Área                | Tecnologia                                                                                              |
| ------------------- | ------------------------------------------------------------------------------------------------------- |
| **Backend & Lógica** | ![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white)                         |
| **Interface Web**    | ![Streamlit](https://img.shields.io/badge/-Streamlit-FF4B4B?logo=streamlit&logoColor=white)                   |
| **Manipulação de Dados** | ![Pandas](https://img.shields.io/badge/-Pandas-150458?logo=pandas&logoColor=white)                           |
| **Automação Desktop**  | `pywin32` (para integração COM com Outlook)                                                             |
| **Configuração**      | `TOML` (para temas Streamlit) & `JSON` (para configurações da aplicação)                                |

## 📂 Estrutura do Projeto

A arquitetura foi projetada para ser modular e de fácil manutenção:

```
/projeto-automacao-streamlit
|-- .streamlit/
|   |-- config.toml         # Configuração de tema do Streamlit (força o modo claro)
|-- app.py                  # Script principal do Streamlit (UI e orquestração)
|-- services.py             # Cérebro da aplicação (processamento de dados e handlers)
|-- config.py               # Configurações padrão e constantes globais
|-- requirements.txt        # Dependências do projeto
|-- config_relatorios.json  # Arquivo de configurações do usuário (gerado/editado via UI)
|-- /static/
|   |-- logo.png            # Logo da empresa
```

## ⚙️ Instalação e Execução

### Pré-requisitos
-   Python 3.9+
-   Microsoft Outlook instalado e configurado no Windows
-   Um ambiente virtual é fortemente recomendado.

### Passos para Rodar

1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/seu-usuario/seu-repositorio.git
    cd seu-repositorio
    ```

2.  **Crie e ative um ambiente virtual:**
    ```bash
    # Criar
    python -m venv venv
    # Ativar no Windows (PowerShell)
    .\venv\Scripts\Activate.ps1
    ```

3.  **Instale as dependências:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Execute a aplicação Streamlit:**
    ```bash
    streamlit run app.py
    ```
    A aplicação será aberta automaticamente no seu navegador padrão.

## 🔧 Configuração Essencial

Na primeira vez que rodar, ou após excluir o `config_relatorios.json`, a aplicação usará os valores padrão. É crucial ajustar as configurações para o seu ambiente através da interface:

1.  Navegue até a página de **Configurações**.
2.  Para cada relatório, preencha os caminhos dos arquivos e diretórios de PDF.
3.  Ajuste a **Linha do Cabeçalho** (lembrando que a contagem inicia em 0).
4.  Defina o **Mapeamento de Colunas** usando o formato `NomeNoExcel:NomePadrão` separado por vírgulas.
    -   **Exemplo:** `Agente:Empresa,Garantia Avulsa (R$):Valor`

## 👨‍💻 Autor

Desenvolvido por **Malik Ribeiro Mourad**.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)]([https://www.linkedin.com/in/malikribeiro/])
