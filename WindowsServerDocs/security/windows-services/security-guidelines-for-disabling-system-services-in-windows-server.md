---
title: Diretrizes de segurança para serviços do sistema no Windows Server 2016
description: Diretrizes de segurança para desabilitar serviços no Windows Server 2016 com Experiência Desktop
ms.custom: na
ms.prod: windows-server
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 1711eb94b622775feaf02f6bada596fe03b08ea9
ms.sourcegitcommit: b8e120fc574450e9eee13e7315424137a43e6a6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74044803"
---
# <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Diretrizes para desabilitar serviços do sistema no Windows Server 2016 com Experiência Desktop

Aplica-se a: Windows Server 2016

O sistema operacional Windows inclui muitos serviços do sistema que fornecem funcionalidades importantes. Diferentes serviços têm políticas de inicialização padrão diferentes: algumas são iniciadas por padrão (automático), outras quando necessário (manual) e outras são desabilitadas por padrão e precisam ser habilitadas explicitamente antes de serem executadas. Esses padrões foram escolhidos cuidadosamente para cada serviço, visando equilibrar o desempenho, a funcionalidade e a segurança para clientes típicos.

No entanto, alguns clientes corporativos podem preferir um equilíbrio mais voltado para segurança para seus servidores e computadores Windows, que reduza sua superfície de ataque ao mínimo absoluto e, portanto, podem desejar desabilitar totalmente todos os serviços que não sejam necessários em seus ambientes específicos. Para esses clientes, a Microsoft® fornece diretrizes complementares em relação a quais serviços podem ser desabilitados com segurança para essa finalidade.

As diretrizes destinam-se apenas ao Windows Server 2016 com Experiência Desktop (a menos que seja usado como uma substituição da área de trabalho para usuários finais). No Windows Server 2019 em diante, essas diretrizes estão configuradas por padrão. Cada serviço do sistema é categorizado da seguinte maneira:

-   **Deve ser desabilitado:** Uma empresa voltada para segurança provavelmente preferirá desabilitar esse serviço e abrir mão de sua funcionalidade (confira os detalhes adicionais abaixo).
- **Pode ser desabilitado:** Esse serviço fornece uma funcionalidade útil para algumas, mas nem todas as empresas, e as empresas voltadas para segurança que não a usarem poderão desabilitá-la com segurança.
- **Não desabilitar:** A desabilitação desse serviço afetará a funcionalidade essencial ou impedirá que funções ou recursos específicos funcionem corretamente. Portanto, ele não deve ser desabilitado.
-  **(Sem diretrizes):** O impacto da desabilitação desses serviços não foi totalmente avaliado. Portanto, a configuração padrão desses serviços não deve ser alterada.


Os clientes podem configurar seus servidores e computadores Windows para desabilitar os serviços selecionados usando os Modelos de Segurança em suas Políticas de Grupo ou usando a automação do PowerShell. Em alguns casos, as diretrizes incluem configurações da Política de Grupo específicas que desabilitam a funcionalidade do serviço diretamente, como uma alternativa para desabilitar o serviço em si.

A Microsoft recomenda que os clientes desabilitem os seguintes serviços e suas respectivas tarefas agendadas no Windows Server 2016 com Experiência Desktop:

Serviços: 
1. Gerenciador de Autenticação Xbox Live
2. Salvamento de Jogo do Xbox Live

Tarefas agendadas: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(Acesse também as informações sobre todos os serviços detalhados neste artigo exibindo a planilha do Microsoft Excel anexada: [Diretrizes para desabilitar serviços do sistema no Windows Server 2016 com Experiência Desktop](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))



### <a name="disabling-services-not-installed-by-default"></a>Como desabilitar serviços não instalados por padrão

A Microsoft recomenda a aplicação de políticas para desabilitar serviços que não estão instalados por padrão.
-  O serviço geralmente é necessário se o recurso está instalado. A instalação do serviço ou do recurso exige direitos administrativos. Rejeite a instalação do recurso, não a inicialização do serviço.
-  O bloqueio do serviço Microsoft Windows não impede que um administrador (ou não administrador, em alguns casos) instale um equivalente de terceiros semelhante, talvez um com um risco de segurança mais alto.
-  Uma linha de base ou um parâmetro de comparação que desabilita um serviço Windows não padrão (por exemplo, o W3SVC) fornecerá a alguns auditores a impressão equivocada de que a tecnologia (por exemplo, o IIS) não é inerentemente segura e nunca deve ser usada.
-  Se o recurso (e o serviço) nunca é instalado, isso apenas adiciona uma pilha desnecessária à linha de base e ao trabalho de verificação.


Para todos os serviços do sistema listados neste documento, as duas seguintes tabelas oferecem uma explicação das colunas e a Microsoft recomenda a habilitação e a desabilitação dos serviços do sistema no Windows Server 2016 com Experiência Desktop: 



### <a name="explanation-of-columns"></a>Explicação das colunas

| | |
|---|---|
|**Descrição do serviço**|   A descrição do serviço, com base na descrição de sc.exe.|
|**Nome** |Nome da chave (interna) do serviço|
|**Instalação** | *Sempre instalado*: o serviço está instalado no Windows Server 2016 Core e no Windows Server 2016 com Experiência Desktop. *Somente com a Experiência Desktop*: O serviço está no Windows Server 2016 com Experiência Desktop, mas ***não*** está instalado no Server Core. |
|**StartType**  |Tipo de início do serviço no Windows Server 2016|
|**Recomendação** |Recomendação/orientação da Microsoft sobre como desabilitar esse serviço no Windows Server 2016 em uma implantação corporativa típica e bem gerenciada e em que o servidor não está sendo usado como uma substituição da área de trabalho do usuário final.|
|**Comentários** |Explicação adicional|



### <a name="explanation-of-microsoft-recommendations"></a>Explicação sobre as recomendações da Microsoft

| | |
|---|---|
|**Não desabilitar** |Esse serviço não deve ser desabilitado|
|**Pode ser desabilitado**| Esse serviço pode ser desabilitado se o recurso ao qual ele dá suporte não está sendo usado.|
|**Já desabilitado**|  Esse serviço está desabilitado por padrão; não há necessidade de imposição com uma política|
|**Deve ser desabilitado** |Esse serviço nunca deve ser habilitado em um sistema corporativo bem gerenciado.|



As seguintes tabelas oferecem as diretrizes da Microsoft de como desabilitar serviços do sistema no Windows Server 2016 com Experiência Desktop:



##  <a name="activex-installer-axinstsv"></a>Instalador do ActiveX (AxInstSV)

| | |
|---|---|
|   **Descrição do serviço** |   Fornece a validação do Controle de Conta de Usuário para a instalação de controles ActiveX na Internet e permite o gerenciamento da instalação de controles ActiveX com base nas configurações da Política de Grupo. Esse serviço é iniciado sob demanda e, se for desabilitado, a instalação de controles ActiveX se comportará de acordo com as configurações do navegador padrão.    |
|   **Nome do serviço**    |   AxInstSV    |
|   **Instalação**    |   Somente com a Experiência Desktop    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Pode ser desabilitado   |
|   **Comentários**    |   Pode ser desabilitado se o recurso não é necessário |




## <a name="alljoyn-router-service"></a>Serviço de roteador AllJoyn   

| | |
|---|---|
|   **Descrição do serviço** |   Encaminha as mensagens do AllJoyn para os clientes do AllJoyn locais. Se esse serviço for interrompido, os clientes do AllJoyn que não tiverem seus próprios roteadores empacotados não poderão executá-lo. |
|   **Nome do serviço**    |   AJRouter    |
|   **Instalação**    |   Somente com a Experiência Desktop    |
|   **StartType**   |   Manual  |
|   **Recomendação**  | Sem diretrizes       |
|   **Comentários**    |       |
| | |



## <a name="app-readiness"></a>Preparação de Aplicativos

| | |
|---|---|
**Descrição do serviço** |   Prepara os aplicativos para uso na primeira vez que um usuário entra neste computador e ao adicionar novos aplicativos.
**Nome do serviço**    |   AppReadiness
**Instalação**    |   Somente com a Experiência Desktop
**StartType**   |   Manual
**Recomendação**  |   Não desabilitar
**Comentários**    |   
| | |



##  <a name="application-identity"></a>Identidade do Aplicativo

| | |       
|---|---|   
**Descrição do serviço** |   Determina e verifica a identidade de um aplicativo. A desabilitação desse serviço impedirá a imposição do AppLocker.
**Nome do serviço**    |   AppIDSvc
**Instalação**    |   Sempre instalado
**StartType**   |   Manual
**Recomendação**  |Sem diretrizes    
**Comentários**    |   
|||     



##  <a name="application-information"></a>Informações do Aplicativo 

| | |       
|---|---|   
|   **Descrição do serviço** |   Facilita a execução de aplicativos interativos com privilégios administrativos adicionais.  Se esse serviço for interrompido, os usuários não poderão iniciar aplicativos com privilégios administrativos adicionais possivelmente necessários para realizar as tarefas de usuário desejadas.
|   **Nome do serviço**    |   Appinfo
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   Dá suporte à elevação do UAC na mesma área de trabalho
|||     



##  <a name="application-layer-gateway-service"></a>Serviço de Gateway de Camada de Aplicativo       

| | |           
|---|---|           
|   **Descrição do serviço** |   Fornece suporte para plug-ins de protocolo de terceiros para o Compartilhamento de Conexão com a Internet
|   **Nome do serviço**    |   ALG
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |Sem diretrizes    
|   **Comentários**    |   
|||     



##  <a name="application-management"></a>Gerenciamento de aplicativos      

| | |           
|---|---|       
|   **Descrição do serviço** |   Processa as solicitações de instalação, remoção e enumeração do software implantado por meio da Política de Grupo. Se o serviço for desabilitado, os usuários não poderão instalar, remover nem enumerar o software implantado por meio da Política de Grupo. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   AppMgmt
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="appx-deployment-service-appxsvc"></a>Serviço de Implantação do AppX (AppXSVC)       

