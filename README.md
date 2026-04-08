# 📘 Caderno Temático: DevOps Fullstack (Java/Spring Boot + Angular + Docker)

## 🎯 Contexto e Objetivos

O assunto de interesse escolhido para este caderno temático é a **Arquitetura Moderna de Desenvolvimento Fullstack e DevOps**, com foco no ecossistema **Java (Spring Boot)** para o back-end, **Angular** para o front-end web (SPA), e **Docker** com CI/CD para a infraestrutura e orquestração. 

O modelo tradicional de aplicações web, onde o servidor renderiza telas HTML, evoluiu para uma arquitetura moderna na qual front-end e back-end são sistemas desacoplados que se comunicam exclusivamente via APIs RESTful trafegando dados em formato JSON.

**Objetivos de Estudo:**
* Compreender os pilares da criação de APIs RESTful utilizando o Spring Boot e sua integração com bancos de dados relacionais (PostgreSQL).
* Entender o mecanismo de segurança stateless utilizando o Spring Security e tokens JWT.
* Explorar a comunicação entre uma Single-Page Application (SPA) em Angular e a API, gerenciando políticas de segurança como CORS.
* Dominar a containerização de ambas as aplicações utilizando Docker Multi-Stage Builds e orquestrando o ambiente com Docker Compose.

---

## 📚 Curadoria de Fontes

Para compor a base de conhecimento deste miniguia, foram selecionadas e processadas as seguintes fontes de referência:

1. **Livro:** *Fullstack Angular e Spring - Guia para se tornar um desenvolvedor moderno* (Alexandre Afonso e Thiago Faria, AlgaWorks) - Base sobre arquitetura moderna, SPAs e introdução aos frameworks.
2. **Artigo DEV Community:** *Arquitetura Fullstack com Angular 19 + Spring Boot* (Lucas Lomeu) - Relato prático de integração entre o novo Angular, Spring Security, JWT e Docker.
3. **Artigo Javalizando:** *JWT no Spring Security com Spring Boot: autenticação moderna passo a passo* - Aprofundamento no fluxo de autenticação stateless e filtros JWT.
4. **Artigo DEV Community:** *Docker Multi-Stage para Aplicações JAVA 21* (Adilson Oliveira) - Guia focado na redução e otimização de imagens Docker para produção.
5. **Transcrição YouTube:** *CI/CD Pipeline & Deployment | GitHub Actions | Docker | Spring boot | Angular* (Ali Bouali) - Passo a passo prático sobre containerização (Nginx para o frontend) e esteiras de CI/CD.

---

## 🛠️ Engenharia de Prompts e "Cicatrizes"

Durante a exploração do tema com a IA, diversas abordagens de *prompting* foram testadas para extrair o melhor conteúdo. Abaixo, o registro das interações estratégicas e o *troubleshooting* (as "cicatrizes" do aprendizado):

*   **Prompt 1 (Fundamentos):** *"Quais são as principais características que definem o Spring Boot?"*
    *   **Resultado:** A IA listou as características nativas das fontes, como servidores embutidos (Tomcat/Jetty), dependências starter, autoconfiguração e ausência de XML. 
*   **Prompt 2 (Aprofundamento com Restrições):** *"quais os principais pilares do spring boot utilizando boas práticas com APIs REST e RESTfull? lembre-se que estamos falando de JPA, Web, Security, Swagger, DevTools e banco de dados PostgreSQL."*
    *   **Cicatriz/Troubleshooting:** A IA foi excelente em estruturar os tópicos, mas apontou honestamente uma lacuna documental: as fontes não detalhavam o uso do **Swagger/OpenAPI** e as **boas práticas estritas arquiteturais de DTOs**. 
    *   **Solução:** Foi necessário instruir a IA a complementar o resumo com informações externas consolidadas pelo mercado, deixando claro o que pertencia aos documentos originais e o que era conhecimento de mercado.
*   **Prompt 3 (Integração):** *"Como integrar o frontend Angular com a API Spring Boot e resolver o CORS?"*
    *   **Resultado:** A IA sintetizou perfeitamente os dois lados da moeda: configurar filtros no Spring Security no back-end para aceitar origens ou usar o arquivo `proxy.conf.js` no CLI do Angular para desenvolvimento.
*   **Prompt 4 (DevOps):** *"como usar containers com docker para manter o servidor? cite os principais tópicos e como configurar."*
    *   **Resultado:** Excelente resposta técnica consolidando o conceito de *Multi-Stage Build* tanto para o `Dockerfile` do Java (compilando com Maven/JDK e rodando com JRE) quanto para o Angular (compilando com Node e servindo com Nginx atuando como Proxy Reverso).

---

## 🎓 Miniguia de Estudo (Entrega Final)

### 📝 Resumos Estruturados do Assunto

