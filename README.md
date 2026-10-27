# Simulador Industrial

Simulador de uma planta industrial com monitoramento de uma rede elétrica, compressor de ar e extrusora. O projeto integra comunicação MQTT, persistência de dados e supervisão industrial via OPC UA, com visualização em tempo real e simulação de falhas.

## Arquitetura

- **Simulator:** simula os ativos industriais, publica medições via MQTT e disponibiliza um dashboard web em Flask para monitoramento e controle de falhas.
- **Middleware:** recebe as mensagens MQTT, mantém um cache em memória, armazena as medições no PostgreSQL e utiliza SQLite como buffer quando o banco está indisponível.
- **OPC UA:** disponibiliza os dados dos ativos para sistemas supervisórios.
- **Elipse E3:** permite visualizar e acompanhar o comportamento da planta industrial.

## Tecnologias

`Python` · `MQTT` · `PostgreSQL` · `SQLite` · `OPC UA` · `Flask` · `Elipse E3`

## Executando o projeto

### Pré-requisitos

- Python 3.10+
- PostgreSQL
- Mosquitto ou outro broker MQTT compatível
- Elipse E3, para utilizar o supervisório

### 1. Clone o repositório

```bash
git clone https://github.com/ViniciusAgst/supervisorio-backend.git
cd supervisorio-backend
```

### 2. Instale as dependências

```bash
python -m venv venv
```

No Windows:

```powershell
venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Configure o banco de dados

Crie o banco no PostgreSQL:

```sql
CREATE DATABASE simulador;
```

Configure as credenciais e os endereços dos serviços conforme as definições em `middleware/main.py`.

As tabelas são inicializadas pelo middleware. Se o PostgreSQL ficar indisponível, as medições podem ser armazenadas localmente no SQLite para sincronização posterior, conforme a implementação do buffer.

### 4. Inicie os serviços

Com o Mosquitto e o PostgreSQL em execução, abra terminais separados.

**Simulador:**

```powershell
cd simulator
python main.py
```

Dashboard: http://localhost:5000

**Middleware:**

```powershell
cd middleware
$env:PYTHONPATH = ".."
python main.py
```

O servidor OPC UA ficará disponível em `opc.tcp://localhost:4840`.

## Supervisório

Com o simulador e o middleware em execução, abra o projeto no Elipse E3 Studio e execute o domínio para acompanhar as medições dos ativos industriais.

