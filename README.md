# Embeddings Neo4j - Sistema de Busca Semântica

## 📋 Descrição do Projeto

Este projeto implementa um **sistema de busca semântica** usando embeddings de texto e Neo4j como banco de dados vetorial. O sistema carrega um PDF, gera representações vetoriais (embeddings) do seu conteúdo e permite realizar buscas por similaridade usando linguagem natural.

### O que o projeto faz:
1. **Carrega um PDF** (`tensores.pdf`) do disco
2. **Divide o texto em chunks** (pedaços menores) para melhor processamento
3. **Gera embeddings** (representações vetoriais) de cada chunk usando modelos de IA
4. **Armazena no Neo4j** como um banco de dados vetorial
5. **Realiza buscas por similaridade** - encontra chunks similares a perguntas em linguagem natural

---

## 🛠️ Ferramentas e Tecnologias

### Core
- **Node.js** v22.13.1 - Runtime JavaScript
- **TypeScript** - Tipagem estática

### IA e Embeddings
- **@huggingface/transformers** - Geração de embeddings localmente (modelo: `all-MiniLM-L6-v2`)
- **LangChain** (@langchain/community, @langchain/core) - Framework para aplicações com IA
- **Neo4j Vector Store** - Integração LangChain com Neo4j

### Banco de Dados
- **Neo4j 5.14.0** - Banco de dados em grafo com suporte a busca vetorial (rodando via Docker)
- **neo4j-driver** - Driver Node.js para Neo4j

### Processamento de Documentos
- **pdf-parse** - Extração de texto de PDFs
- **LangChain Text Splitter** - Divisão inteligente de textos em chunks

### APIs
- **OpenRouter** - Acesso a modelos de IA via API (opcional)

---

## 🚀 Como Usar

### Pré-requisitos
- Node.js v22.13.1 ou superior
- Docker e Docker Compose instalados
- Arquivo `tensores.pdf` na raiz do projeto

### 1️⃣ Iniciar a Infraestrutura (Neo4j)

```bash
npm run infra:up
```

Isso vai:
- Iniciar um container Neo4j na porta **7687** (Bolt Protocol)
- Interface web disponível em: **http://localhost:7474**
- Credenciais padrão:
  - Usuário: `neo4j`
  - Senha: `password`

### 2️⃣ Configurar Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto (já deve existir):

```env
# Neo4j Configuration
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=password

# Embedding Model
EMBEDDING_MODEL=Xenova/all-MiniLM-L6-v2

# OpenRouter Configuration (opcional)
NLP_MODEL=sentence-transformers/all-MiniLM-L6-v2
OPENROUTER_API_KEY=your_api_key_here
OPENROUTER_SITE_URL=http://localhost:3000
OPENROUTER_SITE_NAME=Neo4j Embeddings
```

### 3️⃣ Rodar a Aplicação

**Execução única:**
```bash
npm start
```

**Modo desenvolvimento (com recarregamento automático):**
```bash
npm run dev
```

### 4️⃣ Parar a Infraestrutura

```bash
npm run infra:down
```

---

## 📊 O que o `src/index.ts` faz

### Etapa 1: Carregamento e Processamento do PDF
- Carrega o arquivo `tensores.pdf`
- Divide o texto em chunks de **1000 caracteres** com **200 caracteres de sobreposição**
- Cria objetos de documento para processamento

### Etapa 2: Geração de Embeddings
- Inicializa o modelo `Xenova/all-MiniLM-L6-v2` (executado localmente via HuggingFace Transformers)
- Gera vetores de 384 dimensões para cada chunk
- Usa a configuração `fp32` para qualidade máxima

### Etapa 3: Armazenamento no Neo4j
- Limpa dados anteriores (remove nodes do tipo `Chunk`)
- Cria conexão com Neo4j usando o `Neo4jVectorStore`
- Salva cada documento com seu embedding no banco

### Etapa 4: Busca por Similaridade
- Realiza **6 buscas de exemplo** em linguagem natural:
  - "O que são tensores e como são representados em JavaScript?"
  - "Como converter objetos JavaScript em tensores?"
  - "O que é normalização de dados e por que é necessária?"
  - "Como funciona uma rede neural no TensorFlow.js?"
  - "O que significa treinar uma rede neural?"
  - "O que é hot encoding e quando usar?"
- Retorna os **3 chunks mais similares** para cada pergunta
- Exibe scores de similaridade

---

## 📁 Estrutura de Arquivos

```
.
├── src/
│   ├── index.ts              # Ponto de entrada principal
│   ├── config.ts             # Configurações (credenciais, modelos)
│   ├── documentProcessor.ts  # Carregamento e processamento de PDF
│   └── util.ts               # Funções auxiliares
├── docker-compose.yml        # Configuração do Neo4j
├── package.json              # Dependências e scripts
├── .env                       # Variáveis de ambiente
└── tensores.pdf              # PDF para processar
```

---

## 🔍 Exemplo de Saída

```
🚀 Inicializando sistema de Embeddings com Neo4j...

✅ Adicionando documento 1/10
✅ Adicionando documento 2/10
...

🔍 ETAPA 2: Executando buscas por similaridade...

================================================================================
📌 PERGUNTA: O que são tensores?
================================================================================
[Resultado 1] - Similaridade: 0.92
[Resultado 2] - Similaridade: 0.87
[Resultado 3] - Similaridade: 0.81
```

---

## 🔗 Acessar Neo4j Browser

Enquanto a infra está rodando, acesse:
- **URL**: http://localhost:7474
- **Usuário**: neo4j
- **Senha**: password

Consulta útil para ver os dados:
```cypher
MATCH (n:Chunk) RETURN n LIMIT 10
```

---

## 💡 Próximos Passos

1. Modificar o arquivo PDF para processar outros documentos
2. Ajustar o tamanho dos chunks em `config.ts`
3. Adicionar mais perguntas de teste em `index.ts`
4. Integrar com uma API REST para consultas dinâmicas
5. Usar modelos diferentes de embedding

---

## 📝 Notas

- Os embeddings são gerados **localmente** (não requer API externa)
- O Neo4j armazena os vetores e permite busca vetorial rápida
- Cada execução limpa os dados anteriores e recarrega do PDF
- O tempo de execução inicial é maior (carregamento do modelo de IA)

