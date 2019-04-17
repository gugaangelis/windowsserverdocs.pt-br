---
title: "Recuperação de floresta do AD - reimplantação restante controladores de domínio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 66b0bc65b3b8e5dfbf5f1a85350dab60ac3a11c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Recuperação de floresta do AD - reimplantação restante controladores de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 As etapas até esse ponto se aplicam a todas as florestas: Encontre um backup válido para cada domínio, recuperar os domínios isoladamente, reconectá-los, redefinir o catálogo global e limpar. Nesta etapa próxima reimplantação será feita a floresta. A maneira de fazer isso dependerá muito seu design de floresta, os contratos de nível de serviço, a estrutura do site, largura de banda disponível e vários outros fatores. Você precisará criar seu próprio plano reimplantação com base nos princípios e as sugestões nesta seção, de maneira que é mais adequada às suas necessidades de negócios.  
  
 A próxima etapa é instalar o AD DS em todos os controladores de domínio que estavam presentes antes da recuperação de floresta ocorreu. Se os controladores de domínio ainda existem, o serviço do AD DS precisa ser removido forçadamente ou controladores de domínio podem ser reinstalados. Quaisquer backups existentes para esses controladores de domínio não podem ser reutilizadas, porque os metadados correspondentes foi removido durante a recuperação de floresta. Em um ambiente descomplicado esse processo reimplantação pode ser tão simple quanto reconectar as controladores de domínio recuperados na rede de produção e promover novos controladores de domínio conforme necessário.  
  
 Em uma grande empresa enfrenta uma infraestrutura no mundo inteiro, é necessário um plano mais sofisticado. A primeira fase é geralmente restaurar o anúncio como um serviço; Isso significa para instalar estratégica colocado controladores de domínio que todos os aplicativos e divisões comerciais essenciais podem começar a trabalhar novamente. Ele pode ser aceitável para filiais para temporariamente têm uma redução de desempenho em decorrência disso. Como uma segunda fase, todos os demais e menos críticos controladores de domínio são reimplantados.  
  
 Há dois métodos para instalar os controladores de domínio adicionais, que podem ser automatizadas:  
  
-   Clonagem  
  
     Para ambientes virtualizados que executam o Windows Server 2012, clonagem é a maneira mais rápida e simples para recuperar um grande número de controladores de domínio. Você pode automatizar a recuperação de todos os controladores de domínio virtualizados em um domínio depois de restaurar um único DC virtualizado de backup.  
  
     Para saber mais sobre clonagem e pré-requisitos, consulte [Introdução aos serviços de domínio do Active Directory (AD DS) virtualização (nível 100)](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Instalar o AD DS novamente usando o Windows PowerShell em servidores que executam o Windows Server 2012 (ou Dcpromo.exe em servidores que executam versões anteriores do Windows Server) ou usando a interface do usuário  
  
     Para agilizar a reinstalação do AD DS, você pode usar a instalação da opção de mídia (IFM) para reduzir o tráfego de replicação durante a instalação. Para obter mais informações sobre como usar o **ntdsutil ifm** comando para criar mídia de instalação, consulte [instalando o AD DS da mídia](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
 Considere os seguintes pontos adicionais para cada réplica DC que são recuperadas na floresta por virtualizado DC clonagem ou instalando o AD DS (em oposição a restauração de backup):  
  
-   Todos os softwares em um controlador de domínio que é usado como a origem para clonar devem ser capaz de ser clonados. Aplicativos e serviços que não podem ser clonados devem ser removidos antes de clonagem for iniciada. Se isso não for possível, um controlador de domínio virtualizado alternativo deve ser escolhido como a origem.  
  
-   Se você clonar adicionais controladores de domínio virtualizadas do primeiro DC virtualizado para ser restaurado, o controlador de domínio de origem precisará ser desligado enquanto seu arquivo VHDX é copiado. Em seguida, ele terá de estar em execução e disponível online quando o clone virtuais controladores de domínio são iniciado pela primeira vez. Se o tempo de inatividade exigido pelo desligamento não é aceitável para o primeiro controlador de domínio recuperado, implante um controlador de domínio virtualizado adicional ao instalar o AD DS para atuar como a origem de clonagem.  
  
-   Não há nenhuma restrição no nome do host do controlador de domínio virtualizado clonado ou o servidor no qual você deseja instalar o AD DS. Você pode usar um novo nome de host ou o nome do host que estava anteriormente em uso. Para obter mais informações sobre a sintaxe de nome de host DNS, consulte [criação de nomes de computador DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
  
-   Configure cada servidor com o primeiro servidor DNS na floresta (primeiro controlador de domínio que foi restaurado no domínio raiz) como o servidor DNS preferencial nas propriedades TCP/IP de seu adaptador de rede. Para obter mais informações, consulte [configurar o TCP/IP para usar o DNS](https://technet.microsoft.com/library/cc779282.aspx).  
  
-   Reimplante todos os RODCs no domínio, por meio da virtualizado DC clonagem se vários RODCs são implantados em um local central ou pelo método tradicional de recriá-los, removendo e reinstalando o AD DS se eles são implantados individualmente em locais localizados isolados como filiais.  
  
     Recriar RODCs garante que eles não contêm qualquer objeto remanescente e podem ajudar a evitar conflitos de replicação ocorra mais tarde. Quando você remove o AD DS de um RODC, *escolha a opção de manter os metadados de DC*. Usar essa opção mantém a conta krbtgt para o RODC mantém as permissões para a conta de administrador RODC delegado e a política de replicação de senha (PRP) e impede que você precisar usar as credenciais de administrador de domínio para remover e reinstalar o AD DS em um RODC. Ela também retém as funções de catálogo global e um servidor DNS se eles forem instalados no RODC originalmente.  
  
     Quando você recriar controladores de domínio (RODCs ou controladores de domínio graváveis), pode haver aumento de tráfego de replicação durante a reinstalação. Para ajudar a reduzir esse impacto, você pode programar o cronograma das instalações RODC e você pode usar a opção de instalação da mídia (IFM). Se você usar a opção IFM, execute o **ntdsutil ifm** comando em um controlador de domínio gravável que você confia esteja livre de dados danificados. Isso ajuda a evitar possíveis danos sejam exibidas no RODC após a reinstalação do AD DS estiver concluída. Para saber mais sobre IFM, consulte [instalando o AD DS da mídia](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
     Para obter mais informações sobre como recriar RODCs, consulte [RODC remoção e reinstalação](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
  
-   Se um controlador de domínio estava executando o serviço de servidor DNS antes da falha de floresta, instale e configure o serviço de servidor DNS durante a instalação do AD DS. Caso contrário, configure seus clientes DNS anteriores com outros servidores DNS.  
  
-   Se você precisar catálogos globais adicionais para compartilhar a autenticação ou a carga de consulta para usuários ou aplicativos, você pode adicionar o catálogo global para a fonte virtualizados DC antes de clonagem ou você pode fazer um servidor de catálogo global de um controlador de domínio durante a instalação do AD DS.  
  
## <a name="next-steps"></a>Próximas etapas
-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  