| | |           
|---|---|
|   **Descrição do serviço** |   Fornece suporte de infraestrutura para implantação de aplicativos da Store. Esse serviço é iniciado sob demanda e, se for desabilitado, os aplicativos da Store não serão implantados no sistema e poderão não funcionar corretamente.
|   **Nome do serviço**    |   AppXSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="auto-time-zone-updater"></a>Atualizador de Fuso Horário Automático           

| | |           
|---|---|           
|   **Descrição do serviço** |   Define automaticamente o fuso horário do sistema.
|   **Nome do serviço**    |   tzautoupdate
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



## <a name="background-intelligent-transfer-service"></a>BITS          

| | |           
|---|---|   
|   **Descrição do serviço** |   Transfere arquivos em segundo plano usando a largura de banda de rede ociosa. Se o serviço for desabilitado, todos os aplicativos que dependem do BITS, como o Windows Update ou o MSN Explorer, não poderão baixar automaticamente programas e outras informações.
|   **Nome do serviço**    |   BITS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         




## <a name="background-tasks-infrastructure-service"></a>Serviço de infraestrutura de tarefas em segundo plano      

| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço de infraestrutura do Windows que controla quais tarefas em segundo plano podem ser executadas no sistema.
|   **Nome do serviço**    |   BrokerInfrastructure
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="base-filtering-engine"></a>Mecanismo de Filtragem Base            

| | |           
|---|---|       
|   **Descrição do serviço** |   O BFE (Mecanismo de Filtragem Base) é um serviço que gerencia as políticas do firewall e do protocolo IPsec e implementa a filtragem de modo de usuário. A interrupção ou a desabilitação do serviço BFE reduz significativamente a segurança do sistema. Ela também resultará em um comportamento imprevisível no gerenciamento do IPsec e em aplicativos de firewall.
|   **Nome do serviço**    |   BFE
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="bluetooth-support-service"></a>Serviço de Suporte do Bluetooth            

| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço Bluetooth dá suporte à descoberta e à associação de dispositivos Bluetooth remotos.  A interrupção ou a desabilitação desse serviço pode fazer com que dispositivos Bluetooth já instalados não funcionem corretamente e impedir que novos dispositivos sejam descobertos ou associados.
|   **Nome do serviço**    |   bthserv
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Pode ser desabilitado se não é usado. Outro mecanismo de desabilitação: https://technet.microsoft.com/library/dd252791.aspx
|||         




## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço de usuário é usado para cenários de Plataforma de Dispositivos Conectados
|   **Nome do serviço**    |   CDPUserSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Modelo de serviço de usuário
|||         




##  <a name="certificate-propagation"></a>Propagação de Certificado     

| | |           
|---|---|
|   **Descrição do serviço** |   Copia os certificados de usuário e os certificados raiz de cartões inteligentes para o repositório de certificados do usuário atual, detecta quando um cartão inteligente é inserido em um leitor de cartão inteligente e, se necessário, instala o minidriver de Plug and Play do cartão inteligente.
|   **Nome do serviço**    |   CertPropSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="client-license-service-clipsvc"></a>Serviço de Licença do Cliente (ClipSVC)        

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte de infraestrutura para a Microsoft Store. Esse serviço é iniciado sob demanda e, se for desabilitado, os aplicativos comprados por meio da Microsoft Store não se comportarão corretamente.
|   **Nome do serviço**    |   ClipSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="cng-key-isolation"></a>Isolamento de Chave CNG

| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço de isolamento de chave CNG é hospedado no processo de LSA. O serviço fornece isolamento do processos de chave para chaves privadas e operações criptográficas associadas, conforme exigido pelos Critérios Comuns. O serviço armazena e usa chaves de longa vida em um processo seguro que está em conformidade com os requisitos dos Critérios Comuns.
|   **Nome do serviço**    |   KeyIso
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="com-event-system"></a>Sistema de Eventos COM+       

| | |           
|---|---|       
|   **Descrição do serviço** |   Dá suporte ao SENS (Serviço de Notificação de Eventos do Sistema), que fornece distribuição automática de eventos para assinatura de componentes COM (Component Object Model). Se o serviço for interrompido, o SENS será fechado e não poderá fornecer notificações de logon e logoff. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   EventSystem
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="com-system-application"></a>Aplicativo de Sistema COM+     

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia a configuração e o acompanhamento de componentes baseados em COM+ (Component Object Model). Se o serviço for interrompido, a maioria dos componentes baseados em COM+ não funcionará corretamente. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   COMSysApp
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="computer-browser"></a>Pesquisador de Computadores        

| | |           
|---|---|       
|   **Descrição do serviço** |   Mantém uma lista atualizada dos computadores na rede e fornece essa lista aos computadores designados como pesquisadores. Se esse serviço for interrompido, essa lista não será atualizada nem mantida. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   Navegador
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



## <a name="connected-devices-platform-service"></a>Serviço de Plataforma de Dispositivos Conectados       

| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço é usado para cenários de Dispositivos Conectados e Efeito de Transparência Universal
|   **Nome do serviço**    |   CDPSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="connected-user-experiences-and-telemetry"></a>Experiência do Usuário Conectado e Telemetria     

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço Experiência do Usuário Conectado e Telemetria habilita recursos que dão suporte a experiências do usuário conectado e no aplicativo. Além disso, esse serviço gerencia a coleção controlada por evento e a transmissão de informações de diagnóstico e de uso (usadas para melhorar a experiência e a qualidade da Plataforma Windows) quando as configurações de opção de privacidade de uso e diagnóstico são habilitadas em Comentários e Diagnóstico.
|   **Nome do serviço**    |   DiagTrack
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="contact-data"></a>Dados de Contato        

| | |           
|---|---|       
|   **Descrição do serviço** |   Indexa dados de contato para pesquisa rápida de contatos. Se você interromper ou desabilitar esse serviço, os contatos poderão ficar ausentes dos resultados da pesquisa.
|   **Nome do serviço**    |   PimIndexMaintenanceSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Modelo de serviço de usuário
|||         



## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **Descrição do serviço** |   Gerencia a comunicação entre componentes do sistema.
|   **Nome do serviço**    |   CoreMessagingRegistrar
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="credential-manager"></a>Gerenciador de Credenciais           

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece armazenamento seguro e recuperação de credenciais para usuários, aplicativos e pacotes de serviço de segurança.
|   **Nome do serviço**    |   VaultSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="cryptographic-services"></a>Serviços de Criptografia           

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece três serviços de gerenciamento: Serviço de Banco de Dados de Catálogo, que confirma as assinaturas de arquivos do Windows e permite que novos programas sejam instalados; Serviço de Raiz Protegida, que adiciona e remove os certificados de autoridade de certificação raiz confiável deste computador; e o Serviço de Atualização Automática, que recupera os certificados raiz do Windows Update e habilita cenários como SSL. Se esse serviço for interrompido, esses serviços de gerenciamento não funcionarão corretamente. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   CryptSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="data-sharing-service"></a>Serviço de Compartilhamento de Dados         

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece intermediação de dados entre aplicativos.
|   **Nome do serviço**    |   DsSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço DCP (Coleta e Publicação de Dados) dá suporte a aplicativos internos para fazer upload de dados na nuvem.
|   **Nome do serviço**    |   DcpSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="dcom-server-process-launcher"></a>Inicializador de Processo do Servidor DCOM         

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço DCOMLAUNCH inicia servidores COM e DCOM em resposta a solicitações de ativação de objeto. Se esse serviço for interrompido ou desabilitado, os programas que usam o COM ou o DCOM não funcionarão corretamente. É altamente recomendável que você tenha o serviço DCOMLAUNCH em execução.
|   **Nome do serviço**    |   DcomLaunch
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |Sem diretrizes    
|   **Comentários**    |   
|||         



##  <a name="device-association-service"></a>Serviço de Associação de Dispositivo      

| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita o emparelhamento entre o sistema e dispositivos com ou sem fio.
|   **Nome do serviço**    |   DeviceAssociationService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="device-install-service"></a>Serviço de Instalação de Dispositivo

| | |
|---|---|
|   **Descrição do serviço** |   Permite que um computador reconheça e se adapte às alterações de hardware com pouca ou nenhuma entrada de usuário. A interrupção ou a desabilitação desse serviço resultará na instabilidade do sistema.
|   **Nome do serviço**    |   DeviceInstall
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes
|   **Comentários**    |
|||



##  <a name="device-management-enrollment-service"></a>Serviço de Registro de Gerenciamento de Dispositivos        

| | |           
|---|---|       
|   **Descrição do serviço** |   Executa Atividades de Registro de Dispositivos para Gerenciamento de Dispositivos
|   **Nome do serviço**    |   DmEnrollmentSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="device-setup-manager"></a>Gerenciador de Instalação de Dispositivo         

| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita a detecção, o download e a instalação de software relacionado ao dispositivo. Se esse serviço for desabilitado, os dispositivos poderão ser configurados com um software desatualizado e poderão não funcionar corretamente.
|   **Nome do serviço**    |   DsmSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="devquery-background-discovery-broker"></a>Agente de descoberta em segundo plano de DevQuery         

| | |           
|---|---|           
|   **Descrição do serviço** |   Permite que os aplicativos descubram dispositivos com uma tarefa em segundo plano
|   **Nome do serviço**    |   DevQueryBroker
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="dhcp-client"></a>Cliente DHCP          

| | |           
|---|---|       
|   **Descrição do serviço** |   Registra e atualiza endereços IP e registros DNS deste computador. Se esse serviço for interrompido, esse computador não receberá atualizações de DNS e endereços IP dinâmicos. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   Dhcp
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="diagnostic-policy-service"></a>Serviço de Política de Diagnóstico            

| | |           
|---|---|       
|   **Descrição do serviço** |   O Serviço de Política de Diagnóstico permite a detecção e a solução de problemas, bem como a resolução para componentes do Windows.  Se esse serviço for interrompido, o diagnóstico deixará de funcionar.
|   **Nome do serviço**    |   DPS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="diagnostic-service-host"></a>Host de Serviço de Diagnóstico     

| | |           
|---|---|       
|   **Descrição do serviço** |   O Host de Serviço de Diagnóstico é usado pelo Serviço de Política de Diagnóstico para hospedar diagnósticos que precisam ser executados em um contexto do serviço local.  Se esse serviço for interrompido, os diagnósticos que dependem dele deixarão de funcionar.
|   **Nome do serviço**    |   WdiServiceHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="diagnostic-system-host"></a>Host do Sistema de Diagnóstico           

