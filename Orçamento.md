# Proposta de Projeto MVP - Sistema de Gestão de Clientes e Serviços

**Valor do Projeto**: R$ 5.000 - R$ 6.000  
**Data**: Setembro de 2025  
**Versão**: 1.0

---

## 🎯 Resumo Executivo

Esta proposta apresenta uma versão MVP (Produto Mínimo Viável) do sistema de gestão de clientes e serviços, adaptada para um orçamento de R$ 5.000 - R$ 6.000. O foco está nas funcionalidades essenciais que resolvem os principais problemas do negócio, com possibilidade de evolução futura.

---

## 📋 Escopo do MVP

### ✅ Funcionalidades Incluídas

#### **1. Base do Sistema** - R$ 800

- Setup inicial do projeto
- Configuração do banco PostgreSQL
- Sistema de autenticação básico (usuário único admin)
- Estrutura base da aplicação

#### **2. CRUD de Clientes** - R$ 600

- Cadastro de clientes com dados essenciais
- Edição e exclusão de clientes
- Listagem com busca básica
- Validação de CPF
- Endereço único por cliente

#### **3. CRUD de Ordens de Serviço** - R$ 800

- Criação de ordens de serviço
- Associação com cliente, modelo e serviço
- Controle de quantidade
- Data e horário de chegada
- Campo de referência

#### **4. CRUD de Modelos e Serviços** - R$ 500

- Cadastro de modelos com valores
- Cadastro de serviços com valores
- Listagem e edição
- Controle de ativo/inativo

#### **5. Controle de Status de Serviços** - R$ 700

- Workflow simples: Aguardando → Em Andamento → Concluído
- Registro de responsável por cada atualização
- Timestamp automático de mudanças
- Histórico básico de status

#### **6. Controle Financeiro Básico** - R$ 500

- Status de pagamento: Pendente/Pago
- Cálculo automático do valor total (modelo + serviço × quantidade)
- Listagem de contas a receber
- Relatório financeiro básico

#### **7. Frontend Responsivo** - R$ 1.200

- Interface web responsiva (desktop/mobile)
- Design simples e funcional
- Navegação intuitiva
- Formulários validados
- Listagens com filtros básicos

#### **8. Deploy e Testes** - R$ 400

- Configuração de ambiente de produção
- Testes básicos de funcionalidade
- Documentação de instalação
- Manual básico do usuário

### **💰 Valor Total: R$ 5.500**

---

## ❌ Funcionalidades Removidas (Para Fases Futuras)

- Sistema de notificações WhatsApp
- Cobrança automática
- Endereços múltiplos por cliente
- Serviços secundários
- Sistema de permissões avançado
- Relatórios complexos com gráficos
- Sistema de auditoria detalhado
- Integração com APIs externas

---

## 🛠️ Especificações Técnicas

### **Stack Tecnológica**

- **Backend**: Nest.js
- **Frontend**: Next.js
- **Banco de Dados**: PostgreSQL
- **Deploy**: Heroku ou plataforma similar
- **Controle de Versão**: Git/GitHub

---

## 📊 Distribuição Detalhada do Desenvolvimento

| **Atividade**                  | **Horas** | **Valor/Hora**  | **Total**    |
| ------------------------------ | --------- | --------------- | ------------ |
| Setup e configuração inicial   | 10h       | R$ 40           | R$ 400       |
| Modelagem e criação do banco   | 8h        | R$ 50           | R$ 400       |
| Desenvolvimento da API Backend | 30h       | R$ 45           | R$ 1.350     |
| Sistema de autenticação        | 6h        | R$ 50           | R$ 300       |
| Lógica de negócio e validações | 15h       | R$ 50           | R$ 750       |
| Desenvolvimento Frontend React | 25h       | R$ 50           | R$ 1.250     |
| Integração Frontend + Backend  | 8h        | R$ 45           | R$ 360       |
| Testes e correções             | 6h        | R$ 40           | R$ 240       |
| Deploy e documentação          | 12h       | R$ 40           | R$ 480       |
| **TOTAL**                      | **120h**  | **Média R$ 46** | **R$ 5.530** |

---

## 🚀 Fases Futuras (Opcional)

### **FASE 2 - Automações (R$ 3.500)**

#### **Funcionalidades:**

- Integração WhatsApp Business API
- Notificações automáticas de conclusão
- Sistema de cobrança automática (3 dias)
- Relatórios financeiros avançados
- Dashboard com gráficos básicos

#### **Distribuição Detalhada:**

