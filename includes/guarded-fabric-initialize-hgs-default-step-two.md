Localize seus certificados de guardião do HGS. Você precisará de um certificado de autenticação e um certificado de criptografia para inicializar o cluster HGS.
A maneira mais fácil fornecer certificados para HGS é criar um arquivo PFX protegido por senha para cada certificado que contém as chaves públicas e privadas. Se você estiver usando chaves protegidas por HSM ou outros certificados não exportável, verifique se que o certificado está instalado no repositório de certificados do computador local antes de continuar.
Para obter mais informações sobre quais certificados a serem usados, consulte [obter certificados para HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs).

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->