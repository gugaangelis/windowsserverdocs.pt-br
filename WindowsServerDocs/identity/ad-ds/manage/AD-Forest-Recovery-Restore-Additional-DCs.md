---
title: Recuperação de floresta do AD - reimplantação restantes controladores de domínio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: af9ec02a480b35e573edebcdd5928451174b6c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811997"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Recuperação de floresta do AD - reimplantação restantes controladores de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

As etapas até este ponto se aplicam a todas as florestas: localizar um backup válido para cada domínio, recuperar os domínios isoladamente, reconecte-os, redefina o catálogo global e limpar. Nesta próxima etapa, você irá reimplantar a floresta. A maneira de fazer isso dependerá bastante o design de floresta, os contratos de nível de serviço, estrutura do site, largura de banda disponível e vários outros fatores. Você precisará criar seu próprio plano de reimplantação, com base nos princípios e sugestões nesta seção, de forma que é mais adequada às suas necessidades de negócios.  
  
A próxima etapa é instalar o AD DS em todos os DCs que existiam antes da recuperação de floresta foram realizadas. Se os controladores de domínio ainda existirem, o serviço AD DS deverá ser removida à força, ou os controladores de domínio poderá ser reinstalados. Qualquer backup existente para esses controladores de domínio não pode ser reutilizado, porque os metadados correspondentes foi removido durante a recuperação de floresta. Em um ambiente pouco complicado, esse processo de reimplantação pode ser tão simple quanto se reconectar os controladores de domínio recuperados para a rede de produção e promovendo novos controladores de domínio, conforme necessário.  
  
Em uma grande empresa enfrentam uma infra-estrutura em todo o mundo, um plano mais sofisticado é necessária. A primeira fase geralmente é restaurar do AD como um serviço; Isso significa instalar estrategicamente colocados os controladores de domínio, de modo que todas as divisões de negócios críticos e de aplicativos podem iniciar o trabalho novamente. Pode ser aceitável para filiais para temporariamente têm um desempenho reduzido em decorrência disso. Como uma segunda fase, todos os demais e controladores de domínio menos críticos são reimplantados.  
  
 Há dois métodos para instalar os controladores de domínio adicionais, que podem ser automatizados:  
  
- Clonagem  
   - Em ambientes virtualizados que executam o Windows Server 2012, a clonagem é a maneira mais rápida e simples de recuperar um grande número de controladores de domínio. Você pode automatizar a recuperação de todos os controladores de domínio virtualizados em um domínio depois de restaurar um único controlador de domínio virtualizado de backup.  
   - Para obter mais informações sobre clonagem e pré-requisitos, consulte [Introdução à virtualização de serviços de domínio Active Directory (AD DS) (nível 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Instalar o AD DS novamente usando o Windows PowerShell nos servidores que executam o Windows Server 2012 (ou Dcpromo.exe em servidores que executam versões anteriores do Windows Server) ou usando a interface do usuário  
   - Para agilizar a instalação do AD DS novamente, você pode usar instalação com a opção de mídia (IFM) para reduzir o tráfego de replicação durante a instalação. Para obter mais informações sobre como usar o **ntdsutil ifm** comando para criar mídia de instalação, consulte [instalação do AD DS da mídia](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Considere os seguintes pontos adicionais para cada réplica de controlador de domínio que é recuperado na floresta por meio da clonagem de controlador de domínio virtualizado ou instalando o AD DS (em vez de restaurar do backup):  
  
- Todos os softwares em um controlador de domínio que é usado como a origem para clonagem deve ser capaz de ser clonado. Aplicativos e serviços que não não possível clonar devem ser removidos antes da clonagem é iniciada. Se isso não for possível, um controlador de domínio virtualizado alternativo deve ser escolhido como a origem.  
- Se você clonar controladores de domínio virtualizados adicionais do primeiro controlador de domínio virtualizado a ser restaurado, o DC de origem precisará ser desligada enquanto seu arquivo VHDX é copiado. Em seguida, ele precisará estar em execução e disponíveis online quando o clone controladores de domínio virtuais estiverem iniciado pela primeira vez. Se o tempo de inatividade exigido pelo desligamento não for aceitável para o primeiro controlador de domínio recuperado, implante um controlador de domínio virtualizado adicional instalando o AD DS para atuar como a origem para clonagem.  
- Não há nenhuma restrição no nome do host do DC virtualizado clonado ou do servidor no qual você deseja instalar o AD DS. Você pode usar um novo nome de host ou o nome do host que estava anteriormente em uso. Para obter mais informações sobre a sintaxe de nome de host DNS, consulte [criação de nomes de computador DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configure cada servidor com o primeiro servidor DNS da floresta (o primeiro controlador de domínio que foi restaurado no domínio raiz) como o servidor DNS preferencial nas propriedades de TCP/IP do seu adaptador de rede. Para obter mais informações, consulte [configurar o TCP/IP para usar DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Reimplantar todos os RODCs no domínio, por controlador de domínio virtualizados clonando se vários RODCs forem implantados em um local central, ou pelo método tradicional de recriação-los por remover e reinstalar o AD DS se eles são implantados individualmente nos locais localizados isolados como filiais.  
   - Recompilar os RODCs garante que eles não contêm todos os objetos remanescentes e podem ajudar a impedir conflitos de replicação mais tarde. Quando você remove o AD DS de um RODC *escolha a opção para manter metadados do controlador de domínio*. Usando essa opção retém a conta krbtgt para o RODC e retém as permissões para a conta de administrador do RODC delegado e a política de replicação de senha (PRP) e evita a necessidade de usar credenciais de administrador de domínio para remover e reinstalar o AD DS em um RODC. Ele também mantém as funções de catálogo global e o servidor DNS se estiverem instalados no RODC originalmente.  
   - Quando você recria os controladores de domínio (RODCs ou controladores de domínio graváveis), pode haver aumento do tráfego de replicação durante sua reinstalação. Para ajudar a reduzir esse impacto, você pode equilibrar a agenda das instalações do RODC, e você pode usar a opção de instalar da mídia (IFM). Se você usar a opção IFM, execute as **ntdsutil ifm** comando em um controlador de domínio gravável que você confia que esteja livre de dados danificados. Isso ajuda a evitar a possível corrupção apareça no RODC, após a reinstalação do AD DS é concluída. Para obter mais informações sobre IFM, consulte [instalação do AD DS da mídia](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Para obter mais informações sobre como reconstruir os RODCs, consulte [RODC remoção e reinstalação](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Se um controlador de domínio estava executando o serviço servidor DNS antes do mau funcionamento de floresta, instalar e configurar o serviço servidor DNS durante a instalação do AD DS. Caso contrário, configure seus clientes DNS antigos com outros servidores DNS.  
- Se você precisar de catálogos globais adicionais para compartilhar a carga de consulta para usuários ou aplicativos ou de autenticação, você pode adicionar o catálogo global para a fonte virtualizados controlador de domínio antes de clonar ou você pode fazer um servidor de catálogo global de um controlador de domínio durante a instalação do AD DS.  
  
## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD - elaborar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD - executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD - recuperação de um único domínio dentro de uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD - recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
