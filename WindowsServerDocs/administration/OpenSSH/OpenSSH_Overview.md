---
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendMSFT
keywords: OpenSSH, SSH, Win32-OpenSSH
title: OpenSSH no Windows
ms.product: w10
ms.openlocfilehash: 68ced1ff133495d7658e486e7e72321e18857b21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843357"
---
# <a name="openssh-in-windows"></a>OpenSSH no Windows

OpenSSH é a versão do código-fonte aberto das ferramentas do Secure Shell (SSH) usado por administradores do Linux e outros não Windows para gerenciamento de plataforma cruzada de sistemas remotos. OpenSSH foi adicionado ao Windows a partir do outono de 2018 e está incluído no Windows 10 e Windows Server 2019. 

SSH baseia-se em uma arquitetura cliente-servidor em que o sistema que o usuário está trabalhando é o cliente e o sistema remoto que está sendo gerenciado é o servidor. OpenSSH inclui uma variedade de componentes e ferramentas projetadas para oferecer uma abordagem simples e segura para administração do sistema remoto, incluindo:

* sshd.exe, que é o componente do servidor SSH deve estar em execução no sistema que está sendo gerenciado remotamente 
* SSH.exe, que é o componente de cliente SSH que é executado no sistema local do usuário
* SSH keygen.exe gera, gerencia e converte as chaves de autenticação para o SSH 
* SSH agent.exe armazena as chaves particulares usadas para autenticação de chave pública
* SSH add.exe adiciona chaves privadas à lista de permitidos pelo servidor
* auxílios de SSH keyscan.exe na coleta as chaves públicas de host SSH de um número de hosts
* SFTP.exe é o serviço que fornece o arquivo de protocolo e executa-se via SSH
* SCP.exe é um utilitário de cópia de arquivo que é executado em SSH

Documentação nesta seção se concentra em como o OpenSSH é usado no Windows, incluindo a instalação e casos de configuração e o uso do Windows específicas. Estes são os tópicos:
* Instalando e desinstalando OpenSSH para Windows Server 2019 e Windows 10

Mais documentação detalhada de recursos comuns do OpenSSH está disponível online no [OpenSSH.com](https://www.openssh.com/manual.html). 

O mestre [projeto de software livre do OpenSSH](https://www.openssh.com) é gerenciada pelos desenvolvedores no projeto OpenBSD. A bifurcação Microsoft deste projeto está em [Github](https://github.com/PowerShell/openssh-portable).
Comentários sobre o Windows OpenSSH é bem-vinda e pode ser fornecido com a criação de problemas do Github em nosso [repositório Github do OpenSSH](https://github.com/PowerShell/openssh-portable). 
