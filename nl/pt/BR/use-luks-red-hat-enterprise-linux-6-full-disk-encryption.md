---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando o LUKS no Red Hat Enterprise Linux para criptografia total de disco

O Linux Unified Key Setup-on-disk-format (LUKS) permite criptografar partições em seu Red Hat Enterprise
Linux 6 (servidor), que é particularmente importante quando se trata de computadores móveis e de mídia
removível. O LUKS permite que múltiplas chaves de usuário decriptografem uma chave mestra
que é usada para a criptografia em massa da partição.

## O que o LUKS faz:

- Criptografa dispositivos de bloco inteiros sendo, portanto, adequado para proteção do conteúdo de
dispositivos móveis, como mídia de armazenamento removível ou unidades de disco laptop.
    - O conteúdo subjacente do dispositivo de bloco criptografado é arbitrário, tornando útil para
criptografia de dispositivos de troca. A criptografia também pode ser útil com determinados bancos de dados
que usam dispositivos de bloco especialmente formatados para armazenamento de dados.
- Usa o subsistema de kernel do mapeador de dispositivo existente.
- Fornece reforço de passphrase, que protege contra anexações de dicionário.
- Permite que os usuários incluam chaves ou passphrases de backup, já que os dispositivos LUKS
contêm múltiplos slots de chave.


## O que o LUKS não faz:

- Permitir que aplicativos que requerem muitos usuários (mais de oito) tenham chaves de acesso
distintas para os mesmos dispositivos.
- Trabalhar com aplicativos que requerem criptografia de nível de arquivo,
[mais
informações](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}.

## Como configurar o novo volume LUKS criptografado com
{{site.data.keyword.blockstorageshort}} de resistência

Estas etapas supõem que o servidor já tem acesso a um novo volume
do {{site.data.keyword.blockstoragefull}} não criptografado que não tenha sido formatado ou montado. 
Clique [aqui](accessing_block_storage_linux.html) para saber como acessar
o {{site.data.keyword.blockstorageshort}} com o Linux.

Observe que a execução da criptografia de dados cria uma carga no host que pode potencialmente
impactar o desempenho.

1. Digite o seguinte em um prompt de shell como raiz para instalar o pacote necessário:   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. Obtenha o ID do disco:<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. Localize seu volume na listagem.
4. Criptografe o dispositivo de bloco; 
      1. Este comando inicializa o volume e permite configurar um passphrase: <br/>
      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}
      
      2. Responda com SIM (todas as letras maiúsculas).
      
      3. O dispositivo aparecerá agora como um volume criptografado: 
      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```
      {: pre}
      
5. Abra o volume e crie um mapeamento:   <br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Insira a senha fornecida anteriormente.
7. Verifique o mapeamento e visualize o status do volume criptografado:   <br/>
   ```
   # cryptsetup -v status cryptData
   /dev/mapper/cryptData is active.
     type:  LUKS1
     cipher:  aes-cbc-essiv:sha256
     keysize: 256 bits
     device:  /dev/mapper/3600a0980383034685624466470446564
     offset:  4096 sectors
     size:    41938944 sectors
     mode:    read/write
     Command successful
   ```
8. Gravar dados aleatórios em dispositivos criptografados /dev/mapper/cryptData. Isso assegura que o
mundo exterior veja isso como dados aleatórios, o que significa que eles estão protegidos contra divulgação de
padrões de uso. Lembre-se de que esta etapa pode levar algum tempo.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. Formate o volume:<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Monte o volume:<br/>
   ```
   # mkdir /cryptData
   ```
   {: pre}
   ```
   # mount /dev/mapper/cryptData /cryptData
   ```
   {: pre}
   ```
   # df -H /cryptData
   ```
   {: pre}

### Como desmontar e fechar o volume criptografado com segurança
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Como remontar e montar uma partição criptografada com LUKS existente
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Enter the password previously provided.
   # mount /dev/mapper/cryptData /cryptData
   # df -H /cryptData
   # lsblk
   NAME                                       MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
   xvdb                                       202:16   0    2G  0 disk
   └─xvdb1                                    202:17   0    2G  0 part  [SWAP]
   xvda                                       202:0    0   25G  0 disk
   ├─xvda1                                    202:1    0  256M  0 part  /boot
   └─xvda2                                    202:2    0 24.8G  0 part  /
   sda                                          8:0    0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0 0 20G 0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   sdb                                          8:16   0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0 0 20G 0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   ```