| | |           
|---|---|       
|   **Descrição do serviço** |   O Host do Sistema de Diagnóstico é usado pelo Serviço de Política de Diagnóstico para hospedar os diagnósticos que precisam ser executados em um contexto do Sistema Local.  Se esse serviço for interrompido, os diagnósticos que dependem dele deixarão de funcionar.
|   **Nome do serviço**    |   WdiSystemHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="distributed-link-tracking-client"></a>Cliente de Rastreamento de Link Distribuído            

| | |           
|---|---|   
|   **Descrição do serviço** |   Mantém links entre arquivos NTFS em um computador ou entre computadores em uma rede.
|   **Nome do serviço**    |   TrkWks
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="distributed-transaction-coordinator"></a>Coordenador de Transações Distribuídas     

| | |           
|---|---|   
|   **Descrição do serviço** |   Coordena as transações que abrangem vários gerenciadores de recursos, como bancos de dados, filas de mensagens e sistemas de arquivos. Se esse serviço for interrompido, essas transações falharão. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   MSDTC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="dmwappushsvc"></a>dmwappushsvc        

| | |           
|---|---|       
|   **Descrição do serviço** |   Serviço de Roteamento de Mensagens por Push WAP
|   **Nome do serviço**    |   dmwappushservice
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Serviço necessário em dispositivos cliente do Intune, do MDM e de tecnologias de gerenciamento semelhantes e do Filtro de Gravação Unificado. Não é necessário para o Server.
|||         



##  <a name="dns-client"></a>Cliente DNS      

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço do Cliente DNS (dnscache) faz o cache dos nomes DNS (Sistema de Nomes de Domínio) e registra o nome completo do computador nesse computador. Se o serviço for interrompido, nomes DNS continuarão sendo resolvidos. No entanto, os resultados das consultas de nome DNS não serão armazenados em cache e o nome dos computadores não será registrado. Se o serviço for desabilitado, outros serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   Dnscache
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="downloaded-maps-manager"></a>Gerenciador de Mapas Baixados     

| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço Windows para acesso de aplicativo aos mapas baixados. Esse serviço é iniciado sob demanda pelo aplicativo que acessa os mapas baixados. A desabilitação desse serviço impedirá que os aplicativos acessem mapas.
|   **Nome do serviço**    |   MapsBroker
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   A desabilitação interrompe aplicativos que dependem do serviço; pode ser desabilitado se os aplicativos não dependem dele
|||         



## <a name="embedded-mode"></a>Modo Inserido            

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço Modo Inserido permite cenários relacionados a aplicativos em segundo plano.  A desabilitação desse serviço impedirá a ativação de aplicativos em segundo plano.
|   **Nome do serviço**    |   embeddedmode
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="encrypting-file-system-efs"></a>EFS (Encrypting File System, sistema de arquivos com criptografia)

| | |                   
|---|---|           
|   **Descrição do serviço** | Fornece a principal tecnologia de criptografia de arquivos usada para armazenar arquivos criptografados em volumes do sistema de arquivos NTFS. Se esse serviço for interrompido ou desabilitado, os aplicativos não poderão acessar arquivos criptografados.            
|   **Nome do serviço**  |  EFS            
|   **Instalação**  |  Sempre instalado           
|   **StartType**   |  Manual           
|   **Recomendação**  | Sem diretrizes           
|   **Comentários**   |
|||                 



## <a name="enterprise-app-management-service"></a>Serviço de Gerenciamento de Aplicativos Empresariais            

| | |           
|---|---|       
|   **Descrição do serviço** |   Permite o gerenciamento de aplicativos empresariais.
|   **Nome do serviço**    |   EntAppSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="extensible-authentication-protocol"></a>Protocolo EAP (Extensible Authentication Protocol)           

| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço Protocolo EAP (Extensible Authentication Protocol) fornece autenticação de rede em cenários como 802.1x com e sem fio, VPN e NAP (Proteção de Acesso à Rede).  O EAP também fornece APIs (interfaces de programação de aplicativo) que são usadas por clientes de acesso à rede, incluindo clientes sem fio e VPN, durante o processo de autenticação.  Se você desabilitar esse serviço, este computador será impedido de acessar redes que exijam a autenticação EAP.
|   **Nome do serviço**    |   EapHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="function-discovery-provider-host"></a>Host de Provedor da Descoberta de Função         

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço FDPHOST hospeda os provedores de descoberta de rede de FD (Descoberta de Função). Esses provedores de FD fornecem serviços de descoberta de rede para os protocolos SSDP (Protocolo de Descoberta de Serviços Simples) e WS-D (Serviços Web – Descoberta). A interrupção ou a desabilitação do serviço FDPHOST desabilitará a descoberta de rede para esses protocolos ao usar o FD. Quando esse serviço não estiver disponível, os serviços de rede que usam o FD e dependem desses protocolos de descoberta não poderão encontrar recursos nem dispositivos de rede.
|   **Nome do serviço**    |   fdPHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="function-discovery-resource-publication"></a>Publicação de Recursos da Descoberta de Função      

| | |           
|---|---|       
|   **Descrição do serviço** |   Publica este computador e os recursos anexados a ele, de modo que eles possam ser descobertos na rede.  Se esse serviço for interrompido, os recursos de rede deixarão de ser publicados e não serão descobertos por outros computadores na rede.
|   **Nome do serviço**    |   FDResPub
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="geolocation-service"></a>Serviço de Geolocalização          

| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço monitora a localização atual do sistema e gerencia cercas geográficas (uma localização geográfica com eventos associados).  Se você desligar esse serviço, os aplicativos não poderão usar nem receber notificações para a geolocalização ou cercas geográficas.
|   **Nome do serviço**    |   lfsvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   A desabilitação interrompe aplicativos que dependem do serviço; pode ser desabilitado se os aplicativos não dependem dele
|||         



##  <a name="group-policy-client"></a>Cliente de Diretiva de Grupo     

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço é responsável por aplicar as configurações definidas por administradores ao computador e aos usuários por meio do componente de Política de Grupo. Se o serviço for desabilitado, as configurações não serão aplicadas e os aplicativos e os componentes não serão gerenciáveis por meio da Política de Grupo. Os componentes ou os aplicativos que dependem do componente de Política de Grupo poderão não funcionar se o serviço for desabilitado.
|   **Nome do serviço**    |   gpsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         




## <a name="human-interface-device-service"></a>Serviço de Dispositivos de Interface Humana           

| | |           
|---|---|       
|   **Descrição do serviço** |   Ativa e mantém o uso de botões mais acessados em teclados, controles remotos e outros dispositivos de multimídia. É recomendável que você mantenha esse serviço em execução.
|   **Nome do serviço**    |   hidserv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="hv-host-service"></a>Serviço de Host HV     

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece uma interface ao hipervisor do Hyper-V para fornecer contadores de desempenho por partição ao sistema operacional do host.
|   **Nome do serviço**    |   HvHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Aperfeiçoamento de desempenho para VMs convidadas. Não usado hoje, exceto para VMs explicitamente populadas, mas será usado no Application Guard
|||         



## <a name="hyper-v-data-exchange-service"></a>Serviço de Troca de Dados do Hyper-V        

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece um mecanismo para trocar dados entre a máquina virtual e o sistema operacional executado no computador físico.
|   **Nome do serviço**    |   vmickvpexchange
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||         



## <a name="hyper-v-guest-service-interface"></a>Interface de Serviço de Convidado do Hyper-V          

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece uma interface ao host Hyper-V para interagir com serviços específicos em execução na máquina virtual.
|   **Nome do serviço**    |   vmicguestinterface
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||         



## <a name="hyper-v-guest-shutdown-service"></a>Serviço de Desligamento de Convidado do Hyper-V           

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um mecanismo para desligar o sistema operacional desta máquina virtual das interfaces de gerenciamento no computador físico.
|   **Nome do serviço**    |   vmicshutdown
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||         



## <a name="hyper-v-heartbeat-service"></a>Serviço de Pulsação do Hyper-V
| | |
|---|---|
|   **Descrição do serviço** |   Monitora o estado desta máquina virtual relatando uma pulsação em intervalos regulares. Esse serviço ajuda você a identificar as máquinas virtuais em execução que pararam de responder.
|   **Nome do serviço**    |   vmicheartbeat
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||



## <a name="hyper-v-powershell-direct-service"></a>Serviço do PowerShell Direct do Hyper-V            

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um mecanismo para gerenciar a máquina virtual com o PowerShell por meio da sessão de VM sem uma rede virtual.
|   **Nome do serviço**    |   vmicvmsession
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||         



## <a name="hyper-v-remote-desktop-virtualization-service"></a>Serviço de Virtualização de Área de Trabalho Remota do Hyper-V            

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece uma plataforma para a comunicação entre a máquina virtual e o sistema operacional executado no computador físico.
|   **Nome do serviço**    |   vmicrdv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||         



## <a name="hyper-v-time-synchronization-service"></a>Serviço de Sincronização de Data/Hora do Hyper-V         

| | |           
|---|---|       
|   **Descrição do serviço** |   Sincroniza a hora do sistema desta máquina virtual com a hora do sistema do computador físico.
|   **Nome do serviço**    |   vmictimesync
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||         



## <a name="hyper-v-volume-shadow-copy-requestor"></a>Solicitante de Cópia de Sombra de Volume do Hyper-V         

| | |           
|---|---|           
|   **Descrição do serviço** |   Coordena a comunicação que deve usar o Serviço de Cópias de Sombra de Volume para fazer backup de aplicativos e dados nesta máquina virtual do sistema operacional no computador físico.
|   **Nome do serviço**    |   vmicvss
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Confira HvHost
|||         



