---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: Apêndice de administração simplificada
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 36cdacec27e64586c359146b858a9d68750e5026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858257"
---
# <a name="simplified-administration-appendix"></a>Apêndice de administração simplificada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [Gerenciador do servidor adicione servidores caixa de diálogo (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [Status do servidor do Gerenciador do servidor remoto](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Carregamento de módulo do PowerShell do Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [Os Hotfixes de emissão de RID para sistemas operacionais anteriores](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe Install from Media Changes](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Gerenciador do servidor adicione servidores caixa de diálogo (Active Directory)  

O **adicionar servidores** caixa de diálogo permite pesquisar no Active Directory para servidores, pelo sistema operacional, usando caracteres curinga e por local. A caixa de diálogo também permite usar consultas DNS por nome de domínio totalmente qualificado ou nome de prefixo. Essas pesquisas usam protocolos DNS e LDAP nativos implementados por meio do .NET, não AD Windows PowerShell com o Gateway de gerenciamento do AD por meio do SOAP – que significa que os controladores de domínio contatados pelo Gerenciador de servidores podem até mesmo executar o Windows Server 2003. Você também pode importar um arquivo com nomes de servidor para fins de provisionamento.  
  
A pesquisa do Active Directory usa os seguintes filtros LDAP:  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
A pesquisa do Active Directory retorna os seguintes atributos:  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>Status do servidor do Gerenciador do servidor remoto  
Gerenciador do servidor testa a acessibilidade de servidor remoto usando o protocolo de roteamento de endereço. Todos os servidores não está respondendo a solicitações de ARP não estiverem listados, mesmo se eles estão no pool.  
  
Se responder ARP, WMI e DCOM conexões são feitas para o servidor para retornar informações de status. Se RPC, DCOM e o WMI estiverem inacessível, o Gerenciador do servidor não pode gerenciar totalmente o servidor.  
  
## <a name="BKMK_PSLoadModule"></a>Carregamento de módulo do PowerShell do Windows  
Windows PowerShell 3.0 implementa o carregamento de módulo dinâmico. Usando o **Import-Module** cmdlet normalmente não é mais necessário; em vez disso, simplesmente invocar o cmdlet, alias ou função automaticamente carrega o módulo.  
  
Para ver os módulos carregados, use o **Get-Module** cmdlet.  
  
```  
Get-Module  
  
```  
  
![Administração simplificada](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Para ver todos os módulos instalados com seus cmdlets e funções exportadas, use:  
  
```  
Get-Module -ListAvailable  
  
```  
  
O principal caso para usar o **import-module** comando é quando você precisa acessar a "AD:" Unidade virtual do Windows PowerShell e nada mais já carregou o módulo. Por exemplo, usando os seguintes comandos:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>Os Hotfixes de emissão de RID para sistemas operacionais anteriores  
Ver [uma atualização está disponível para detectar e impedir o excesso consumo do pool RID global em um controlador de domínio que está executando o Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe Install from Media Changes  
Windows Server 2012 adiciona duas opções adicionais para a ferramenta de linha de comando Ntdsutil.exe para o **IFM (criação de mídia IFM)** menu. Elas permitem que você criar repositórios de IFM sem executar uma desfragmentação offline do NTDS exportado. Arquivo de banco de dados DIT. Quando o espaço em disco não é um bônus, isso economiza tempo criando o IFM.  
  
A tabela a seguir descreve os dois novos itens de menu:  
  
|||  
|-|-|  
|Item de menu|Explicação|  
|Criar NoDefrag completo %s|Criar a mídia IFM sem desfragmentar para um controlador de domínio do AD completo ou uma instância de AD/LDS na pasta %s|  
|Criar Sysvol completo NoDefrag %s|Criar a mídia IFM com SYSVOL e sem desfragmentar para um controlador de domínio do AD completo na pasta %s|  
  
![Administração simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Administração simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


