
🧠 TechMind
Hackathon Squad Solutions

Java Spring Boot Python Oracle OCI Redis REST API Tests

Organização inteligente de conteúdos técnicos com Ciência de Dados, Machine Learning e Cloud.

Projeto desenvolvido pelo G9-BR-Team-04 para o Hackathon ONE G9 | Oracle + Alura, com o objetivo de transformar conteúdos técnicos dispersos em conhecimento estruturado, pesquisável e reutilizável.

❗ O problema
Profissionais e estudantes de tecnologia consomem diariamente grandes volumes de documentação, artigos, cursos, tutoriais e anotações. Com o tempo, organizar e recuperar essas informações se torna um desafio.

O TechMind foi desenvolvido para reduzir esse esforço, utilizando Inteligência Artificial e Ciência de Dados para automatizar a organização do conhecimento técnico.

💡 A solução
A plataforma recebe conteúdos técnicos e utiliza um modelo de Machine Learning para processá-los e retornar informações estruturadas, como:

classificação temática; nível de confiança da classificação; palavras-chave; conteúdos relacionados; consulta por categorias; processamento individual ou em lote.

Os resultados são disponibilizados em JSON por meio de uma API REST, permitindo integração com outras aplicações. O cadastro e a consulta dos conteúdos também podem ser feitos por uma interface web.

🎯 Objetivo
Desenvolver um MVP funcional para organização inteligente de conteúdos técnicos, utilizando Ciência de Dados e Machine Learning para automatizar sua classificação e enriquecimento, facilitando a consulta, descoberta e reutilização do conhecimento por estudantes, profissionais e equipes.

✨ Funcionalidades
MVP

✅ Processamento de conteúdos técnicos

✅ Classificação automática por Machine Learning

✅ Retorno estruturado em JSON

✅ API REST

✅ Validação de entrada e tratamento de erros

✅ Persistência de dados

✅ Integração com OCI Object Storage

✅ Interface web para cadastro e consulta

Funcionalidades Adicionais

🔎 Consulta por título e categoria

🔗 Recomendação de conteúdos relacionados

📄 Processamento em lote via CSV

⚡ Cache com Redis

🔐 Deduplicação de conteúdo com SHA-256

🛡️ Resiliência com Circuit Breaker, Retry e Bulkhead

🧪 Testes automatizados

📊 Observabilidade e métricas

📈 Testes de carga com k6

🏗️ Arquitetura e Tecnologias
Camada	Tecnologia / Ferramenta
Ciência de Dados / ML	Python, TF-IDF, modelo de classificação
API de ML	Python, FastAPI
Back-End	Java 21, Spring Boot
Arquitetura	Spring Modulith
API	REST / JSON
Front-End	Angular, TypeScript, Tailwind CSS, PrimeNG
Persistência	Oracle Database (executado localmente, via Docker)
Cloud	Oracle Cloud Infrastructure (OCI) — Object Storage (armazenamento dos artefatos do modelo)
Cache	Redis
Resiliência	Resilience4j — Circuit Breaker, Retry e Bulkhead
Observabilidade	Spring Boot Actuator, Prometheus e OpenTelemetry
Testes	JUnit e k6
Build / Dependências	Maven

🔄 Arquitetura Funcional — Fluxo da Solução
O TechMind utiliza uma arquitetura desacoplada, separando a aplicação Back-End do serviço responsável pela inferência de Machine Learning.

Os artefatos do modelo treinado são armazenados no OCI Object Storage e carregados pela API Python durante sua inicialização.

                     OCI Object Storage
                            │
              ┌─────────────┴─────────────┐
              │  modelo.pkl               │
              │  vectorizer.pkl           │
              │  tfidf_keywords.pkl       │
              └─────────────┬─────────────┘
                            │
                     download no startup
                            ↓
                      API Python / ML
                            │
                            ↓
Conteúdo Técnico → API REST Spring Boot
                            │
                            ↓
                    Serviço de ML
                            │
                            ↓
          Categoria + Confiança + Palavras-chave
                            │
                            ↓
                  Oracle Database / Redis
                            │
                            ↓
             Consulta e Reutilização

🧠 Ciência de Dados
O modelo de Machine Learning é responsável por analisar o conteúdo técnico e gerar sua classificação.

O pipeline contempla etapas de preparação e tratamento dos textos, transformação dos dados, treinamento, avaliação e disponibilização do modelo para consumo pela aplicação.

