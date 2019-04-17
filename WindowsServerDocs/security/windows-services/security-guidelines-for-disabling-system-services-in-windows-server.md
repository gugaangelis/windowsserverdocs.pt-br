---
title: Diretrizes de segurança para serviços do sistema no Windows Server 2016
description: Diretrizes de segurança para desabilitar serviços no Windows Server 2016 com a experiência Desktop
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 5/22/2017
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 8f60d5095a3e4cebdffba5d3c02c69bc06326b3a
ms.sourcegitcommit: 68952ac7a29d0e2f07023958ad949fecc1b783b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/30/2018
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Diretrizes sobre como desativar os serviços do sistema no Windows Server 2016 com a experiência Desktop

Aplica-se a: Windows Server 2016

O sistema operacional Windows inclui muitos serviços do sistema que fornecem funcionalidade importante. Serviços diferentes têm políticas de inicialização padrão diferente: alguns são iniciados por padrão (automático), alguns quando necessário (manuais), e alguns são desabilitados por padrão e devem ser explicitamente ativados antes de serem executados. Esses padrões foram escolhidas com cuidado para cada serviço equilibrar o desempenho, a funcionalidade e segurança para os clientes em geral.

No entanto, alguns clientes corporativos podem preferir um saldo mais voltados para segurança em seus computadores com Windows e servidores, o que reduz a superfície de ataque no mínimo absoluto e, portanto, talvez queira desabilitar completamente todos os serviços que não são necessárias em seus ambientes específicos. Para os clientes, Microsoft® está fornecendo a diretriz de acompanhamento sobre quais serviços com segurança podem ser desabilitados para essa finalidade.

A orientação é para Windows Server 2016 com a experiência Desktop (a menos que usado como um substituto da área de trabalho para os usuários finais). Cada serviço no sistema é categorizado da seguinte maneira:

-   **Desative:** uma empresa focado em segurança provavelmente preferirá desabilitar esse serviço e deixarem de sua funcionalidade (veja detalhes adicionais abaixo).
- **Okey para desabilitar:** esse serviço fornece funcionalidade que é útil para alguns, mas não todas as empresas e empresas focado em segurança que não usá-lo com segurança podem desabilitá-la.
- **Não desabilite:** desativar este serviço será afeta a funcionalidade essencial ou impedir que determinadas funções ou recursos funcionem corretamente. Portanto, não deve ser desabilitada.
-  **(Não inclua orientações):** o impacto de desabilitar esses serviços não foi totalmente avaliado. Portanto, a configuração padrão desses serviços não deve ser alterada.


Os clientes podem configurar seus computadores com Windows e servidores para desabilitar serviços selecionados usando os modelos de segurança em suas políticas de grupo ou usando PowerShell automação. Em alguns casos, a diretriz inclui configurações de política de grupo específicas que desabilitar a funcionalidade do serviço diretamente, como uma alternativa para desabilitar o serviço em si.

A Microsoft recomenda que os clientes desabilitar os seguintes serviços e suas respectivas tarefas agendadas no Windows Server 2016 com a experiência Desktop:

Serviços: 
1. Gerenciador de autorização Live do Xbox
2. Xbox Live Salvar jogo

