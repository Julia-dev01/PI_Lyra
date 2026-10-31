# Lyra

**Plataforma web de apoio e acolhimento de mulheres em situação de violência doméstica**

Trabalho de Conclusão de Curso (TCC) – Técnico em Informática para Internet
SESI/SENAI Candeias (BA) · 3º ano A · 2026

> ⚠️ **Aviso importante:** o Lyra é um **protótipo acadêmico**. Ele **não deve ser usado em situações reais**: os dados ficam apenas em memória, não há criptografia, autenticação nem conformidade com a LGPD. Em caso de violência, ligue **180** (Central de Atendimento à Mulher, gratuito, 24 h), **190** (Polícia) ou **192** (SAMU).

---

## Sobre o projeto

A violência contra a mulher acontece, na maioria das vezes, dentro de casa e por alguém próximo. Medo, vergonha e dependência financeira dificultam a denúncia, e nem toda mulher conhece os canais de ajuda.

O Lyra reúne em um só lugar **informação, canais de ajuda e um formulário de registro**, como porta de entrada para a rede de proteção já existente (Ligue 180, 190 etc.). Ele **não substitui** esses serviços.

**Objetivo geral:** desenvolver um protótipo de plataforma web que reúna informação, canais de ajuda e um formulário de registro de denúncia, ampliando o acesso a direitos e fortalecendo redes de apoio.

## Funcionalidades

- Página inicial de acolhimento ("Alerta Segura") com acesso rápido a 190, 192 e 193 e aviso de atendimento 24 h
- Botão **Saída rápida** (clique ou `ESC` duas vezes)
- Formulário de cadastro com nome, endereço, tipo de violência, presença/tipo de arma, contato de emergência e telefone de um familiar
- Cinco tipos de violência da Lei Maria da Penha (art. 7º): física, psicológica, moral, patrimonial e sexual
- Tela **Apoio e informações**: telefones, ONGs e orientações
- Seção de pesquisas sobre violência contra a mulher e feminicídio (conteúdo estático)
- Modo escuro e design responsivo
- API REST com rotas GET, POST, PUT e DELETE

## Tecnologias

| Camada | Tecnologia |
|---|---|
| Front-end | HTML, CSS, JavaScript |
| Back-end | Node.js + Express |
| API | REST (JSON) |
| Banco de dados | MySQL (apenas modelagem – ainda não conectado) |
| Testes | Manuais, no navegador e no Postman |
| Versionamento | Git / GitHub |

## Arquitetura

```
Navegador (HTML · CSS · JS)
        │  requisições HTTP (JSON)
        ▼
API REST (Node.js + Express)
        │
        ├──► Dados em memória   (protótipo atual)
        └──► MySQL – tabela usuarios   (planejado)
```

## Estrutura do repositório

> Ajuste esta árvore para refletir exatamente as pastas do projeto.

```
lyra/
├── public/            # front-end (páginas, CSS e JS)
├── routes/            # rotas da API
├── controllers/       # regras de cada rota
├── database/
│   └── lyra.sql       # script de criação do banco
├── server.js          # inicialização do servidor
├── package.json
└── README.md
```

## Como executar

**Pré-requisitos:** [Node.js](https://nodejs.org/) (versão LTS) e Git.

```bash
# 1. Clonar o repositório
git clone <URL-DO-REPOSITORIO>
cd lyra

# 2. Instalar as dependências
npm install

# 3. Iniciar o servidor
node server.js
```

O servidor sobe em `http://localhost:3000` (ajuste a porta se o projeto usar outra). Abra o endereço no navegador para acessar a interface.

## API REST

Base: `http://localhost:3000`

| Método | Rota | Descrição |
|---|---|---|
| GET | `/usuarios` | Lista os registros |
| GET | `/usuarios/:id` | Busca um registro pelo id (404 se não existir) |
| POST | `/usuarios` | Cria um novo registro |
| PUT | `/usuarios/:id` | Atualiza um registro existente |
| DELETE | `/usuarios/:id` | Remove um registro |

**Exemplo – POST /usuarios**

```json
{
  "nome": "Nome de exemplo",
  "endereco": "Rua Exemplo, 123",
  "tipo_violencia": "psicologica",
  "agressor_tem_arma": 0,
  "tipo_arma": null,
  "contato_emergencia": "71 90000-0000",
  "telefone_familiar": "71 90000-0001"
}
```

> Use apenas dados fictícios nos testes.

## Banco de dados (modelagem)

```sql
CREATE DATABASE lyra;
USE lyra;

CREATE TABLE usuarios (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nome VARCHAR(150) NOT NULL,
  endereco VARCHAR(255),
  tipo_violencia ENUM('fisica','psicologica','moral','patrimonial','sexual'),
  agressor_tem_arma TINYINT(1) DEFAULT 0,
  tipo_arma VARCHAR(100),
  contato_emergencia VARCHAR(20),
  telefone_familiar VARCHAR(20),
  criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

A conexão da API com o MySQL **ainda não foi implementada**.

## Testes realizados

12 casos manuais, todos aprovados:

- **Front-end (CT01–CT06):** abrir página inicial, ir ao formulário, modo escuro, responsividade, envio vazio (alerta) e envio preenchido
- **API (CT07–CT12):** GET lista, GET por id, POST, PUT, DELETE e GET com id inexistente (404)

Não foram feitos testes automatizados, de carga, de segurança nem com usuárias reais.

## Limitações conhecidas

- Dados temporários: se o servidor reiniciar, tudo se perde
- MySQL não conectado
- Sem autenticação e controle de acesso
- Sem criptografia
- Sem conformidade com a LGPD (consentimento, política de privacidade, exclusão de dados)
- Contador de denúncias e seção de pesquisas estáticos
- Sem painel para órgãos de apoio e sem integração com canais oficiais

## Próximos passos

1. Conectar o MySQL e persistir os dados
2. Criptografia (HTTPS/TLS), autenticação e controle de acesso
3. Adequação à LGPD
4. Painel para órgãos de apoio
5. Integração formal com o Ligue 180 e o 190
6. Testes com mulheres e especialistas
7. Testes automatizados, acessibilidade e conteúdo dinâmico

## Referencial

- Lei nº 11.340/2006 (Lei Maria da Penha)
- Lei nº 13.104/2015 (Lei do Feminicídio)
- Lei nº 13.709/2018 (LGPD)
- Saffioti, H. *Gênero, patriarcado e violência* (2015)
- Minayo, M. C. S. *Violência e saúde* (2006)
- Instituto AzMina – aplicativo PenhaS (2025)
- Fórum Brasileiro de Segurança Pública – Anuário Brasileiro de Segurança Pública


## Equipe

- Alexandre Brito de Santana
- Fredson Rafael Silva Souza
- Júlia das Neves Paim
- Luma Cristina de Souza Barbosa
- Murilo de Assis dos Santos de Oliveira

**Orientador:** Prof. Adalberto Teixeira Santana
