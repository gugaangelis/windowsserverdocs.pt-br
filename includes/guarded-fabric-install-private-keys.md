Se você não forneceu um arquivo PFX para a criptografia ou assinatura de certificados no primeiro servidor HGS, somente a chave pública será replicada para este servidor.
Você precisará instalar a chave privada, importando um arquivo PFX que contém a chave privada no repositório de certificados local ou, no caso de chaves protegidas por HSM, configuração do provedor de armazenamento de chave e associá-lo com os certificados por seu fabricante HSM instruções.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->