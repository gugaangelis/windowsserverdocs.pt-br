---
title: Virtualização com segurança do AD DS (Active Directory Domain Services)
description: Reversão de USN e virtualização segura do Active Directory
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 25a5c2222f50b37bff2bcfe41184d6d9fa35995c
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465500"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualização com segurança do AD DS (Active Directory Domain Services)

>Aplica-se a: Windows Server

Desde o Windows Server 2012 o AD DS oferece maior compatibilidade com a virtualização de controladores de domínio, pois inclui funcionalidades de virtualização segura. Este artigo explica a função dos USNs e de InvocationIDs na replicação do Controlador de Domínio e discute alguns problemas que podem ocorrer.

## <a name="update-sequence-number-and-invocationid"></a>Números de sequência de atualização e InvocationID

Os ambientes virtuais apresentam desafios exclusivos a cargas de trabalho distribuídas que dependem de um esquema de replicação lógico baseado no relógio. A replicação do AD DS, por exemplo, usa um valor que aumenta continuamente (conhecido como USN ou Números de Sequência de Atualização) atribuído às transações em cada controlador de domínio. A instância de banco de dados de cada controlador de domínio também recebe uma identidade, conhecida como InvocationID. A InvocationID do controlador de domínio juntamente com seu USN atuam como um identificador exclusivo associado a cada transação de gravação executada em cada controlador de domínio e deve ser única na floresta.

A replicação do AD DS usa a InvocationID e os USNs em cada controlador de domínio para determinar as alterações que devem ser replicadas para outros controladores de domínio. Se um controlador de domínio for revertido sem o reconhecimento do controlador e um USN for reutilizado em uma transação totalmente diferente, a replicação não será convergida porque outros controladores de domínio vão entender que já receberam as atualizações associadas ao USN reutilizado no contexto dessa InvocationID.

Por exemplo, a ilustração a seguir mostra a sequência de eventos que ocorre no Windows Server 2008 R2 e em sistemas operacionais anteriores quando é detectada a reversão de USN no VDC2, o controlador de domínio de destino em execução na máquina virtual. Nesta ilustração, a detecção da reversão de USN ocorre no VDC2 quando um parceiro de replicação detecta que o VDC2 enviou um valor de USN atual visto anteriormente pelo parceiro de replicação, indicando que o banco de dados do VDC2 foi revertido incorretamente.

