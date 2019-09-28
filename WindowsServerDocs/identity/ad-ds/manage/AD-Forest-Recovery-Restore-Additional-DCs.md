---
title: Recuperação de floresta do AD-reimplantar controladores de domínio restantes
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fbab907c5624a76540ab6a28c568afbd9192c028
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390253"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Recuperação de floresta do AD-reimplantar controladores de domínio restantes

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

As etapas até este ponto se aplicam a todas as florestas: Encontre um backup válido para cada domínio, recupere os domínios isoladamente, reconecte-os, redefina o catálogo global e limpe-os. Nesta próxima etapa, você reimplantará a floresta. A maneira de fazer isso dependerá muito do design da floresta, dos contratos de nível de serviço, da estrutura do site, da largura de banda disponível e de vários outros fatores. Você precisará projetar seu próprio plano de reimplantação com base nos princípios e sugestões nesta seção, de uma forma que seja mais adequada para seus requisitos de negócios.  
  
A próxima etapa é instalar AD DS em todos os controladores de domínio que estavam presentes antes da recuperação da floresta acontecer. Se os DCs ainda existirem, o serviço de AD DS precisará ser removido forçosamente ou os DCs poderão ser reinstalados. Os backups existentes para esses controladores de domínio não podem ser reutilizados, pois os metadados correspondentes foram removidos durante a recuperação da floresta. Em um ambiente não complicado, esse processo de reimplantação pode ser tão simples quanto reconectar os DCs recuperados à rede de produção e promover novos DCs, conforme necessário.  
  
Em uma grande empresa enfrentando uma infra-estrutura mundial, é necessário um plano mais sofisticado. A primeira fase geralmente é restaurar o AD como um serviço; Isso significa instalar controladores de trabalho estrategicamente posicionados, de modo que todos os aplicativos e divisões de negócios essenciais possam começar a funcionar novamente. Pode ser aceitável que as filiais tenham o desempenho reduzido temporariamente como resultado disso. Como uma segunda fase, todos os DCs restantes e menos críticos são reimplantados.  
  
 Há dois métodos para instalar DCs adicionais, os quais podem ser automatizados:  
  
- Clonagem  
   - Para ambientes virtualizados que executam o Windows Server 2012, a clonagem é a maneira mais rápida e simples de recuperar um grande número de DCs. Você pode automatizar a recuperação de todos os DCs virtualizados em um domínio depois de restaurar um único DC virtualizado a partir do backup.  
   - Para obter mais informações sobre clonagem e pré-requisitos, consulte [introdução à virtualização de Active Directory Domain Services (AD DS) (nível 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Reinstale AD DS usando o Windows PowerShell em servidores que executam o Windows Server 2012 (ou Dcpromo. exe em servidores que executam versões anteriores do Windows Server) ou usando a interface do usuário  
   - Para agilizar a reinstalação AD DS, você pode usar a opção instalar da mídia (IFM) para reduzir o tráfego de replicação durante a instalação. Para obter mais informações sobre como usar o comando **Ntdsutil IFM** para criar uma mídia de instalação, consulte [instalando o AD DS da mídia](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Considere os seguintes pontos adicionais para cada DC de réplica que é recuperado na floresta por clonagem de DC virtualizado ou instalando AD DS (em oposição a restaurar do backup):  
  
- Todo o software em um DC que é usado como a origem para a clonagem deve ser capaz de ser clonado. Os aplicativos e serviços que não podem ser clonados devem ser removidos antes que a clonagem seja iniciada. Se isso não for possível, um controlador de domínio virtualizado alternativo deverá ser escolhido como a origem.  
- Se você clonar DCs virtualizados adicionais do primeiro controlador de domínio virtualizado a ser restaurado, o controlador de domínio de origem precisará ser desligado enquanto seu arquivo VHDX é copiado. Em seguida, ele precisará estar em execução e disponível online quando os DCs virtuais de clonagem forem iniciados pela primeira vez. Se o tempo de inatividade exigido pelo desligamento não for aceitável para o primeiro controlador de domínio recuperado, implante um controlador de domínio virtualizado adicional instalando AD DS para atuar como a origem para clonagem.  
- Não há nenhuma restrição no nome do host do controlador de domínio virtualizado clonado ou no servidor no qual você deseja instalar o AD DS. Você pode usar um novo nome de host ou o nome de host que estava em uso anteriormente. Para obter mais informações sobre a sintaxe de nome de host DNS, consulte [CREATING DNS Computer Names](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configure cada servidor com o primeiro servidor DNS na floresta (o primeiro DC que foi restaurado no domínio raiz) como o servidor DNS preferencial nas propriedades TCP/IP de seu adaptador de rede. Para obter mais informações, consulte [Configurar TCP/IP para usar o DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Reimplante todos os RODCs no domínio, seja por clonagem de DC virtualizado se vários RODCs estiverem implantados em um local central ou pelo método tradicional de recriá-los removendo e reinstalando AD DS se eles forem implantados individualmente em locais isolados localizados como filiais.  
   - A recriação de RODCs garante que eles não contenham objetos remanescentes e possam ajudar a impedir que conflitos de replicação ocorram posteriormente. Ao remover AD DS de um RODC, *escolha a opção para reter os metadados do DC*. O uso dessa opção retém a conta krbtgt para o RODC e retém as permissões para a conta de administrador do RODC delegado e o Política de Replicação de Senha (PRP) e impede que você precise usar credenciais de administrador de domínio para remover e reinstalar o AD DS em um RODC. Ele também retém o servidor DNS e as funções de catálogo global se eles estiverem instalados no RODC originalmente.  
   - Ao recriar DCs (RODCs ou DCs graváveis), pode haver um aumento no tráfego de replicação durante a reinstalação. Para ajudar a reduzir esse impacto, você pode escalonar a agenda das instalações do RODC e usar a opção instalar da mídia (IFM). Se você usar a opção IFM, execute o comando **Ntdsutil IFM** em um controlador de domínio gravável que você confia para liberar dados danificados. Isso ajuda a impedir a exibição de possíveis danos no RODC após a conclusão da reinstalação do AD DS. Para obter mais informações sobre o IFM, consulte [instalando o AD DS da mídia](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Para obter mais informações sobre como recompilar RODCs, consulte [remoção e reinstalação do RODC](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Se um controlador de domínio estava executando o serviço do servidor DNS antes da falha da floresta, instale e configure o serviço do servidor DNS durante a instalação do AD DS. Caso contrário, configure seus clientes DNS antigos com outros servidores DNS.  
- Se você precisar de catálogos globais adicionais para compartilhar autenticação ou carregamento de consulta para usuários ou aplicativos, poderá adicionar o catálogo global ao controlador de domínio virtualizado de origem antes da clonagem ou pode tornar um controlador de domínio um servidor de catálogo global durante a instalação do AD DS.  
  
## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD-planejar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD – identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD-determine como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD-executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD-perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD-recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
