# 🚀 N8N + Evolution API - Docker Setup

Sistema completo de automação com N8N integrado à Evolution API para WhatsApp, executando em containers Docker.

## 📋 Visão Geral

Este projeto combina:
- **N8N**: Plataforma de automação de workflows
- **Evolution API**: API para integração com WhatsApp
- **PostgreSQL**: Banco de dados principal
- **Redis**: Cache e sistema de filas

## 🏗️ Arquitetura

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│      N8N        │    │  Evolution API  │    │   PostgreSQL    │
│   (Workflows)   │◄──►│   (WhatsApp)    │◄──►│   (Database)    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │     Redis       │
                    │  (Cache/Queue)  │
                    └─────────────────┘
```

## 🚀 Início Rápido

### Pré-requisitos

- Docker Desktop instalado e rodando
- Git (opcional)

### 1. Clone o Repositório

```bash
git clone <seu-repositorio>
cd rocketseat_ian8n
```

### 2. Configure as Variáveis de Ambiente

```bash
# Copie o arquivo de configuração
cp env.local .env

# Edite o arquivo .env com suas configurações
nano .env  # ou use seu editor preferido
```

### 3. Execute o Sistema

**Opção A - Script Automático (Recomendado):**

```bash
# Windows
start-local.bat

# Linux/Mac
chmod +x start-local.sh
./start-local.sh
```

**Opção B - Comando Manual:**

```bash
# Iniciar todos os serviços
docker-compose up -d

# Ver logs
docker-compose logs -f
```

## 🌐 URLs de Acesso

| Serviço | URL | Descrição |
|---------|-----|-----------|
| **N8N** | http://localhost:5678 | Interface principal de automação |
| **Evolution API** | http://localhost:8080 | API do WhatsApp |
| **PostgreSQL** | localhost:5432 | Banco de dados |
| **Redis** | localhost:6379 | Cache/Queue |

## ⚙️ Configuração

### Variáveis de Ambiente (.env)

```bash
# URLs e Domínios
SERVICE_FQDN_N8N=http://localhost:5678
SERVICE_FQDN_EVOLUTION=http://localhost:8080

# Banco de Dados
POSTGRES_DB=n8n
SERVICE_USER_POSTGRES=n8n_user
SERVICE_PASSWORD_POSTGRES=sua_senha_postgres

# Chave de Criptografia N8N (OBRIGATÓRIA)
SERVICE_BASE64_N8N=sua_chave_base64_32_caracteres

# Senhas
REDIS_PASSWORD=sua_senha_redis
SERVICE_PASSWORD_EVOLUTION=sua_senha_evolution
```

### Gerar Chave de Criptografia

```bash
# Gere uma chave segura para o N8N
openssl rand -base64 32
```

## 🔧 Comandos Úteis

### Gerenciamento de Containers

```bash
# Ver status dos serviços
docker-compose ps

# Ver logs em tempo real
docker-compose logs -f

# Ver logs de um serviço específico
docker-compose logs -f n8n
docker-compose logs -f evolution-api

# Parar todos os serviços
docker-compose down

# Parar e remover volumes (dados)
docker-compose down -v

# Reiniciar um serviço
docker-compose restart n8n
```

### Backup e Restore

```bash
# Backup do banco de dados
docker-compose exec postgresql pg_dump -U n8n_user n8n > backup_n8n.sql

# Restore do banco de dados
docker-compose exec -T postgresql psql -U n8n_user n8n < backup_n8n.sql
```

## 📁 Estrutura do Projeto

```
rocketseat_ian8n/
├── docker-compose.yml          # Configuração principal do Docker
├── docker-compose-evolution.yml # Configuração de referência
├── env.local                   # Variáveis de ambiente para local
├── env.example                 # Exemplo de variáveis de ambiente
├── start-local.bat            # Script de inicialização (Windows)
├── start-local.sh             # Script de inicialização (Linux/Mac)
└── README.md                  # Este arquivo
```

## 🔒 Segurança

### Para Produção

1. **Altere todas as senhas padrão**
2. **Use domínios HTTPS reais**
3. **Configure firewall adequadamente**
4. **Monitore logs regularmente**
5. **Faça backups regulares**

### Senhas Padrão (ALTERE!)

- PostgreSQL: `n8n_password_123`
- Redis: `redis_password_123`
- Evolution API: `evolution_password_123`

## 🐛 Troubleshooting

### Problemas Comuns

#### Redis desconectando
```bash
# Verificar logs do Redis
docker-compose logs redis

# Reiniciar Redis
docker-compose restart redis
```

#### N8N não inicia
```bash
# Verificar logs do N8N
docker-compose logs n8n

# Verificar se a chave de criptografia está configurada
grep SERVICE_BASE64_N8N .env
```

#### Evolution API com erro
```bash
# Verificar logs da Evolution API
docker-compose logs evolution-api

# Verificar conexão com banco
docker-compose logs postgresql
```

### Verificar Portas

```bash
# Windows
netstat -an | findstr :5678
netstat -an | findstr :8080

# Linux/Mac
netstat -tulpn | grep :5678
netstat -tulpn | grep :8080
```

## 📊 Monitoramento

### Health Checks

Todos os serviços possuem health checks configurados:

- **N8N**: Verifica se a interface web responde
- **PostgreSQL**: Verifica se o banco está acessível
- **Redis**: Verifica se o Redis responde ao PING

### Logs

```bash
# Logs de todos os serviços
docker-compose logs

# Logs com timestamp
docker-compose logs -t

# Logs de um serviço específico
docker-compose logs -f n8n
```

## 🔄 Atualizações

### Atualizar Imagens

```bash
# Parar serviços
docker-compose down

# Atualizar imagens
docker-compose pull

# Iniciar com novas imagens
docker-compose up -d
```

### Atualizar Configurações

1. Edite o arquivo `docker-compose.yml`
2. Execute `docker-compose down`
3. Execute `docker-compose up -d`

## 📞 Suporte

### Logs Importantes

```bash
# Logs completos
docker-compose logs > logs.txt

# Logs de erro apenas
docker-compose logs 2>&1 | grep -i error
```

### Informações do Sistema

```bash
# Versão do Docker
docker --version

# Informações dos containers
docker-compose config

# Uso de recursos
docker stats
```

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

---

**🎉 Pronto!** Seu sistema N8N + Evolution API está funcionando perfeitamente!

Para mais informações, consulte a documentação oficial:
- [N8N Documentation](https://docs.n8n.io/)
- [Evolution API Documentation](https://doc.evolution-api.com/)
