# PROJETO: NEW IDEIAS (SELL IDEAS)

# DESCRIÇÃO
A plataforma New Ideias é um sistema colaborativo onde usuários podem
compartilhar ideias, interagir com a comunidade e atrair investidores.
Funciona como um fórum com foco em projetos open-source e negociação.

# ARQUITETURA DO SISTEMA

Frontend → API (Node.js) → Backend → Banco de Dados (SQL)

# Tecnologias:
- Frontend: HTML5, CSS3, JavaScript / React
- Backend: Node.js
- Banco de Dados: PostgreSQL / MySQL

# 1. CONEXÃO COM BANCO DE DADOS

Arquivo: backend/config/database.js

Código:

const { Pool } = require('pg');

const pool = new Pool({
  user: process.env.DB_USER,
  host: process.env.DB_HOST,
  database: process.env.DB_NAME,
  password: process.env.DB_PASSWORD,
  port: 5432,
});

module.exports = pool;

# Boas práticas:
- Utilizar variáveis de ambiente (.env)
- Validar conexão ao iniciar servidor
- Separar configuração por ambiente (dev/prod)


# 2. ESTRUTURA DO BANCO DE DADOS

# Tabela: usuarios
CREATE TABLE usuarios (
  id SERIAL PRIMARY KEY,
  nome VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  senha TEXT,
  tipo_usuario VARCHAR(20),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# Tabela: ideias
CREATE TABLE ideias (
  id SERIAL PRIMARY KEY,
  titulo VARCHAR(255),
  descricao TEXT,
  categoria VARCHAR(100),
  user_id INT REFERENCES usuarios(id),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# Tabela: transacoes
CREATE TABLE transacoes (
  id SERIAL PRIMARY KEY,
  ideia_id INT,
  investidor_id INT,
  valor NUMERIC,
  data TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# 3. CRUD DE IDEIAS (BACKEND)

# Rotas:
POST    /ideias        → Criar ideia
GET     /ideias        → Listar ideias
GET     /ideias/:id    → Buscar ideia
PUT     /ideias/:id    → Atualizar ideia
DELETE  /ideias/:id    → Deletar ideia

# Implementação:

## Criar Ideia
app.post('/ideias', authMiddleware, async (req, res) => {
  const { titulo, descricao, categoria } = req.body;
  const userId = req.user.id;

  const result = await pool.query(
    'INSERT INTO ideias (titulo, descricao, categoria, user_id) VALUES ($1, $2, $3, $4) RETURNING *',
    [titulo, descricao, categoria, userId]
  );

  res.json(result.rows[0]);
});

## Listar Ideias
app.get('/ideias', async (req, res) => {
  const result = await pool.query('SELECT * FROM ideias');
  res.json(result.rows);
});

## Atualizar Ideia
app.put('/ideias/:id', authMiddleware, async (req, res) => {
  const { id } = req.params;
  const { titulo, descricao } = req.body;

  await pool.query(
    'UPDATE ideias SET titulo=$1, descricao=$2 WHERE id=$3',
    [titulo, descricao, id]
  );

  res.send("Atualizado");
});

## Deletar Ideia
app.delete('/ideias/:id', authMiddleware, async (req, res) => {
  const { id } = req.params;

  await pool.query('DELETE FROM ideias WHERE id=$1', [id]);

  res.send("Deletado");
});

# Regras de negócio:
- Apenas o criador pode editar/deletar
- Validar dados antes de salvar
- Sanitizar inputs


# 4. LOGIN E AUTENTICAÇÃO

# Fluxo:
1. Usuário envia email e senha
2. Backend valida credenciais
3. Geração de token JWT
4. Frontend armazena token
5. Token enviado nas requisições protegidas

# Código de Login:
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

app.post('/login', async (req, res) => {
  const { email, senha } = req.body;

  const user = await pool.query(
    'SELECT * FROM usuarios WHERE email=$1',
    [email]
  );

  if (user.rows.length === 0) {
    return res.status(401).send("Usuario nao encontrado");
  }

  const validPassword = await bcrypt.compare(senha, user.rows[0].senha);

  if (!validPassword) {
    return res.status(401).send("Senha invalida");
  }

  const token = jwt.sign(
    { id: user.rows[0].id },
    process.env.JWT_SECRET,
    { expiresIn: '1d' }
  );

  res.json({ token });
});

# Middleware de Autenticação:
function authMiddleware(req, res, next) {
  const token = req.headers.authorization;

  if (!token) return res.status(401).send("Sem token");

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch {
    res.status(401).send("Token invalido");
  }
}

# 5. INTEGRAÇÃO COM FRONTEND


fetch('/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ email, senha })
})
.then(res => res.json())
.then(data => {
  localStorage.setItem('token', data.token);
});

# 🔎 6. SISTEMA DE BUSCA

app.get('/ideias/busca', async (req, res) => {
  const { termo } = req.query;

  const result = await pool.query(
    "SELECT * FROM ideias WHERE titulo LIKE $1 OR descricao LIKE $1",
    [`%${termo}%`]
  );

  res.json(result.rows);
});

# 7. HISTÓRICO DE TRANSAÇÕES

app.post('/transacoes', async (req, res) => {
  const { ideia_id, investidor_id, valor } = req.body;

  await pool.query(
    'INSERT INTO transacoes (ideia_id, investidor_id, valor) VALUES ($1, $2, $3)',
    [ideia_id, investidor_id, valor]
  );

  res.send("Transacao registrada");
});

# CONSIDERAÇÕES FINAIS

- Banco de dados é a base do sistema
- CRUD de ideias é a funcionalidade central
- Autenticação garante segurança
- Sistema de busca melhora a experiência do usuário

# 👨‍💻 EQUIPE

PO: Alexandre Bonissoni  
Backend / Scrum Master: Adilson  
Frontend: Gustavo Lacerda  
