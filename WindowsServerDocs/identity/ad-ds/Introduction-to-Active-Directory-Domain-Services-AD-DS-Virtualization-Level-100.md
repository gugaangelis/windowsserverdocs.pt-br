---
title: Virtualização segura de serviços de domínio Active Directory (AD DS)
description: A reversão do USN e virtualização segura do Active Directory
ms.topic: article
ms.prod: windows-server-threshold
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: aa84e09e8a958193fee82c7b9c03cd1dca910c55
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63684172"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualização segura de serviços de domínio Active Directory (AD DS)

>Aplica-se a: Windows Server

Começando com o Windows Server 2012, o AD DS fornece um suporte mais abrangente para a virtualização de controladores de domínio com a introdução de recursos de virtualização segura. Este artigo explica a função de USNs e InvocationIDs na replicação do controlador de domínio e aborda alguns problemas potenciais que podem ocorrer.

## <a name="update-sequence-number-and-invocationid"></a>Número de sequência de atualização e InvocationID

Os ambientes virtuais apresentam desafios exclusivos a cargas de trabalho distribuídas que dependem de um esquema de replicação lógico baseado no relógio. A replicação do AD DS, por exemplo, usa um valor que aumenta continuamente (conhecido como USN ou Números de Sequência de Atualização) atribuído às transações em cada controlador de domínio. Instância de banco de dados de cada controlador de domínio também recebe uma identidade, conhecida como InvocationID. A InvocationID do controlador de domínio juntamente com seu USN atuam como um identificador exclusivo associado a cada transação de gravação executada em cada controlador de domínio e deve ser única na floresta.

A replicação do AD DS usa a InvocationID e os USNs em cada controlador de domínio para determinar as alterações que devem ser replicadas para outros controladores de domínio. Se um controlador de domínio é revertido no tempo fora de reconhecimento do controlador de domínio e o USN for reutilizado para uma transação totalmente diferente, a replicação não será convergida porque outros controladores de domínio vão achar que já receberam as atualizações associado com o USN reutilizado no contexto dessa invocationid.

Por exemplo, a ilustração a seguir mostra a sequência de eventos que ocorre no Windows Server 2008 R2 e em sistemas operacionais anteriores quando é detectada a reversão de USN no VDC2, o controlador de domínio de destino em execução na máquina virtual. Nesta ilustração, a detecção da reversão do USN ocorre no VDC2 quando um parceiro de replicação detecta que o VDC2 enviou um valor de USN atual visto anteriormente pelo parceiro de replicação, que indica que banco de dados do VDC2 foi revertido tempo incorretamente.

