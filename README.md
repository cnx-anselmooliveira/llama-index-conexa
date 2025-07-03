# Gerador de Documentação para o Datalake da Conexa

## 📌 Visão Geral

O projeto **LLM** tem como objetivo automatizar a geração de documentação DBT a partir da coleta de amostras de dados de produção. Utiliza notebooks Jupyter para organizar o fluxo de coleta, análise e documentação, simplificando o processo de prototipação e manutenção de modelos.

---

## 📁 Estrutura do Projeto

```bash
llama-index-conexa/
│
├── .env.example            # Exemplo de configuração das variáveis
├── .gitignore              # Arquivos ignorados pelo Git
├── LICENSE                 # Licença de uso
├── README.md               # Instruções básicas do projeto
├── docker/
│   └── docker-compose.yaml # Banco de dados postgres para auxiliar na ingestão
├── scripts/
│   ├── step_01.ipynb       # Coleta de amostras de dados e salva no banco postgres
│   ├── step_02.ipynb       # Geração da documentação DBT
│   ├── step_03.ipynb       # Função auxiliar para separar tabelas/view
│   └── step_04.ipynb       # Realiza o merge do model + teste no DBT
```

---

## ⚙️ Pré-requisitos

Certifique-se de ter os seguintes itens instalados:

* Python 3.8+
* Docker e Docker Compose
* Jupyter Notebook

---

## 🚀 Como iniciar

1. **Clone o repositório:**

```bash
git clone https://github.com/cnx-anselmooliveira/llama-index-conexa.git
cd llama-index-conexa
```

2. **Configure as variáveis de ambiente:**

Crie um arquivo `.env` baseado no `.env.example` e preencha com suas credenciais e configurações locais.

3. **Execute os notebooks Jupyter:**

Use o Jupyter Lab ou Notebook para executar os notebooks em ordem sequencial (step\_01 ao step\_04).

---

## 📋 Etapas do Projeto

---

### ✅ Etapa 01 – Coleta de Amostras (`step_01.ipynb`)

**Objetivo:**
Acelerar o desenvolvimento de modelos ao coletar amostras reais dos bancos de produção e armazená-las localmente.

**Etapas executadas:**

1. Conecta aos bancos Trino e PostgreSQL com variáveis de ambiente.
2. Lista as tabelas e views de schemas definidos.
3. Ignora objetos que estejam em listas de exclusão.
4. Coleta até 10 linhas de cada objeto.
5. Aplica transformações nos dados (limite de strings, formatos).
6. Armazena localmente em um banco PostgreSQL.
7. Exibe logs no terminal sobre o progresso.

---

### ✅ Etapa 02 – Geração de Documentação DBT (`step_02.ipynb`)

**Objetivo:**
Automatizar a criação de arquivos YAML para o DBT com descrições de tabelas e colunas.

**Etapas executadas:**

1. **Gerar descrição da tabela**:

   * A função `gerar_descricao_tabela` analisa amostras e gera uma descrição em YAML.

2. **Criar dicionário da tabela**:

   * Consulta se já existe a descrição. Caso contrário, gera e armazena.

3. **Gerar arquivo DBT**:

   * Usa LLM (modelo de linguagem) para gerar a estrutura DBT com descrições.

4. **Salvar arquivo DBT**:

   * Salva os arquivos YAML no diretório do schema correspondente.

5. **Processar múltiplos schemas**:

   * Laço principal percorre schemas (`bronze`, `silver`, `gold`) e executa os passos acima para cada tabela.

---

### ✅ Etapa 03 – \[Reservado para uso futuro] (`step_03.ipynb`)

1. Realiza a consulta no banco de daods no Trino e move os arquivos para o pasta correta.

### ✅ Etapa 04 – Geração de Testes DBT (`step_04.ipynb`)

**Objetivo:**
Automatizar a criação e atualização de arquivos DBT com testes genéricos e descrição de tabelas usando LLM.

**Funcionalidades principais:**

1. **Conexão e configuração:**

   * Utiliza `.env` para conectar ao PostgreSQL com SQLAlchemy.
   * Configura o modelo de linguagem (LLM) e embeddings para prompts.

2. **Reflexão de metadados:**

   * Inspeciona tabelas existentes no banco e extrai metadados.

3. **Geração de arquivos de teste:**

   * Lê arquivos de testes existentes para os esquemas (`bronze`, `silver`, `gold`, `lina`).

4. **Atualização de YAMLs:**

   * Atualiza os arquivos DBT com os testes gerados.
   * Remove duplicações e caracteres inválidos automaticamente.

---

## 📦 Saídas Geradas

* Arquivos `.yml` com descrição das tabelas.
* Arquivos de testes DBT localizados nas pastas dos squemas apontados

---
