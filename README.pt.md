# IntelliRisk

Este projeto está disponível em vários idiomas.
- [한국어](README.ko.md)
- [English](README.md)

# IntelliRisk: Plataforma Inteligente de Previsão de Doenças Infecciosas

O **IntelliRisk** é uma plataforma inteligente de previsão de doenças infecciosas de ponta a ponta (End-to-End) que integra um sistema de hardware IoT com uma plataforma web para monitorar, em tempo real, o risco de surtos de dengue em regiões específicas.

Através da análise de dados ambientais (temperatura e umidade) coletados no nível do dispositivo de borda (ESP32), o sistema prevê a probabilidade de ocorrência da doença, armazena essas informações em um banco de dados e as visualiza em um painel web (dashboard). Simultaneamente, o sistema controla atuadores físicos (LEDs, LCD) na borda para sincronizar informações de alerta intuitivas diretamente no ambiente local.

---

## Componentes do Sistema e Arquitetura Tecnológica

* **Hardware e Sensores:** Placa principal `ESP32`, sensor de temperatura e umidade `DHT11`
* **Atuadores:** `LED` de alerta tricolor, `Display LCD` para exibição de status em tempo real
* **Arquitetura de Backend:** Servidor API REST baseado em `Python (Flask)`
* **Armazenamento de Dados:** `MySQL` (armazenamento de dados ambientais em série temporal e logs de risco)

---

## Principais Fluxos de Trabalho e Pipeline de Dados

### 1. Coleta e Recebimento de Dados em Tempo Real
* O sensor DHT11 integrado ao ESP32 mede periodicamente e com precisão os dados de temperatura e umidade.
* Os dados físicos coletados são transmitidos via módulo Wi-Fi para o endpoint da API REST do servidor backend Flask.

### 2. Análise de Risco e Armazenamento em Banco de Dados
* O servidor Flask executa um algoritmo de avaliação de risco de dengue com base nos dados recebidos.
* As informações sobre o nível de risco calculado são armazenadas com segurança (`Insert`) no banco de dados MySQL, junto com os logs de data e hora da coleta.

### 3. Feedback Bidirecional em Tempo Real
* **Controle de Hardware:** Dependendo do nível de risco calculado, o texto na tela LCD conectada ao ESP32 é atualizado dinamicamente, e os LEDs de alerta correspondentes a cada nível de risco são alternados em tempo real.
* **Painel de Monitoramento Web:** Administradores e usuários podem visualizar e analisar rapidamente as tendências de temperatura/umidade por região e as estatísticas de risco de surto de dengue através do painel web.

---

## Capturas de Tela do Sistema e Dashboard

<table width="100%">
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/638825a6-7b35-4d36-8c6a-6f54e289bc61" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/0316fda4-4dcb-4a0d-9499-e9d91f2c8fae" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/61cd375b-c995-4298-864b-a6bbff0a2424" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/18bcc3c1-5de0-4238-be01-ae33d8f90cfe" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/653192c4-7066-4d67-b99a-13b575ae5b11" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/57bde822-8a70-4d15-8af4-6a15fda5df34" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/e80a4598-8b3c-47c6-9b3b-41142cf1510f" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/6d0c5161-9b97-46a8-bef3-6ca369ec0bb0" width="100%"/></td>
  </tr>
</table>

---

## Membros da Equipe

* **Yejin Chung**
* **Ana Flávia**
* **Isabella Berkembrock**
* **Michele Cristina Otta**