![A sequência de eventos quando a reversão do USN é detectada](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Uma máquina virtual (VM) facilita para os administradores do hipervisor reverter um domínio USNs do controlador (seu relógio lógico), por exemplo, aplicar um instantâneo fora de reconhecimento do controlador de domínio. Para obter mais informações sobre USN e reversão de USN, inclusive outra ilustração para demonstrar instâncias não detectadas de reversão de USN, consulte [USN e Reversão de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

Começando com o Windows Server 2012, controladores de domínio virtual do AD DS hospedados em plataformas de hipervisor que expõem um identificador chamado ID de geração de VM detectam e tomam medidas de segurança necessárias para proteger o ambiente do AD DS, se a máquina virtual é revertida de volta no tempo pelo aplicativo de um instantâneo VM. O design da ID de Geração de VM utiliza um mecanismo do fornecedor do hipervisor independente para expor esse identificador no espaço do endereço da máquina virtual convidada, assim a experiência de virtualização segura fica igualmente disponível em qualquer hipervisor com suporte à ID de Geração de VM. Esse identificador pode ser exemplificado por serviços e aplicativos executados na máquina virtual para detectar se a máquina virtual foi revertida.

## <a name="effects-of-usn-rollback"></a>Efeitos da reversão do USN

Quando ocorrem reversões de USN, modificações em objetos e atributos não são entradas replicados por controladores de domínio de destino que tenham visto anteriormente o USN.

Como esses controladores de domínio de destino acreditam que estão atualizados, nenhum erro de replicação é relatado nos logs de eventos do serviço de diretório ou pelas ferramentas de diagnóstico e monitoramento.

A reversão de USN pode afetar a replicação de qualquer objeto ou atributo em qualquer partição. O efeito colateral observado com mais frequência é que contas de usuário e contas de computador são criadas no controlador de domínio de reversão não existem em um ou mais parceiros de replicação. Ou, as atualizações de senha originadas no controlador de domínio de reversão não existem em parceiros de replicação.

Uma reversão de USN pode impedir que qualquer tipo de objeto em qualquer partição de diretório Active Directory replicando. Esses tipos de objeto incluem o seguinte:

* A topologia de replicação do Active Directory e a agenda
* A existência de controladores de domínio na floresta e as funções que contêm esses controladores de domínio
* A existência de partições de aplicativo e o domínio da floresta
* A existência de grupos de segurança e suas associações de grupo atual
* Registro de registro de DNS em zonas DNS integradas ao Active Directory

O tamanho do orifício USN pode representar centenas, milhares ou até mesmo dezenas de milhares de alterações para os usuários, computadores, relações de confiança, senhas e grupos de segurança. O buraco USN é definido pela diferença entre o maior número de USN que existia quando o backup de estado do sistema restaurado foi feito e altera o número de origem que foram criados no controlador de domínio revertido antes que ele foi colocado offline.

## <a name="detecting-a-usn-rollback"></a>Detectando uma reversão de USN

Como a reversão de USN é difícil de detectar, um controlador de domínio registra o evento 2095 quando um controlador de domínio de origem envia um número de USN confirmado anteriormente a um controlador de domínio de destino sem uma alteração correspondente na ID de invocação.

Para evitar exclusivos provenientes de atualizações para o Active Directory que está sendo criado no controlador de domínio restaurado incorretamente, o serviço de Logon de rede está em pausa. Quando o serviço de Logon de rede está em pausa, contas de usuário e computador não é possível alterar a senha em um controlador de domínio que será não saída replicar essas alterações. Da mesma forma, as ferramentas de administração do Active Directory irão favorecer um controlador de domínio íntegro quando eles fizerem as atualizações para objetos no Active Directory.

Em um controlador de domínio, as mensagens de evento semelhantes aos seguintes são registradas, se as seguintes condições forem verdadeiras:

* Um controlador de domínio de origem envia um número de USN confirmado anteriormente a um controlador de domínio de destino.
* Não há nenhuma alteração correspondente na ID de invocação.

Esses eventos podem ser capturados no log de eventos do serviço de diretório. No entanto, eles poderão ser substituídos antes que eles são observados pelo administrador.

Se você suspeitar de uma reversão de USN ocorreu, mas não vir um evento correspondente no caso de logs, verifique a entrada DSA não podem ser gravadas no registro. Essa entrada fornece evidência forense que uma reversão de USN ocorreu.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> Excluir ou alterar manualmente o valor de entrada do registro Dsa não gravável coloca o controlador de domínio de reversão em um estado permanentemente sem suporte. Portanto, essas alterações não são suportadas. Especificamente, a modificação do valor remove o comportamento de quarentena adicionado pelo código de detecção de reversão do USN. As partições de diretório Active Directory no controlador de domínio de reversão será permanentemente inconsistentes com parceiros de replicação transitivas e diretas na mesma floresta do Active Directory.

Obter mais informações sobre esse registro chave e etapas de resolução podem ser encontradas no artigo de suporte [Active Directory Replication erro 8456 ou 8457: "O código-fonte | servidor de destino está rejeitando solicitações de replicação"](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Garantias de virtualização com base em

Durante a instalação do controlador de domínio, o AD DS inicialmente armazena o identificador ID de geração de VM como parte do atributo msDS-GenerationID no objeto de computador do controlador de domínio em seu banco de dados (também conhecido como a árvore de informações do diretório ou a DIT). A ID de Geração de VM é acompanhada de forma independente por um driver do Windows na máquina virtual.

Quando o administrador restaura a máquina virtual de um instantâneo anterior, o valor atual da ID de Geração de VM do driver da máquina virtual é comparado com o valor na DIT.

Se os dois valores forem diferentes, a invocationID será redefinida e o pool RID descartado, impedindo assim a reutilização do USN. Se os valores forem iguais, a transação será confirmada como de costume.

O AD DS também compara o valor atual da ID de Geração de VM da máquina virtual com o valor na DIT toda vez que o controlador de domínio é reinicializado e, se forem diferentes, ele redefinirá a invocationID, descartará o pool RID e atualizará a DIT com o novo valor. Ele também sincronizará a pasta SYSVOL de maneira não autoritativa para concluir a restauração segura. Isso permite estender as garantias para a aplicação de instantâneos às VMs que foram desligadas. Essas garantias introduzidas no Windows Server 2012 permitem que os administradores do AD DS aproveitar as vantagens exclusivas da implantação e gerenciamento de controladores de domínio em um ambiente virtualizado.

A ilustração a seguir mostra como as garantias de virtualização são aplicadas quando o reversão do mesmo USN é detectado em um controlador de domínio virtualizado que executa o Windows Server 2012 em um hipervisor que dá suporte a VM-GenerationID.

![Proteções aplicadas quando o reversão do mesmo USN é detectado](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Neste caso, quando o hipervisor detecta uma alteração no valor da ID de Geração de VM, as garantias de virtualização são disparadas, incluindo a redefinição da InvocationID no controlador de domínio virtualizado (de A a B no exemplo anterior) e a atualização do valor da ID de Geração de VM salvo na VM para corresponder ao novo valor (G2) armazenado pelo hipervisor. As garantias asseguram que a replicação seja convergida nos dois controladores de domínio.

Com o Windows Server 2012, o AD DS aplica garantias a controladores de domínio virtuais hospedados em hipervisores com reconhecimento de VM-GenerationID e garante que a aplicação acidental de instantâneos ou outros hipervisor-mecanismos similares habilitados por pôde reverter uma máquina virtual estado da máquina não prejudique o ambiente do AD DS (por evitando problemas de replicação, como USNs ou objetos remanescentes).

Restaurar um controlador de domínio aplicando um instantâneo de máquina virtual não é recomendado como um mecanismo alternativo para fazer backup de um controlador de domínio. É recomendado continuar usando o Backup do Windows Server ou outras soluções de backup baseadas no gravador VSS.

> [!CAUTION]
> Se um controlador de domínio em um ambiente de produção for revertido acidentalmente para um instantâneo, é recomendável que você consulte os fornecedores para os aplicativos e serviços hospedados na máquina virtual, para obter orientação sobre como verificar o estado desses programas após restauração de instantâneo.

Para obter mais informações, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Recuperação de uma reversão de USN

Há duas abordagens para se recuperar de uma reversão de USN:

* Remover o controlador de domínio do domínio
* Restaurar o estado do sistema de um bom backup

### <a name="remove-the-domain-controller-from-the-domain"></a>Remover o controlador de domínio do domínio

1. Remova o Active Directory do controlador de domínio para forçá-lo como um servidor autônomo.
2. Desligue o servidor rebaixado.
3. Em um controlador de domínio íntegro, limpe os metadados do controlador de domínio rebaixado.
4. Se as funções de mestre de operações de hosts de controlador de domínio restaurado incorretamente, transfira essas funções para um controlador de domínio íntegro.
5. Reinicie o servidor rebaixado.
6. Se é necessário, instale o Active Directory no servidor autônomo novamente.
7. Se o controlador de domínio anteriormente era um catálogo global, configure o controlador de domínio para ser um catálogo global.
8. Se o controlador de domínio anteriormente hospedado funções de mestre de operações, transfira as operações de funções de mestre de volta para o controlador de domínio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurar o estado do sistema de um bom backup

Avalie se backups de estado do sistema válido existirem para esse controlador de domínio. Se um backup de estado do sistema válido tiver sido feito antes que o controlador de domínio revertido tiver sido restaurado incorretamente, e o backup contém alterações recentes feitas no controlador de domínio, restaure o estado do sistema do backup mais recente.

Você também pode usar o instantâneo como uma fonte de um backup. Ou você pode definir o banco de dados para fornecer uma nova ID de invocação usando o procedimento na seção [restaurando um controlador de domínio virtual quando um backup de dados de estado do sistema adequado não está disponível](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Próximas etapas

* Para obter mais informações sobre solução de problemas dos controladores de domínio virtualizados, consulte [Solução de problemas do controlador de domínio virtualizado](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informações detalhadas sobre o serviço de tempo do Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
