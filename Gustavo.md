# PROJETO: NEW IDEIAS

# PLANEJAMENTO E ARQUITETURA DO SISTEMA

A arquitetura será baseada no modelo cliente-servidor, com separação clara de responsabilidades
entre frontend, backend e banco de dados.

Fluxo do sistema:

Usuário → Frontend → API → Backend → Banco de Dados

Estrutura:

Frontend:
Responsável pela interface e experiência do usuário, incluindo telas de login,
cadastro, feed de ideias e perfil.

Backend:
Responsável pelas regras de negócio, autenticação, controle de acesso e processamento
das requisições.

Banco de Dados:
Responsável pela persistência das informações, garantindo integridade e relacionamento
entre os dados.

Organização do projeto:

/project
  /frontend
  /backend
    /config
    /routes
    /controllers
    /models

Diretrizes:
- Separação de responsabilidades (arquitetura MVC)
- Padronização de rotas e respostas da API
- Uso de variáveis de ambiente para segurança
- Estrutura preparada para escalabilidade

# MODELAGEM DO BANCO DE DADOS

A modelagem do banco será realizada com foco em organização, integridade e suporte
às funcionalidades principais da aplicação.

Entidades principais:

Usuários:
Armazena dados dos usuários da plataforma, incluindo identificação e tipo de perfil
(investidor ou criador de ideias).

Ideias:
Representa as ideias publicadas na plataforma, contendo título, descrição,
categoria e vínculo com o usuário criador.

Transações:
Registra interações financeiras ou negociações relacionadas às ideias,
garantindo rastreabilidade e transparência.

Relacionamentos:
- Um usuário pode criar várias ideias
- Uma ideia pode possuir múltiplas transações
- Transações estão associadas a ideias e usuários

Boas práticas adotadas:
- Uso de chaves primárias e estrangeiras
- Normalização do banco de dados
- Estrutura preparada para otimização futura (índices)

# FUNCIONALIDADES PRINCIPAIS DO SISTEMA

CRUD de Ideias:
Será o núcleo do sistema, permitindo que usuários criem, visualizem,
editem e removam ideias. Essa funcionalidade será protegida por autenticação
e seguirá regras de autorização baseadas no usuário criador.

Regras de negócio:
- Apenas o autor pode editar ou excluir sua ideia
- Validação de dados obrigatórios
- Garantia de consistência das informações

Autenticação e Login:
O sistema contará com autenticação baseada em token, garantindo segurança
no acesso às funcionalidades protegidas.

Fluxo de autenticação:
- Usuário envia credenciais
- Sistema valida os dados
- Token de acesso é gerado
- Token é utilizado nas requisições subsequentes

Integração com Frontend:
O frontend será responsável por consumir a API, enviar dados e armazenar
informações de autenticação para manter a sessão do usuário ativa.

Sistema de Busca:
Será implementado para permitir que investidores encontrem ideias com facilidade,
utilizando filtros por texto, categorias ou palavras-chave.

Objetivo:
- Melhorar a navegação
- Facilitar descoberta de ideias relevantes
- Otimizar experiência do usuário

Histórico de Transações:
Responsável por registrar interações financeiras dentro da plataforma,
garantindo transparência e confiabilidade para investidores e criadores.

# DESIGN E EXPERIÊNCIA DO USUÁRIO

Antes da implementação do frontend, serão desenvolvidos wireframes
para definir a estrutura visual e fluxo das telas principais.

Telas planejadas:

Tela de Login:
Entrada do sistema com autenticação do usuário.

Tela de Cadastro:
Registro de novos usuários com definição de perfil.

Tela de Feed de Ideias:
Listagem das ideias disponíveis na plataforma.

Tela de Detalhes da Ideia:
Visualização completa de uma ideia específica.

Tela de Perfil:
Informações do usuário e histórico de atividades.

Diretrizes de interface:
- Interface simples e intuitiva
- Foco na experiência do usuário (UX)
- Layout responsivo para diferentes dispositivos

# CONSIDERAÇÕES SOBRE DESENVOLVIMENTO

O desenvolvimento será guiado por prioridades definidas em sprint,
com foco inicial na estrutura do sistema e banco de dados.

Ordem de prioridade:
1. Definição da arquitetura
2. Modelagem do banco de dados
3. Implementação das funcionalidades principais
4. Integração frontend e backend
5. Refinamento e melhorias

Pontos críticos:
- Estabilidade da conexão com banco de dados
- Segurança na autenticação
- Consistência nas regras de negócio

# EQUIPE

PO: Alexandre Bonissoni  
Backend / Scrum Master: Adilson  
Frontend: Gustavo Lacerda  
