---
title: Virtualizando Active Directory Domain Services com segurança (AD DS)
description: Reversão de USN e virtualização segura do Active Directory
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 67e35a47467b1f5f66bfd073c6f9db06094ea3f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391033"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualizando Active Directory Domain Services com segurança (AD DS)

>Aplica-se a: Windows Server

A partir do Windows Server 2012, o AD DS fornece maior suporte para a virtualização de controladores de domínio introduzindo recursos seguros de virtualização. Este artigo explica a função de USNs e InvocationIDs na replicação do controlador de domínio e discute alguns problemas potenciais que podem ocorrer.

## <a name="update-sequence-number-and-invocationid"></a>Atualizar número de sequência e invocação

Os ambientes virtuais apresentam desafios exclusivos a cargas de trabalho distribuídas que dependem de um esquema de replicação lógico baseado no relógio. A replicação do AD DS, por exemplo, usa um valor que aumenta continuamente (conhecido como USN ou Números de Sequência de Atualização) atribuído às transações em cada controlador de domínio. A instância de banco de dados de cada controlador de domínio também recebe uma identidade, conhecida como uma invocação. A InvocationID do controlador de domínio juntamente com seu USN atuam como um identificador exclusivo associado a cada transação de gravação executada em cada controlador de domínio e deve ser única na floresta.

A replicação do AD DS usa a InvocationID e os USNs em cada controlador de domínio para determinar as alterações que devem ser replicadas para outros controladores de domínio. Se um controlador de domínio for revertido no tempo fora do reconhecimento do controlador de domínio e um USN for reutilizado para uma transação totalmente diferente, a replicação não convergirá porque outros controladores de domínio acreditarão que já receberam as atualizações associado ao USN reutilizado no contexto dessa invocação.

Por exemplo, a ilustração a seguir mostra a sequência de eventos que ocorre no Windows Server 2008 R2 e em sistemas operacionais anteriores quando é detectada a reversão de USN no VDC2, o controlador de domínio de destino em execução na máquina virtual. Nesta ilustração, a detecção de reversão de USN ocorre em VDC2 quando um parceiro de replicação detecta que o VDC2 enviou um valor USN atualizado que foi visto anteriormente pelo parceiro de replicação, que indica que o banco de dados VDC2's foi revertido no tempo incorretamente.

