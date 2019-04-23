---
title: Introdução ao usuário acessar o registro em log
desctription: Describes the User Access Logging feature and how to start using it.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c395b8b-3b35-4042-b9cc-07e438f86d50
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8656bf278519b48f8d26008fd98e46428106e511
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861497"
---
# <a name="get-started-with-user-access-logging"></a>Introdução ao usuário acessar o registro em log

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usuário acesso UAL (log) é um recurso no Windows Server que agrega dados de uso do cliente por função e produtos em um servidor local. Ele ajuda os administradores de servidor do Windows a quantificar solicitações de computadores cliente para funções e serviços em um servidor local.  
  
O UAL é instalado e habilitado por padrão e coleta de dados em quase em tempo real. Nenhuma configuração de Administrador é necessária, embora o UAL possa ser desabilitado ou habilitado. Para obter mais informações, consulte [Manage User Access Logging](Manage-User-Access-Logging.md). O serviço de registro de acesso do usuário agrega dados de uso do cliente por funções e produtos em arquivos de banco de dados local.  Os administradores de TI podem fazer uso de WMIx (Instrumentação de Gerenciamento do Windows) posterior ou dos cmdlets do Windows PowerShell para recuperar quantidades e instâncias por função de servidor (ou produto de software), por usuário, dispositivo, servidor local e data.  
  
