<p align="center"><img src="https://github.com/AlanMarquesFerreira/FCAccess-Superlogica/assets/124633669/078d05df-a5cf-4dbe-a29f-30c9108b9cb3.png" alt="drawing" width="200"/></p>

> # FCAccess-Superlogica
>> ## Resolva o fechamento do software FCAccess Client

##### ░ By Alan Marques ░

O script e bem simples pode ser executado no  **PowerShell, CMD**. 
Esse aquivo foi criado para resolver o problema do fechamento do FCAccess client os que ficam instalados nas portarias.

> ### PERMISSÃO DE ADMINISTRADOR
>
> para executar o  precisar de permissão de **ADMINISTRADOR** recomento criar um arquivo .CMD ou .BAT
> e executar como adm.

>### OBSERVAÇÕES IMPORTANTES:
> **Oque é desativado no script.**
>
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

* Regras de firewall são adicionadas para permitir o tráfego de entrada e saída para o _`firstControl_gui.exe e pg_ctl.exe`_. do PostgreSQL.
* Uma exceção de firewall é adicionada para a _`porta 5432`_, utilizada pelo PostgreSQL.

#### Reinício do Serviço FCAccess Central System Installation:

O serviço _`FCAccess Central System Installation`_ é reiniciado após as configurações.


___

># Código do script
>
```
@echo off
echo :: Parar o servico FCAccess Central System Installation
powershell -Command "Stop-Service -Name 'FCAccess Central System Installation' -ErrorAction Stop"

echo :: Verificar se o servico foi parado
if %errorlevel% neq 0 (
    echo Erro ao parar o serviço FCAccess Central System Installation.
    exit /b 1
) else (
    echo :: O servico FCAccess Central System Installation foi parado com sucesso.
)

echo :: Desativar firewall na rede
netsh advfirewall set domainprofile state off
netsh advfirewall set privateprofile state off
netsh advfirewall set publicprofile state off

echo :: Verificar se desativou
netsh advfirewall show allprofiles

echo :: Verificar aplicativos e arquivos
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer" /v "SmartScreenEnabled" /t REG_SZ /d "Off" /f

echo :: SmartScreen para Microsoft Edge
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Edge" /v "SmartScreenEnabled" /t REG_DWORD /d 0 /f

echo :: Protecao contra phishing (para Microsoft Edge)
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge" /v "SmartScreenEnabled" /t REG_DWORD /d 0 /f

echo :: Bloqueio de aplicativo potencialmente indesejado
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Defender\SmartScreen" /v "PUAProtection" /t REG_DWORD /d 0 /f

echo :: SmartScreen para Microsoft Store
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\AppHost" /v "SmartScreenEnabled" /t REG_SZ /d "Off" /f

echo :: Adicionar exclusao de pasta ao Windows Defender
powershell -Command "Add-MpPreference -ExclusionPath 'C:\Program Files (x86)\FCAccess Client'"

echo :: Verificar as exclusoes aplicadas
powershell -Command "Get-MpPreference | Select-Object -ExpandProperty ExclusionPath"

echo :: Variaveis de ambiente para os caminhos dos arquivos
set "firstControlGuiPath=C:\Program Files (x86)\FCAccess Client\firstControl_gui.exe"
set "pgCtlPath=C:\Program Files\PostgreSQL\12\bin\pg_ctl.exe"
set "port=5432"

echo :: Add firewall para firstControl_gui.exe (entrada e saida)
netsh advfirewall firewall add rule name="FC Client (Inbound)" dir=in action=allow program="%firstControlGuiPath%" enable=yes profile=any protocol=any
netsh advfirewall firewall add rule name="FC Client (Outbound)" dir=out action=allow program="%firstControlGuiPath%" enable=yes profile=any protocol=any

echo :: Add firewall bin do PostgreSQL pg_ctl.exe (entrada e saida)
netsh advfirewall firewall add rule name="PostgreSQL (Inbound)" dir=in action=allow program="%pgCtlPath%" enable=yes profile=any protocol=any
netsh advfirewall firewall add rule name="PostgreSQL (Outbound)" dir=out action=allow program="%pgCtlPath%" enable=yes profile=any protocol=any

echo :: Add firewall excecao para a porta 5432 (entrada e saida)
netsh advfirewall firewall add rule name="PostgreSQL Port (Inbound)" dir=in action=allow protocol=TCP localport=%port% enable=yes profile=any
netsh advfirewall firewall add rule name="PostgreSQL Port (Outbound)" dir=out action=allow protocol=TCP localport=%port% enable=yes profile=any

echo :: Iniciar o servico FCAccess Central System Installation
powershell -Command "Start-Service -Name 'FCAccess Central System Installation' -ErrorAction Stop"

echo :: Verificar se o servico foi iniciado
if %errorlevel% neq 0 (
    echo Erro ao iniciar o servico FCAccess Central System Installation.
    exit /b 1
) else (
    echo O servico FCAccess Central System Installation foi iniciado com sucesso.
)

echo O script foi concluido com sucesso.
pause
```




| [<img loading="lazy" src="https://avatars.githubusercontent.com/u/124633669?v=4" width=115><br><sub> Alan Marques Ferreira </sub>](https://github.com/alanmarquesferreira) |  
| :---: |
