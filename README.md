<p align="center"><img src="https://github.com/AlanMarquesFerreira/FCAccess-Superlogica/assets/124633669/078d05df-a5cf-4dbe-a29f-30c9108b9cb3.png" alt="drawing" width="150"/></p>

# FCAccess-Superlogica
## Resolva o fechamento do software FCAccess Client

##### ░ By Alan Marques ░

O script e bem simples pode ser executado no  **PowerShell, CMD**. 
Esse aquivo foi criado para resolver o problema do fechamento do FCAccess client os que ficam instalados nas portarias.

### PERMISSÃO DE ADMINNISTRADOR

para executar o  precisar de permissão de **ADMINISTRADOR** recomento criar um arquivo .CMD ou .BAT
e executar como adm.

### OBSERVAÇÕES IMPORTANTES:
> **Oque é desativado no script.**

#### 1. Parada do Serviço FCAccess Central System Installation:

O serviço _`FCAccess Central System Installation`_ é interrompido para realizar as configurações necessárias.

#### 2.Desativação do Firewall:

O firewall do Windows é desativado para todos os perfis de rede _`(domínio, privado e público)`_.

#### 3. Desativação do SmartScreen:

* O recurso SmartScreen do Windows Explorer e do Microsoft Edge é desativado.
* A proteção contra phishing no Microsoft Edge é desativada.
* A proteção contra aplicativos potencialmente indesejados pelo Windows Defender é desativada.
* O SmartScreen da Microsoft Store também é desativado.
  
#### 4. Exclusão de Pasta no Windows Defender:

A pasta do _`FCAccess Client`_ é adicionada como uma exclusão no Windows Defender.

#### 5. Configuração de Regras no Firewall:

* Regras de firewall são adicionadas para permitir o tráfego de entrada e saída para o _`firstControl_gui.exe e pg_ctl.exe do PostgreSQL`_.
* Uma exceção de firewall é adicionada para a _`porta 5432`_, utilizada pelo PostgreSQL.

#### Reinício do Serviço FCAccess Central System Installation:

O serviço _`FCAccess Central System Installation`_ é reiniciado após as configurações.


___