| **Atividade**                       | **Horas** | **Valor/Hora**  | **Total**    |
| ----------------------------------- | --------- | --------------- | ------------ |
| Integração WhatsApp Business API    | 20h       | R$ 60           | R$ 1.200     |
| Sistema de notificações automáticas | 15h       | R$ 55           | R$ 825       |
| Sistema de cobrança automática      | 12h       | R$ 55           | R$ 660       |
| Relatórios financeiros avançados    | 10h       | R$ 50           | R$ 500       |
| Dashboard com gráficos básicos      | 8h        | R$ 50           | R$ 400       |
| Testes e ajustes                    | 6h        | R$ 45           | R$ 270       |
| **TOTAL FASE 2**                    | **71h**   | **Média R$ 49** | **R$ 3.855** |

_Valor comercial: R$ 3.500 (desconto aplicado)_

### **FASE 3 - Funcionalidades Avançadas (R$ 2.500)**

#### **Funcionalidades:**

- Sistema de permissões completo (ADMIN, MANAGER, OPERATOR, VIEWER)
- Múltiplos endereços por cliente
- Serviços secundários
- Dashboard avançado com métricas
- Sistema de auditoria e logs detalhados
- Backup automático

#### **Distribuição Detalhada:**

| **Atividade**                         | **Horas** | **Valor/Hora**  | **Total**    |
| ------------------------------------- | --------- | --------------- | ------------ |
| Sistema de permissões completo        | 15h       | R$ 55           | R$ 825       |
| Múltiplos endereços por cliente       | 8h        | R$ 50           | R$ 400       |
| Implementação de serviços secundários | 6h        | R$ 50           | R$ 300       |
| Dashboard avançado com métricas       | 12h       | R$ 50           | R$ 600       |
| Sistema de auditoria e logs           | 10h       | R$ 55           | R$ 550       |
| Sistema de backup automático          | 6h        | R$ 45           | R$ 270       |
| Testes e documentação                 | 5h        | R$ 45           | R$ 225       |
| **TOTAL FASE 3**                      | **62h**   | **Média R$ 51** | **R$ 3.170** |

_Valor comercial: R$ 2.500 (desconto aplicado)_

---

## 📅 Cronograma de Desenvolvimento

| **Semana** | **Atividades**                        | **Entregáveis**                   |
| ---------- | ------------------------------------- | --------------------------------- |
| **1-2**    | Backend, banco de dados, autenticação | API funcional + banco configurado |
| **3-4**    | Frontend React, interfaces principais | Interface web responsiva          |
| **5**      | Integração, testes, ajustes           | Sistema integrado funcionando     |
| **6**      | Deploy, documentação, treinamento     | Sistema em produção + manuais     |

**Duração Total**: 6 semanas

---

## 💰 Condições Comerciais

### **Investimento Total: R$ 5.500**

### **Forma de Pagamento:**

- **40% (R$ 2.200)** - Assinatura do contrato
- **60% (R$ 3.300)** - Entrega e aprovação final

### **O Que Está Incluído:**

✅ Sistema completo funcional  
✅ Código fonte com documentação  
✅ Deploy em ambiente de produção  
✅ Manual do usuário  
✅ 1 mês de suporte para correções  
✅ Treinamento básico (2 horas remotas)

### **O Que NÃO Está Incluído:**

❌ Design customizado elaborado  
❌ Hospedagem mensal (cliente responsável)  
❌ Integrações com APIs externas  
❌ Funcionalidades além do escopo definido  
❌ Treinamento presencial

---

## 🎯 Benefícios do MVP

1. **Solução Imediata**: Resolve os principais problemas de gestão
2. **Baixo Investimento**: Valor acessível para pequenas empresas
3. **Escalável**: Possibilidade de evolução por fases
4. **ROI Rápido**: Retorno do investimento em poucos meses
5. **Validação**: Permite testar o sistema antes de investir mais

---

## 📞 Próximos Passos

1. **Aprovação da Proposta**: Análise e aceite do escopo
2. **Contrato**: Assinatura do documento formal
3. **Kickoff**: Reunião de início do projeto
4. **Desenvolvimento**: Execução conforme cronograma
5. **Entrega**: Homologação e treinamento

---

## 📋 Critérios de Aceitação

- [ ] Todos os CRUDs funcionando corretamente
- [ ] Sistema de status operacional
- [ ] Cálculos financeiros precisos
- [ ] Interface responsiva funcionando
- [ ] Sistema deployado e acessível
- [ ] Documentação entregue
- [ ] Treinamento realizado

---

**Elaborado por**: Esdras Gabriel Farias dos Santos
**Data**: Setembro de 2025  
**Validade da Proposta**: 30 dias

---

_Esta proposta foi elaborada com base na documentação completa do sistema, priorizando as funcionalidades essenciais dentro do orçamento disponível._
