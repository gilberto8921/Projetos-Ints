
# 🖥️ Projeto: Auto-Power-On Hospitalar (BIOS/UEFI)


![Ambiente](https://img.shields.io/badge/Ambiente-Hospitalar-red)

Este repositório documenta a implementação e automação do ligamento agendado de computadores via **RTC (Real-Time Clock)** na BIOS/UEFI. O objetivo é otimizar o fluxo de trabalho da equipe de saúde, garantindo que as estações de trabalho estejam prontas para uso antes do início do plantão.

---

## 📌 Motivação
Em ambientes críticos como hospitais, o tempo de boot e carregamento de sistemas de prontuário eletrônico pode atrasar o atendimento. A configuração de **Auto-On** elimina a espera manual e permite que atualizações de segurança sejam aplicadas antes da chegada dos usuários.

## ⚙️ Métodos de Implementação

### 1. Configuração Manual (BIOS/UEFI)
Para máquinas individuais, o ajuste é feito diretamente na interface de firmware:
* **Caminho comum:** `Power Management` > `Auto On Time`
* **Frequência:** Diária ou Dias úteis.
* **Horário:** Definido conforme a escala do setor.

### 2. Automação via Fabricante (Recomendado)
Para frotas padronizadas, utilizamos ferramentas de linha de comando para aplicar a configuração em lote:

| Fabricante | Ferramenta | Comando Exemplo |
| :--- | :--- | :--- |
| **Dell** | CCTK (Dell Command) | `cctk --autoon=EveryDay --autontime=07:00:00` |
| **HP** | HP BCU | `BiosConfigUtility.exe /setconfig:"config.txt"` |
| **Lenovo** | WMI Scripts | `(Get-WmiObject -Class Lenovo_SetBiosSetting)...` |

---

## 🚀 Requisitos de Sistema (Checklist T.I.)

Para que o agendamento funcione de forma confiável, os seguintes ajustes devem ser replicados via **GPO** ou script de inicialização:

1.  **Desativar Inicialização Rápida (Windows):**
    ```cmd
    powercfg /h off
    ```
    *Nota: O Fast Startup pode impedir que a placa-mãe processe o evento RTC corretamente.*

2.  **Desativar Modo ErP (BIOS):**
    * Certifique-se de que o modo de economia profunda de energia (ErP/EuP) esteja **Disabled**, para manter o relógio interno ativo para o despertar.

3.  **Sincronização NTP:**
    * As máquinas devem estar com o horário sincronizado com o servidor do hospital para evitar atrasos no ligamento.

---

## ⚡ Considerações Elétricas e de Rede
* **Escalonamento (Staggering):** Evite ligar todos os PCs simultaneamente para prevenir picos de corrente (*inrush current*) que possam sobrecarregar os nobreaks (UPS) das alas médicas.
* **VLANs:** O método via BIOS é superior ao Wake-on-LAN em redes hospitalares segmentadas, pois não depende de pacotes mágicos atravessando roteadores.

---

## 🛠️ Como contribuir
Sinta-se à vontade para abrir uma *Issue* com novos scripts de configuração para diferentes modelos de placas-mãe encontrados no hospital.

---
**Mantido pela Equipe de T.I. Hospitalar** 🏥