O dataset utilizado no treinamento possui 2.560 textos distribuídos em 13 categorias, com um vocabulário TF-IDF de 23.860 termos. O modelo escolhido para produção foi um SGDClassifier combinado com CalibratedClassifierCV, alcançando 82,81% de acurácia no conjunto de teste.

O serviço de classificação é desacoplado do backend Java e disponibilizado por API, facilitando sua evolução e integração com outros sistemas.

📋 Como Executar
Clone este repositório.
Instale as dependências do projeto.
Execute o modelo treinado.
Inicie a API REST.
Utilize o Postman para testar os endpoints.

📡 Exemplo de Utilização da API
Requisição

POST /conteudo

Content-Type: application/json
{
  "titulo": "Introdução ao Spring Boot",
  "texto": "Neste conteúdo são apresentados conceitos para criação de APIs REST utilizando Java e Spring Boot."
}
Resposta

{
  "categoria": "Backend",
  "probabilidade": 0.89,
  "informacoes_adicionais": [
    "Java",
    "Spring Boot",
    "API REST"
  ]
}
A estrutura da resposta pode variar conforme o processamento realizado pelo modelo.

Além do cadastro, a API também disponibiliza endpoints para busca por título (GET /conteudo/titulo), busca por categoria (GET /conteudo/categoria) e consulta de conteúdos relacionados (GET /conteudo/relacionados/{id}).

📦 Processamento em Lote
A plataforma também permite processar múltiplos conteúdos por meio de arquivos CSV:

POST /conteudo/lote

Content-Type: multipart/form-data

O arquivo deve conter as colunas titulo e texto, com limite de 10 MB. Ao final do processamento, é apresentado um relatório com o total de registros, sucessos, falhas e o motivo de cada falha.

Esse recurso permite ampliar o uso da solução para bases maiores de conhecimento.

▶️ Como Executar
Pré-requisitos

Java 21
Maven
Python
Oracle Database
Redis
Git
Clone o projeto :

git clone https://github.com/No-Country-simulation/G9-BR-Team-04.git
cd G9-BR-Team-04
Configure as variáveis de ambiente necessárias para banco de dados e serviços externos.

Execute o backend :

./mvnw spring-boot:run
Para funcionamento completo da aplicação, o serviço de Machine Learning e as dependências utilizadas pela arquitetura também devem estar disponíveis.

☁️ Oracle Cloud Infrastructure
O OCI Object Storage é utilizado para armazenar os três artefatos gerados durante o treinamento do modelo de Machine Learning:

modelo.pkl — classificador treinado;
vectorizer.pkl — vetorizador TF-IDF utilizado na classificação;
tfidf_keywords.pkl — vetorizador TF-IDF utilizado na extração de palavras-chave.
Na inicialização, a API Python realiza o download desses artefatos diretamente do OCI Object Storage e os carrega para execução das inferências.

O banco de dados Oracle utilizado pela aplicação roda localmente, via Docker. Neste momento, a integração com a nuvem está concentrada no armazenamento dos artefatos do modelo de ML; o deploy da aplicação como um todo na OCI ainda não foi realizado e faz parte das próximas evoluções do projeto.

Essa arquitetura desacopla os artefatos de Machine Learning da aplicação, facilitando seu armazenamento, distribuição e atualização.

🧪 Exemplos de Uso
Exemplos de Uso

Classificação de conteúdo técnico
Um estudante salva um material sobre desenvolvimento de APIs com Java e Spring Boot. O TechMind processa o texto e identifica automaticamente sua categoria e informações relevantes.

Entrada:

{
  "titulo": "Introdução ao Spring Boot",
  "texto": "Neste conteúdo são apresentados conceitos para criação de APIs REST utilizando Java e Spring Boot."
}
Resultado esperado:

{
  "categoria": "Backend",
  "probabilidade": 0.89,
  "informacoes_adicionais": [
    "Java",
    "Spring Boot",
    "API REST"
  ]
}
Consulta de conteúdos organizados
Após o processamento, estudantes ou profissionais podem localizar conteúdos armazenados por título ou categoria, facilitando a recuperação de materiais relacionados a determinado assunto.

Exemplo:

GET /conteudo/categoria/Backend
O sistema retorna os conteúdos classificados naquela categoria, permitindo reutilizar o conhecimento já organizado.

Processamento de conteúdos em lote
Uma plataforma educacional ou equipe técnica pode importar vários conteúdos de uma única vez por meio de um arquivo CSV.

