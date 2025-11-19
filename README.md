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

## 📈 Dashboards Recomendados

O Grafana vem "limpo". Para visualizar os dados imediatamente, recomenda-se importar os seguintes Dashboards da comunidade (Menu: *Dashboards > New > Import*):

1.  **Node Exporter Full** (Monitoramento da VPS)
    * **ID:** `1860`
    * *O que mostra:* CPU Total, RAM, I/O de Disco, Rede, Uptime do servidor.

2.  **cAdvisor Exporter** (Monitoramento de Containers)
    * **ID:** `14282`
    * *O que mostra:* Uso de memória e CPU individual por container, tráfego de rede por container.

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