## <a name="ike-and-authip-ipsec-keying-modules"></a>Módulos de Chave IKE e AuthIP IPsec          

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço IKEEXT hospeda os módulos de chave do IKE (protocolo IKE) e do AuthIP (protocolo Authenticated IP). Esses módulos de chave são usados para autenticação e troca de chaves no IPsec (protocolo IPsec). A interrupção ou a desabilitação do serviço IKEEXT desabilitará a troca de chaves do IKE e do AuthIP com computadores pares. O IPsec é normalmente configurado para usar o IKE ou o AuthIP; portanto, a interrupção ou a desabilitação do serviço IKEEXT poderá resultar em uma falha do IPsec e comprometer a segurança do sistema. É altamente recomendável que você tenha o serviço IKEEXT em execução.
|   **Nome do serviço**    |   IKEEXT
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |    
|||         



## <a name="interactive-services-detection"></a>Detecção de Serviços Interativos           

| | |           
|---|---|   
|   **Descrição do serviço** |   Habilita a notificação e a entrada de usuário para serviços interativos, o que permite o acesso às caixas de diálogo criadas por serviços interativos quando elas são exibidas. Se esse serviço for interrompido, as notificações de novas caixas de diálogo de serviços interativos deixarão de funcionar e poderá não haver acesso às caixas de diálogo de serviços interativos. Se esse serviço for desabilitado, as notificações de novas caixas de diálogo de serviços interativos e o acesso a elas deixarão de funcionar.
|   **Nome do serviço**    |   UI0Detect
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="internet-connection-sharing-ics"></a>ICS (Compartilhamento de Conexão com a Internet)            

| | |           
|---|---|           
|   **Descrição do serviço** |   Fornece conversão de endereços de rede, endereçamento, serviços de prevenção contra intrusões e/ou resolução de nomes para uma rede de escritório em casa ou pequena empresa.
|   **Nome do serviço**    |   SharedAccess
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Necessário para clientes usados como hotspots Wi-Fi e também em ambas as extremidades da projeção Miracast. O ICS pode ser bloqueado com a configuração de GPO "Proibir o uso do Compartilhamento de Conexão com a Internet na rede de domínio DNS"
|||         



## <a name="ip-helper"></a>Auxiliar de IP            

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece conectividade de túnel usando tecnologias de transição IPv6 (6to4, ISATAP, Proxy de Porta e Teredo) e IP-HTTPS. Se esse serviço for interrompido, o computador não terá os benefícios de conectividade aprimorada oferecidos por essas tecnologias.
|   **Nome do serviço**    |   iphlpsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         




##  <a name="ipsec-policy-agent"></a>Agente de Política IPsec      

| | |           
|---|---|       
|   **Descrição do serviço** |   O IPsec (protocolo IPsec) dá suporte à autenticação de par no nível da rede, à autenticação de origem de dados, à integridade dos dados, à confidencialidade dos dados (criptografia) e à proteção contra reprodução.  Esse serviço impõe políticas IPsec criadas por meio do snap-in Políticas de Segurança IP ou da ferramenta de linha de comando "netsh ipsec".  Se você interromper esse serviço, poderá ter problemas de conectividade de rede, caso a política exija o uso do IPsec nas conexões.  Além disso, o gerenciamento remoto do Firewall do Windows não está disponível quando esse serviço é interrompido.
|   **Nome do serviço**    |   PolicyAgent
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="kdc-proxy-server-service-kps"></a>Serviço KPS (Servidor Proxy do KDC)      

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço Servidor Proxy do KDC é executado em servidores de borda para usar um proxy a fim de enviar mensagens do protocolo Kerberos para controladores de domínio na rede corporativa.
|   **Nome do serviço**    |   KPSSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes    
|   **Comentários**    |   
|||         



## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KtmRm para Coordenador de Transações Distribuídas            

| | |           
|---|---|       
|   **Descrição do serviço** |   Coordena as transações entre o MSDTC (Coordenador de Transações Distribuídas) e o KTM (Gerenciador de Transação de Kernel). Se ele não for necessário, será recomendável que esse serviço permaneça interrompido. Se ele for necessário, o MSDTC e o KTM iniciarão esse serviço automaticamente. Se esse serviço for desabilitado, todas as transações do MSDTC que interagem com um Resource Manager de Kernel falharão e todos os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   KtmRm
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="link-layer-topology-discovery-mapper"></a>Mapeador da Descoberta de Topologia da Camada de Link        

| | |       
|---|---|       
|   **Descrição do serviço** |   Cria um Mapa de Rede, consistindo nas informações (conectividade) de topologia do computador e dispositivo e nos metadados que descrevem cada computador e dispositivo.  Se esse serviço for desabilitado, o Mapa de Rede não funcionará corretamente.
|   **Nome do serviço**    |   lltdsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Pode ser desabilitado se não há dependências do Mapa de Rede
|||         



## <a name="local-session-manager"></a>Gerenciador de Sessão Local                    

| | |                   
|---|---|   
|   **Descrição do serviço** |   Serviço Windows principal que gerencia as sessões de usuário local. A interrupção ou a desabilitação desse serviço resultará na instabilidade do sistema.    
|   **Nome do serviço**    |   LSM |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Automática   |
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||                 



## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Coletor Padrão do Hub de Diagnóstico da Microsoft (R)         

| | |           
|---|---|           
|   **Descrição do serviço** |   Serviço Windows principal que gerencia as sessões de usuário local. A interrupção ou a desabilitação desse serviço resultará na instabilidade do sistema.
|   **Nome do serviço**    |   LSM
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="microsoft-account-sign-in-assistant"></a>Assistente de Credenciais de Conta Microsoft
| | |
|---|---|
|   **Descrição do serviço** |   Habilita as credenciais do usuário por meio de serviços de identidade de conta Microsoft. Se esse serviço for interrompido, os usuários não poderão fazer logon no computador com suas contas Microsoft.
|   **Nome do serviço**    |   wlidsvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   As contas Microsoft não estão disponíveis no Windows Server
|||



##  <a name="microsoft-app-v-client"></a>Cliente Microsoft App-V      

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia os usuários e os aplicativos virtuais do App-V
|   **Nome do serviço**    |   AppVClient
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



## <a name="microsoft-iscsi-initiator-service"></a>Serviço Iniciador Microsoft iSCSI            

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as sessões do iSCSI (Internet SCSI) deste computador para dispositivos de destino iSCSI remotos. Se esse serviço for interrompido, este computador não poderá fazer logon nem acessar destinos iSCSI. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   MSiSCSI
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Nossos dados de diagnóstico indicam que isso é usado no cliente, bem como no servidor. Não há nenhum benefício em desabilitá-lo.
|||         



## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece isolamento de processo para as chaves de criptografia usadas para autenticação nos provedores de identidade associados de um usuário. Se esse serviço for desabilitado, todos os usos e o gerenciamento dessas chaves não estarão disponíveis, o que inclui o logon de computador e o logon único para aplicativos e sites. Esse serviço é iniciado e interrompido automaticamente. É recomendável que você não reconfigure esse serviço.
|   **Nome do serviço**    |   NgcSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Necessário para logons com PIN/Hello, que não são compatíveis com o Server
|||         



## <a name="microsoft-passport-container"></a>Contêiner do Microsoft Passport         

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as chaves de identidade do usuário local usadas para autenticar o usuário em provedores de identidade, bem como os cartões inteligentes virtuais de TPM. Se esse serviço for desabilitado, as chaves de identidade do usuário local e os cartões inteligentes virtuais de TPM não estarão acessíveis. É recomendável que você não reconfigure esse serviço.
|   **Nome do serviço**    |   NgcCtnrSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="microsoft-software-shadow-copy-provider"></a>Provedor de Cópias de Sombra de Software da Microsoft          

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as cópias de sombra de volume baseadas em software feitas pelo Serviço de Cópias de Sombra de Volume. Se esse serviço for interrompido, as cópias de sombra de volume baseadas em software não poderão ser gerenciadas. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   swprv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="microsoft-storage-spaces-smp"></a>SMP dos Espaços de Armazenamento da Microsoft         

| | |           
|---|---|       
|   **Descrição do serviço** |   Serviço de host para o provedor de gerenciamento dos Espaços de Armazenamento do Microsoft. Se esse serviço for interrompido ou desabilitado, os Espaços de Armazenamento não poderão ser gerenciados.
|   **Nome do serviço**    |   smphost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   As APIs de gerenciamento de armazenamento falharão sem esse serviço. Exemplo: "Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage".
|||         



## <a name="nettcp-port-sharing-service"></a>Serviço de Compartilhamento de Porta Net.Tcp         

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a capacidade de compartilhar portas TCP no protocolo net.tcp.
|   **Nome do serviço**    |   NetTcpPortSharing
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



## <a name="netlogon"></a>Netlogon         

| | |           
|---|---|           
|   **Descrição do serviço** |   Mantém um canal de segurança entre este computador e o controlador de domínio para autenticar usuários e serviços. Se esse serviço for interrompido, o computador poderá não autenticar usuários e serviços e o controlador de domínio não poderá registrar registros DNS. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   Netlogon
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="network-connection-broker"></a>Agente de Conexão de Rede            

| | |           
|---|---|       
|   **Descrição do serviço** |   Agencia conexões que permitem que os Aplicativos da Microsoft Store recebam notificações da Internet.
|   **Nome do serviço**    |   NcbService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



##  <a name="network-connections"></a>Conexões de Rede         

| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia objetos na pasta Conexões Discada e de Rede, na qual você pode exibir as conexões remotas e de rede local.
|   **Nome do serviço**    |   Netman
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="network-connectivity-assistant"></a>Assistente de Conectividade de Rede      

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece notificação de status do DirectAccess para componentes de interface do usuário
|   **Nome do serviço**    |   NcaSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="network-list-service"></a>Serviço da Lista de Redes        

| | |           
|---|---|   
|   **Descrição do serviço** |   Identifica as redes ao qual o computador se conectou, coleta e armazena as propriedades dessas redes e notifica os aplicativos quando essas propriedades são alteradas.
|   **Nome do serviço**    |   netprofm
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="network-location-awareness"></a>Reconhecimento de localizações de rede           

| | |           
|---|---|       
|   **Descrição do serviço** |   Coleta e armazena informações de configuração da rede e notifica os programas quando essas informações são modificadas. Se esse serviço for interrompido, as informações de configuração poderão não estar disponíveis. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   NlaSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="network-setup-service"></a>Serviço de Configuração de Rede       

