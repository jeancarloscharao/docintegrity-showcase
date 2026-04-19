# 🧠 Arquitetura — DocIntegrity

## 📌 Visão geral

O DocIntegrity é uma plataforma SaaS para validação de integridade de documentos digitais, baseada em hashing criptográfico SHA-256.

A arquitetura foi projetada para suportar múltiplos clientes (multi-tenant), validação pública e integração via API. 

---

## 🎯 Objetivos arquiteturais

* garantir integridade de documentos
* permitir validação pública segura
* suportar múltiplos clientes (SaaS)
* fornecer API para integração externa
* manter rastreabilidade completa

---

## 🧩 Arquitetura do sistema

### 🌐 Frontend

* landing page institucional
* página pública de verificação (`/verify/{token}`)
* interface otimizada para validação rápida

---

### 🛠️ Backend (Filament)

* painel administrativo por tenant
* gestão de documentos
* histórico de validações
* gestão de planos e usuários

---

### 🔌 API REST

* autenticação com Sanctum
* endpoints para documentos e validações
* isolamento por tenant via middleware

---

## 🏗️ Arquitetura em camadas

### Camada de apresentação

* site público
* painel administrativo
* página de verificação

---

### Camada de aplicação

* registro de documentos
* validação de integridade
* geração de QR Code
* controle de uso por plano

---

### Camada de domínio

* Document (hash, token, metadados)
* Validation (resultado da verificação)
* Tenant (isolamento multi-empresa)
* Subscription (controle de planos)

---

### Camada de dados

* persistência de documentos e hashes
* histórico de validações
* dados de tenants e usuários

---

## 🔐 Modelo de integridade

O sistema utiliza hashing SHA-256 para garantir autenticidade:

* cada documento gera um hash único
* o hash é armazenado no banco
* validações comparam hashes usando `hash_equals`

---

## 🔄 Fluxo de validação

1. usuário envia documento
2. sistema gera hash SHA-256
3. hash é armazenado
4. documento recebe token público
5. outro usuário envia documento para validação
6. sistema gera novo hash
7. hashes são comparados
8. resultado é retornado

---

## 🧩 Multi-tenancy

* cada tenant representa uma empresa
* dados isolados por `tenant_id`
* middleware garante isolamento na API
* painel usa slug no URL (`/admin/{tenant}`)

---

## 📊 Controle de uso

* planos com limites mensais
* contagem de documentos por tenant
* fallback para plano gratuito (20 documentos/mês)

---

## 🔗 Verificação pública

* token único por documento (UUID)
* endpoint `/verify/{token}`
* não exige autenticação
* permite validação por terceiros

---

## ⚙️ Processamento assíncrono

* geração de QR Code via job
* execução em fila
* melhora performance do sistema

---

## 📈 Escalabilidade

A arquitetura permite evolução para:

* armazenamento distribuído
* auditoria completa de acessos
* integração com sistemas externos
* validação em larga escala

---

## 💡 Decisões arquiteturais

* uso de SHA-256 para integridade
* separação entre registro e validação
* token público para compartilhamento
* multi-tenancy para SaaS
* API-first para integração

---

## 🧾 Conclusão

O DocIntegrity foi projetado como uma solução escalável para validação de documentos, combinando segurança, rastreabilidade e integração. Sua arquitetura permite uso tanto por usuários finais quanto por sistemas externos via API.