> [!NOTE]  
> O UAL dá suporte ao [Microsoft Assessment and Planning Toolkit](https://go.microsoft.com/fwlink/?LinkID=111000).  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
O UAL agrega cliente exclusivo dispositivo e usuário solicitação eventos que são registrados em um banco de dados local. Esses registros são disponibilizados (através de uma consulta por um administrador de servidor) para recuperar quantidades e instâncias por função de servidor, usuário, dispositivo, servidor local e data.  Além disso, o UAL foi aprimorado para permitir que os desenvolvedores de software não Microsoft instrumentem os eventos do UAL para serem agregados pelo Windows Server.  
  
O UAL pode executar as seguintes tarefas:  
  
-   Quantificar solicitações do usuário do cliente por servidores locais físicos ou virtuais.  
  
-   Quantificar solicitações do usuário do cliente por produtos de software instalados em um servidor físico ou virtual local.  
  
-   Recuperar dados em um servidor local que executa o Hyper-V para identificar períodos de alta e baixa demanda em um computador virtual do Hyper-V.  
  
-   Recuperar dados do UAL de vários servidores remotos.  
  
Além disso, os desenvolvedores de software podem instrumentar eventos do UAL que podem ser agregados e recuperados pelas interfaces do WMI e do Windows PowerShell.  
  
As seguintes funções de servidor e serviços podem ter suporte no UAL:  
  
-   AD CS (Serviços de Certificados do Active Directory)  
  
-   AD RMS (Active Director Rights Management Services)  
  
-   BranchCache  
  
-   Sistema de nome de domínio (DNS)  
  
    > [!NOTE]  
    > O UAL coleta dados do DNS a cada 24 horas e existe um cmdlet do UAL separado para este cenário.  
  
-   Protocolo DHCP  
  
-   Servidor de Fax  
  
-   Serviços de arquivo  
  
-   O servidor de protocolo FTP  
  
-   Hyper-V  
  
    > [!NOTE]  
    > O UAL coleta dados do Hyper-V a cada 24 horas e existe um cmdlet do UAL separado para este cenário.  
  
-   Servidor Web (IIS)  
  
    > [!WARNING]  
    > Para usar o UAL com o IIS, você deve usar o iisual.exe. Para obter mais informações, consulte [Analisando dados de uso de cliente com o registro de acesso de usuário do IIS](http://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging).  
  
-   Serviços de fila de mensagens da Microsoft (MSMQ)  
  
-   Network Policy and Access Services  
  
-   Serviços de impressão e documentos  
  
-   Serviço de Roteamento e Acesso Remoto (RRAS)  
  
-   WDS (Serviços de Implantação do Windows)  
  
-   Windows Server Update Services (WSUS)  
  
> [!IMPORTANT]  
> O UAL não é recomendado para uso em servidores conectados diretamente na Internet, como servidores Web em um espaço de endereço acessível pela Internet, ou nos cenários em que um desempenho extremamente alto é a principal função do servidor (como nos ambientes de carga de trabalho HPC). O UAL é destina principalmente a pequeno, médio e cenários de intranet corporativo em que o volume elevado é esperado, mas não tão altas quanto implantações que servem o volume de tráfego da Internet regularmente.  
  
## <a name="BKMK_NEW"></a>Funcionalidade importante  
A tabela a seguir descreve as funções chave do UAL e seus possíveis valores.  
  
|Funcionalidade|Valor|  
|-----------------|---------|  
|Coletar e agregar dados do evento de solicitação de cliente quase em tempo real.|Até três anos de dados podem ser salvos. **Importante:** Os administradores precisam forçar a conformidade dos dados coletados e dos períodos de retenção de dados com a política de privacidade da organização e regulamentos locais.|  
|Consulte o UAL usando interfaces WMI ou do Windows PowerShell para recuperar dados de solicitação de cliente em um servidor local ou remoto.|O UAL permite um único modo de exibição de dados de uso contínuo. Administradores corporativos e de servidor podem recuperar esses dados e coordenar com os administradores de negócios para otimizar o uso de suas licenças de software por volume.|  
|Habilitado por padrão.|Os administradores de servidor não precisam configurar ou, de alguma forma, instalar esse recurso para todas as funcionalidades principais ficarem disponíveis e funcionando.|  
  
## <a name="data-logged-with-ual"></a>Dados registrados no UAL  
Os seguintes dados relacionados ao usuário são registrados no UAL.  
  
|Dados|Descrição|  
|--------|---------------|  
|**UserName**|Se aplicável, o nome de usuário do cliente acompanha as entradas do UAL de produtos e funções instaladas.|  
|**ActivityCount**|O número de vezes que um usuário específico acessa uma função ou um serviço.|  
|**FirstSeen**|A data e hora em que um usuário acessa primeiro uma função ou um serviço.|  
|**LastSeen**|A data e hora em que um usuário acessou pela última vez uma função ou um serviço.|  
|**ProductName**|O nome do produto de software principal, como o Windows, que está fornecendo os dados do UAL.|  
|**RoleGUID**|O GUID atribuído ou registrado do UAL que representa a função de servidor ou o produto instalado.|  
|**RoleName**|O nome da função, do componente ou do subproduto que está fornecendo os dados do UAL. Também é associado a um ProductName e a um RoleGUID.|  
|**TenantIdentifier**|Um GUID exclusivo de um cliente locatário de uma função instalada ou de um produto que acompanha os dados do UAL, se aplicável.|  
  
Os seguintes dados relacionados ao dispositivo são registrados no UAL.  
  
|Dados|Descrição|  
|--------|---------------|  
|**IPAddress**|O endereço IP de um dispositivo cliente que é usado para acessar uma função ou um serviço.|  
|**ActivityCount**|O número de vezes que um determinado dispositivo acessou a função ou um serviço.|  
|**FirstSeen**|A data e hora em que um endereço IP foi usado pela primeira vez para acessar uma função ou um serviço.|  
|**LastSeen**|A data e hora em que um endereço IP foi usado pela última vez para acessar uma função ou um serviço.|  
|**ProductName**|O nome do produto de software principal, como o Windows, que está fornecendo os dados do UAL.|  
|**RoleGUID**|O GUID registrado ou UAL atribuído que representa a função de servidor ou o produto instalado.|  
|**RoleName**|O nome da função, do componente ou do subproduto que está fornecendo os dados do UAL. Também é associado a um ProductName e a um RoleGUID.|  
|**TenantIdentifier**|Um GUID exclusivo de um cliente locatário de uma função instalada ou de um produto que acompanha os dados do UAL, se aplicável.|  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
O UAL pode ser usado em qualquer computador executando versões do Windows Server após o Windows Server 2012.  
  
## <a name="see-also"></a>Consulte também  
[Usuário acessar o registro em log](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) no MSDN.  
[Gerenciar usuário acessar o registro em log](Manage-User-Access-Logging.md)  
  

