# ClickTime - Gerenciamento de Tempo e Lucro

> ℹ️ **Nota sobre o Fork**: Este é um fork do projeto original criado por [arismarioneves](https://github.com/arismarioneves/ClickTime). Mantido neste repositório para fins de estudo, customização pessoal e backup.

Um sistema web para gerenciar tempo e calcular lucro mensal baseado no tempo gasto nas tasks do ClickUp.

## 🚀 Funcionalidades

- **Dashboard Financeiro**: Visualize horas trabalhadas e faturamento em tempo real
- **Integração ClickUp API v2**: Conexão direta com a API do ClickUp
- **Visualização por Data**: Entradas de tempo agrupadas por dia com horário de início
- **Múltiplos Períodos**: Esta semana, este mês ou mês anterior
- **Timezone Brasileiro**: Ajuste automático para America/Sao_Paulo
- **Relatórios Exportáveis**: Gere tabelas formatadas com observações
- **Debug Integrado**: Visualize dados brutos da API do ClickUp
- **Armazenamento Local**: Token e configurações salvas no navegador
- **Docker Ready**: Ambiente containerizado com PHP 8.2 e Apache

## 🛠️ Tecnologias

- **Backend**: PHP 8.2 com extensão cURL
- **Frontend**: HTML5, JavaScript ES6+, TailwindCSS (CDN)
- **Servidor Web**: Apache 2.4
- **Container**: Docker + Docker Compose
- **API Externa**: ClickUp REST API v2

## 🐳 Início Rápido com Docker (Recomendado)

### Pré-requisitos

- Docker instalado
- Docker Compose instalado

### Executar o projeto

```bash
# Build da imagem
docker-compose build

# Iniciar o container
docker-compose up -d
```

Acesse a aplicação em: http://localhost:8080

### Parar o container

```bash
docker-compose down
```

### Ver logs

```bash
docker-compose logs -f clicktime-app
```

---

## �📋 Instalação Manual

### Pré-requisitos

- Servidor web com PHP (Apache, Nginx, etc.)
- PHP 8.0 ou superior
- Extensão cURL habilitada no PHP
- Token da API do ClickUp

### Passos

1. **Clone ou baixe os arquivos** para seu servidor web:

   ```
   ClickTime/
   ├── index.html
   ├── api.php
   | ...
   └── README.md
   ```

2. **Configure seu servidor web** para servir os arquivos PHP

3. **Obtenha seu token da API do ClickUp**:
   - Acesse: https://clickup.com/api/developer-portal/authentication/#personal-token
   - Gere um Personal Token
   - Copie o token gerado

## 🎯 Como Usar

### 1. Obter Token do ClickUp

Acesse as configurações do ClickUp:

- Vá para Settings > Apps
- Ou acesse: [ClickUp API Settings](https://app.clickup.com/settings/apps)
- Gere um token em "API Token"
- Copie o token gerado

### 2. Acessar a Aplicação

**Com Docker (porta 8080)**:

```
http://localhost:8080
```

**Instalação Manual (porta do seu servidor)**:

```
http://localhost/ClickTime
```

### 3. Configurar

- Clique no ícone de engrenagem (⚙️)
- Cole seu token da API do ClickUp
- Defina seu valor por hora em R$
- Clique em "Salvar Configurações"

### 4. Navegar pelos Dados

- **Períodos**: Alterne entre "Esta Semana", "Este Mês" ou "Mês Anterior"
- **Visualização**: Tarefas agrupadas por dia com horário de início
- **Ordenação**: Por data (padrão) ou por tempo gasto
- **Relatório**: Clique em "Gerar Relatório" para exportar
- **Debug**: Expanda "Dados brutos do ClickUp" para ver o JSON original

## 🔒 Segurança

- O token da API é armazenado apenas no localStorage do seu navegador
- As requisições são feitas server-side através do PHP
- Não há armazenamento permanente de dados sensíveis no servidor

## 🐛 Solução de Problemas

### Container não inicia

```bash
# Verificar logs
docker-compose logs -f

# Reconstruir imagem
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

### Porta 8080 já em uso

Edite `docker-compose.yml` e altere:

```yaml
ports:
  - "8081:80" # Usar porta 8081 ao invés de 8080
```

### "Erro ao carregar dados do ClickUp"

- Verifique se seu token da API está correto e ativo
- Confirme se você tem permissões nas tasks/projetos
- Teste o token direto na [API do ClickUp](https://clickup.com/api)

### Dados não aparecem

- Confirme se você tem time entries registrados no período selecionado
- Verifique se as tasks estão atribuídas a você no ClickUp
- Abra a seção "Dados brutos" para inspecionar o retorno da API

### Problemas de timezone

O sistema usa `America/Sao_Paulo` automaticamente. Se precisar alterar, edite a linha em `api.php`:

```php
$timezone = new DateTimeZone('America/Sao_Paulo');
```

## 📊 Arquitetura

### Backend (`api.php`)

- Recebe requisições POST com token, start_date e end_date
- Busca dados do usuário e teams via API do ClickUp
- Controla duplicatas usando IDs únicos de entrada
- Agrega entradas por task + data
- Aplica timezone America/Sao_Paulo automaticamente
- Retorna dados processados (`tasks_by_date`) e brutos (`raw_entries`)

### Frontend (`index.html`)

- SPA com Vanilla JavaScript (sem frameworks)
- TailwindCSS via CDN para estilização
- LocalStorage para token e valor/hora
- Dashboard com cards de métricas em tempo real
- Agrupamento visual de tarefas por data
- Gerador de relatórios exportáveis

### Fluxo de Dados

```
┌─────────────┐      ┌──────────┐      ┌─────────────────┐
│  Frontend   │ ───> │  api.php │ ───> │  ClickUp API v2 │
│ (Browser)   │ <─── │  (PHP)   │ <─── │   (Externo)     │
└─────────────┘      └──────────┘      └─────────────────┘
```

## 🤝 Contribuições

Contribuições são bem-vindas! Como este é um fork pessoal, sinta-se à vontade para:

- 🐛 Reportar bugs via Issues
- 💡 Sugerir melhorias e novas funcionalidades
- 🔧 Fazer fork e enviar Pull Requests
- 📝 Melhorar a documentação

### Para Contribuir

```bash
# 1. Fork este repositório
# 2. Clone seu fork
git clone https://github.com/seu-usuario/ClickTime.git

# 3. Crie uma branch
git checkout -b feature/minha-feature

# 4. Faça suas alterações e commit
git commit -m "feat: adicionar minha feature"

# 5. Push e abra um PR
git push origin feature/minha-feature
```

---

**⏱️ Desenvolvido para otimizar controle de tempo e produtividade**