![A sequência de eventos quando a reversão de USN é detectada](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Por exemplo, uma VM (máquina virtual) facilita para os administradores do hipervisor reverterem os USNs (seu relógio lógico) de um controlador de domínio aplicando um instantâneo sem o reconhecimento do controlador de domínio. Para obter mais informações sobre USN e reversão de USN, inclusive outra ilustração para demonstrar instâncias não detectadas de reversão de USN, consulte [USN e Reversão de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

Desde o Windows Server 2012, os controladores de domínio virtuais do AD DS hospedados em plataformas do hipervisor que expõem um identificador chamado ID de Geração de VM detectam e tomam as medidas de segurança necessárias para proteger o ambiente do AD DS quando a máquina virtual é revertida através da aplicação de um instantâneo de VM. O design da ID de Geração de VM utiliza um mecanismo do fornecedor do hipervisor independente para expor esse identificador no espaço do endereço da máquina virtual convidada, assim a experiência de virtualização segura fica igualmente disponível em qualquer hipervisor com suporte à ID de Geração de VM. Esse identificador pode ser exemplificado por serviços e aplicativos executados na máquina virtual para detectar se a máquina virtual foi revertida.

## <a name="effects-of-usn-rollback"></a>Efeitos da reversão de USN

Quando ocorrem reversões de USN, as modificações em objetos e atributos não são replicadas de entrada por controladores de domínio de destino que já viram o USN.

Como esses controladores de domínio de destino acreditam estar atualizados, nenhum erro de replicação é relatado nos logs de eventos do Serviço de Diretório ou por ferramentas de monitoramento e diagnóstico.

A reversão de USN pode afetar a replicação de qualquer objeto ou atributo em qualquer partição. O efeito colateral observado com mais frequência é que contas de usuário e de computador criadas no controlador de domínio de reversão não existem em um ou mais parceiros de replicação. Ou, as atualizações de senha originadas no controlador de domínio de reversão não existem nos parceiros de replicação.

Uma reversão de USN pode impedir a replicação de qualquer tipo de objeto em qualquer partição do Active Directory. Esses tipos de objeto incluem o seguinte:

* A topologia e a agenda de replicação do Active Directory
* A existência de controladores de domínio na floresta e as funções que esses controladores de domínio mantêm
* A existência de partições de domínio e de aplicativo na floresta
* A existência de grupos de segurança e suas associações de grupo atuais
* Registro de registro DNS em zonas DNS integradas do Active Directory

O tamanho da falha de USN pode representar centenas, milhares ou até mesmo dezenas de milhares de alterações em usuários, computadores, relações de confiança, senhas e grupos de segurança. A falha de USN é definida pela diferença entre o número USN mais alto que existia quando o backup do estado do sistema restaurado foi feito e o número de alterações de origem que foram criadas no controlador de domínio revertido antes de ele ser colocado offline.

## <a name="detecting-a-usn-rollback"></a>Detectando uma reversão de USN

Como uma reversão de USN é difícil de detectar, um controlador de domínio registra o evento 2095 quando um controlador de domínio de origem envia um número USN confirmado anteriormente para um controlador de domínio de destino sem uma alteração correspondente na ID de invocação.

Para impedir que atualizações de origem exclusivas do Active Directory sejam criadas no controlador de domínio restaurado incorretamente, o serviço Logon de Rede está em pausa. Quando o serviço Logon de Rede é pausado, as contas de usuário e computador não podem alterar a senha em um controlador de domínio que não fará a replicação de saída dessas alterações. Da mesma forma, as ferramentas de administração do Active Directory favorecerão um controlador de domínio íntegro quando fizerem atualizações em objetos no Active Directory.

Em um controlador de domínio, as mensagens de evento semelhantes às seguintes serão registradas se as seguintes condições forem verdadeiras:

* Um controlador de domínio de origem envia um número de USN confirmado anteriormente para um controlador de domínio de destino.
* Não há alteração correspondente na ID de invocação.

Esses eventos podem ser capturados no log de eventos do Serviço de Diretório. No entanto, eles podem ser substituídos antes de serem observados por um administrador.

Caso você suspeite de que uma reversão de USN ocorreu, mas não veja um evento correspondente nos logs de eventos, verifique a entrada DSA Not Writable no Registro. Essa entrada fornece evidências forenses de que uma reversão de USN ocorreu.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> A exclusão ou a alteração manual do valor de entrada de registro DSA Not Writable coloca o controlador de domínio de reversão em um estado permanentemente sem suporte. Portanto, essas alterações não têm suporte. Especificamente, modificar o valor remove o comportamento de quarentena adicionado pelo código de detecção de reversão de USN. As partições do Active Directory no controlador de domínio de reversão ficarão permanentemente inconsistentes com parceiros de replicação direta e transitiva na mesma floresta do Active Directory.

Mais informações sobre essa chave do Registro e as etapas de resolução podem ser encontradas no artigo de suporte [Erro de replicação 8456 ou 8457 do Active Directory: "O servidor de origem | destino está rejeitando solicitações de replicação"](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Proteções baseadas em virtualização

Durante a instalação do controlador de domínio, o AD DS inicialmente armazena o identificador GenerationID da VM como parte do atributo msDS-GenerationID no objeto de computador do controlador de domínio em seu banco de dados (geralmente chamado de árvore de informações do diretório ou DIT). A ID de Geração de VM é acompanhada de forma independente por um driver do Windows na máquina virtual.

Quando o administrador restaura a máquina virtual de um instantâneo anterior, o valor atual da ID de Geração de VM do driver da máquina virtual é comparado com o valor na DIT.

Se os dois valores forem diferentes, a invocationID será redefinida e o pool RID descartado, impedindo assim a reutilização do USN. Se os valores forem iguais, a transação será confirmada como de costume.

O AD DS também compara o valor atual da ID de Geração de VM da máquina virtual com o valor na DIT toda vez que o controlador de domínio é reinicializado e, se forem diferentes, ele redefinirá a invocationID, descartará o pool RID e atualizará a DIT com o novo valor. Ele também sincronizará a pasta SYSVOL de maneira não autoritativa para concluir a restauração segura. Isso permite estender as garantias para a aplicação de instantâneos às VMs que foram desligadas. As garantias introduzidas no Windows Server 2012 permitem que os administradores do AD DS aproveitem as vantagens exclusivas da implantação e do gerenciamento de controladores de domínio em um ambiente virtualizado.

A ilustração a seguir mostra como são aplicadas as garantias de virtualização quando é detectada a reversão do mesmo USN em um controlador de domínio virtualizado que executa o Windows Server 2012 em um hipervisor com suporte ao VM-GenerationID.

![Proteções aplicadas quando a mesma reversão de USN é detectada](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Neste caso, quando o hipervisor detecta uma alteração no valor da ID de Geração de VM, as garantias de virtualização são disparadas, incluindo a redefinição da InvocationID no controlador de domínio virtualizado (de A a B no exemplo anterior) e a atualização do valor da ID de Geração de VM salvo na VM para corresponder ao novo valor (G2) armazenado pelo hipervisor. As garantias asseguram que a replicação seja convergida nos dois controladores de domínio.

Com o Windows Server 2012, o AD DS aplica garantias a controladores de domínio virtuais hospedados em hipervisores com reconhecimento de VM-GenerationID e assegura que a aplicação acidental de instantâneos ou de outros mecanismos similares habilitados por hipervisor capazes de reverter o estado de uma máquina virtual não prejudique o ambiente do AD DS (evitando problemas de replicação, como confusão entre USNs ou objetos remanescentes).

A restauração de um controlador de domínio por meio da aplicação do instantâneo de uma máquina virtual não é recomendada como mecanismo alternativo de backup do controlador de domínio. É recomendado continuar usando o Backup do Windows Server ou outras soluções de backup baseadas no gravador VSS.

> [!CAUTION]
> Se um controlador de domínio em um ambiente de produção for revertido acidentalmente para um instantâneo, será aconselhável consultar os fornecedores dos aplicativos e dos serviços hospedados na máquina virtual, para receber orientação sobre como verificar o estado desses programas após a restauração com instantâneo.

Para obter mais informações, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Recuperando de uma reversão de USN

Há duas abordagens para recuperar-se de uma reversão de USN:

* Remover o controlador de domínio do domínio
* Restaurar o estado do sistema de um bom backup

### <a name="remove-the-domain-controller-from-the-domain"></a>Remover o controlador de domínio do domínio

1. Remova o Active Directory do controlador de domínio para forçá-lo a ser um servidor autônomo.
2. Desligue o servidor rebaixado.
3. Em um controlador de domínio íntegro, limpe os metadados do controlador de domínio rebaixado.
4. Se o controlador de domínio restaurado incorretamente hospeda funções de mestre de operações, transfira essas funções para um controlador de domínio íntegro.
5. Reinicie o servidor rebaixado.
6. Se for necessário, instale o Active Directory novamente no servidor autônomo.
7. Se o controlador de domínio anteriormente era um catálogo global, configure o controlador de domínio para ser um catálogo global.
8. Se o controlador de domínio anteriormente hospedou funções de mestre de operações, transfira as funções de mestre de operações de volta para o controlador de domínio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurar o estado do sistema de um bom backup

Avalie se existem backups de estado do sistema válidos para esse controlador de domínio. Se um backup de estado do sistema válido tiver sido feito antes de o controlador de domínio revertido ter sido restaurado incorretamente e o backup contiver alterações recentes feitas no controlador de domínio, restaure o estado do sistema com base no backup mais recente.

Você também pode usar o instantâneo como uma origem de um backup. Ou pode definir o banco de dados para dar a si mesmo uma nova ID de invocação usando o procedimento na seção [Restaurando um controlador de domínio virtual quando um backup de dados de estado do sistema apropriado não estiver disponível](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Próximas etapas

* Para obter informações de solução de problemas sobre controladores de domínio virtualizados, consulte [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informações detalhadas sobre o Serviço de Tempo do Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