| | |           
|---|---|       
|   **Descrição do serviço** |   O Serviço de Configuração de Rede gerencia a instalação de drivers de rede e permite a definição das configurações de rede de baixo nível.  Se esse serviço for interrompido, as instalações de driver que estiverem em andamento poderão ser canceladas.
|   **Nome do serviço**    |   NetSetupSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="network-store-interface-service"></a>Serviço de interface NSI      

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço fornece notificações de rede (por exemplo, adição/exclusão de interface etc.) para clientes no modo de usuário. A interrupção desse serviço causará a perda de conectividade de rede. Se esse serviço for desabilitado, todos os outros serviços que dependem explicitamente desse serviço não serão iniciados.
|   **Nome do serviço**    |   nsi
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="offline-files"></a>Arquivos Offline            

| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço Arquivos Offline executa atividades de manutenção no cache dos Arquivos Offline, responde a eventos de logon e logoff do usuário, implementa as informações internas da API pública e expede eventos interessantes para aqueles interessados nas atividades dos Arquivos Offline e nas alterações no estado do cache.
|   **Nome do serviço**    |   CscService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



## <a name="optimize-drives"></a>Otimizar unidades          

| | |           
|---|---|   
|   **Descrição do serviço** |   Ajuda o computador a ser executado com mais eficiência pela otimização de arquivos em unidades de armazenamento.
|   **Nome do serviço**    |   defragsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="performance-counter-dll-host"></a>Host de DLL de Contador de Desempenho         

| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que usuários remotos e processos de 64 bits consultem os contadores de desempenho fornecidos por DLLs de 32 bits. Se esse serviço for interrompido, apenas os usuários locais e os processos de 32 bits poderão consultar os contadores de desempenho fornecidos por DLLs de 32 bits.
|   **Nome do serviço**    |   PerfHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes    
|   **Comentários**    |   
|||         



## <a name="performance-logs--alerts"></a>Logs e Alertas de Desempenho            

| | |           
|---|---|   
|   **Descrição do serviço** |   Logs e Alertas de Desempenho Coleta dados de desempenho de computadores locais ou remotos com base em parâmetros de agendamento pré-configurados e, em seguida, grava os dados em um log ou dispara um alerta. Se esse serviço for interrompido, as informações de desempenho não serão coletadas. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   pla
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="phone-service"></a>Serviço de Telefone       

| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia o estado de telefonia no dispositivo
|   **Nome do serviço**    |   PhoneSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Usado por aplicativos modernos de VoIP
|||         



##      <a name="plug-and-play"></a>Plug and Play       

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que um computador reconheça e se adapte às alterações de hardware com pouca ou nenhuma entrada de usuário. A interrupção ou a desabilitação desse serviço resultará na instabilidade do sistema.
|   **Nome do serviço**    |   PlugPlay
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="portable-device-enumerator-service"></a>Serviço Enumerador de Dispositivos Portáteis           

| | |           
|---|---|       
|   **Descrição do serviço** |   Impõe a Política de Grupo para dispositivos removíveis de armazenamento em massa. Permite que aplicativos, como o Windows Media Player e o Assistente de Importação de Imagens, transfiram e sincronizem o conteúdo usando dispositivos removíveis de armazenamento em massa.
|   **Nome do serviço**    |   WPDBusEnum
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="power"></a>Potência            

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia a política de energia e a entrega de notificação da política de energia.
|   **Nome do serviço**    |   Potência
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="print-spooler"></a>Spooler de impressão            

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço armazena no spool trabalhos de impressão e manipula a interação com a impressora.  Se você desligar esse serviço, não conseguirá imprimir nem ver as impressoras.
|   **Nome do serviço**    |   Spooler
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Pode ser desabilitado se não é um servidor de impressão nem um controlador de domínio
|   **Comentários**    |   Em um controlador de domínio, a instalação da função de controlador de domínio adiciona um thread ao serviço de spooler responsável por executar a remoção de impressão – remoção dos objetos obsoletos da fila de impressão do Active Directory.  Se o serviço de spooler não estiver em execução em, pelo menos, um controlador de domínio em cada site, o AD não terá meios para remover as filas antigas que não existem mais. [https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/](https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/ )
|||         



##  <a name="printer-extensions-and-notifications"></a>Extensões e Notificações da Impressora        

| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço abre caixas de diálogo personalizadas da impressora e manipula as notificações de uma impressora ou um servidor de impressão remoto. Se você desligar esse serviço, não conseguirá ver as extensões nem as notificações da impressora.
|   **Nome do serviço**    |   PrintNotify
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado se não é um servidor de impressão
|   **Comentários**    |   
|||         



##  <a name="problem-reports-and-solutions-control-panel-support"></a>Suporte do Painel de Controle Relatórios de Problemas e Soluções     

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço fornece suporte para exibição, envio e exclusão de relatórios de problemas no nível do sistema para o painel de controle Relatórios de Problemas e Soluções.
|   **Nome do serviço**    |   wercplsupport
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="program-compatibility-assistant-service"></a>Serviço Auxiliar de Compatibilidade de Programas     

| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço fornece suporte para o ACP (Auxiliar de Compatibilidade de Programas).  O ACP monitora os programas instalados e executados pelo usuário e detecta problemas de compatibilidade conhecidos. Se esse serviço for interrompido, o ACP não funcionará corretamente.
|   **Nome do serviço**    |   PcaSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



##  <a name="quality-windows-audio-video-experience"></a>Quality Windows Audio-Video Experience      

| | |           
|---|---|   
|   **Descrição do serviço** |   O qWave (Quality Windows Audio-Video Experience) é uma plataforma de rede para aplicativos de streaming AV (Áudio/Vídeo) em redes domésticas IP. O qWave aprimora o desempenho e a confiabilidade de streaming AV, garantindo a QoS (Qualidade de Serviço) de rede para aplicativos AV. Ele fornece mecanismos para controle de admissão, monitoramento e imposição em tempo de execução, comentários sobre o aplicativo e priorização de tráfego.
|   **Nome do serviço**    |   QWAVE
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Serviço QoS do lado do cliente
|||         



##      <a name="radio-management-service"></a>Serviço de Gerenciamento de Rádio        

| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço de Gerenciamento de Rádio e Modo Avião
|   **Nome do serviço**    |   RmSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="remote-access-auto-connection-manager"></a>Gerenciador de Conexões de Acesso Remoto Automático            

| | |           
|---|---|   
|   **Descrição do serviço** |   Cria uma conexão com uma rede remota sempre que um programa referencia um DNS remoto, um nome NetBIOS ou um endereço.
|   **Nome do serviço**    |   RasAuto
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="remote-access-connection-manager"></a>Gerenciador de Conexões de Acesso Remoto         

| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia as conexões discadas e de VPN (rede virtual privada) deste computador com a Internet ou outras redes remotas. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   RasMan
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="remote-desktop-configuration"></a>Configuração da Área de Trabalho Remota         

| | |           
|---|---|   
|   **Descrição do serviço** |   O RDCS (serviço de Configuração da Área de Trabalho Remota) é responsável por todos os Serviços de Área de Trabalho Remota e as atividades de manutenção de sessão e configuração relacionadas à Área de Trabalho Remota que exigem o contexto do sistema. Isso inclui pastas temporárias por sessão, além de temas e certificados de Área de Trabalho Remota.
|   **Nome do serviço**    |   SessionEnv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   
|||         



## <a name="remote-desktop-services"></a>Serviços da Área de Trabalho Remota          

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que os usuários se conectem de maneira interativa a um computador remoto. A Área de Trabalho Remota e o servidor Host da Sessão da Área de Trabalho Remota dependem desse serviço.  Para impedir o uso remoto deste computador, desmarque as caixas de seleção na guia Remoto do item do painel de controle Propriedades do sistema.
|   **Nome do serviço**    |   TermService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   
|||         



##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirecionador de Portas do Modo de Usuário dos Serviços de Área de Trabalho Remota        

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite o redirecionamento de impressoras/unidades/portas para conexões RDP
|   **Nome do serviço**    |   UmRdpService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Dá suporte a redirecionamentos no servidor da conexão.
|||         



## <a name="remote-procedure-call-rpc"></a>RPC (Chamada de Procedimento Remoto)          

| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço RPCSS é o Gerenciador de Controle de Serviço para servidores COM e DCOM. Ele executa solicitações de ativações de objeto, resoluções de exportador de objeto e coleta de lixo distribuída para servidores COM e DCOM. Se esse serviço for interrompido ou desabilitado, os programas que usam o COM ou o DCOM não funcionarão corretamente. É altamente recomendável que você tenha o serviço RPCSS em execução.
|   **Nome do serviço**    |   RpcSs
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="remote-procedure-call-rpc-locator"></a>Localizador RPC (chamada de procedimento remoto)             

| | |               
|---|---|   
|   **Descrição do serviço** |   No Windows 2003 e em versões anteriores do Windows, o serviço Localizador RPC (chamada de procedimento remoto) gerencia o banco de dados do serviço de nomes RPC. No Windows Vista e em versões posteriores do Windows, esse serviço não fornece nenhuma funcionalidade e está presente para compatibilidade do aplicativo.   |
|   **Nome do serviço**    |   RpcLocator  |
|   **Instalação**    |   Somente com a Experiência Desktop    |
|   **StartType**   |   Manual  |
|   **Recomendação**  | Sem diretrizes   |
|   **Comentários**    |       |
|||             



## <a name="remote-registry"></a>Registro Remoto          

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que usuários remotos modifiquem as configurações do Registro neste computador. Se esse serviço for interrompido, o Registro só poderá ser modificado por usuários neste computador. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   RemoteRegistry
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   
|||         



##  <a name="resultant-set-of-policy-provider"></a>Conjunto Resultante do Provedor de Políticas            

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um serviço de rede que processa solicitações para simular a aplicação das configurações da Política de Grupo a um usuário ou um computador de destino em várias situações e calcula as configurações do Conjunto de Políticas Resultante.
|   **Nome do serviço**    |   RSoPProv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |Sem diretrizes    
|   **Comentários**    |   
|||         



## <a name="routing-and-remote-access"></a>Roteamento e Acesso Remoto            

