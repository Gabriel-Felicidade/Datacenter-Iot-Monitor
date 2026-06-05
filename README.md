# 🛡️ Monitoramento Inteligente de Data Center com IoT e Cloud Computing

![Status](https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge)
![IoT](https://img.shields.io/badge/Edge_Computing-ESP32-blue?style=for-the-badge)
![Cloud](<https://img.shields.io/badge/Cloud-FlowFuse_(Node--RED)-red?style=for-the-badge>)
![Protocol](https://img.shields.io/badge/Protocolo-MQTT-yellow?style=for-the-badge)

## 📌 1. Contexto e Problema de Negócio

A empresa **CloudTech Solutions** enfrentou recentemente um incidente crítico em uma de suas unidades operacionais remotas: o sistema de climatização falhou, causando um superaquecimento e o desligamento automático do servidor principal (_thermal shutdown_). Isso resultou em 4 horas de _downtime_ (inatividade) e severos prejuízos financeiros.

Este projeto visa solucionar esse problema aplicando o conceito de **IoT Industrial**, introduzindo um sistema de **monitoramento inteligente e autônomo** para garantir alta disponibilidade e proteção dos ativos tecnológicos da empresa.

---

## 🏗️ 2. Arquitetura da Solução

O projeto opera através de uma arquitetura modular que interliga dispositivos na borda com serviços de nuvem:

1. **Camada de Borda (Edge Computing):**
   - **Hardware:** Microcontrolador **ESP32** atuando como cérebro local.
   - **Sensores:** **DHT22** realiza a leitura de Temperatura e Umidade.
   - **Atuadores:** Relé / LED (simulando um exaustor de resfriamento).
   - **Edge Continuity:** Independência da nuvem para segurança crítica. Se a temperatura ultrapassar **28°C**, o ESP32 aciona fisicamente o resfriamento de forma imediata e autônoma, emitindo um alerta de "Modo Automático Prioritário".

2. **Camada de Comunicação (Rede e Broker):**
   - **Protocolo:** **MQTT** sobre Wi-Fi.
   - **Broker:** **HiveMQ Cloud**.
   - **Segurança:** Autenticação obrigatória com usuário e senha para evitar interceptações.

3. **Camada de Nuvem (Cloud Computing):**
   - **Plataforma:** **FlowFuse** (Node-RED operando 100% na Nuvem).
   - **Dashboard UI:** Interface moderna em _Dark Mode_ para controle visual, geração de gráficos históricos (24h) e controle manual remoto do exaustor.

---

## 📡 3. Especificação do Protocolo (MQTT)

### 📤 Telemetria (ESP32 → Nuvem)

O dispositivo publica as leituras a cada **5 segundos**.

- **Tópico:** `cloudtech/datacenter/monitor`
- **Formato do Payload (JSON):**
  ```json
  {
    "temperatura": 25.5,
    "umidade": 60.0,
    "exaustor": "OFF"
  }
  ```

### 📥 Controle Remoto (Nuvem → ESP32)

O dispositivo "assina" (subscribe) este tópico para ouvir os comandos do usuário vindos do Node-RED.

- **Tópico:** `cloudtech/datacenter/cmd`
- **Formatos Aceitos:** `ON` ou `OFF` (String pura).

---

## 🔌 4. Diagrama de Conexões Físicas (Pinout)

Caso deseje replicar este projeto fisicamente além do Wokwi, utilize as seguintes conexões lógicas definidas no firmware:

| Componente                | Pino do ESP32 | Função                                   |
| :------------------------ | :-----------: | :--------------------------------------- |
| **Sensor DHT22** (Data)   |   `GPIO 15`   | Leitura digital de dados climáticos      |
| **Relé / LED** (Exaustor) |   `GPIO 2`    | Saída digital para acionamento do cooler |

---

## ⚙️ 5. Como Executar e Replicar o Projeto

### A. Preparando o Dispositivo (Simulador Wokwi)

1. Acesse o diretório `projeto/esp32`.
2. Abra o projeto no **Wokwi** carregando os arquivos `diagram.json` (layout do circuito), `esp32.ino` (código-fonte em C++) e `libraries.txt`.
3. Certifique-se de estar logado no simulador e inicie a simulação. O ESP32 conectará automaticamente à rede Wi-Fi virtual e ao Broker MQTT.

### B. Inicializando a Nuvem (Node-RED via FlowFuse)

1. Crie uma instância no [FlowFuse](https://flowfuse.com/) para hospedar seu Node-RED.
2. Na interface do Node-RED, clique no menu superior direito (≡) e vá em **Import**.
3. Faça o upload do arquivo `flows.json` (localizado em `projeto/dashboard/`) ou cole o seu conteúdo.
4. Clique em **Deploy** para publicar as rotas no seu servidor Node-RED.
5. Acesse o link oficial do Dashboard (fornecido na sua interface FlowFuse) para visualizar o painel em tempo real!

---

_Desenvolvido como Trabalho Final para a disciplina de Cloud Computing e Internet das Coisas (IoT)._