![A sequência de eventos quando a reversão de USN é detectada](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Uma VM (máquina virtual) facilita para os administradores do hipervisor reverterem os USNs de um controlador de domínio (seu relógio lógico), por exemplo, aplicando um instantâneo fora do reconhecimento do controlador de domínio. Para obter mais informações sobre USN e reversão de USN, inclusive outra ilustração para demonstrar instâncias não detectadas de reversão de USN, consulte [USN e Reversão de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partir do Windows Server 2012, AD DS controladores de domínio virtuais hospedados em plataformas de hipervisor que expõem um identificador chamado ID de geração de VM podem detectar e empregar as medidas de segurança necessárias para proteger o ambiente de AD DS se a máquina virtual for revertida de volta ao tempo pelo aplicativo de um instantâneo de VM. O design da ID de Geração de VM utiliza um mecanismo do fornecedor do hipervisor independente para expor esse identificador no espaço do endereço da máquina virtual convidada, assim a experiência de virtualização segura fica igualmente disponível em qualquer hipervisor com suporte à ID de Geração de VM. Esse identificador pode ser exemplificado por serviços e aplicativos executados na máquina virtual para detectar se a máquina virtual foi revertida.

## <a name="effects-of-usn-rollback"></a>Efeitos da reversão de USN

Quando ocorrem reversões de USN, as modificações em objetos e atributos não são replicadas de entrada por controladores de domínio de destino que já viram o USN.

Como esses controladores de domínio de destino acreditam estar atualizados, nenhum erro de replicação é relatado nos logs de eventos do serviço de diretório ou por ferramentas de monitoramento e diagnóstico.

A reversão do USN pode afetar a replicação de qualquer objeto ou atributo em qualquer partição. O efeito colateral observado com mais frequência é que contas de usuário e contas de computador criadas no controlador de domínio de reversão não existem em um ou mais parceiros de replicação. Ou, as atualizações de senha originadas no controlador de domínio de reversão não existem nos parceiros de replicação.

Uma reversão de USN pode impedir qualquer tipo de objeto em qualquer partição de Active Directory da replicação. Esses tipos de objeto incluem o seguinte:

* A topologia e a agenda de replicação de Active Directory
* A existência de controladores de domínio na floresta e as funções que esses controladores de domínio mantêm
* A existência de partições de domínio e de aplicativo na floresta
* A existência de grupos de segurança e suas associações de grupo atuais
* Registro de registro DNS em zonas DNS integradas Active Directory

O tamanho da brecha de USN pode representar centenas, milhares ou até mesmo dezenas de milhares de alterações para usuários, computadores, relações de confiança, senhas e grupos de segurança. A brecha de USN é definida pela diferença entre o número USN mais alto que existia quando o backup do estado do sistema restaurado foi feito e o número de alterações de origem que foram criadas no controlador de domínio revertido antes de ele ser colocado offline.

## <a name="detecting-a-usn-rollback"></a>Detectando uma reversão de USN

Como uma reversão de USN é difícil de detectar, um controlador de domínio registra o evento 2095 quando um controlador de domínio de origem envia um número USN confirmado anteriormente para um controlador de domínio de destino sem uma alteração correspondente na ID de invocação.

Para impedir que atualizações de origem exclusivas Active Directory sejam criadas no controlador de domínio restaurado incorretamente, o serviço de logon de rede está em pausa. Quando o serviço de logon de rede é pausado, as contas de usuário e computador não podem alterar a senha em um controlador de domínio que não fará a replicação de saída dessas alterações. Da mesma forma, as ferramentas de administração do Active Directory favorecerão um controlador de domínio íntegro quando fizerem atualizações em objetos no Active Directory.

Em um controlador de domínio, as mensagens de evento semelhantes às seguintes são registradas se as seguintes condições forem verdadeiras:

* Um controlador de domínio de origem envia um número de USN confirmado anteriormente para um controlador de domínio de destino.
* Não há nenhuma alteração correspondente na ID de invocação.

Esses eventos podem ser capturados no log de eventos do serviço de diretório. No entanto, eles podem ser substituídos antes de serem observados por um administrador.

Se você suspeitar que uma reversão de USN ocorreu, mas não ver um evento correspondente nos logs de eventos, verifique a entrada DSA não gravável no registro. Essa entrada fornece evidências forenses de que uma reversão de USN ocorreu.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> A exclusão ou a alteração manual do valor de entrada de registro DSA não gravável coloca o controlador de domínio de reversão em um estado permanentemente sem suporte. Portanto, essas alterações não têm suporte. Especificamente, modificar o valor remove o comportamento de quarentena adicionado pelo código de detecção de reversão de USN. As partições de Active Directory no controlador de domínio de reversão serão permanentemente inconsistentes com parceiros de replicação direta e transitiva na mesma floresta de Active Directory.

Mais informações sobre essa chave do registro e as etapas de resolução podem ser encontradas no artigo de suporte [Active erro de replicação de diretório 8456 ou 8457: "A origem | Atualmente, o servidor de destino está rejeitando as solicitações de replicação "](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Proteções baseadas em virtualização

Durante a instalação do controlador de domínio, AD DS inicialmente armazena o identificador de geração de VM como parte do atributo msDS-Generationid no objeto de computador do controlador de domínio em seu banco de dados (geralmente conhecido como a árvore de informações de diretório ou DIT). A ID de Geração de VM é acompanhada de forma independente por um driver do Windows na máquina virtual.

Quando o administrador restaura a máquina virtual de um instantâneo anterior, o valor atual da ID de Geração de VM do driver da máquina virtual é comparado com o valor na DIT.

Se os dois valores forem diferentes, a invocationID será redefinida e o pool RID descartado, impedindo assim a reutilização do USN. Se os valores forem iguais, a transação será confirmada como de costume.

O AD DS também compara o valor atual da ID de Geração de VM da máquina virtual com o valor na DIT toda vez que o controlador de domínio é reinicializado e, se forem diferentes, ele redefinirá a invocationID, descartará o pool RID e atualizará a DIT com o novo valor. Ele também sincronizará a pasta SYSVOL de maneira não autoritativa para concluir a restauração segura. Isso permite estender as garantias para a aplicação de instantâneos às VMs que foram desligadas. Essas proteções introduzidas no Windows Server 2012 permitem que os administradores de AD DS se beneficiem das vantagens exclusivas da implantação e do gerenciamento de controladores de domínio em um ambiente virtualizado.

A ilustração a seguir mostra como as proteções de virtualização são aplicadas quando a mesma reversão de USN é detectada em um controlador de domínio virtualizado que executa o Windows Server 2012 em um hipervisor que dá suporte a VM-Generationid.

![Proteções aplicadas quando a mesma reversão de USN é detectada](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Neste caso, quando o hipervisor detecta uma alteração no valor da ID de Geração de VM, as garantias de virtualização são disparadas, incluindo a redefinição da InvocationID no controlador de domínio virtualizado (de A a B no exemplo anterior) e a atualização do valor da ID de Geração de VM salvo na VM para corresponder ao novo valor (G2) armazenado pelo hipervisor. As garantias asseguram que a replicação seja convergida nos dois controladores de domínio.

Com o Windows Server 2012, o AD DS emprega proteções em controladores de domínio virtuais hospedados em hipervisores cientes da VM-Generationid e garante que o aplicativo acidental de instantâneos ou outros mecanismos habilitados para hipervisor possam reverter uma máquina virtual o estado da máquina não interrompe o ambiente de AD DS (impedindo problemas de replicação, como uma bolha USN ou objetos remanescentes).

A restauração de um controlador de domínio aplicando um instantâneo de máquina virtual não é recomendável como um mecanismo alternativo para fazer backup de um controlador de domínio. É recomendado continuar usando o Backup do Windows Server ou outras soluções de backup baseadas no gravador VSS.

> [!CAUTION]
> Se um controlador de domínio em um ambiente de produção for acidentalmente revertido para um instantâneo, é recomendável que você consulte os fornecedores para os aplicativos e serviços hospedados nessa máquina virtual, para obter orientação sobre como verificar o estado desses programas após restauração de instantâneo.

Para obter mais informações, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Recuperando de uma reversão de USN

Há duas abordagens para recuperar-se de uma reversão de USN:

* Remover o controlador de domínio do domínio
* Restaurar o estado do sistema de um bom backup

### <a name="remove-the-domain-controller-from-the-domain"></a>Remover o controlador de domínio do domínio

1. Remova Active Directory do controlador de domínio para forçá-lo a ser um servidor autônomo.
2. Desligue o servidor rebaixado.
3. Em um controlador de domínio íntegro, limpe os metadados do controlador de domínio rebaixado.
4. Se o controlador de domínio restaurado incorretamente hospedar funções de mestre de operações, transfira essas funções para um controlador de domínio íntegro.
5. Reinicie o servidor rebaixado.
6. Se for necessário, instale Active Directory no servidor autônomo novamente.
7. Se o controlador de domínio era anteriormente um catálogo global, configure o controlador de domínio para ser um catálogo global.
8. Se o controlador de domínio tiver hospedado anteriormente funções de mestre de operações, transfira as funções de mestre de operações de volta para o controlador de domínio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurar o estado do sistema de um bom backup

Avalie se existem backups de estado do sistema válidos para esse controlador de domínio. Se um backup de estado do sistema válido tiver sido feito antes de o controlador de domínio revertido ter sido restaurado incorretamente e o backup contiver alterações recentes feitas no controlador de domínio, restaure o estado do sistema a partir do backup mais recente.

Você também pode usar o instantâneo como uma origem de um backup. Ou você pode definir o banco de dados para dar a si mesmo uma nova ID de invocação usando o procedimento na seção [restaurando um controlador de domínio virtual quando um backup de dados de estado do sistema apropriado não estiver disponível](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Próximas etapas

* Para obter mais informações sobre solução de problemas dos controladores de domínio virtualizados, consulte [Solução de problemas do controlador de domínio virtualizado](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informações detalhadas sobre o serviço de tempo do Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