| | |           
|---|---|   
|   **Descrição do serviço** |   Oferece serviços de roteamento a empresas em ambientes de rede local e rede de longa distância.
|   **Nome do serviço**    |   RemoteAccess
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   Já desabilitado
|||         



## <a name="rpc-endpoint-mapper"></a>Mapeador de ponto de extremidade RPC          

| | |           
|---|---|   
|   **Descrição do serviço** |   Resolve identificadores de interfaces RPC em pontos de extremidade de transporte. Se esse serviço for interrompido ou desabilitado, os programas que usam os serviços RPC (chamada de procedimento remoto) não funcionarão corretamente.
|   **Nome do serviço**    |   RpcEptMapper
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="secondary-logon"></a>Logon Secundário     

| | |           
|---|---|       
|   **Descrição do serviço** |   Permite a inicialização de processos com credenciais alternativas. Se esse serviço for interrompido, esse tipo de acesso de logon não estará disponível. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   seclogon
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="secure-socket-tunneling-protocol-service"></a>Serviço Protocolo SSTP            

| | |               
|---|---|       
|   **Descrição do serviço** |   Fornece suporte ao Protocolo SSTP para se conectar a computadores remotos usando a VPN. Se esse serviço for desabilitado, os usuários não poderão usar o SSTP para acessar servidores remotos.    |
|   **Nome do serviço**    |   SstpSvc |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Não desabilitar  |
|   **Comentários**    |   A desabilitação interrompe o RRAS   |
|||             



## <a name="security-accounts-manager"></a>Gerenciador de Contas de Segurança            

| | |           
|---|---|       
|   **Descrição do serviço** |   A inicialização desse serviço indica a outros serviços que o SAM (Gerenciador de Contas de Segurança) está pronto para aceitar solicitações.  A desabilitação desse serviço impedirá que outros serviços do sistema sejam notificados quando o SAM estiver pronto, o que, por sua vez, poderá fazer com que esses serviços não sejam iniciados corretamente. Esse serviço não deve ser desabilitado.
|   **Nome do serviço**    |   SamSs
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Não desabilitar
|   **Comentários**    |   
|||         



## <a name="sensor-data-service"></a>Serviço de Dados de Sensor  

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece dados de uma variedade de sensores
|   **Nome do serviço**    |   SensorDataService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="sensor-monitoring-service"></a>Serviço de monitoramento de sensor            

| | |           
|---|---|       
|   **Descrição do serviço** |   Monitora vários sensores para expor os dados e adaptar-se ao estado do sistema e do usuário.  Se esse serviço for interrompido ou desabilitado, o brilho da tela não se adaptará às condições de iluminação. A interrupção desse serviço também poderá afetar outros recursos e outras funcionalidades do sistema.
|   **Nome do serviço**    |   SensrSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         

## <a name="sensor-service"></a>Serviço de Sensor

| | |
|---|---|
|   **Descrição do serviço** |   Um serviço para sensores que gerencia a funcionalidade de diferentes sensores. Gerencia o SDO (Orientação de Dispositivo Simples) e o Histórico dos sensores. Carrega o sensor de SDO que relata alterações de orientação do dispositivo.  Se esse serviço for interrompido ou desabilitado, o sensor de SDO não será carregado e, portanto, a rotação automática não ocorrerá. A coleta do histórico dos sensores também será interrompida.
|   **Nome do serviço**    |   SensorService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |
|||

## <a name="server"></a>Servidor           

| | |           
|---|---|   
|   **Descrição do serviço** |   Dá suporte ao compartilhamento de arquivos, impressões e pipes nomeados na rede deste computador. Se esse serviço for interrompido, essas funções não estarão disponíveis. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   LanmanServer
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Necessário para gerenciamento remoto, IPC$ e compartilhamento de arquivos SMB
|||         



## <a name="shell-hardware-detection"></a>Detecção do Hardware do Shell             

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece notificações de eventos de hardware de Reprodução Automática.
|   **Nome do serviço**    |   ShellHWDetection
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="smart-card"></a>Cartão inteligente           

| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia o acesso aos cartões inteligentes lidos por este computador. Se esse serviço for interrompido, esse computador não poderá ler cartões inteligentes. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   SCardSvr
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



## <a name="smart-card-device-enumeration-service"></a>Serviço de Enumeração de Dispositivos de Cartão Inteligente                    

| | |               
|---|---|       
|   **Descrição do serviço** |   Cria nós de dispositivo de software para todos os leitores de cartão inteligente acessíveis para uma sessão especificada. Se esse serviço for desabilitado, as APIs do WinRT não poderão enumerar leitores de cartão inteligente.   |
|   **Nome do serviço**    |   ScDeviceEnum    |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Pode ser desabilitado   |
|   **Comentários**    |   Necessário quase que exclusivamente para aplicativos do WinRT    |
|||             



## <a name="smart-card-removal-policy"></a>Política de Remoção de Cartão Inteligente        

| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que o sistema seja configurado para bloquear a área de trabalho do usuário após a remoção do cartão inteligente.
|   **Nome do serviço**    |   SCPolicySvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="snmp-trap"></a>Interceptação SNMP            

| | |           
|---|---|       
|   **Descrição do serviço** |   Recebe mensagens de interceptação geradas por agentes do protocolo SNMP locais ou remotos e encaminha as mensagens aos programas de gerenciamento do SNMP em execução neste computador. Se esse serviço for interrompido, os programas baseados no SNMP neste computador não receberão mensagens de interceptação SNMP. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   SNMPTRAP
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="software-protection"></a>Proteção de Software             

| | |           
|---|---|       
|   **Descrição do serviço** |   Permite o download, a instalação e a imposição de licenças digitais para o Windows e aplicativos do Windows. Se o serviço for desabilitado, o sistema operacional e os aplicativos licenciados poderão ser executados em um modo de notificação. É altamente recomendável que você não desabilite o serviço Proteção de Software.
|   **Nome do serviço**    |   sppsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="special-administration-console-helper"></a>Assistente do Console de Administração Especial        

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que os administradores acessem um prompt de comando remotamente usando os serviços de gerenciamento de emergência.
|   **Nome do serviço**    |   sacsvr
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="spot-verifier"></a>Verificador de Pontos            

| | |           
|---|---|   
|   **Descrição do serviço** |   Verifica possíveis danos no sistema de arquivos.
|   **Nome do serviço**    |   svsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="ssdp-discovery"></a>Descoberta SSDP           

| | |           
|---|---|   
|   **Descrição do serviço** |   Descobre os dispositivos de rede e os serviços que usam o protocolo de descoberta SSDP, como dispositivos UPnP. Também anuncia os dispositivos SSDP e os serviços em execução no computador local. Se esse serviço for interrompido, os dispositivos baseados no SSDP não serão descobertos. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   SSDPSRV
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="state-repository-service"></a>Serviço de Repositório de Estado         

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece o suporte de infraestrutura necessário para o modelo de aplicativo.
|   **Nome do serviço**    |   StateRepository
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="still-image-acquisition-events"></a>Eventos de Aquisição de Imagens Estáticas

| | |           
|---|---|   
|   **Descrição do serviço** |   Inicia os aplicativos associados aos eventos de aquisição de imagens estáticas.
|   **Nome do serviço**    |   WiaRpc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="storage-service"></a>Serviço de Armazenamento          

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece serviços de ativação para configurações de armazenamento e expansão de armazenamento externo
|   **Nome do serviço**    |   StorSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="storage-tiers-management"></a>Gerenciamento de Camadas de Armazenamento        

| | |           
|---|---|   
|   **Descrição do serviço** |   Otimiza a colocação de dados em camadas de armazenamento em todos os espaços de armazenamento em camadas no sistema.
|   **Nome do serviço**    |   TieringEngineService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="superfetch"></a>Superfetch          

| | |           
|---|---|       
|   **Descrição do serviço** |   Mantém e melhora o desempenho do sistema ao longo do tempo.
|   **Nome do serviço**    |   SysMain
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="sync-host"></a>Host de Sincronização            

| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço sincroniza emails, contatos, calendário e vários outros dados do usuário. Os aplicativos de email e outros aplicativos dependentes dessa funcionalidade não funcionarão corretamente quando esse serviço não estiver em execução.
|   **Nome do serviço**    |   OneSyncSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Modelo de serviço de usuário
|||         



## <a name="system-event-notification-service"></a>Serviço de Notificação de Eventos do Sistema            

| | |           
|---|---|       
|   **Descrição do serviço** |   Monitora eventos do sistema e notifica os assinantes do Sistema de Eventos COM+ desses eventos.
|   **Nome do serviço**    |   SENS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="system-events-broker"></a>Agente de Eventos do Sistema             

| | |           
|---|---|       
|   **Descrição do serviço** |   Coordena a execução do trabalho em segundo plano para o aplicativo do WinRT. Se esse serviço for interrompido ou desabilitado, o trabalho em segundo plano poderá não ser disparado.
|   **Nome do serviço**    |   SystemEventsBroker
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Apesar do fato de sua descrição implicar que ele só se destine a aplicativos do WinRT, ele é necessário para o Agendador de Tarefas, o serviço de infraestrutura do agente e outros componentes internos.
|||         



## <a name="task-scheduler"></a>Agendador de Tarefas           

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que um usuário configure e agende tarefas automatizadas neste computador. O serviço também hospeda várias tarefas críticas do sistema do Windows. Se esse serviço for interrompido ou desabilitado, essas tarefas não serão executadas nos horários agendados. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   Agendamento
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="tcpip-netbios-helper"></a>Auxiliar NetBIOS TCP/IP            

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte ao serviço NetBT (NetBIOS sobre TCP/IP) e à resolução de nomes NetBIOS para clientes na rede, permitindo, portanto, que os usuários compartilhem arquivos, façam impressões e façam logon na rede. Se esse serviço for interrompido, essas funções poderão não estar disponíveis. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   lmhosts
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="telephony"></a>Telefonia           

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece suporte à TAPI (API de telefonia) para programas que controlam dispositivos de telefonia no computador local e, pela LAN, em servidores que também estejam executando o serviço.
|   **Nome do serviço**    |   TapiSrv
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   A desabilitação interrompe o RRAS
|||         



## <a name="themes"></a>Temas           

