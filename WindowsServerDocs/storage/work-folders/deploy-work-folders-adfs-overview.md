---
title: "Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: visão geral"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 48c7d771c7ec75a4bc340608a96410ea388418e9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: visão geral

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Os tópicos nesta seção fornecem instruções para implantar Pastas de Trabalho com o AD FS (Serviços de Federação do Active Directory) e o Proxy de aplicativo Web. As instruções são projetadas para ajudar você a criar uma configuração funcional completa de Pastas de Trabalho, com computadores cliente prontos para começar a usar Pastas de Trabalho localmente ou pela Internet.  
  
Pastas de Trabalho é um componente introduzido no Windows Server 2012 R2 que permite aos operadores de informações sincronizar arquivos de trabalho entre seus dispositivos. Para obter mais informações sobre Pastas de Trabalho, consulte [Visão geral de Pastas de Trabalho](Work-Folders-Overview.md).  
  
Para habilitar usuários a sincronizar suas Pastas de Trabalho na Internet, você precisa publicar Pastas de Trabalho por meio de um proxy reverso, tornando as Pastas de Trabalho disponíveis externamente na Internet. O Proxy de aplicativo Web, que está incluído no AD FS, é uma opção que você pode usar para fornecer funcionalidade de proxy reverso. O Proxy de aplicativo Web autentica previamente o acesso ao aplicativo Web de Pastas de Trabalho, usando o AD FS para que usuários em qualquer dispositivo possam acessar Pastas de Trabalho fora da rede corporativa. 

> [!NOTE]
>   As instruções abordadas nesta seção destinam-se a um ambiente do Windows Server 2016. Se você estiver usando o Windows Server 2012 R2, siga as [instruções do Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).
  
Estes tópicos fornecem os seguintes detalhes:  
  
-   Instruções passo a passo para configurar e implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web por meio da interface de usuário do Windows Server. As instruções descrevem como configurar um ambiente de teste simples com certificados autoassinados. Em seguida, você pode usar o exemplo de teste como um guia para ajudá-lo a criar um ambiente de produção que usa certificados confiáveis publicamente.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Para seguir os procedimentos e exemplos nestes tópicos, você precisa ter os seguintes componentes já prontos:  
  
-   Uma floresta do Active Directory® Domain Services com extensões de esquema no Windows Server 2012 R2 para dar suporte à referência automática de computadores e dispositivos no servidor de arquivos correto ao usar vários servidores de arquivos. É preferível que o DNS seja habilitado na floresta, mas isso não é obrigatório.  
  
-   Um controlador de domínio: um servidor que tenha a função do AD DS habilitada e seja configurado com um domínio (no exemplo de teste, contoso.com).  
  
    Um controlador de domínio que esteja executando pelo menos o Windows Server 2012 R2 é necessário para dar suporte ao registro de dispositivo para Workplace Join. Se você não quiser usar o Workplace Join, poderá executar o Windows Server 2012 no controlador de domínio.  
  
-   Dois servidores que ingressaram no domínio (por exemplo, contoso.com) e que estejam executando o Windows Server 2016. Um servidor será usado para AD FS e o outro será usado para Pastas de Trabalho.  
  
-   Um servidor que seja não ingressado no domínio e que esteja executando o Windows Server 2016. Esse servidor executará o Proxy de aplicativo Web, deve ter uma placa de rede para o domínio de rede (por exemplo, contoso.com) e outra para a rede externa.  
  
-   Um computador ingressado no domínio que está executando o Windows 7 ou posterior.  
  
-   Um computador não ingressado no domínio que esteja executando o Windows 7 ou posterior.  
  
Para o ambiente de teste que estamos abordando neste guia, você deve ter a topologia mostrada no diagrama a seguir. Os computadores podem ser computadores físicos ou VMs (máquinas virtuais). 
  
![Diagrama mostrando a Internet, o DMZ e os segmentos de rede da Contoso. No segmento de Internet: Cliente2; no DMZ: um servidor WAP; no segmento da Contoso: o servidor de Pastas de Trabalho, um controlador de domínio, um servidor do AD FS e Cliente1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>Visão geral da implantação  
Nesse grupo de tópicos, você verá um exemplo passo a passo de como configurar o AD FS, o Proxy de aplicativo Web e Pastas de Trabalho em um ambiente de teste. Os componentes serão configurados nesta ordem:  
  
1.  AD FS  
  
2.  Pastas de Trabalho  
  
3.  Proxy de aplicativo Web  
  
4.  A estação de trabalho ingressada no domínio e a estação de trabalho não ingressada no domínio  
  
Você também usará um Script do Windows PowerShell para criar certificados autoassinados.  
  
## <a name="deployment-steps"></a>Etapas de implantação  
Para executar a implantação usando a interface de usuário do Windows Server, siga as etapas nestes tópicos:  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 1, Configurar o AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 2, Trabalho de pós-configuração do AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 3, Configurar Pastas de Trabalho](deploy-work-folders-adfs-step3.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 4, Configurar o Proxy de aplicativo Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web: Etapa 5, Configurar clientes](deploy-work-folders-adfs-step5.md)  

## <a name="see-also"></a>Consulte também  
[Visão geral de Pastas de Trabalho](Work-Folders-Overview.md)  
[Projetando uma implementação de Pastas de Trabalho](Plan-Work-Folders.md)  
[Implantando Pastas de Trabalho](Deploy-Work-Folders.md)  
  

