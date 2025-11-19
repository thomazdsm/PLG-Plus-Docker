# 📊 VPS & Docker Observability Stack

Este projeto implementa uma solução completa de monitoramento e observabilidade para servidores VPS e containers Docker, utilizando a stack moderna **PLG** (Promtail, Loki, Grafana).

A infraestrutura é gerenciada inteiramente via Docker Compose, facilitando o deploy, versionamento e migração entre servidores.

## 🏗 Arquitetura

A solução é composta por 6 serviços integrados:

| Serviço | Função | Descrição |
| :--- | :--- | :--- |
| **Grafana** | *Frontend* | Interface visual para criação de dashboards e exploração de dados. |
| **Prometheus** | *Time Series DB* | Coleta e armazena métricas numéricas (CPU, RAM, Rede). |
| **Loki** | *Log Aggregation* | Sistema de logs centralizado (similar ao Splunk/ELK, mas otimizado). |
| **Node Exporter** | *Agent (Host)* | Coleta métricas de hardware do servidor (VPS) hospedeiro. |
| **cAdvisor** | *Agent (Docker)* | Coleta métricas de consumo de recursos de cada container ativo. |
| **Promtail** | *Log Agent* | Lê os logs dos containers e do sistema e envia para o Loki. |

---

## 🚀 Começando

### Pré-requisitos

* Docker Engine
* Docker Compose (Plugin V2 ou `docker-compose` legacy)
* Git

### Instalação

1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/thomazdsm/PLG-Plus-Docker.git monitoramento
    cd monitoramento
    ```

2.  **Configure as variáveis de ambiente:**
    Crie o arquivo `.env` a partir do exemplo fornecido.
    ```bash
    cp .env.example .env
    ```
    > ⚠️ **Importante:** Edite o arquivo `.env` e altere a senha padrão (`GRAFANA_PASSWORD`) antes de subir em produção.

3.  **Inicie a stack:**
    ```bash
    docker compose up -d
    ```

4.  **Verifique os status:**
    ```bash
    docker compose ps
    ```

---

## 🖥 Acessando o Monitoramento

Após a inicialização, os serviços estarão rodando internamente. O acesso principal é feito via Grafana.

* **URL:** `http://seu-ip-vps:3000` (ou domínio configurado via Proxy Reverso)
* **Usuário:** Definido no `.env` (Padrão: `admin`)
* **Senha:** Definida no `.env`

---

## 🔌 Configuração Inicial (Pós-instalação)

Ao acessar o Grafana pela primeira vez, ele estará "vazio". Siga estes passos para conectar os dados:

### 1. Conectar Data Sources
Vá em **Connections** (ou Administration) > **Data Sources** > **Add new data source**.

* **Prometheus (Métricas):**
    * Selecione "Prometheus".
    * URL: `http://prometheus:9090` (Atenção: Use o nome do serviço Docker, não use localhost).
    * Clique em "Save & Test".

* **Loki (Logs):**
    * Selecione "Loki".
    * URL: `http://loki:3100`
    * Clique em "Save & Test".

### 2. Importar Dashboards
Para visualizar os gráficos imediatamente, importe dashboards da comunidade:
Vá em **Dashboards** > **New** > **Import**.

| ID do Dashboard | Função | Data Source Necessário |
| :--- | :--- | :--- |
| **1860** | Monitoramento do Host (VPS Completa) | Prometheus |
| **14282** | Monitoramento de Containers (Docker) | Prometheus |

### 3. Explorar Logs
Para ver os logs sem criar dashboards:
1.  Vá no menu **Explore** (ícone de bússola).
2.  Selecione **Loki** no topo da página.
3.  Use o "Label Browser" para filtrar por `container_name` e ver os logs em tempo real.

---

## 📂 Estrutura do Projeto

```text
monitoramento/
├── docker-compose.yml      # Orquestração dos serviços
├── .env                    # Variáveis sensíveis (NÃO COMMITAR)
├── .env.example            # Template de variáveis
├── grafana/                # Configurações do Grafana
├── prometheus/             # Configurações de scrape de métricas
├── loki/                   # Configurações de retenção e armazenamento de logs
└── promtail/               # Configurações de coleta de logs
```

---

## 📝 Licença

Este projeto é Open Source e distribuído sob a licença MIT.

Você é livre para usar, modificar e distribuir este setup para fins pessoais ou comerciais, sem garantia de qualquer tipo. As ferramentas utilizadas (Grafana, Prometheus, Loki, etc) possuem suas próprias licenças (geralmente AGPLv3 ou Apache 2.0), verifique a documentação oficial de cada uma para conformidade em grandes escalas.

---

**Dica**: Acesse o repositório abaixo e basta inserir no service 'grafana' a network 'caddy_network' como external, atualizar o Caddyfile e terás acesso com certificado SSL ao seu monitoramento.

```
    https://github.com/thomazdsm/Caddy.git
```