| | |           
|---|---|
|   **Descrição do serviço** |   Fornece gerenciamento de temas de experiência do usuário.
|   **Nome do serviço**    |   Temas
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Não é possível definir temas de acessibilidade quando esse serviço é desabilitado
|||         



## <a name="tile-data-model-server"></a>Servidor de modelo de Dados de Bloco           

| | |           
|---|---|   
|   **Descrição do serviço** |   Servidor de Bloco para atualizações de bloco.
|   **Nome do serviço**    |   tiledatamodelsvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   O menu Iniciar é interrompido se esse serviço é desabilitado
|||         



##  <a name="time-broker"></a>Agente de Tempo     

| | |           
|---|---|       
|   **Descrição do serviço** |   Coordena a execução do trabalho em segundo plano para o aplicativo do WinRT. Se esse serviço for interrompido ou desabilitado, o trabalho em segundo plano poderá não ser disparado.
|   **Nome do serviço**    |   TimeBrokerSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Apesar do fato de sua descrição implicar que ele só se destine a aplicativos do WinRT, ele é necessário para o Agendador de Tarefas, o serviço de infraestrutura do agente e outros componentes internos.
|||         



## <a name="touch-keyboard-and-handwriting-panel-service"></a>Serviço de Teclado Virtual e Painel de Manuscrito         

| | |           
|---|---|   
|   **Descrição do serviço** |   Habilita a funcionalidade de caneta e tinta do Teclado Virtual e do Painel de Manuscrito
|   **Nome do serviço**    |   TabletInputService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="update-orchestrator-service-for-windows-update"></a>Serviço de Atualizações do Orchestrator para Windows Update           

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as Atualizações do Windows. Se ele for interrompido, os dispositivos não poderão baixar nem instalar as atualizações mais recentes.
|   **Nome do serviço**    |   UsoSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   A descrição do serviço estava ausente na v1607; o Windows Update (incluindo o WSUS) depende desse serviço.
|||         



## <a name="upnp-device-host"></a>Host de Dispositivo UPnP         

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que os dispositivos UPnP sejam hospedados neste computador. Se esse serviço for interrompido, todos os dispositivos UPnP hospedados deixarão de funcionar e nenhum outro dispositivo hospedado poderá ser adicionado. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   upnphost
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="user-access-logging-service"></a>Serviço de Log de Acesso do Usuário          

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço registra em log as solicitações de acesso de cliente exclusivo, na forma de endereços IP e nomes de usuário, de produtos instalados e funções no servidor local. Essas informações podem ser consultadas, por meio do PowerShell, por administradores que precisam quantificar a demanda de clientes do software para servidores para o gerenciamento de CAL (licença de acesso para cliente) offline. Se o serviço for desabilitado, as solicitações de cliente não serão registradas em log e não poderão ser recuperadas por meio de consultas do PowerShell. A interrupção do serviço não afetará a consulta de dados históricos (confira a documentação de suporte para obter as etapas para excluir os dados históricos). O administrador do sistema local precisa consultar seus termos de licença do Windows Server para determinar o número de CALs necessárias para que o software para servidores seja licenciado adequadamente; o uso dos dados e do serviço UAL não altera essa obrigação.
|   **Nome do serviço**    |   UALSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="user-data-access"></a>Acesso a Dados de Usuário        

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece acesso de aplicativos para dados estruturados de usuário, incluindo informações de contato, calendários, mensagens e outros tipos de conteúdo. Se você interromper ou desabilitar esse serviço, os aplicativos que usam esses dados poderão não funcionar corretamente.
|   **Nome do serviço**    |   UserDataSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Modelo de serviço de usuário
|||         



## <a name="user-data-storage"></a>Armazenamento de Dados de Usuário            

| | |           
|---|---|       
|   **Descrição do serviço** |   Manipula o armazenamento de dados estruturados de usuário, incluindo informações de contato, calendários, mensagens e outros tipos de conteúdo. Se você interromper ou desabilitar esse serviço, os aplicativos que usam esses dados poderão não funcionar corretamente.
|   **Nome do serviço**    |   UnistoreSvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Modelo de serviço de usuário
|||         



## <a name="user-experience-virtualization-service"></a>Serviço User Experience Virtualization           

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece suporte para o roaming de configurações de aplicativo e do sistema operacional
|   **Nome do serviço**    |   UevAgentService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



##  <a name="user-manager"></a>Gerenciador de Usuários        

| | |           
|---|---|   
|   **Descrição do serviço** |   O Gerenciador de Usuários fornece os componentes do runtime necessários para a interação de vários usuários.  Se esse serviço for interrompido, alguns aplicativos poderão não funcionar corretamente.
|   **Nome do serviço**    |   UserManager
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="user-profile-service"></a>Serviço Perfil do Usuário         

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço é responsável por carregar e descarregar perfis de usuário. Se esse serviço for interrompido ou desabilitado, os usuários não poderão mais entrar no serviço ou sair dele com êxito, os aplicativos poderão ter problemas em obter os dados dos usuários e os componentes registrados para receber notificações de eventos de perfil não os receberão.
|   **Nome do serviço**    |   ProfSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="virtual-disk"></a>Disco Virtual             

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de gerenciamento para discos, volumes, sistemas de arquivos e matrizes de armazenamento.
|   **Nome do serviço**    |   vds
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Sem diretrizes
|   **Comentários**    |   
|||         



## <a name="volume-shadow-copy"></a>Cópias de Sombra de Volume           

| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia e implementa Cópias de Sombra de Volume usadas para backup e outros fins. Se esse serviço for interrompido, as cópias de sombra não estarão disponíveis para backup e o backup poderá falhar. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   VSS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Sem diretrizes
|   **Comentários**    |   
|||         



##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **Descrição do serviço** |   Hospeda objetos usados pelos clientes da carteira
|   **Nome do serviço**    |   WalletService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="windows-audio"></a>Áudio do Windows            

| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia o áudio para programas baseados no Windows.  Se esse serviço for interrompido, os dispositivos de áudio e os efeitos não funcionarão corretamente.  Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados
|   **Nome do serviço**    |   Audiosrv
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="windows-audio-endpoint-builder"></a>Construtor de Pontos de Extremidade de Áudio do Windows           

| | |           
|---|---|
|   **Descrição do serviço** |   Gerencia os dispositivos de áudio para o serviço Áudio do Windows.  Se esse serviço for interrompido, os dispositivos de áudio e os efeitos não funcionarão corretamente.  Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados
|   **Nome do serviço**    |   AudioEndpointBuilder
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="windows-biometric-service"></a>Serviço de Biometria do Windows            

| | |           
|---|---|   
|   **Descrição do serviço** |   O Serviço de Biometria do Windows permite que os aplicativos cliente capturem, comparem, manipulem e armazenem dados biométricos, sem obter acesso direto a amostras ou nenhum hardware biométrico. O serviço está hospedado em um processo do SVCHOST privilegiado.
|   **Nome do serviço**    |   WbioSrvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="windows-camera-frame-server"></a>Servidor de Quadros de Câmera do Windows         

| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que vários clientes acessem os quadros de vídeo em dispositivos de câmera.
|   **Nome do serviço**    |   FrameServer
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="windows-connection-manager"></a>Gerenciador de Conexões do Windows           

| | |           
|---|---|   
|   **Descrição do serviço** |   Toma decisões automáticas de conexão/desconexão com base nas opções de conectividade de rede atualmente disponíveis para o computador e permite o gerenciamento da conectividade de rede com base nas configurações da Política de Grupo.
|   **Nome do serviço**    |   Wcmsvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-defender-network-inspection-service"></a>Serviço de Inspeção de Rede do Windows Defender          

| | |           
|---|---|       
|   **Descrição do serviço** |   Ajuda a proteger contra tentativas de intrusão direcionadas a vulnerabilidades conhecidas e recém-descobertas em protocolos de rede
|   **Nome do serviço**    |   WdNisSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes    
|   **Comentários**    |   
|||         



## <a name="windows-defender-service"></a>Serviço Windows Defender         

| | |           
|---|---|       
|   **Descrição do serviço** |   Ajuda a proteger os usuários contra malware e outros programas de software potencialmente indesejado
|   **Nome do serviço**    |   WinDefend
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation – Estrutura do Driver de Modo de Usuário           

| | |           
|---|---|   
|   **Descrição do serviço** |   Cria e gerencia os processos do driver de modo de usuário. Esse serviço não pode ser interrompido.
|   **Nome do serviço**    |   wudfsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-encryption-provider-host-service"></a>Serviço de Host do Provedor de Criptografia do Windows     

| | |           
|---|---|   
|   **Descrição do serviço** |   O Serviço de Host do Provedor de Criptografia do Windows agencia funcionalidades relacionadas à criptografia de provedores de criptografia de terceiros para processos que precisam avaliar e aplicar políticas EAS. A interrupção disso comprometerá as verificações de conformidade do EAS que foram estabelecidas pelas contas de email conectadas
|   **Nome do serviço**    |   WEPHOSTSVC
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-error-reporting-service"></a>Serviço de Relatório de Erros do Windows          

| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que os erros sejam relatados quando os programas pararem de funcionar ou de responder e permite que as soluções existentes sejam entregues. Também permite que os logs sejam gerados para serviços de diagnóstico e reparo. Se esse serviço for interrompido, o relatório de erros poderá não funcionar corretamente e os resultados dos serviços de diagnóstico e os reparos poderão não ser exibidos.
|   **Nome do serviço**    |   WerSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Coleta e envia dados de falha/travamento usados pela MS e por ISVs/IHVs de terceiros. Os dados são usados para diagnosticar bugs que induzem falhas, que podem incluir bugs de segurança. Também necessário para o Relatório de Erros Corporativo
|||         



## <a name="windows-event-collector"></a>Coletor de Eventos do Windows          

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço gerencia as assinaturas persistentes de eventos de origens remotas que dão suporte ao protocolo WS-Management. Isso inclui logs de eventos do Windows Vista, origens de eventos de hardware e habilitados para IPMI. O serviço armazena os eventos encaminhados em um log de eventos local. Se esse serviço for interrompido ou desabilitado, as assinaturas de eventos não poderão ser criadas e os eventos encaminhados não poderão ser aceitos.
|   **Nome do serviço**    |   Wecsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Coleta eventos ETW (incluindo eventos de segurança) para capacidade de gerenciamento e diagnóstico.  Muitos dos recursos e muitas das ferramentas de terceiros dependem dele, incluindo ferramentas de auditoria de segurança
|||         



