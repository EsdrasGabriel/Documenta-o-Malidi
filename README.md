# Documentação Completa - Sistema de Gestão de Clientes e Serviços

## Índice

1. [Diagrama do Banco de Dados](#1-diagrama-do-banco-de-dados)
2. [Requisitos do Sistema](#2-requisitos-do-sistema)

---

# 1. Diagrama do Banco de Dados

## 1.1 Estrutura das Tabelas e Relacionamentos

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                DIAGRAMA ER                                                 │
└─────────────────────────────────────────────────────────────────────────────────────────┘

    ┌─────────────────────┐                    ┌─────────────────────┐
    │      CLIENTE        │                    │      USUÁRIO        │
    ├─────────────────────┤                    ├─────────────────────┤
    │ • id (PK)           │                    │ • id (PK)           │
    │ • nome              │                    │ • nome              │
    │ • telefone          │                    │ • email             │
    │ • cpf               │                    │ • telefone          │
    │ • nivel_prioridade  │                    │ • cpf               │
    │   (1, 2, 3)         │                    │ • permissoes        │
    └─────────────────────┘                    │ • senha             │
              │                                └─────────────────────┘
              │ 1:N                                        │
              │                                            │ (Independente)
              ▼                                            │
    ┌─────────────────────┐                                ▼
    │      ENDERECO       │                    ┌─────────────────────┐
    ├─────────────────────┤                    │     PERMISSÕES      │
    │ • id (PK)           │                    │   (Enum/Tabela)     │
    │ • cliente_id (FK)   │                    └─────────────────────┘
    │ • logradouro        │
    │ • numero            │
    │ • complemento       │
    │ • bairro            │
    │ • cidade            │
    │ • estado            │
    │ • cep               │
    │ • tipo_endereco     │
    │   (residencial,     │
    │    comercial,       │
    │    entrega, etc)    │
    └─────────────────────┘
              │
              │
              │ 1:N
              │
              ▼
    ┌─────────────────────┐
    │   ORDEM_SERVICO     │
    │   (Dados Entrada)   │
    ├─────────────────────┤
    │ • id (PK)           │
    │ • cliente_id (FK)   │
    │ • modelo_id (FK)    │─────────┐
    │ • servico_id (FK)   │─────────┼─────┐
    │ • servico_sec_id    │─────────┼─────┼───┐
    │   (FK - opcional)   │         │     │   │
    │ • quantidade        │         │     │   │
    │ • data_chegada      │         │     │   │
    │ • horario_chegada   │         │     │   │
    │ • referencia        │         │     │   │
    └─────────────────────┘         │     │   │
                                    │     │   │
                                    │     │   │
              ┌─────────────────────┘     │   │
              │                           │   │
              ▼                           │   │
    ┌─────────────────────┐               │   │
    │       MODELO        │               │   │
    ├─────────────────────┤               │   │
    │ • id (PK)           │               │   │
    │ • nome              │               │   │
    │ • valor             │               │   │
    └─────────────────────┘               │   │
                                          │   │
                    ┌─────────────────────┘   │
                    │                         │
                    ▼                         │
          ┌─────────────────────┐             │
          │      SERVICO        │             │
          ├─────────────────────┤             │
          │ • id (PK)           │             │
          │ • nome              │             │
          │ • valor             │             │
          └─────────────────────┘             │
                                              │
                        ┌─────────────────────┘
                        │
                        ▼
              ┌─────────────────────┐
              │ SERVICO_SECUNDARIO  │
              ├─────────────────────┤
              │ • id (PK)           │
              │ • nome              │
              └─────────────────────┘
```

## 1.2 Relacionamentos

### 1.2.1 CLIENTE → ENDERECO (1:N)

- Um cliente pode ter múltiplos endereços cadastrados
- Cada endereço pertence a um único cliente

### 1.2.2 CLIENTE → ORDEM_SERVICO (1:N)

- Um cliente pode ter múltiplas ordens de serviço
- Cada ordem de serviço pertence a um único cliente

### 1.2.3 MODELO → ORDEM_SERVICO (1:N)

- Um modelo pode ser usado em múltiplas ordens de serviço
- Cada ordem de serviço tem um único modelo

### 1.2.4 SERVICO → ORDEM_SERVICO (1:N)

- Um serviço pode ser usado em múltiplas ordens de serviço
- Cada ordem de serviço tem um único serviço principal

### 1.2.5 SERVICO_SECUNDARIO → ORDEM_SERVICO (1:N)

- Um serviço secundário pode ser usado em múltiplas ordens de serviço
- Cada ordem de serviço pode ter um serviço secundário (opcional)

### 1.2.6 USUÁRIO (Independente)

- Tabela para controle de acesso e autenticação
- Não possui relacionamento direto com outras entidades

## 1.3 Observações

### 1.3.1 Níveis de Prioridade do Cliente:

- **1**: Prioridade Alta
- **2**: Prioridade Média
- **3**: Prioridade Baixa

### 1.3.2 Tipos de Endereço:

Sugestão de tipos:

- **residencial**: Endereço residencial do cliente
- **comercial**: Endereço comercial/trabalho
- **entrega**: Endereço específico para entregas
- **cobrança**: Endereço para correspondências de cobrança
- **outros**: Outros tipos de endereço

### 1.3.3 Níveis de Permissão do Usuário:

Sugestão de estrutura:

- **ADMIN**: Acesso total ao sistema
- **MANAGER**: Acesso a relatórios e gerenciamento
- **OPERATOR**: Acesso a operações básicas
- **VIEWER**: Apenas visualização

### 1.3.4 Campos Importantes:

- **PK**: Primary Key (Chave Primária)
- **FK**: Foreign Key (Chave Estrangeira)
- Todos os IDs são auto-incrementais
- CPF deve ter validação e ser único
- Email do usuário deve ser único

## 1.4 SQL Sugerido (Estrutura Base)

```sql
-- Tabela Cliente
CREATE TABLE cliente (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    telefone VARCHAR(20),
    cpf VARCHAR(14) UNIQUE NOT NULL,
    nivel_prioridade INTEGER CHECK (nivel_prioridade IN (1, 2, 3)) DEFAULT 3,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabela Endereço
CREATE TABLE endereco (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER REFERENCES cliente(id) ON DELETE CASCADE,
    logradouro VARCHAR(255) NOT NULL,
    numero VARCHAR(20),
    complemento VARCHAR(255),
    bairro VARCHAR(255),
    cidade VARCHAR(255) NOT NULL,
    estado VARCHAR(2) NOT NULL,
    cep VARCHAR(10) NOT NULL,
    tipo_endereco VARCHAR(50) DEFAULT 'residencial',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabela Modelo
CREATE TABLE modelo (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabela Serviço
CREATE TABLE servico (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabela Serviço Secundário
CREATE TABLE servico_secundario (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabela Ordem de Serviço
CREATE TABLE ordem_servico (
    id SERIAL PRIMARY KEY,
    cliente_id INTEGER REFERENCES cliente(id),
    modelo_id INTEGER REFERENCES modelo(id),
    servico_id INTEGER REFERENCES servico(id),
    servico_secundario_id INTEGER REFERENCES servico_secundario(id),
    quantidade INTEGER NOT NULL DEFAULT 1,
    data_chegada DATE NOT NULL,
    horario_chegada TIME NOT NULL,
    referencia VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabela Usuário
CREATE TABLE usuario (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    telefone VARCHAR(20),
    cpf VARCHAR(14) UNIQUE NOT NULL,
    permissoes VARCHAR(50) NOT NULL,
    senha VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 2. Requisitos do Sistema

## 2.1 Requisitos Funcionais

### 2.1.1 Gestão de Ordens de Serviço

#### RF001 - Entrada de Ficha de Serviço

- **Descrição**: Quando der entrada em uma ficha, deve constar no sistema que está aguardando para ser feita
- **Critérios de Aceitação**:
  - O sistema deve registrar automaticamente o status "Aguardando" ao criar uma nova ordem de serviço
  - A data e hora de entrada devem ser registradas automaticamente
  - A ordem deve ser visível na lista de serviços pendentes

#### RF002 - Fluxo de Processo de Serviços

- **Descrição**: Fluxo de processo, quando cada serviço for feito deverá atualizar no sistema
- **Critérios de Aceitação**:
  - Possibilidade de atualizar status do serviço (Aguardando → Em Andamento → Concluído)
  - Registro de cada etapa do processo com timestamp
  - Histórico completo de mudanças de status

#### RF003 - Responsável pelo Serviço

- **Descrição**: Quando atualizar o serviço deverá informar quem foi o responsável por fazer o tal
- **Critérios de Aceitação**:
  - Campo obrigatório para identificar o usuário responsável
  - Registro do responsável em cada atualização de status
  - Rastreabilidade de quem executou cada etapa

#### RF004 - Registro de Tempo de Conclusão

- **Descrição**: Informar a hora exata em que cada serviço foi finalizado na atualização
- **Critérios de Aceitação**:
  - Timestamp automático quando status for alterado para "Concluído"
  - Cálculo do tempo total de execução do serviço
  - Histórico temporal de todas as etapas

#### RF005 - Notificação Automática de Conclusão

- **Descrição**: Quando o serviço for atualizado para concluído deverá automaticamente enviar uma mensagem para o cliente informando-o
- **Critérios de Aceitação**:
  - Envio automático de notificação via WhatsApp/SMS quando status = "Concluído"
  - Template de mensagem configurável
  - Log de notificações enviadas

### 2.1.2 Gestão Financeira

#### RF006 - Controle Financeiro

- **Descrição**: Necessitará também de uma parte financeira para métricas sobre os pagamentos dos serviços já que poderão ser feitos posteriormente a entrega
- **Critérios de Aceitação**:
  - Controle de status de pagamento (Pendente, Pago, Vencido)
  - Cálculo automático de valores baseado em modelo + serviço + quantidade
  - Relatórios financeiros (contas a receber, valores pagos, em atraso)
  - Data de vencimento configurável

#### RF007 - Cobrança Automática

- **Descrição**: Caso se passe um tempo de 3 dias para o pagamento deverá enviar uma mensagem via WhatsApp para o cliente alertando o débito
- **Critérios de Aceitação**:
  - Sistema de verificação diária de pagamentos em atraso
  - Envio automático de mensagem de cobrança após 3 dias
  - Configuração de templates de mensagem de cobrança
  - Escalação de cobrança (3 dias, 7 dias, 15 dias)

### 2.1.3 Operações CRUD

#### RF008 - CRUD de Clientes

- **Descrição**: CRUD para criação de clientes
- **Critérios de Aceitação**:
  - Criar, visualizar, editar e excluir clientes
  - Validação de CPF único
  - Controle de nível de prioridade (1, 2, 3)
  - Histórico de alterações

#### RF009 - CRUD de Entrada de Serviço

- **Descrição**: CRUD para criação de entrada de serviço
- **Critérios de Aceitação**:
  - Criar, visualizar, editar e excluir ordens de serviço
  - Associação obrigatória com cliente, modelo e serviço
  - Campos de quantidade, referência e observações
  - Não permitir exclusão de ordens com pagamento associado

#### RF010 - CRUD de Modelos

- **Descrição**: CRUD para criação de modelo
- **Critérios de Aceitação**:
  - Criar, visualizar, editar e excluir modelos
  - Campo de valor obrigatório
  - Controle de modelos ativos/inativos
  - Histórico de alterações de preços

#### RF011 - CRUD de Serviços

- **Descrição**: CRUD para criação de serviço
- **Critérios de Aceitação**:
  - Criar, visualizar, editar e excluir serviços
  - Campo de valor obrigatório
  - Categorização de serviços
  - Controle de serviços ativos/inativos

#### RF012 - CRUD de Serviços Secundários

- **Descrição**: CRUD para criação de serviço secundário
- **Critérios de Aceitação**:
  - Criar, visualizar, editar e excluir serviços secundários
  - Associação opcional às ordens de serviço
  - Controle de serviços secundários ativos/inativos

### 2.1.4 Gestão de Usuários e Permissões

#### RF013 - Controle de Usuários e Permissões

- **Descrição**: Criação de usuário e suas permissões só deverão ser possíveis no usuário admin
- **Critérios de Aceitação**:
  - Apenas usuários com permissão ADMIN podem criar/editar usuários
  - Sistema de permissões hierárquico (ADMIN > MANAGER > OPERATOR > VIEWER)
  - Controle de acesso por funcionalidade baseado em permissão
  - Auditoria de criação/alteração de usuários

## 2.2 Requisitos Não Funcionais

### 2.2.1 Performance

#### RNF001 - Tempo de Resposta

- **Descrição**: O sistema deve responder às requisições do usuário em tempo adequado
- **Critérios**:
  - Tempo de resposta para consultas: máximo 2 segundos
  - Tempo de resposta para operações CRUD: máximo 3 segundos
  - Carregamento de relatórios: máximo 5 segundos

#### RNF002 - Capacidade

- **Descrição**: O sistema deve suportar o volume esperado de dados e usuários
- **Critérios**:
  - Suportar até 10.000 clientes cadastrados
  - Suportar até 50.000 ordens de serviço por ano
  - Suportar até 50 usuários simultâneos

### 2.2.2 Segurança

#### RNF003 - Autenticação e Autorização

- **Descrição**: O sistema deve garantir acesso seguro e controlado
- **Critérios**:
  - Autenticação obrigatória para todos os usuários
  - Senhas devem ter no mínimo 8 caracteres com letras, números e símbolos
  - Sessões devem expirar após 4 horas de inatividade
  - Bloqueio de conta após 5 tentativas de login incorretas

#### RNF004 - Proteção de Dados

- **Descrição**: Os dados dos clientes devem ser protegidos adequadamente
- **Critérios**:
  - Dados sensíveis (CPF) devem ser mascarados na interface
  - Comunicação via HTTPS obrigatório
  - Conformidade com LGPD

### 2.2.3 Usabilidade

#### RNF005 - Interface do Usuário

- **Descrição**: O sistema deve ser intuitivo e fácil de usar
- **Critérios**:
  - Interface responsiva (desktop, tablet, mobile) para Web
  - Navegação intuitiva com no máximo 3 cliques para qualquer funcionalidade
  - Mensagens de erro claras e orientativas

#### RNF006 - Acessibilidade

- **Descrição**: O sistema deve ser acessível a usuários com diferentes necessidades
- **Critérios**:
  - Contraste adequado entre texto e fundo
  - Textos alternativos em imagens

### 2.2.4 Confiabilidade

#### RNF007 - Disponibilidade

- **Descrição**: O sistema deve estar disponível durante o horário comercial
- **Critérios**:
  - Disponibilidade de 99,5% durante horário comercial (08h às 18h)
  - Janela de manutenção programada fora do horário comercial

### 2.2.5 Manutenibilidade

#### RNF009 - Código e Documentação

- **Descrição**: O sistema deve ser fácil de manter e evoluir
- **Critérios**:
  - Código documentado seguindo padrões de desenvolvimento
  - Arquitetura modular e escalável
  - Testes automatizados com cobertura mínima de 80%
  - Documentação técnica atualizada

### 2.2.6 Integração

#### RNF012 - APIs e Integrações

- **Descrição**: O sistema deve permitir integrações futuras
- **Critérios**:
  - API REST documentada para integrações externas
  - Integração com serviços de mensageria (WhatsApp Business API)

### 2.2.8 Compliance e Regulamentações

#### RNF013 - Conformidade Legal

- **Descrição**: O sistema deve atender às regulamentações aplicáveis
- **Critérios**:
  - Conformidade com LGPD (Lei Geral de Proteção de Dados)
  - Auditoria de acesso a dados pessoais
  - Possibilidade de exclusão de dados a pedido do titular

## 2.3 Restrições Técnicas

### 2.3.1 Tecnologias

- Backend: Node.js com framework Nest.js
- Banco de Dados: PostgreSQL
- Frontend: React com Next.js
- Notificações: WhatsApp Business API
- Hospedagem: A escolher

### 2.3.2 Integrações Obrigatórias

- WhatsApp Business API para notificações
- Sistema de pagamento (opcional, para futuro)

### 2.3.3 Ambiente

- Desenvolvimento: Docker containers
- Versionamento: Github

## 2.4 Critérios de Aceitação Gerais

1. **Testes**: Todos os requisitos funcionais devem ser testados e validados
2. **Documentação**: Manual do usuário e documentação técnica completos
3. **Treinamento**: Capacitação da equipe para uso do sistema
4. **Suporte**: Definição de SLA para suporte pós-implementação (até prazo determinado previamente)

---

**Documento elaborado em**: setembro de 2025  
**Versão**: 1.0  
**Status**: Aprovação Pendente