Tarefas agendadas: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(Você também pode acessar as informações em todos os serviços detalhado neste artigo exibindo a planilha do Microsoft Excel anexada: [orientação sobre como desabilitar serviços do sistema no Windows Server 2016 com a experiência Desktop](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>Desabilitar serviços não são instalados por padrão

A Microsoft recomenda contra Aplicando políticas para desabilitar os serviços que não são instalados por padrão.
-  O serviço geralmente é necessária se o recurso está instalado. Instalar o serviço ou o recurso requer direitos administrativos. Não permitir a instalação do recurso, não a inicialização do serviço.
-  Bloqueando o serviço do Microsoft Windows não parar um administrador (ou o administrador em alguns casos) instalem um equivalente de terceiros semelhante, talvez um com um risco maior de segurança.
-  Uma linha de base ou a avaliação de desempenho que desabilita um serviço do Windows não padrão (por exemplo, W3SVC) vai dar alguns auditores a impressão incorreta que a tecnologia (por exemplo, o IIS) é inerentemente insegura e nunca deve ser usada.
-  Se o recurso (e serviço) nunca são instalados, isso apenas adiciona desnecessárias em massa para a linha de base e o trabalho de verificação.

<br />
Para todos os serviços de sistema listados neste documento, as duas tabelas a seguirem oferecem uma explicação de colunas e as recomendações da Microsoft para habilitar e desabilitar serviços do sistema no Windows Server 2016 com a experiência Desktop: 
 
<br />

### <a name="explanation-of-columns"></a>Explicação das colunas

| | |
|---|---|
|**Descrição do serviço**|   Descrição do serviço, de sc.exe qdescription.|
|**Nome** |Nome da chave (interno) do serviço|
|**Instalação** |Sempre instalados: serviço está em Server Core e o servidor com a experiência de área de trabalho  <br /> Somente no Datacenter Edition: serviço está ativado Server 2016 com a experiência Desktop, mas ***não*** no núcleo do servidor |
|**StartType**  |Tipo de inicialização de serviço no Windows Server 2016|
|**Recomendação** |Microsoft recomendação/conselhos sobre como desabilitar esse serviço no Windows Server 2016 em uma implantação corporativa comum e bem gerenciada e onde o servidor não está sendo usado como uma substituição de desktop do usuário final.|
|**Comentários** |Explicação adicional|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Explicação sobre as recomendações da Microsoft

| | |
|---|---|
|**Não desabilite** |Esse serviço não deve ser desabilitado|
|**Okey desabilitar**| Esse serviço pode ser desabilitado se o recurso que dá suporte a ele não está sendo usado.|
|**Já desabilitada**|  Esse serviço está desabilitado por padrão. Não há necessidade de aplicá-los com a política|
|**Deve ser desabilitada** |Esse serviço nunca deve ser habilitado em um sistema bem gerenciado empresarial.|

<br />

As tabelas a seguir oferecem orientações a Microsoft sobre como desativar os serviços do sistema no Windows Server 2016 com a experiência Desktop:

<br />

##  <a name="activex-installer-axinstsv"></a>Instalador do ActiveX (AxInstSV)

| | |
|---|---|
|   **Descrição do serviço** |   Fornece validação de controle de conta de usuário para a instalação dos controles ActiveX da Internet e permite o gerenciamento de instalação do controle ActiveX com base nas configurações de política de grupo. Esse serviço é iniciado sob demanda e se desabilitado a instalação dos controles ActiveX se comportará acordo com as configurações do navegador padrão.    |
|   **Nome do serviço**    |   AxInstSV    |
|   **Instalação**    |   Somente no Datacenter Edition  |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Okey desabilitar   |
|   **Comentários**    |   Okey desabilitar se o recurso não necessário |


<br />

## <a name="alljoyn-router-service"></a>Serviço de roteador AllJoyn   
| | |
|---|---|
|   **Descrição do serviço** |   Rotas AllJoyn mensagens para os clientes AllJoyn locais. Se esse serviço é interrompido os clientes AllJoyn que não têm seus próprios roteadores embutidas poderão executar. |
|   **Nome do serviço**    |   AJRouter    |
|   **Instalação**    |   Somente no Datacenter Edition  |
|   **StartType**   |   Manual  |
|   **Recomendação**  | Não inclua orientações       |
|   **Comentários**    |       |
| | |

<br />

## <a name="app-readiness"></a>Preparar o aplicativo
| | |
|---|---|
**Descrição do serviço** |   Os aplicativos se prepara para uso na primeira vez que um usuário entra neste computador e ao adicionar novos aplicativos.
**Nome do serviço**    |   AppReadiness
**Instalação**    |   Somente no Datacenter Edition
**StartType**   |   Manual
**Recomendação**  |   Não desabilite
**Comentários**    |   
| | |

<br />

##  <a name="application-identity"></a>Identidade do aplicativo
| | |       
|---|---|   
**Descrição do serviço** |   Determina e verifica a identidade de um aplicativo. A desabilitação desse serviço impedirá que o AppLocker sendo aplicada.
**Nome do serviço**    |   AppIDSvc
**Instalação**    |   Sempre instalado
**StartType**   |   Manual
**Recomendação**  |Não inclua orientações    
**Comentários**    |   
|||     

<br />

##  <a name="application-information"></a>Informações do aplicativo 
| | |       
|---|---|   
|   **Descrição do serviço** |   Facilita a execução de aplicativos interativos com privilégios administrativos adicionais.  Se esse serviço for interrompido, os usuários poderão inicie aplicativos com os privilégios administrativos adicionais precisam executar tarefas de usuário desejada.
|   **Nome do serviço**    |   AppInfo
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   Dá suporte a elevação do UAC mesma área de trabalho
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Serviço de Gateway de camada de aplicativo       
| | |           
|---|---|           
|   **Descrição do serviço** |   Fornece suporte para plug-ins de protocolo de terceiros para o compartilhamento de Conexão com a Internet
|   **Nome do serviço**    |   ALG
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |Não inclua orientações    
|   **Comentários**    |   
|||     

<br />

##  <a name="application-management"></a>Gerenciamento de aplicativos      
| | |           
|---|---|       
|   **Descrição do serviço** |   Processa solicitações de instalação, remoção e enumeração de software implantado por meio da política de grupo. Se o serviço estiver desabilitado, os usuários poderão instalar, remover ou enumerar o software implantado por meio da política de grupo. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   AppMgmt
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Serviço de implantação de AppX (AppXSVC)       
| | |           
|---|---|
|   **Descrição do serviço** |   Fornece suporte à infraestrutura para implantar aplicativos da loja. Esse serviço é iniciado sob demanda e se desabilitada aplicativos da loja não serão implantados no sistema e podem não funcionar corretamente.
|   **Nome do serviço**    |   AppXSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>Atualizador de fuso horário automaticamente           
| | |           
|---|---|           
|   **Descrição do serviço** |   Define o fuso horário automaticamente.
|   **Nome do serviço**    |   tzautoupdate
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>Serviço de transferência inteligente em segundo plano          
| | |           
|---|---|   
|   **Descrição do serviço** |   Transferências de arquivos em segundo plano usando largura de banda de rede ociosa. Se o serviço está desabilitado, todos os aplicativos que dependem de BITS, como o Windows Update ou o MSN Explorer, será impossível baixar automaticamente os programas e outras informações.
|   **Nome do serviço**    |   BITS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>Serviço de infraestrutura de tarefas em segundo plano      
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço de infraestrutura do Windows que controla quais tarefas em segundo plano pode ser executado no sistema.
|   **Nome do serviço**    |   BrokerInfrastructure
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>Mecanismo de filtragem básica            
| | |           
|---|---|       
|   **Descrição do serviço** |   O mecanismo de filtragem Base (BFE) é um serviço que gerencia firewall e políticas de segurança (IPsec) do protocolo de Internet e implementa a filtragem de modo de usuário. Interromper ou desabilitar o serviço BFE reduz significativamente a segurança do sistema. Isso também resultará em um comportamento imprevisível no gerenciamento de IPsec e aplicativos de firewall.
|   **Nome do serviço**    |   BFE
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>Serviço de suporte para Bluetooth            
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço de Bluetooth oferece suporte a descoberta e associação de dispositivos Bluetooth remotos.  Interromper ou desabilitar esse serviço pode fazer com que dispositivos Bluetooth já instalados falhar funcionar corretamente e impedir que novos dispositivos descobertos ou associados.
|   **Nome do serviço**    |   BthServ
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Okey desabilitar se não usadas. Outro mecanismo desabilitantes:https://technet.microsoft.com/en-us/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço de usuário é usado para cenários de plataforma de dispositivos conectados
|   **Nome do serviço**    |   CDPUserSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          


##  <a name="certificate-propagation"></a>Propagação de certificado     
| | |           
|---|---|
|   **Descrição do serviço** |   Copia os certificados de usuário e certificados raiz de cartões inteligentes para o repositório de certificados do usuário atual, detecta quando um cartão inteligente é inserido em um leitor de cartão inteligente e se necessário, instala o minidriver de Plug and Play de cartão inteligente.
|   **Nome do serviço**    |   CertPropSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>Serviço de licença do cliente (ClipSVC)        
| | |           
|---|---|   
|   **Descrição do serviço** |   Dá suporte a infraestrutura da Microsoft Store. Esse serviço é iniciado sob demanda e se comprou aplicativos desabilitados usar Microsoft Store não se comportará corretamente.
|   **Nome do serviço**    |   ClipSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>Isolamento de chave CNG
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço de isolamento de chave CNG é hospedado no processo da LSA. O serviço fornece isolamento do processo de chave para chaves privadas e operações criptográficas associadas conforme exigido pelos critérios comuns. O serviço armazena e usa chaves de longa duração em um processo seguro estar em conformidade com os requisitos de critérios comuns.
|   **Nome do serviço**    |   KeyIso
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>Sistema de eventos COM+       
| | |           
|---|---|       
|   **Descrição do serviço** |   Dá suporte a sistema evento de notificação de serviço (SENS), que fornece a distribuição automática de eventos para assinar os componentes de modelo COM (Component Object). Se o serviço for interrompido, SENS fechará e não será capaz de fornecer notificações de logon e logoff. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   EventSystem
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>Aplicativo de sistema COM+     
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia a configuração e o rastreamento de modelo COM (Component Object) + componentes baseados em. Se o serviço for interrompido, a maioria dos COM+-componentes baseados em não funcionará corretamente. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   COMSysApp
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>Localizador de computadores        
| | |           
|---|---|       
|   **Descrição do serviço** |   Mantém uma lista atualizada de computadores na rede e fornece essa lista para computadores designados navegadores. Se esse serviço for interrompido, essa lista não será atualizada ou mantida. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   Navegador
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>Serviço de plataforma de dispositivos conectados       
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço é usado para cenários de dispositivos conectados e vidro Universal
|   **Nome do serviço**    |   CDPSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Experiências do usuário conectado e telemetria     
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de experiências de usuário conectado e telemetria habilita os recursos que dão suporte a experiências do usuário no aplicativo e conectada. Além disso, esse serviço gerencia a coleção controlado por eventos e a transmissão de informações de diagnóstico e uso (usadas para melhorar a experiência e a qualidade da plataforma do Windows) quando o diagnóstico e as configurações de opção de privacidade de uso estão habilitadas em comentários e diagnóstico.
|   **Nome do serviço**    |   DiagTrack
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>Dados de contato        
| | |           
|---|---|       
|   **Descrição do serviço** |   Dados de contato de índices para pesquisar rapidamente contatos. Se você para ou desativar este serviço, contatos estejam faltando em seus resultados de pesquisa.
|   **Nome do serviço**    |   PimIndexMaintenanceSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="coremessaging"></a>CoreMessaging            
| | |           
|---|---|           
|   **Descrição do serviço** |   Gerencia a comunicação entre componentes do sistema.
|   **Nome do serviço**    |   CoreMessagingRegistrar
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>Gerenciador de credenciais           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece armazenamento seguro e recuperação de credenciais de usuários, aplicativos e pacotes de serviço de segurança.
|   **Nome do serviço**    |   VaultSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>Serviços de criptografia           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece três serviços de gerenciamento: serviço de banco de dados de catálogo, que confirma as assinaturas dos arquivos do Windows e permite que novos programas sejam instalados; Serviço de raiz protegida, que adiciona e remove certificados de autoridade de certificação raiz confiáveis deste computador; e o serviço de atualização automática do certificado raiz, que recupera certificados raiz do Windows Update e habilita cenários como SSL. Se esse serviço for interrompido, esses serviços de gerenciamento não funcionará corretamente. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   CryptSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>Serviço de compartilhamento de dados         
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece dados intermediar entre aplicativos.
|   **Nome do serviço**    |   DsSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço DCP (a coleta de dados e publicação) dá suporte a aplicativos de terceiros para carregar dados na nuvem.
|   **Nome do serviço**    |   DcpSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>Iniciador de processo de servidor DCOM         
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço DCOMLAUNCH inicia DCOM e COM servidores em resposta a solicitações de ativação de objeto. Se esse serviço for interrompido ou desabilitado, programas usando COM ou DCOM não funcionará corretamente. É altamente recomendável que você tenha a execução de serviço DCOMLAUNCH.
|   **Nome do serviço**    |   DcomLaunch
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  |Não inclua orientações    
|   **Comentários**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>Serviço de associação de dispositivo      
| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita a emparelhamento entre o sistema e os dispositivos com ou sem fio.
|   **Nome do serviço**    |   DeviceAssociationService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>Serviço de instalação de dispositivo      
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que um computador para reconhecer e se adaptar a alterações de hardware com pouca ou nenhuma entrada do usuário. Interromper ou desabilitar esse serviço resultará em instabilidade do sistema.
|   **Nome do serviço**    |   DeviceInstall
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>Serviço de registro de gerenciamento de dispositivo        
| | |           
|---|---|       
|   **Descrição do serviço** |   Executa as atividades de registro do dispositivo para gerenciamento de dispositivos
|   **Nome do serviço**    |   DmEnrollmentSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>Gerenciador de instalação de dispositivo         
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite a detecção, download e instalação de software relacionados ao dispositivo. Se esse serviço está desabilitado, dispositivos podem ser configurados com software desatualizado e podem não funcionar corretamente.
|   **Nome do serviço**    |   DsmSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>Agente de descoberta de DevQuery em segundo plano         
| | |           
|---|---|           
|   **Descrição do serviço** |   Permite que os aplicativos descobrir dispositivos com uma tarefa de fundo
|   **Nome do serviço**    |   DevQueryBroker
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>Cliente DHCP          
| | |           
|---|---|       
|   **Descrição do serviço** |   Registra e atualiza os registros DNS para esse computador e endereços IP. Se esse serviço é interrompido, este computador não receberão atualizações DNS e endereços IP dinâmicos. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   DHCP
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>Serviço de política de diagnóstico            
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de diretiva de diagnóstico permite a detecção de problemas, solução de problemas e resolução de componentes do Windows.  Se esse serviço for interrompido, diagnóstico deixará de funcionar.
|   **Nome do serviço**    |   DPS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>Host do serviço de diagnóstico     
| | |           
|---|---|       
|   **Descrição do serviço** |   O Host do serviço de diagnóstico é usado pelo serviço de política de diagnóstico para diagnósticos do host que precisam ser executados em um contexto de serviço Local.  Se esse serviço for interrompido, qualquer diagnóstico depende dele não funcionarão.
|   **Nome do serviço**    |   WdiServiceHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>Host de diagnóstico do sistema           
| | |           
|---|---|       
|   **Descrição do serviço** |   O Host diagnóstico do sistema é usado pelo serviço de política de diagnóstico para diagnósticos do host que precisam ser executados em um contexto de sistema Local.  Se esse serviço for interrompido, qualquer diagnóstico depende dele não funcionarão.
|   **Nome do serviço**    |   WdiSystemHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>Cliente de rastreamento de Link distribuído            
| | |           
|---|---|   
|   **Descrição do serviço** |   Mantém vínculos entre arquivos NTFS em um computador ou em todos os computadores em uma rede.
|   **Nome do serviço**    |   TrkWks
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>Coordenador de transações distribuídas     
| | |           
|---|---|   
|   **Descrição do serviço** |   Coordena transações que incluem vários gerenciadores de recursos, como bancos de dados, filas de mensagens e sistemas de arquivos. Se esse serviço for interrompido, essas transações falhará. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   MSDTC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **Descrição do serviço** |   Serviço de roteamento de mensagem do WAP Push
|   **Nome do serviço**    |   dmwappushservice
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Serviço necessário em dispositivos cliente para Intune, MDM e tecnologias semelhantes de gerenciamento e para o filtro de gravação unificado. Não é necessário para o servidor.
|||         
            
<br />      

##  <a name="dns-client"></a>Cliente DNS      
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de cliente DNS (dnscache) armazena em cache o sistema de nomes de domínio (DNS) e registra o nome completo do computador para este computador. Se o serviço for interrompido, nomes DNS continuará a ser resolvidos. No entanto, os resultados de consultas de nomes DNS serão não ser armazenados em cache e o nome do computador não será registrado. Se o serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   DnsCache
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>Gerenciador de mapas baixados     
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço do Windows para acessar aplicativo mapas baixados. Esse serviço é iniciado sob demanda por aplicativo acessando mapas baixados. A desabilitação desse serviço impedirá que aplicativos acessem mapas.
|   **Nome do serviço**    |   MapsBroker
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Desabilitando quebras aplicativos que dependem do serviço; Okey desabilitar se aplicativos não contando com ele
|||         
            
<br />          

## <a name="embedded-mode"></a>Modo incorporado            
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de modo incorporado permite cenários relacionados a aplicativos em segundo plano.  Desabilitar esse serviço evitará de aplicativos em segundo plano sendo ativado.
|   **Nome do serviço**    |   embeddedmode
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>Encrypting File System (EFS)
| | |                   
|---|---|           
|   **Descrição do serviço** | Fornece a tecnologia de criptografia de arquivo principal usada para armazenar arquivos criptografados em volumes do sistema de arquivos NTFS. Se esse serviço for interrompido ou desabilitado, os aplicativos não poderão acessar arquivos criptografados.            
|   **Nome do serviço**  |  EFS            
|   **Instalação**  |  Sempre instalado           
|   **StartType**   |  Manual           
|   **Recomendação**  | Não inclua orientações           
|   **Comentários**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>Serviço de gerenciamento de aplicativo empresarial            
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite o gerenciamento de aplicativo empresarial.
|   **Nome do serviço**    |   EntAppSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>Protocolo de autenticação extensível           
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço EAP Extensible Authentication Protocol () fornece autenticação de rede em situações como 802.1 x com e sem fio, VPN e proteção de acesso à rede (NAP).  EAP também fornece aplicativo interfaces de programação (APIs) que é usado pelos clientes de acesso de rede, incluindo sem fio e clientes VPN, durante o processo de autenticação.  Se você desabilitar esse serviço, este computador é impedido de acessar redes que requerem autenticação EAP.
|   **Nome do serviço**    |   EapHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>Host de provedor de descoberta de função         
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço FDPHOST hospeda os provedores de descoberta de rede de descoberta de função (FD). Esses provedores FD fornecem serviços de descoberta de rede para os serviços SSDP Simple Discovery Protocol () e serviços Web - protocolo RDP (WS-D). Interromper ou desabilitar o serviço FDPHOST desabilitará a descoberta de rede para esses protocolos ao usar FD. Quando esse serviço não estiver disponível, serviços de rede usando FD e contar com esses protocolos de descoberta conseguirá localizar dispositivos de rede ou recursos.
|   **Nome do serviço**    |   fdPHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>Publicação de recursos de descoberta de função      
| | |           
|---|---|       
|   **Descrição do serviço** |   Publica este computador e os recursos anexados a este computador para que eles podem ser descobertos pela rede.  Se esse serviço for interrompido, recursos de rede não serão publicados e eles não serão descobertos por outros computadores na rede.
|   **Nome do serviço**    |   FDResPub
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>Serviço de localização geográfica          
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço monitora a localização atual do sistema e gerencia as cercas geográficas (uma localização geográfica com eventos associados).  Se você desativar esse serviço, aplicativos não poderão usar ou receber notificações de localização geográfica ou cercas geográficas.
|   **Nome do serviço**    |   lfsvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Desabilitando quebras aplicativos que dependem do serviço; Okey desabilitar se aplicativos não contando com ele
|||         
            
<br />          

##  <a name="group-policy-client"></a>Cliente de política de grupo     
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço é responsável por aplicar configurações definidas por administradores para o computador e os usuários por meio do componente de política de grupo. Se o serviço for desativado, as configurações não serão aplicadas e aplicativos e componentes não poderão ser gerenciados pela política de grupo. Quaisquer componentes ou aplicativos que dependem do componente de política de grupo podem não ser funcionais se o serviço está desabilitado.
|   **Nome do serviço**    |   gpsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>Serviço de dispositivo de Interface Humana           
| | |           
|---|---|       
|   **Descrição do serviço** |   Ativa e mantém o uso dos botões ativados em teclados, controles remotos e outros dispositivos de multimídia. É recomendável que você mantenha a execução desse serviço.
|   **Nome do serviço**    |   Hidserv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>Serviço de Host HV     
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece uma interface para o hipervisor do Hyper-V fornecer os contadores de desempenho por meio de partições para o sistema operacional host.
|   **Nome do serviço**    |   HvHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Enhancers de desempenho para convidadas. Não é usado hoje, exceto para explicitamente preenchido VMs, mas será usada no aplicativo Guard
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Serviço de troca de dados do Hyper-V        
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece um mecanismo para trocar dados entre a máquina virtual e o sistema operacional em execução no computador físico.
|   **Nome do serviço**    |   vmickvpexchange
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Interface de serviço de convidado Hyper-V          
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece uma interface para o host do Hyper-V interagir com serviços específicos executado na máquina virtual.
|   **Nome do serviço**    |   vmicguestinterface
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Serviço de desligamento de convidado Hyper-V           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um mecanismo para desligar o sistema operacional dessa máquina virtual das interfaces de gerenciamento no computador físico.
|   **Nome do serviço**    |   vmicshutdown
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Serviço de pulsação Hyper-V            
| | |           
|---|---|           
|   **Descrição do serviço** |   Monitora o estado dessa máquina virtual relatando uma pulsação em intervalos regulares. Esse serviço ajuda você a identificar máquinas virtuais em execução que pararam de responder.
|   **Nome do serviço**    |   vmicheartbeat
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-powershell-direct-service"></a>Serviço do Hyper-V PowerShell direta            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um mecanismo para gerenciar a máquina virtual com o PowerShell por meio de sessão VM sem uma rede virtual.
|   **Nome do serviço**    |   vmicvmsession
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Serviço de virtualização da área de trabalho remota do Hyper-V            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece uma plataforma para comunicação entre a máquina virtual e o sistema operacional em execução no computador físico.
|   **Nome do serviço**    |   vmicrdv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Serviço de sincronização de tempo do Hyper-V         
| | |           
|---|---|       
|   **Descrição do serviço** |   Sincroniza a hora do sistema desta máquina virtual com a hora do sistema do computador físico.
|   **Nome do serviço**    |   vmictimesync
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Solicitante de cópia de sombra de Volume do Hyper-V         
| | |           
|---|---|           
|   **Descrição do serviço** |   Coordenadas as comunicações que são necessárias para usar o serviço de cópias de sombra de Volume para fazer backup de aplicativos e dados nessa máquina virtual do sistema operacional no computador físico.
|   **Nome do serviço**    |   vmicvss
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IKE e AuthIP IPsec módulos de criação          
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço IKEEXT hospeda a Internet Key Exchange (IKE) e autenticado Internet protocolo Authenticated módulos de criação. Esses módulos são usados para autenticação e troca de chaves no protocolo IPSec (IPsec). Interromper ou desabilitar o serviço IKEEXT desabilitará troca de chaves IKE e AuthIP com computadores pares. IPsec geralmente é configurado para usar IKE ou AuthIP; Portanto, interromper ou desabilitar o serviço IKEEXT pode resultar em uma falha de IPsec e pode comprometer a segurança do sistema. É altamente recomendável que você tenha a execução de serviço IKEEXT.
|   **Nome do serviço**    |   IKEEXT
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>Detecção de serviços interativos           
| | |           
|---|---|   
|   **Descrição do serviço** |   Habilita a notificação do usuário do usuário de entrada para serviços interativos, que permite o acesso a caixas de diálogo criados por serviços interativos quando ele aparecer. Se esse serviço for interrompido, notificações de caixas de diálogo Novo serviço interativo deixarão de funcionar e não pode haver acesso a caixas de diálogo de serviço interativo. Se esse serviço está desabilitado, notificações de e o acesso a caixas de diálogo Novo serviço interativo deixarão de funcionar.
|   **Nome do serviço**    |   UI0Detect
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>Conexão de Internet (ICS) de compartilhamento            
| | |           
|---|---|           
|   **Descrição do serviço** |   Fornece conversão de endereços de rede, endereçamento, serviços de prevenção de resolução e/ou invasões nomes para uma rede doméstica ou de pequena empresa.
|   **Nome do serviço**    |   SharedAccess
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Obrigatório para que os clientes usados como hotspots Wi-Fi e também nas duas extremidades do projeção Miracast. ICS podem ser bloqueados com a configuração do GPO, "Proibir o uso do compartilhamento de Conexão de Internet em sua rede de domínio DNS"
|||         
            
<br />          

## <a name="ip-helper"></a>Auxiliar de IP            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece conectividade de túnel usando tecnologias de transição IPv6 (6to4, Teredo, Proxy de porta e ISATAP) e IP-HTTPS. Se esse serviço for interrompido, o computador não terá os benefícios de conectividade aprimorada que oferecem essas tecnologias.
|   **Nome do serviço**    |   iphlpsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>Agente de política IPsec      
| | |           
|---|---|       
|   **Descrição do serviço** |   Protocolo de Internet segurança (IPsec) oferece suporte à autenticação de par de nível de rede, autenticação de origem de dados, integridade de dados, a confidencialidade de dados (criptografia) e proteção de repetição.  Esse serviço impõe políticas IPsec criadas por meio do snap-in de políticas de segurança IP ou a ferramenta de linha de comando "netsh ipsec".  Se você parar esse serviço, você poderá ter problemas de conectividade de rede se sua política requer que as conexões usem IPsec.  Além disso, o gerenciamento remoto do Firewall do Windows não está disponível quando esse serviço é interrompido.
|   **Nome do serviço**    |   PolicyAgent
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>Serviço de servidor de Proxy KDC (KPS)      
| | |           
|---|---|       
|   **Descrição do serviço** |   Servidor Proxy de KDC serviço é executado em servidores de borda para proxy Kerberos mensagens de protocolo para controladores de domínio na rede corporativa.
|   **Nome do serviço**    |   KPSSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KtmRm para coordenador de transações distribuídas            
| | |           
|---|---|       
|   **Descrição do serviço** |   Coordenadas transações entre a Distributed Transaction coordenador (MSDTC) e a transação Manager KTM (Kernel). Se não for necessário, é recomendável que parou de manter esse serviço. Se for necessário, MSDTC e KTM iniciará esse serviço automaticamente. Se esse serviço está desabilitado, qualquer transação MSDTC interagindo com um Gerenciador de recursos do Kernel falhará e todos os serviços que dependem explicitamente dele não serão iniciado.
|   **Nome do serviço**    |   KtmRm
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Mapeador de descoberta de topologia de camada de link        
| | |       
|---|---|       
|   **Descrição do serviço** |   Cria um mapa de rede, consiste em computador e informações de topologia (conectividade) do dispositivo e metadados que descrevem cada computador e o dispositivo.  Se esse serviço está desabilitado, o mapa de rede não funcionará corretamente.
|   **Nome do serviço**    |   lltdsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Okey desabilitar se nenhuma dependência em mapa de rede
|||         
            
<br />

## <a name="local-session-manager"></a>Gerenciador de sessão local                    
| | |                   
|---|---|   
|   **Descrição do serviço** |   Principais serviço do Windows que gerencia as sessões de usuário local. Interromper ou desabilitar esse serviço resultará em instabilidade do sistema.    
|   **Nome do serviço**    |   LSM |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Automático   |
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Coletor padrão do Hub de diagnóstico de Microsoft (R)         
| | |           
|---|---|           
|   **Descrição do serviço** |   Principais serviço do Windows que gerencia as sessões de usuário local. Interromper ou desabilitar esse serviço resultará em instabilidade do sistema.
|   **Nome do serviço**    |   LSM
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Assistente de entrada da conta da Microsoft          
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que usuário entrar por meio dos serviços de identidade de conta Microsoft. Se esse serviço for interrompido, os usuários não poderão fazer logon no computador com sua conta da Microsoft.
|   **Nome do serviço**    |   wlidsvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Accounts da Microsoft são n/d no Windows Server
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Cliente de App-V da Microsoft      
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia usuários de App-V e aplicativos virtuais
|   **Nome do serviço**    |   AppVClient
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Serviço iniciador Microsoft iSCSI            
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as sessões de Internet SCSI (iSCSI) deste computador para dispositivos de destino iSCSI remotos. Se esse serviço for interrompido, este computador não poderão fazer logon ou acessar destinos iSCSI. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   MSiSCSI
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Nossos dados de diagnóstico indicam que isso é usado no cliente, bem como servidor. Nenhum benefício para desativá-lo.
|||         
            
<br />          

## <a name="microsoft-passport"></a>O Microsoft Passport           
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece o isolamento do processo de chaves de criptografia usado para autenticar a provedores de identidade associados do usuário. Se esse serviço está desabilitado, todos os usos e gerenciamento dessas teclas não estarão disponíveis, que inclui máquina logon e logon único em aplicativos e sites. Esse serviço inicia e para automaticamente. É recomendável que você não reconfigurar esse serviço.
|   **Nome do serviço**    |   NgcSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Necessário para logons de FIXAR/Hello, que não têm suporte no servidor
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Contêiner do Microsoft Passport         
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia chaves de identidade do usuário local usadas para autenticar o usuário a provedores de identidade, bem como cartões inteligentes virtuais TPM. Se esse serviço está desabilitado, chaves de identidade do usuário locais e cartões inteligentes virtuais TPM não estarão acessíveis. É recomendável que você não reconfigurar esse serviço.
|   **Nome do serviço**    |   NgcCtnrSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Provedor de cópia de sombra de Software da Microsoft          
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia cópias de sombra de volume baseada em software criadas pelo serviço de cópia de sombra de Volume. Se esse serviço for interrompido, as cópias de sombra de volume baseadas em software não podem ser gerenciadas. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   Swprv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>Espaços de armazenamento do Microsoft SMP         
| | |           
|---|---|       
|   **Descrição do serviço** |   Serviço de host para o provedor de gerenciamento de espaços de armazenamento da Microsoft. Se esse serviço for interrompido ou desabilitado, espaços de armazenamento não podem ser gerenciados.
|   **Nome do serviço**    |   smphost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Gerenciamento de armazenamento APIs falharem sem esse serviço. Exemplo: "Get-WmiObject-classe MSFT_Disk - Namespace Root\Microsoft\Windows\Storage".
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Serviço de compartilhamento de porta NET         
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a capacidade de compartilhar portas TCP no protocolo NET.
|   **Nome do serviço**    |   NetTcpPortSharing
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Logon de rede         
| | |           
|---|---|           
|   **Descrição do serviço** |   Mantém um canal seguro entre esse computador e o controlador de domínio para autenticar usuários e serviços. Se esse serviço for interrompido, o computador não pode autenticar usuários e serviços e o controlador de domínio não podem registrar registros DNS. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   Logon de rede
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>Agente de Conexão de rede            
| | |           
|---|---|       
|   **Descrição do serviço** |   Conexões de agentes que permitem aos aplicativos do Microsoft Store receber notificações da internet.
|   **Nome do serviço**    |   NcbService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>Conexões de rede         
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia objetos na pasta Conexões de rede e Dial-Up, no qual você pode exibir conexões remotas e rede local.
|   **Nome do serviço**    |   NETMAN
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>Assistente de conectividade de rede      
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece uma notificação de status DirectAccess para componentes de interface do usuário
|   **Nome do serviço**    |   NcaSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>Serviço de lista de rede        
| | |           
|---|---|   
|   **Descrição do serviço** |   Identifica as redes à qual o computador tenha se conectado, coleta e armazena as propriedades dessas redes e notifica os aplicativos quando essas propriedades forem alteradas.
|   **Nome do serviço**    |   netprofm
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>Reconhecimento de local de rede           
| | |           
|---|---|       
|   **Descrição do serviço** |   Coleta e armazena informações de configuração para a rede e notifica os programas quando essas informações são modificadas. Se esse serviço for interrompido, informações de configuração podem estar indisponíveis. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   NlaSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>Serviço de configuração de rede       
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de configuração de rede gerencia a instalação de drivers de rede e permite que a configuração das configurações de rede de baixo nível.  Se esse serviço for interrompido, qualquer instalação de driver que estão em andamento pode ser cancelada.
|   **Nome do serviço**    |   NetSetupSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Serviço de Interface de rede Store      
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço envia notificações de rede (por exemplo, interface adição/exclusão etc) para clientes de modo de usuário. Parar esse serviço causará a perda de conectividade de rede. Se esse serviço está desabilitado, todos os outros serviços que dependem explicitamente esse serviço falhará na tela inicial.
|   **Nome do serviço**    |   NSI
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="offline-files"></a>Arquivos offline            
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de arquivos Offline realiza atividades de manutenção no cache Arquivos Offline, responde logoff e logon de usuário eventos, implementa os internos da API pública e despacha eventos interessantes para aqueles interessados em atividades de arquivos Offline e as alterações no estado do cache.
|   **Nome do serviço**    |   CscService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>Otimizar unidades          
| | |           
|---|---|   
|   **Descrição do serviço** |   Ajuda do computador executados com mais eficiência por meio da otimização arquivos em unidades de armazenamento.
|   **Nome do serviço**    |   defragsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>Host DLL do contador de desempenho         
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que usuários remotos e processos de 64 bits consultar contadores de desempenho fornecidos por DLLs de 32 bits. Se esse serviço for interrompido, somente os usuários locais e processos de 32 bits poderá consultar contadores de desempenho fornecidos por DLLs de 32 bits.
|   **Nome do serviço**    |   PerfHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>Alertas e Logs de desempenho            
| | |           
|---|---|   
|   **Descrição do serviço** |   Logs de desempenho e alertas coleta dados de desempenho de computadores locais ou remotos com base nos parâmetros de agendamento pré-configurados, em seguida, grava os dados em um log ou dispara um alerta. Se esse serviço for interrompido, informações sobre o desempenho não será coletado. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   PLA
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>Serviço de telefone       
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia o estado de telefonia no dispositivo
|   **Nome do serviço**    |   PhoneSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Usado por aplicativos de VoIP modernos
|||         
            
<br />          

##      <a name="plug-and-play"></a>Plug and Play       
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que um computador para reconhecer e se adaptar a alterações de hardware com pouca ou nenhuma entrada do usuário. Interromper ou desabilitar esse serviço resultará em instabilidade do sistema.
|   **Nome do serviço**    |   PlugPlay
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>Serviço enumerador de dispositivos portáteis           
| | |           
|---|---|       
|   **Descrição do serviço** |   Impõe a política de grupo para dispositivos removíveis de armazenamento em massa. Permite que os aplicativos, como o Windows Media Player e o Assistente para importação de imagem para transferir e sincronizar conteúdo usando dispositivos removíveis de armazenamento em massa.
|   **Nome do serviço**    |   WPDBusEnum
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="power"></a>Consumo de energia            
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia a entrega de notificação de política de energia e política de energia.
|   **Nome do serviço**    |   Consumo de energia
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>Spooler de impressão            
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço faz spool trabalhos de impressão e manipula a interação com a impressora.  Se você desativar esse serviço, não será capaz de imprimir ou ver suas impressoras.
|   **Nome do serviço**    |   Spooler
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  |   Okey desabilitar se não um servidor de impressão ou um controlador de domínio
|   **Comentários**    |   Em um controlador de domínio, a instalação da função DC adiciona um thread para o serviço de spooler é responsável por executar impressa remoção – remoção dos objetos de fila de impressão obsoletos do Active Directory.  Se o serviço de spooler não está em execução em pelo menos um controlador de domínio em cada site, em seguida, o anúncio não tem meios para remover filas antigas que não existem mais. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>Notificações e extensões de impressora        
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço abre caixas de diálogo de impressora personalizado e manipula notificações de um servidor de impressão remoto ou uma impressora. Se você desativar esse serviço, você não conseguirá ver notificações ou extensões de impressora.
|   **Nome do serviço**    |   PrintNotify
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar, senão um servidor de impressão
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Relatórios de problemas e soluções suporte ao painel de controle     
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço fornece suporte para exibição, o envio e a exclusão de relatórios de problemas de nível do sistema para o painel de controle relatórios de problemas e soluções.
|   **Nome do serviço**    |   wercplsupport
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>Serviço de auxiliar de compatibilidade do programa     
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço fornece suporte para o programa de compatibilidade PCA (auxiliar).  PCA monitora programas instalados e executados pelo usuário e detecta problemas de compatibilidade conhecidos. Se esse serviço for interrompido, PCA não funcionará corretamente.
|   **Nome do serviço**    |   PcaSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>Qualidade Windows áudio/vídeo      
| | |           
|---|---|   
|   **Descrição do serviço** |   Qualidade Windows Audio Video Experience (qWave) é uma plataforma de rede para áudio vídeo (AV) streaming aplicativos em redes domésticas IP. qWave aprimora AV para streaming de desempenho e confiabilidade, garantindo a rede qualidade de serviço (QoS) para aplicativos de AV. Ele fornece mecanismos para um controle para tanto, execute o monitoramento de tempo e imposição, comentários sobre aplicativos e priorização de tráfego.
|   **Nome do serviço**    |   QWAVE
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Serviço de QoS do lado do cliente
|||         
            
<br />          

##      <a name="radio-management-service"></a>Serviço de gerenciamento de rádio        
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerenciamento de rádio e serviço de modo avião
|   **Nome do serviço**    |   RmSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Gerenciador de Conexão de acesso remoto automático            
| | |           
|---|---|   
|   **Descrição do serviço** |   Cria uma conexão a uma rede remota sempre que um programa faz referência a um nome DNS ou NetBIOS ou endereço remoto.
|   **Nome do serviço**    |   RasAuto
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>Gerenciador de Conexão de acesso remoto         
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia conexões discadas e rede virtual privada (VPN) deste computador à Internet ou outras redes remotas. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   RasMan
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>Configuração da área de trabalho remota         
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço de configuração de área de trabalho remoto (RDCS) é responsável por todos os serviços de área de trabalho remota e área de trabalho remota configuração relacionados e atividades de manutenção de sessão que exigem contexto do sistema. Isso inclui pastas temporárias por sessão, temas de área de trabalho remota e certificados de área de trabalho remota.
|   **Nome do serviço**    |   SessionEnv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>Serviços de área de trabalho remota          
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que os usuários se conectem interativamente a um computador remoto. Área de trabalho e servidor Host de sessão de área de trabalho remota dependem desse serviço.  Para impedir a utilização remota deste computador, desmarque as caixas de seleção na guia remoto do item do painel de controle de propriedades do sistema.
|   **Nome do serviço**    |   Serviço de terminal
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirecionador porta de modo do usuário de serviços de área de trabalho remota        
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que o redirecionamento de impressoras/unidades/portas para conexões RDP
|   **Nome do serviço**    |   UmRdpService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Dá suporte a redirecionamentos no lado do servidor da conexão.
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>Chamada de procedimento remoto (RPC)          
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço RPCSS está o Gerenciador de controle de serviço para servidores DCOM e COM. Ele executa solicitações de ativações de objeto, resoluções de exportador de objeto e coleta de lixo distribuído para servidores DCOM e COM. Se esse serviço for interrompido ou desabilitado, programas usando COM ou DCOM não funcionará corretamente. É altamente recomendável que você tenha o RPCSS serviço em execução.
|   **Nome do serviço**    |   RpcSs
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Procedimento remoto (RPC) chamada localizador             
| | |               
|---|---|   
|   **Descrição do serviço** |   No Windows 2003 e versões anteriores do Windows, o serviço localizador de chamada de procedimento remoto (RPC) gerencia o banco de dados de serviço de nomes RPC. No Windows Vista e versões posteriores do Windows, esse serviço não fornece nenhuma funcionalidade e está presente para compatibilidade de aplicativos.   |
|   **Nome do serviço**    |   RpcLocator  |
|   **Instalação**    |   Somente no Datacenter Edition  |
|   **StartType**   |   Manual  |
|   **Recomendação**  | Não inclua orientações   |
|   **Comentários**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>Registro remoto          
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que usuários remotos modificar as configurações do registro neste computador. Se esse serviço for interrompido, o registro pode ser modificado somente por usuários neste computador. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   RemoteRegistry
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>Conjunto de provedor de diretivas resultante            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um serviço de rede que processa solicitações para simular a aplicação das configurações de política de grupo para um usuário de destino ou o computador em várias situações e calcula as configurações de conjunto de políticas resultante.
|   **Nome do serviço**    |   RSoPProv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |Não inclua orientações    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>Roteamento e acesso remoto            
| | |           
|---|---|   
|   **Descrição do serviço** |   Oferece serviços de roteamento para empresas na rede local e ambientes de rede de longa distância.
|   **Nome do serviço**    |   Acesso remoto
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   Já desabilitada
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>Mapeador          
| | |           
|---|---|   
|   **Descrição do serviço** |   Resolve identificadores de interfaces RPC para pontos de extremidade de transporte. Se esse serviço for interrompido ou desabilitado, programas usando os serviços de chamada de procedimento remoto (RPC) não funcionará corretamente.
|   **Nome do serviço**    |   RpcEptMapper
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>Logon secundário     
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite iniciar processos sob credenciais alternativas. Se esse serviço for interrompido, esse tipo de acesso de logon estará disponível. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   seclogon
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Serviço de protocolo de encapsulamento de soquete segura            
| | |               
|---|---|       
|   **Descrição do serviço** |   Fornece suporte para o segura soquete encapsulamento SSTP (Protocol) para se conectar a computadores remotos usando uma VPN. Se esse serviço está desabilitado, os usuários não poderão usar SSTP para acessar servidores remotos.    |
|   **Nome do serviço**    |   SstpSvc |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Não desabilite  |
|   **Comentários**    |   Desabilitando quebras de RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>Gerenciador de contas de segurança            
| | |           
|---|---|       
|   **Descrição do serviço** |   A inicialização desse serviço indica a outros serviços que o Gerenciador de contas de segurança (SAM) está pronto para aceitar solicitações.  Desabilitar esse serviço evitará que outros serviços do sistema sejam notificados quando o SAM estiver pronto, que por sua vez pode causar esses serviços falhar iniciar corretamente. Esse serviço não deve ser desabilitado.
|   **Nome do serviço**    |   SamSs
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>Serviço de dados do sensor  
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece dados de uma variedade de sensores
|   **Nome do serviço**    |   SensorDataService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>Serviço de monitoramento de sensor            
| | |           
|---|---|       
|   **Descrição do serviço** |   Monitora vários sensores para expor dados e se adaptar a estado de usuário e sistema.  Se esse serviço for interrompido ou desabilitado, o brilho do vídeo não se adaptar às condições de iluminação. Parar esse serviço pode afetar outros recursos também e a funcionalidade do sistema.
|   **Nome do serviço**    |   SensrSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>Serviço do sensor           
| | |           
|---|---|       
|   **Descrição do serviço** |   Um serviço para sensores que gerencia a funcionalidade dos diferentes sensores. Gerencia a orientação de dispositivo simples (SDO) e o histórico dos sensores. Carrega o sensor SDO que relata as alterações de orientação do dispositivo.  Se esse serviço for interrompido ou desabilitado, o sensor SDO não será carregado e então rotação automática não ocorrerá. Coleção de histórico de sensores também será interrompida.
|   **Nome do serviço**    |   SensorService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="server"></a>Servidor           
| | |           
|---|---|   
|   **Descrição do serviço** |   Dá suporte a arquivos, impressão e pipes nomeados compartilhamento na rede para esse computador. Se esse serviço for interrompido, essas funções estará disponíveis. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   LanmanServer
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Necessário para o gerenciamento remoto, IPC$, compartilhamento de arquivos SMB
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Detecção de Hardware do shell             
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece notificações para eventos de hardware de reprodução automática.
|   **Nome do serviço**    |   ShellHWDetection
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="smart-card"></a>Cartão inteligente           
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia o acesso a cartões inteligentes lidos por este computador. Se esse serviço é interrompido, este computador poderá ler cartões inteligentes. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   SCardSvr
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>Serviço de enumeração de dispositivo de cartão inteligente                    
| | |               
|---|---|       
|   **Descrição do serviço** |   Nós de dispositivo de software para todos os leitores de cartão inteligente cria acessíveis para uma determinada sessão. Se esse serviço está desabilitado, APIs do WinRT não poderão enumerar leitores de cartão inteligente.   |
|   **Nome do serviço**    |   ScDeviceEnum    |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Okey desabilitar   |
|   **Comentários**    |   Necessário quase que exclusivamente para aplicativos do WinRT    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>Política de remoção de cartão inteligente        
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que o sistema esteja configurado para bloquear a área de trabalho do usuário após a remoção de cartão inteligente.
|   **Nome do serviço**    |   SCPolicySvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>Condition            
| | |           
|---|---|       
|   **Descrição do serviço** |   Recebe mensagens de interceptação geradas por agentes de protocolo de gerenciamento de rede simples (SNMP) locais ou remotos e encaminha as mensagens aos programas de gerenciamento de SNMP em execução nesse computador. Se esse serviço for interrompido, com base em SNMP programas neste computador não receberão mensagens de interceptação SNMP. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   SNMPTRAP
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>Proteção de software             
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que o download, instalação e a imposição de licenças digitais do Windows e Windows aplicativos. Se o serviço estiver desabilitado, o sistema operacional e os aplicativos licenciados podem ser executado em um modo de notificação. É altamente recomendável que você não desative o serviço de proteção de Software.
|   **Nome do serviço**    |   sppsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>Auxiliar de Console de administração especial        
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que administradores acessem remotamente um prompt de comando usando os serviços de gerenciamento de emergência.
|   **Nome do serviço**    |   Sacsvr
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>Verificador especial            
| | |           
|---|---|   
|   **Descrição do serviço** |   Verifica possíveis corrupções de sistema de arquivos.
|   **Nome do serviço**    |   svsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>Descoberta SSDP           
| | |           
|---|---|   
|   **Descrição do serviço** |   Detecta dispositivos em rede e serviços que usam o protocolo de descoberta SSDP, como dispositivos UPnP. Também anuncia SSDP dispositivos e serviços em execução no computador local. Se esse serviço for interrompido, dispositivos baseados em SSDP não serão descobertos. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   SSDPSRV
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>Serviço de estado do repositório         
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte à infraestrutura necessária para o modelo de aplicativo.
|   **Nome do serviço**    |   StateRepository
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>Eventos de aquisição de imagem ainda
| | |           
|---|---|   
|   **Descrição do serviço** |   Inicia aplicativos associados a eventos de aquisição de imagem ainda.
|   **Nome do serviço**    |   WiaRpc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />  

## <a name="storage-service"></a>Serviço de armazenamento          
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece serviços de ativação para expansão de armazenamento externo e configurações de armazenamento
|   **Nome do serviço**    |   StorSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>Gerenciamento de níveis de armazenamento        
| | |           
|---|---|   
|   **Descrição do serviço** |   Otimiza o posicionamento dos dados em camadas de armazenamento em todos os espaços de armazenamento em camadas do sistema.
|   **Nome do serviço**    |   TieringEngineService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>SuperFetch          
| | |           
|---|---|       
|   **Descrição do serviço** |   Mantém e melhora o desempenho do sistema ao longo do tempo.
|   **Nome do serviço**    |   SysMain
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="sync-host"></a>Host de sincronização            
| | |           
|---|---|       
|   **Descrição do serviço** |   Este serviço sincroniza email, contatos, calendário e vários outros dados do usuário. Email e outros aplicativos dependentes essa funcionalidade não funcionará corretamente quando esse serviço não estiver em execução.
|   **Nome do serviço**    |   OneSyncSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="system-event-notification-service"></a>Serviço de notificação de eventos do sistema            
| | |           
|---|---|       
|   **Descrição do serviço** |   Monitora os eventos do sistema e notifica os assinantes do sistema de eventos COM+ desses eventos.
|   **Nome do serviço**    |   SENS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>Agente de eventos do sistema             
| | |           
|---|---|       
|   **Descrição do serviço** |   Coordenadas de execução do trabalho em segundo plano para aplicativos do WinRT. Se esse serviço for interrompido ou desabilitado, trabalho em segundo plano não pode ser disparado.
|   **Nome do serviço**    |   SystemEventsBroker
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Apesar do fato de que sua descrição implica é somente para aplicativos do WinRT, ele é necessário para o Agendador de tarefas, serviço de infraestrutura de agente e outros componentes internos.
|||         
            
<br />          

## <a name="task-scheduler"></a>Agendador de tarefas           
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que um usuário configurar e agendar tarefas automatizadas neste computador. O serviço também hospeda várias tarefas críticas de sistema do Windows. Se esse serviço for interrompido ou desabilitado, essas tarefas não serão executadas em agendados vezes. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   Agendamento
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>Auxiliar de NetBIOS de TCP/IP            
| | |           
|---|---|   
|   **Descrição do serviço** |   Oferece suporte para o NetBIOS sobre serviço TCP/IP (NetBT) e resolução de nome NetBIOS para clientes na rede, permitindo que os usuários compartilhem arquivos, imprimam e fazer logon rede. Se esse serviço for interrompido, essas funções podem estar indisponíveis. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   Lmhosts
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Telefonia           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece suporte à API de telefonia (TAPI) para programas que controlam dispositivos no computador local e, através da rede local, em servidores que estejam executando o serviço de telefonia.
|   **Nome do serviço**    |   TapiSrv
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Desabilitando quebras de RRAS
|||         
            
<br />          

## <a name="themes"></a>Temas           
| | |           
|---|---|
|   **Descrição do serviço** |   Fornece gerenciamento de tema de experiência do usuário.
|   **Nome do serviço**    |   Temas
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Não é possível definir temas de acessibilidade quando esse serviço está desabilitado
|||         
            
<br />  

## <a name="tile-data-model-server"></a>Servidor de modelo de dados de bloco           
| | |           
|---|---|   
|   **Descrição do serviço** |   Bloco servidor atualizações de bloco.
|   **Nome do serviço**    |   tiledatamodelsvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Iniciar quebras de menu se esse serviço está desabilitado
|||         
            
<br />          

##  <a name="time-broker"></a>Agente do tempo     
| | |           
|---|---|       
|   **Descrição do serviço** |   Coordenadas de execução do trabalho em segundo plano para aplicativos do WinRT. Se esse serviço for interrompido ou desabilitado, trabalho em segundo plano não pode ser disparado.
|   **Nome do serviço**    |   TimeBrokerSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Apesar do fato de que sua descrição implica é somente para aplicativos do WinRT, ele é necessário para o Agendador de tarefas, serviço de infraestrutura de agente e outros componentes internos.
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Teclado virtual e serviço de painel de manuscrito         
| | |           
|---|---|   
|   **Descrição do serviço** |   Habilita a funcionalidade de caneta e tinta teclado virtual e painel de manuscrito
|   **Nome do serviço**    |   TabletInputService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Serviço do Orchestrator de atualização para o Windows Update           
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as atualizações do Windows. Se interrompido, seus dispositivos não poderão baixar e instalar as atualizações mais recentes.
|   **Nome do serviço**    |   UsoSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Descrição do serviço estava ausente no v1607; Windows Update (incluindo WSUS) depende desse serviço.
|||         
            
<br />          

## <a name="upnp-device-host"></a>Host de dispositivo UPnP         
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que dispositivos UPnP sejam hospedados neste computador. Se esse serviço for interrompido, os dispositivos UPnP hospedados deixarão de funcionar e nenhum outro dispositivo hospedado pode ser adicionado. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   UPnPHost
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>Serviço de registro em log de acesso do usuário          
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço registra as solicitações de acesso do cliente exclusivo, na forma de endereços IP e nomes de usuário, de produtos instalados e funções no servidor local. Essas informações podem ser consultadas, por meio do Powershell, por administradores precisar quantificar demanda de cliente de software de servidor de gerenciamento de licença de acesso para cliente (CAL) offline. Se o serviço está desabilitado, solicitações de cliente não serão registradas e não serão recuperáveis via consultas do Powershell. Parar o serviço não afetará a consulta de dados históricos (consulte a documentação para obter as etapas excluir dados históricos de suporte). O administrador de sistema local deve consultar, jid, termos de licença do Windows Server para determinar o número de CALs que são necessários para o software de servidor seja devidamente licenciado; uso do serviço real e dados não alterar essa obrigação.
|   **Nome do serviço**    |   UALSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>Acesso de dados do usuário        
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece acesso de aplicativos para dados de usuário estruturados, incluindo informações de contato, calendários, mensagens e outros tipos de conteúdo. Se você para ou desativar este serviço, os aplicativos que usam esses dados podem não funcionar corretamente.
|   **Nome do serviço**    |   UserDataSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="user-data-storage"></a>Armazenamento de dados do usuário            
| | |           
|---|---|       
|   **Descrição do serviço** |   Manipula armazenamento estruturado de dados do usuário, incluindo informações de contato, calendários, mensagens e outros tipos de conteúdo. Se você para ou desativar este serviço, os aplicativos que usam esses dados podem não funcionar corretamente.
|   **Nome do serviço**    |   UnistoreSvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>Serviço de virtualização de experiência do usuário           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece suporte para o aplicativo e roaming de configurações do sistema operacional
|   **Nome do serviço**    |   UevAgentService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>Gerenciador de usuários        
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerenciador de usuários fornece os componentes de tempo de execução necessários para interação do usuário multi.  Se esse serviço for interrompido, alguns aplicativos podem não funcionar corretamente.
|   **Nome do serviço**    |   UserManager
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>Serviço de perfil de usuário         
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço é responsável por carregar e descarregar perfis de usuário. Se esse serviço for interrompido ou desabilitado, os usuários poderão mais com êxito entrar ou sair, aplicativos podem ter problemas para acessar os dados do usuário e componentes registrados para receber notificações de eventos de perfil não recebem-los.
|   **Nome do serviço**    |   ProfSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>Disco virtual             
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de gerenciamento de discos, volumes, sistemas de arquivos e matrizes de armazenamento.
|   **Nome do serviço**    |   VDS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não inclua orientações
|   **Comentários**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>Cópia de sombra de volume           
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia e implementa cópias de sombra de Volume usadas para backup e outros fins. Se esse serviço for interrompido, as cópias de sombra não estarão disponíveis para o backup e o backup pode falhar. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   VSS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não inclua orientações
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **Descrição do serviço** |   Objetos de hosts usados pelos clientes da carteira
|   **Nome do serviço**    |   WalletService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Áudio do Windows            
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia áudio para programas baseados em Windows.  Se esse serviço for interrompido, os dispositivos de áudio e efeitos não funcionarão corretamente.  Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele não serão iniciado
|   **Nome do serviço**    |   Audiosrv
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Construtor de ponto de extremidade de áudio do Windows           
| | |           
|---|---|
|   **Descrição do serviço** |   Gerencia dispositivos de áudio para o serviço de áudio do Windows.  Se esse serviço for interrompido, os dispositivos de áudio e efeitos não funcionarão corretamente.  Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele não serão iniciado
|   **Nome do serviço**    |   AudioEndpointBuilder
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Serviço biométrico do Windows            
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço biométrico do Windows oferece a capacidade de capturar, comparar, manipular e armazenar dados biométricos sem acesso direto para qualquer hardware biométrico ou amostras de aplicativos cliente. O serviço está hospedado em um processo privilegiado do SVCHOST.
|   **Nome do serviço**    |   WbioSrvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Servidor de quadros de câmera do Windows         
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que vários clientes acessem quadros de vídeo de dispositivos de câmera.
|   **Nome do serviço**    |   FrameServer
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Gerenciador de Conexão do Windows           
| | |           
|---|---|   
|   **Descrição do serviço** |   Torna automática são conectados/desconectados decisões com base nas opções de conectividade de rede disponíveis atualmente para o computador e permite o gerenciamento de conectividade de rede com base nas configurações de política de grupo.
|   **Nome do serviço**    |   Wcmsvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Serviço de inspeção de rede do Windows Defender          
| | |           
|---|---|       
|   **Descrição do serviço** |   Ajuda a proteger contra tentativas de invasão direcionamento vulnerabilidades conhecidas e recém-descobertas em protocolos de rede
|   **Nome do serviço**    |   WdNisSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Serviço do Windows Defender         
| | |           
|---|---|       
|   **Descrição do serviço** |   Ajuda a proteger os usuários contra malware e outro software potencialmente indesejado
|   **Nome do serviço**    |   WinDefend
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation - estrutura de Driver de modo de usuário           
| | |           
|---|---|   
|   **Descrição do serviço** |   Cria e gerencia os processos de driver de modo de usuário. Esse serviço não pode ser interrompido.
|   **Nome do serviço**    |   wudfsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Serviço de Host de provedor de criptografia do Windows     
| | |           
|---|---|   
|   **Descrição do serviço** |   Criptografia de agentes do serviço de Host de provedor de criptografia do Windows relacionados funcionalidades de provedores de criptografia de terceiros para processos que precisam para avaliar e aplicar políticas EAS. Interromper isso comprometa verificações de conformidade do EAS que foram estabelecidas as contas de email conectado
|   **Nome do serviço**    |   WEPHOSTSVC
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Serviço de relatório de erros do Windows          
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que os erros sejam relatados quando programas param de funcionar ou de responder e permite que as soluções existentes sejam entregues. Também permite que os logs sejam gerados para diagnóstico e reparo de serviços. Se esse serviço é interrompido, relatório de erros pode não funcionar corretamente e resultados de diagnósticos serviços e reparos podem não ser exibidos.
|   **Nome do serviço**    |   WerSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Coleta e envia dados de falhas/travar usados pelo MS e ISVs/IHVs de terceiros. Os dados são usados para diagnosticar bugs indução falhas, que podem incluir erros de segurança. Também é necessária para o relatório de erros corporativo
|||         
            
<br />          

## <a name="windows-event-collector"></a>Coletor de eventos do Windows          
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço gerencia inscrições persistentes em eventos de origens remotas que dão suporte ao protocolo WS-Management. Isso inclui logs de eventos do Windows Vista, hardware e as origens de evento IPMI habilitado. O serviço armazena encaminhado eventos em um Log de eventos local. Se esse serviço for interrompido ou desabilitado não não possível criar assinaturas de eventos e eventos encaminhados não podem ser aceito.
|   **Nome do serviço**    |   Wecsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Coleta os eventos ETW (incluindo eventos de segurança) para gerenciamento, diagnóstico.  Muitos recursos e ferramentas de terceiros confiam nele, incluindo ferramentas de auditoria de segurança
|||         
            
<br />          

## <a name="windows-event-log"></a>Log de eventos do Windows            
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço gerencia eventos e logs de eventos. Ele suporta o log de eventos, consultar os eventos, Assinando eventos, arquivamento de logs de eventos e gerenciamento de metadados de eventos. Ele pode exibir eventos no formato XML e texto sem formatação. Parar esse serviço pode comprometer a segurança e confiabilidade do sistema.
|   **Nome do serviço**    |   Log de eventos
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Firewall do Windows         
| | |           
|---|---|   
|   **Descrição do serviço** |   O Firewall do Windows ajuda a proteger seu computador, impedindo que usuários não autorizados tenham acesso ao seu computador através da Internet ou uma rede.
|   **Nome do serviço**    |   MpsSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Serviço de Cache de fontes do Windows      
| | |           
|---|---|   
|   **Descrição do serviço** |   Otimiza o desempenho de aplicativos armazenando em cache os dados de fontes comumente usados. Aplicativos serão iniciados esse serviço se ele não estiver sendo executado. Ele pode ser desativado, embora isso diminuirá o desempenho do aplicativo.
|   **Nome do serviço**    |   FontCache
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>Windows Image Acquisition (WIA)          
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de aquisição de imagens para scanners e câmeras
|   **Nome do serviço**    |   stisvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Serviço do Windows Insider     
| | |           
|---|---|   
|   **Descrição do serviço** |   wisvc
|   **Nome do serviço**    |   wisvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Servidor não dá suporte a enviando, portanto, é um não-op no servidor. Recurso também pode ser desabilitado por meio de GP.
|||         
            
<br />          

##  <a name="windows-installer"></a>Do Windows Installer       
| | |           
|---|---|
|   **Descrição do serviço** |   Adiciona, modifica e remove aplicativos fornecidos como um pacote do Windows Installer (MSI, *.msp). Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   MSIServer
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Serviço de Gerenciador de licença do Windows          
| | |           
|---|---|   
|   **Descrição do serviço** |   Dá suporte a infraestrutura da Microsoft Store.  Esse serviço é iniciado sob demanda e se desabilitado, em seguida, conteúdo adquirido por meio de Microsoft Store não funcionará corretamente.
|   **Nome do serviço**    |   LicenseManager
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>Instrumentação de gerenciamento do Windows       
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um modelo de interface e objeto comuns para acessar informações sobre o sistema operacional, dispositivos, aplicativos e serviços de gerenciamento. Se esse serviço for interrompido, a maioria dos softwares baseados no Windows não funcionará corretamente. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   Winmgmt
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Serviço de Hotspot móvel do Windows          
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a capacidade de compartilhar uma conexão de dados da rede celular com outro dispositivo.
|   **Nome do serviço**    |   icssvc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>Módulos do Windows Installer        
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite a instalação, modificação e remoção de atualizações do Windows e componentes opcionais. Se esse serviço está desabilitado, instalar ou desinstalar do Windows podem ser reprovado atualizações para esse computador.
|   **Nome do serviço**    |   TrustedInstaller
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>Serviço de sistema de notificações por Push do Windows            
| | |           
|---|---|
|   **Descrição do serviço** |   Esse serviço é executado na sessão 0 e hospeda o provedor de plataforma e conexão de notificação que lida com a conexão entre o dispositivo e o servidor do WNS.
|   **Nome do serviço**    |   WpnService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Automático
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Necessário para blocos dinâmicos e outros recursos
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>Serviço de usuário de notificações por Push do Windows          
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço hospeda a plataforma de notificação do Windows que oferece suporte para o local e notificações por push. As notificações com suporte são bloco, notificação do sistema e brutas.
|   **Nome do serviço**    |   WpnUserService
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>Gerenciamento remoto do Windows (WS-Management)            
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço de gerenciamento remoto do Windows (WinRM) implementa o protocolo WS-Management para o gerenciamento remoto. WS-Management é um protocolo de serviços da web padrão usado para gerenciamento remoto do hardware e software. O serviço WinRM escuta na rede solicitações WS-Management e processá-las. O WinRM Service precisa ser configurado com um ouvinte usando a ferramenta de linha de comando WinRM ou por meio da política de grupo na ordem para que ele escute pela rede. O serviço WinRM fornece acesso a dados WMI e habilita a coleta de eventos. Coleta de eventos e a assinatura de eventos exigem que o serviço está em execução. Mensagens de WinRM usam HTTP e HTTPS como transportes. O serviço WinRM não depende do IIS, mas é pré-configurado para compartilhar uma porta com o IIS no mesmo computador.  O serviço WinRM reserva para si o prefixo de URL /wsman.. Para evitar conflitos com o IIS, os administradores devem garantir que qualquer site hospedado no IIS não use o prefixo de URL /wsman..
|   **Nome do serviço**    |   WinRM
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Necessário para o gerenciamento remoto
|||         
            
<br />          

##  <a name="windows-search"></a>O Windows Search      
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a indexação de conteúdo, cache de propriedade e resultados de pesquisa para arquivos, email e outros tipos de conteúdo.
|   **Nome do serviço**    |   WSearch
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Desabilitado
|   **Recomendação**  |   Já desabilitada
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Tempo do Windows        
| | |           
|---|---|   
|   **Descrição do serviço** |   Mantém a sincronização de data e hora em todos os clientes e servidores na rede. Se esse serviço for interrompido, sincronização de data e hora estará indisponível. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   W32Time
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Atualização do Windows           
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite a detecção, download e instalação de atualizações do Windows e outros programas. Se esse serviço está desabilitado, os usuários deste computador não poderão usar o Windows Update ou o recurso de atualização automática e programas não será capazes de usar a API do Windows Update Agent (WUA).
|   **Nome do serviço**    |   wuauserv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Serviço de detecção automática de Proxy da Web do WinHTTP         
| | |           
|---|---|   
|   **Descrição do serviço** |   WinHTTP implementa a pilha HTTP de cliente e fornece aos desenvolvedores uma API do Win32 e o componente de automação COM para enviar solicitações HTTP e receber respostas. Além disso, WinHTTP fornece suporte para uma configuração de proxy por meio de sua implementação do protocolo descoberta automática de Proxy da Web (WPAD) a detecção automática.
|   **Nome do serviço**    |   WinHttpAutoProxySvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Algo que usa a pilha de rede pode ter uma dependência funcional neste serviço. Muitas organizações contam com esta opção para configurar proxy HTTP de suas redes internas de roteamento.  Sem ela, internamente originados conexões HTTP com a Internet todos falhará.
|||         
            
<br />          

## <a name="wired-autoconfig"></a>Configuração automática com fio         
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de rede com fio configuração automática (DOT3SVC) é responsável por executar IEEE 802.1 X autenticação em interfaces Ethernet. Se a implantação de rede com fio atual impõe autenticação 802.1 X, o serviço DOT3SVC deve ser configurado para ser executado para estabelecer conectividade de camada 2 e/ou fornecendo acesso aos recursos de rede. Redes com fio que não impor autenticação 802.1 X não são afetados pelo serviço DOT3SVC.
|   **Nome do serviço**    |   Dot3Svc
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>Adaptador de desempenho WMI          
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece informações de biblioteca de desempenho de provedores de Windows Management Instrumentation (WMI) aos clientes na rede. Esse serviço é executado apenas quando auxiliar de dados de desempenho é ativado.
|   **Nome do serviço**    |   wmiApSrv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Não inclua orientações       
|   **Comentários**    |   
|||         
            
<br />          

## <a name="workstation"></a>Estação de trabalho          
| | |           
|---|---|   
|   **Descrição do serviço** |   Cria e mantém conexões de rede do cliente para servidores remotos usando o protocolo SMB. Se esse serviço for interrompido, essas conexões estará disponíveis. Se esse serviço está desabilitado, todos os serviços que dependem explicitamente dele falhará na tela inicial.
|   **Nome do serviço**    |   LanmanWorkstation
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automático
|   **Recomendação**  | Não inclua orientações       
|   **Comentários**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Gerenciador de autorização Live do Xbox           
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de autenticação e autorização para interagir com o Xbox Live. Se esse serviço for interrompido, alguns aplicativos podem não funcionar corretamente.
|   **Nome do serviço**    |   XblAuthManager
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Deve ser desabilitada
|   **Comentários**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Xbox Live Salvar jogo          
| | |           
|---|---|   
|   **Descrição do serviço** |   São sincronizados este serviço salvar dados para o Xbox Live salvar jogos habilitados.  Se esse serviço for interrompido, o jogo salvar dados não carregar ou baixar do Xbox Live.
|   **Nome do serviço**    |   XblGameSave
|   **Instalação**    |   Somente no Datacenter Edition
|   **StartType**   |   Manual
|   **Recomendação**  |   Deve ser desabilitada
|   **Comentários**    |   São sincronizados este serviço salvar dados para o Xbox Live salvar jogos habilitados.  Se esse serviço for interrompido, o jogo salvar dados não carregar ou baixar do Xbox Live.
|||         
                
<br /> 
<br /> 

