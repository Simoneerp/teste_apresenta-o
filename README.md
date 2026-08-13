🚀 TechMind
============

Projeto do G9 BR Team 04 (SolutionSquad/Esquadrão das Soluções)
Hackathon ONE G9 BR - Alura + Oracle

`Python` `Java` `Angular` `Oracle Cloud Infrastructure`

## 📌 Sobre o Projeto

O TechMind é uma solução inteligente para organizar, classificar e enriquecer conteúdos técnicos (documentações, cursos, artigos, tutoriais, anotações) usando Ciência de Dados e integração com a Oracle Cloud Infrastructure (OCI).

**Problema:** o grande volume de conteúdo técnico consumido diariamente por estudantes e profissionais é difícil de organizar, encontrar e reutilizar.

**Solução:** recebe um conteúdo técnico, classifica automaticamente com Machine Learning e devolve categoria, palavras-chave e conteúdos relacionados em JSON.

## ✨ Funcionalidades

- ✅ Classificação automática de conteúdo técnico
- ✅ Extração de palavras-chave
- ✅ Recomendação de conteúdos relacionados
- ✅ API REST para integração
- ✅ Cache de classificação por hash SHA-256 (evita reprocessar texto repetido)
- ✅ Busca paginada por título e categoria
- ✅ Processamento em lote via CSV
- ✅ Persistência em Oracle Database e OCI Object Storage
- ✅ Dashboard de visualização

## 🏗️ Arquitetura

```
Usuário → Front-end (Angular) → API REST (Spring Boot)
       → API de Ciência de Dados (FastAPI)
       → Modelo ML (TF-IDF + SGDClassifier)
       → Oracle Database / OCI Object Storage
```

A API Java é o único ponto de entrada exposto ao usuário. Ela chama o serviço Python internamente, persiste o resultado e responde ao cliente. A classificação é abstraída por uma interface (`ClassifierService`), o que desacopla a regra de negócio da implementação do modelo/OCI.

## 🛠️ Tecnologias

| Camada | Tecnologias |
|---|---|
| **Ciência de Dados** | Python 3.11, FastAPI, Uvicorn, Pydantic, Pandas, Scikit-Learn 1.6.1, TF-IDF, Docker |
| **Back-end** | Java 21, Spring Boot 4.1, Spring Modulith, Spring Web, Spring Data JPA, Jakarta Validation, Oracle JDBC, springdoc-openapi (Swagger), OpenCSV, Actuator/Micrometer/OpenTelemetry, Maven |
| **Front-end** | Angular v20+, TypeScript, Tailwind CSS, PrimeNG, Chart.js + ng2 Charts |
| **Cloud** | Oracle Cloud Infrastructure (OCI), Object Storage, Oracle Database |

## 🤖 Ciência de Dados

Modelo treinado para classificar conteúdos por categoria e extrair palavras-chave via NLP.

**Fluxo:** coleta e limpeza dos dados → vetorização TF-IDF → treino do `SGDClassifier` (calibrado) → disponibilização via FastAPI.

Usa dois vetorizadores TF-IDF: um para a predição da categoria e outro (ngram 1-2) dedicado à extração de palavras-chave, capaz de identificar termos compostos como "Spring Boot".

## 🌐 Endpoints principais

**API Java (back-end):**

| Endpoint | Método | Descrição |
|---|---|---|
| `/conteudo` | POST | Cria e classifica um conteúdo técnico |
| `/conteudo/titulo` | GET | Busca paginada por título |
| `/conteudo/categoria` | GET | Busca paginada por categoria |
| `/conteudo/relacionados/{id}` | GET | Conteúdos relacionados por categoria |
| `/conteudo/lote` | POST | Upload de CSV para processamento em lote |

**API Python (serviço interno de classificação):**

| Endpoint | Método | Descrição |
|---|---|---|
| `/health` | GET | Verifica se o serviço e o modelo estão ativos |
| `/api/v1/classificar` ⚠️ | POST | Recebe título + texto, retorna categoria, probabilidade e palavras-chave |

> ⚠️ **Confirmar:** apresentações técnicas mais recentes citam esse endpoint como `/predizer`. Verificar qual está implementado no código antes de publicar.

Exemplo de requisição/resposta:
```json
// POST /conteudo
{ "titulo": "Introdução ao Spring Boot", "texto": "Conceitos básicos de APIs REST com Java e Spring Boot." }

// Resposta
{
  "categoria": "Backend",
  "probabilidade": 0.89,
  "informacoesAdicionais": ["Java", "Spring Boot", "API REST"]
}
```

Erros comuns: `422` (dados inválidos), `503` (modelo/serviço indisponível), `500` (erro interno).

## 📋 Como Executar

1. Clone o repositório e instale as dependências do back-end e da API de Ciência de Dados.
2. Suba a API Python: `uvicorn main:app --reload` (ou via Docker).
3. Suba a API Java: `./mvnw spring-boot:run` (Windows: `.\mvnw.cmd spring-boot:run`).
4. Documentação interativa: `/docs` (Python) e Swagger UI (Java), ou use a collection do Postman.

## 📂 Estrutura do Projeto

```
TechMind/
├── backend/         → API REST (Spring Boot)
├── ciencia-dados/    → API FastAPI, Modelos, Notebooks
├── dashboard/        → Interface visual
├── dataset/          → Dados utilizados
├── postman/          → Collection da API
└── README.md
```

## 👥 Equipe

G9-BR-Team-04 – SolutionSquad (Esquadrão das Soluções)

| Integrante | Função |
|---|---|
| Arthur Carvalho Ferreira | 💻 Back End Developer |
| Carlos Caique Borges de Souza | 💻 Back End Developer |
| Gabriel Leal | ☁️ DevOps Engineer |
| Jaqueline Silva Broccolo | 🔗 Full Stack Developer |
| Lucas Aoki | 📊 Data Analyst |
| Marcus Corrêa Lopes Guedes | 📌 Project Manager / Front End Developer |
| Rayssa Santos | 🤖 Data Scientist |
| Simone Silva | 💻 Back End Developer / 📚 Documentation & Demo |

## 🔄 Status do Projeto

- ✅ Escopo, dataset e modelo treinado
- ✅ API de Ciência de Dados (FastAPI) operacional
- ✅ API Java desenvolvida (endpoints, cache, lote)
- ✅ Dashboard e documentação inicial
- 🔄 Integração completa com OCI
- 🔄 Deploy oficial (OCI Compute)

🚧 Projeto em desenvolvimento contínuo.

## 🙏 Agradecimentos

Oracle Next Education (ONE) G9 BR, OCI e mentores/organizadores pela oportunidade, infraestrutura e suporte.

⭐ Projeto desenvolvido para o Hackathon Oracle Next Education (ONE) G9 BR.