## <a name="windows-event-log"></a>Log de eventos do Windows            

| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço gerencia eventos e logs de eventos. Ele dá suporte ao log de eventos, à consulta de eventos, à assinatura de eventos, ao arquivamento do logs de eventos e ao gerenciamento de metadados de evento. Ele pode exibir eventos no formato XML e de texto sem formatação. A interrupção desse serviço pode comprometer a segurança e a confiabilidade do sistema.
|   **Nome do serviço**    |   EventLog
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-firewall"></a>Firewall do Windows         

| | |           
|---|---|   
|   **Descrição do serviço** |   O Firewall do Windows ajuda a proteger seu computador impedindo que usuários não autorizados obtenham acesso ao computador por meio da Internet ou de uma rede.
|   **Nome do serviço**    |   MpsSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="windows-font-cache-service"></a>Serviço de Cache de Fontes do Windows      

| | |           
|---|---|   
|   **Descrição do serviço** |   Otimiza o desempenho de aplicativos armazenando em cache os dados de fontes comumente usados. Os aplicativos iniciarão esse serviço se ele ainda não estiver sendo executado. Ele poderá ser desabilitado, embora essa ação diminua o desempenho do aplicativo.
|   **Nome do serviço**    |   FontCache
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-image-acquisition-wia"></a>WIA (Windows Image Acquisition)          

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de aquisição de imagens para scanners e câmeras
|   **Nome do serviço**    |   stisvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



##  <a name="windows-insider-service"></a>Serviço do Participante do Programa Windows Insider     

| | |           
|---|---|   
|   **Descrição do serviço** |   wisvc
|   **Nome do serviço**    |   wisvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   O Server não dá suporte à liberação de versões de pré-lançamento e, portanto, ele não é operacional no Server. O recurso também pode ser desabilitado por meio do GP.
|||         



##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **Descrição do serviço** |   Adiciona, modifica e remove aplicativos fornecidos como um pacote do Windows Installer (*.msi, *.msp). Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   msiserver
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-license-manager-service"></a>Serviço de Gerenciador de Licença do Windows          

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte de infraestrutura para a Microsoft Store.  Esse serviço é iniciado sob demanda e se for desabilitado, o conteúdo adquirido por meio da Microsoft Store não funcionará corretamente.
|   **Nome do serviço**    |   LicenseManager
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-management-instrumentation"></a>Instrumentação de Gerenciamento do Windows       

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um modelo de objeto e interface comum para acessar informações de gerenciamento sobre o sistema operacional, dispositivos, aplicativos e serviços. Se esse serviço for interrompido, a maioria dos programas de software baseados no Windows não funcionará corretamente. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   Winmgmt
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



##  <a name="windows-mobile-hotspot-service"></a>Serviço de Hotspot Móvel do Windows          

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a capacidade de compartilhar uma conexão de dados da rede celular com outro dispositivo.
|   **Nome do serviço**    |   icssvc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   
|||         



## <a name="windows-modules-installer"></a>Instalador de Módulos do Windows        

| | |           
|---|---|   
|   **Descrição do serviço** |   Permite a instalação, a modificação e a remoção das atualizações do Windows e de componentes opcionais. Se esse serviço for desabilitado, a instalação ou a desinstalação de atualizações do Windows poderá falhar neste computador.
|   **Nome do serviço**    |   TrustedInstaller
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="windows-push-notifications-system-service"></a>Serviço do Sistema de Notificações por Push do Windows            

| | |           
|---|---|
|   **Descrição do serviço** |   Esse serviço é executado na sessão 0 e hospeda o provedor de conexão e plataforma de notificação que manipula a conexão entre o dispositivo e o servidor WNS.
|   **Nome do serviço**    |   WpnService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Necessário para blocos dinâmicos e outros recursos
|||         



## <a name="windows-push-notifications-user-service"></a>Serviço de Usuário de Notificações por Push do Windows          

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço hospeda a plataforma de notificação do Windows que fornece suporte para notificações locais e por push. As notificações compatíveis são bloco, notificação do sistema e bruto.
|   **Nome do serviço**    |   WpnUserService
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Pode ser desabilitado
|   **Comentários**    |   Modelo de serviço de usuário
|||         



## <a name="windows-remote-management-ws-management"></a>Gerenciamento Remoto do Windows (WS-Management)
| | |
|---|---|
|   **Descrição do serviço** |   O serviço Windows Remote Management (WinRM) implementa o protocolo WS-Management para gerenciamento remoto. O WS-Management é um protocolo de serviços Web padrão usado para gerenciamento remoto de software e hardware. O serviço WinRM lê na rede as solicitações do WS-Management e as processa. O serviço WinRM precisa ser configurado com um ouvinte usando a ferramenta de linha de comando winrm.cmd ou por meio da Política de Grupo para que ele ouça a rede. O serviço WinRM fornece acesso aos dados WMI e possibilita a coleção de eventos. A coleção de eventos e a assinatura de eventos exigem que o serviço esteja em execução. As mensagens WinRM usam HTTP e HTTPS à medida que são transportadas. O serviço WinRM não depende do IIS, mas é pré-configurado para compartilhar uma porta com IIS no mesmo computador.  O serviço WinRM reserva o prefixo de URL /wsman. Para evitar conflito com IIS, os administradores devem garantir que quaisquer sites da Web hospedados no IIS não usem o prefixo de URL /wsman.
|   **Nome do serviço**    |   WinRM
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Necessário para gerenciamento remoto
|||



##  <a name="windows-search"></a>Windows Search      

| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece indexação de conteúdo, cache de propriedades e resultados da pesquisa para arquivos, emails e outros tipos de conteúdo.
|   **Nome do serviço**    |   WSearch
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desabilitado
|   **Comentários**    |   
|||         



##  <a name="windows-time"></a>Tempo do Windows        

| | |           
|---|---|   
|   **Descrição do serviço** |   Mantém a sincronização de data e hora em todos os clientes e servidores da rede. Se esse serviço for interrompido, a sincronização de data e hora não estará disponível. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   W32Time
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes
|   **Comentários**    |   
|||         



## <a name="windows-update"></a>Windows Update           

| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita a detecção, o download e a instalação de atualizações do Windows e outros programas. Se esse serviço for desabilitado, os usuários deste computador não poderão usar o Windows Update nem seu recurso de atualização automática e os programas não poderão usar a API do WUA (Windows Update Agent).
|   **Nome do serviço**    |   wuauserv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="winhttp-web-proxy-auto-discovery-service"></a>Serviço de Descoberta Automática de Proxy Web do WinHTTP         

| | |           
|---|---|   
|   **Descrição do serviço** |   O WinHTTP implementa a pilha HTTP de cliente e fornece aos desenvolvedores uma API do Win32 e um componente de Automação COM para enviar solicitações HTTP e receber respostas. Além disso, o WinHTTP fornece suporte para a descoberta automática de uma configuração de proxy por meio de sua implementação do protocolo WPAD (Descoberta Automática de Proxy Web).
|   **Nome do serviço**    |   WinHttpAutoProxySvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilitar
|   **Comentários**    |   Qualquer recurso que use a pilha de rede pode ter uma dependência funcional desse serviço. Muitas organizações dependem disso para configurar o roteamento de proxy HTTP de suas redes internas.  Sem ele, todas as conexões HTTP originadas internamente com a Internet falharão.
|||         



## <a name="wired-autoconfig"></a>Serviço de Configuração Automática de Rede com Fio         

| | |           
|---|---|       
|   **Descrição do serviço** |   O Serviço de Configuração Automática de Rede com Fio (DOT3SVC) é responsável por executar a autenticação IEEE 802.1X em interfaces Ethernet. Caso sua implantação atual de rede com fio imponha a autenticação 802.1X, o serviço DOT3SVC deverá ser configurado para ser executado, a fim de estabelecer a conectividade de Camada 2 e/ou fornecer acesso aos recursos da rede. As redes com fio que não impõem a autenticação 802.1X não são afetadas pelo serviço DOT3SVC.
|   **Nome do serviço**    |   dot3svc
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes   
|   **Comentários**    |   
|||         



## <a name="wmi-performance-adapter"></a>Adaptador de Desempenho WMI          

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece informações da biblioteca de desempenho de provedores WMI (Instrumentação de Gerenciamento do Windows) para clientes na rede. Esse serviço é executado apenas quando o Auxiliar de Dados de Desempenho é ativado.
|   **Nome do serviço**    |   wmiApSrv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Sem diretrizes       
|   **Comentários**    |   
|||         



## <a name="workstation"></a>Estação de Trabalho          

| | |           
|---|---|   
|   **Descrição do serviço** |   Cria e mantém conexões de rede de cliente com servidores remotos usando o protocolo SMB. Se esse serviço for interrompido, essas conexões não estarão disponíveis. Se esse serviço for desabilitado, os serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   LanmanWorkstation
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Sem diretrizes       
|   **Comentários**    |   
|||         



## <a name="xbox-live-auth-manager"></a>Gerenciador de Autenticação Xbox Live           

| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de autenticação e autorização para interação com o Xbox Live. Se esse serviço for interrompido, alguns aplicativos poderão não funcionar corretamente.
|   **Nome do serviço**    |   XblAuthManager
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Deve ser desabilitado
|   **Comentários**    |   
|||         



## <a name="xbox-live-game-save"></a>Salvamento de Jogo do Xbox Live          

| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço sincroniza os dados de salvamento de jogos habilitados para salvamento do Xbox Live.  Se esse serviço for interrompido, não será feito upload dos dados de salvamento de jogos no Xbox Live nem baixados dele.
|   **Nome do serviço**    |   XblGameSave
|   **Instalação**    |   Somente com a Experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Deve ser desabilitado
|   **Comentários**    |   Esse serviço sincroniza os dados de salvamento de jogos habilitados para salvamento do Xbox Live.  Se esse serviço for interrompido, não será feito upload dos dados de salvamento de jogos no Xbox Live nem baixados dele.
|||         