**1. Back-end: APIs RESTful com Spring Boot**
O Spring Boot facilita a criação de aplicações Java prontas para produção. Através do uso de dependências "starters" (ex: `spring-boot-starter-web`), ele autoconfigura um servidor web embutido, permitindo a criação rápida de controladores REST (`@RestController`) que expõem URIs para a manipulação de recursos, devolvendo respostas no formato JSON. A persistência de dados no PostgreSQL é abstraída através do Spring Data JPA, reduzindo a complexidade de conexão e mapeamento de tabelas.

**2. Segurança: Autenticação Stateless com JWT**
No modelo de arquitetura moderna, não mantemos estado (sessão) no servidor. O Spring Security intercepta requisições através de um filtro; quando o usuário insere credenciais válidas, a API devolve um JSON Web Token (JWT) assinado. O frontend Angular armazena este token e o envia no cabeçalho `Authorization: Bearer <token>` em todas as chamadas futuras. Isso permite escalar a aplicação facilmente, já que qualquer nó do servidor pode validar a assinatura do token.

**3. Front-end: Single-Page Applications com Angular**
O Angular é um framework e plataforma baseado em TypeScript, mantido pelo Google. Ele permite a construção de SPAs (Single-Page Applications), onde o navegador carrega a estrutura HTML/CSS/JS apenas uma vez e as renderizações de tela ocorrem no lado do cliente. O Angular consome os dados do back-end usando o módulo `HttpClient` e frequentemente necessita de tratamento de CORS, seja por autorização da porta `8080` no Spring Boot ou por um proxy de desenvolvimento no Angular CLI na porta `4200`.

**4. DevOps: Docker e Multi-Stage Build**
Para implantar essa arquitetura, a técnica de *Multi-Stage Build* do Docker é essencial. 
*   **No Java:** O estágio 1 usa uma imagem pesada (JDK + Maven) para compilar o `.jar`. O estágio 2 usa uma imagem leve (apenas JRE) para rodar o `.jar`, otimizando tamanho e segurança.
*   **No Angular:** O estágio 1 usa Node.js para instalar dependências e rodar o `npm run build`. O estágio 2 usa um servidor **Nginx** Alpine, que não apenas serve os arquivos estáticos velozmente, mas atua como *Proxy Reverso*, repassando rotas com prefixo `/api` diretamente para o contêiner do Spring Boot. Todos esses serviços, junto com o banco PostgreSQL, são orquestrados simultaneamente via `docker-compose.yml`.

### 📖 Glossário

*   **Spring Boot:** Extensão opinativa da plataforma Spring que minimiza configurações XML e provê autoconfiguração e servidores embutidos para rodar aplicações Java.
*   **Angular:** Framework web open-source baseado em TypeScript para construir aplicações modulares (com componentes e serviços) focadas em performance e fluidez.
*   **SPA (Single-Page Application):** Aplicação web que consiste de uma única página HTML, onde as atualizações de interface ocorrem dinamicamente via JavaScript sem a necessidade de recarregar a aba do navegador.
*   **RESTful:** API que implementa e obedece os princípios e restrições arquiteturais do REST (Transferência de Estado Representacional), comunicando-se via métodos HTTP de forma *stateless*.
*   **JWT (JSON Web Token):** Padrão aberto utilizado para transmitir informações de forma segura e compacta entre partes como um objeto JSON, muito usado para autorização de APIs sem estado.
*   **CORS (Cross-Origin Resource Sharing):** Mecanismo de segurança dos navegadores que restringe páginas web de fazerem requisições a domínios/portas diferentes da origem do front-end.
*   **Docker Multi-Stage Build:** Técnica de elaboração de `Dockerfile` que divide a construção da imagem em múltiplos estágios (ex: build e runtime) para criar uma imagem final enxuta, descartando as ferramentas de compilação.
*   **Nginx:** Servidor web de alto desempenho e proxy reverso, amplamente usado para hospedar aplicações Angular e rotear requisições para APIs back-end ocultas.

### 🔁 Prompts Reutilizáveis para Revisão

Copie e cole os prompts abaixo no NotebookLM (ou em seu assistente de IA) para continuar testando seus conhecimentos no futuro:

1. **Revisão de Arquitetura:** *"Resuma as diferenças principais entre uma arquitetura web tradicional (renderizada no servidor) e a arquitetura moderna (SPA Angular + API Spring Boot), listando as vantagens de cada uma."*
2. **Desafio de Segurança (JWT):** *"Elabore um quiz de 5 perguntas de múltipla escolha focado exclusivamente nas melhores práticas de segurança com Spring Security e JWT, com base nos meus documentos. Forneça o gabarito no final."*
3. **Simulação de DevOps (Docker):** *"Gere um exemplo prático de um arquivo `docker-compose.yml` que orquestre um banco PostgreSQL, uma API Spring Boot e um frontend Angular servido pelo Nginx. Adicione comentários no código explicando o que faz o 'depends_on' e como a comunicação interna das portas funciona."*
4. **Revisão de Erros de Rede:** *"Aja como um desenvolvedor Sênior. Meu frontend Angular na porta 4200 está recebendo 'CORS error' ao tentar fazer um POST na minha API Spring Boot na porta 8080. Explique por que isso ocorre e me dê 2 soluções possíveis para resolver o problema."*