POST /conteudo/lote

Content-Type: multipart/form-data

O TechMind processa os registros, identifica conteúdos já existentes e organiza os novos materiais automaticamente.

Resultado: Menos catalogação manual e uma base de conhecimento mais estruturada, pesquisável e reutilizável.

📂 Estrutura do Projeto
G9-BR-Team-04/
├── backend/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/          # API e regras de negócio
│   │   │   └── resources/     # Configurações
│   │   └── test/              # Testes automatizados
│   └── pom.xml                # Dependências Maven
├── api-python/
│   ├── app/                   # API FastAPI de Ciência de Dados
│   └── models/                # Artefatos do modelo de ML
├── frontend/                  # Interface Angular
├── grafana-load-tests/        # Testes de carga com k6
└── README.md

👥 Equipe
👥 G9-BR-Team-04 – SolutionSquad (Esquadrão das Soluções)
Integrante	Função	GitHub	LinkedIn
Arthur Carvalho Ferreira	💻 Back End Developer	GitHub	LinkedIn
Carlos Caique Borges de Souza	💻 Back End Developer	GitHub	LinkedIn
Gabriel Leal	☁️ DevOps Engineer	GitHub	LinkedIn
Jaqueline Silva Broccolo	🔗 Full Stack Developer	GitHub	LinkedIn
Marcus Corrêa Lopes Guedes	📌 Project Manager / Front End Developer	GitHub	LinkedIn
Rayssa Santos	🤖 Data Scientist	GitHub	LinkedIn
Simone Silva	💻 Back End Developer / 📚 Documentation	GitHub	LinkedIn

🔄 Status do Projeto
✅ Definição do escopo
✅ Criação do dataset
✅ Treinamento do modelo
✅ Desenvolvimento da API
✅ Integração com OCI Object Storage
✅ Definição do escopo Dashboard
✅ Interface Angular
✅ Documentação inicial

Próximas evoluções
🔄 Deploy da aplicação na OCI
🔄 Login e autenticação no front-end
🔄 Dashboard de visualização
🔄 Autenticação entre Back-end e API de Ciência de Dados
🔄 Busca semântica
🔄 Sistema de recomendação

🚧 Projeto em desenvolvimento contínuo.

🙏 Agradecimentos
Oracle Next Education (ONE) G9 BR - Pela Oportunidade e Mentoria
OCI - Pela Infraestrutura
Mentores e Organizadores - Pelo Suporte e Orientação
⭐ Projeto Desenvolvido para o Hackathon Oracle Next Education (ONE) G9 BR.



## 👥 G9-BR-Team-04 – SolutionSquad (Esquadrão das Soluções)

| Integrante                        | Função                                          | GitHub | LinkedIn |
| --------------------------------- | ----------------------------------------------- | ------ | -------- |
| **Arthur Carvalho Ferreira**      | 💻 Back End Developer                           | [GitHub](https://github.com/ArthurFerreira13) | [LinkedIn](https://www.linkedin.com/in/arthur-fernando-carvalho-ferreira-96542772/) |
| **Carlos Caique Borges de Souza** | 💻 Back End Developer                           |  [GitHub](https://github.com/devcaiqueborges) | [LinkedIn](https://www.linkedin.com/in/devcaiqueborges) |
| **Gabriel Leal**                  | ☁️ DevOps Engineer                              | [GitHub](https://github.com/Gabriel-Lincoln-Leal) | [LinkedIn](https://www.linkedin.com/in/gabriellincolnleal) |
| **Jaqueline Silva Broccolo**      | 🔗 Full Stack Developer                         | [GitHub](https://github.com/jlinebsilva ) | [LinkedIn](https://www.linkedin.com/in/jaqueline-silva-broccolo/) |
| **Marcus Corrêa Lopes Guedes**    | 📌 Project Manager / Front End Developer        | [GitHub](https://github.com/MCLG1661) | [LinkedIn](https://www.linkedin.com/in/marcusguedes/) |
| **Rayssa Santos**                 | 🤖 Data Scientist                               | [GitHub](LINK) | [LinkedIn](https://www.linkedin.com/in/rayssasnt) |
| **Simone Silva**                  | 💻 Back End Developer / 📚 Documentation        | [GitHub](https://github.com/Simoneerp ) | [LinkedIn](https://www.linkedin.com/in/simone-fsilva/) |
