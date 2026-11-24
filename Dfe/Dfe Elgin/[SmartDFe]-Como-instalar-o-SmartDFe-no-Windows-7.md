# Como instalar o SmartDFe no Windows 7

Para realizar a instalação do SmartDFe no Windows 7, é necessário atualizar previamente alguns componentes do sistema antes da instalação inicial.

## Pré-requisitos obrigatórios

Certifique-se de que os seguintes itens estejam instalados e atualizados:

• Windows 7 Service Pack 1 (SP1)
• Microsoft Visual C++ Redistributable 2015–2022

## Ajuste de data e hora

Após a instalação de todos os componentes acima, é necessário desmarcar a opção de horário de verão nas configurações de data e hora do Windows.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/68e5fbb5-da56-4798-a418-f735b5555805" />


Agora deve seguir a instalação padrão conforme vídeo baixo:

<a href="https://youtu.be/eMpv0Ky9rK4">

<img width="600" alt="videoframe_2004" src="https://github.com/user-attachments/assets/5f087eb8-91a5-4ca6-b8a5-ce84d0ac8295" />

</a>

**[▶️ Assistir no YouTube](https://www.youtube.com/watch?v=eMpv0Ky9rK4)**

É necessário copiar o arquivo SmartNFCeConfig.json, fornecido pelo time de Integração da Elgin, para que a Software House realize a ativação no Windows 7.

O arquivo deve ser copiado para o diretório:
C:\SmartNFCe\emissor

Após a cópia, é necessário iniciar os serviços:

• SmartAgent  
• SmartExpress  
• SmartMonitor



Em algumas versões do Windows 7 é necessário executar o aplicativo SmartAgent.exe na pasta C:\SmartNFCe\emissor esperar alguns segundos e permitir o aplicativo no Windows Defender Firewall.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/cdb581c0-a282-48ca-8c5e-3009430f712a" />


Após a permissão é necessário reiniciar o serviço SmartAgent.