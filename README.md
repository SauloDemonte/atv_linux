<h1 style="color:blue; font-size: 2em;">Manual do Projeto</h1>

## Acessando o Console AWS

Acessar o console AWS access portal (awsapps.com) e fazer o login com a conta Compass;
Ao acessar o console AWS entre em sua conta;

![Imagem 0001](imagens/imagem0001.png)

A próxima tela será a página Home do console que pode variar dependendo de sua personalização;

![Imagem 0002](imagens/imagem0002.png)

## Criando uma instância EC2

Para criar uma instância EC2 (Elastic Compute Cloud) na AWS você pode seguir o passo a passo abaixo:

No console vá para o serviço EC2. Para isso clique em All Services no canto superior esquerdo e em seguida escolha EC2 na seção Compute. Ou utilize o campo de pesquisa na parte superior da tela digitando EC2.

![Imagem 0003](imagens/imagem0003.png)

No painel do EC2 clique no botão **Launch Instance**.

![Imagem 0004](imagens/imagem0004.png)

Na próxima página você precisará fornecer um nome para a instância e escolher algumas configurações de TAGs.

**Por que usar TAGs?**

- **Organização**: TAGs ajudam a organizar recursos permitindo categorizá-los por projeto departamento ambiente (produção teste etc.) ou qualquer outro critério relevante.
- **Gerenciamento de custos**: Ao atribuir TAGs a recursos você pode rastrear e associar custos a diferentes departamentos projetos ou equipes facilitando a alocação de custos.
- **Busca e filtragem**: TAGs facilitam a busca e filtragem de recursos dentro do console AWS especialmente quando você tem muitos recursos.
- **Automação**: Tags podem ser usadas em scripts e automações para identificar e operar em um conjunto específico de recursos.

Escolha um nome para sua atividade, no caso usarei "Atividade Linux", e em seguida clique em "Add additional tags".

![Imagem 0005](imagens/imagem0005.png)

Na tela aberta, crie as TAGs de acordo com sua necessidade na opção **Add new tag** (1). Para os projetos da Compass usamos o padrão da imagem abaixo.

![Imagem 0006](imagens/imagem0006.png)

Em **Application and OS Images** (Amazon Machine Image - AMI), você pode escolher uma imagem baseada em Linux (Ubuntu, Amazon Linux, Red Hat, etc.) ou Windows. Neste caso, usaremos a imagem solicitada no projeto: **"Criar 1 instância EC2 com o sistema operacional Amazon Linux 2"**.

![Imagem 0007](imagens/imagem0007.png)

Em **Instance type**, nosso projeto solicita uma configuração **"Família t3.small"**.

![Imagem 0008](imagens/imagem0008.png)

Para gerar uma chave pública para acesso ao ambiente AWS, você precisa criar um par de chaves (chave pública e chave privada) que será usado para autenticação. Ao criar uma instância EC2, você será solicitado a escolher ou criar um par de chaves na seção **Key Pair (login)**. Vamos criar uma nova.

**Restante do conteúdo...**

---

**Nota**: Este é apenas uma parte do documento completo com as primeiras imagens. O conteúdo completo do documento será similarmente estruturado, incluindo todas as imagens até `imagem0081.png`.

## Configurando um Elastic IP (EIP)

Um Elastic IP (EIP) é um endereço IPv4 público fornecido pela Amazon Web Services (AWS) que pode ser alocado para uso com instâncias do EC2. Ele é um recurso de IP fixo que permanece constante, diferentemente dos endereços IP públicos padrão que podem mudar se a instância for interrompida ou reiniciada.

![Imagem 0009](imagens/imagem0009.png)

1. Entre no console AWS (Passo 1 do tutorial anterior);
2. No painel de navegação à esquerda, clique em **All services** no grupo **Compute**, acesse **EC2**.

![Imagem 0010](imagens/imagem0010.png)

No painel esquerdo, vá em **Elastic IPs**.

![Imagem 0011](imagens/imagem0011.png)

Clique em **Allocate Elastic IP address**.

![Imagem 0012](imagens/imagem0012.png)

Na tela seguinte, selecione a opção **Amazon's pool of IPv4 addresses**, adicione a tag do projeto para manter a organização e clique em **Allocate**.

![Imagem 0013](imagens/imagem0013.png)

Anote o Elastic IP address que foi gerado e clique em **Associate this Elastic IP address**.

![Imagem 0014](imagens/imagem0014.png)

Na janela de associação, escolha a instância EC2 que você deseja associar e clique em **Associate**.

![Imagem 0015](imagens/imagem0015.png)

Agora o Elastic IP está vinculado à sua instância EC2.

## Liberando as portas de comunicação

Um Security Group na Amazon Web Services (AWS) é uma configuração de firewall virtual que controla o tráfego de entrada e saída de recursos da AWS, como instâncias EC2. Ele define as regras que determinam quais tipos de tráfego podem alcançar ou sair de uma instância EC2, funcionando como uma camada adicional de segurança.

Acessar o Console AWS:
No console do EC2, clique em **Security Groups** no painel de navegação à esquerda e em **Create Security Group**.

![Imagem 0016](imagens/imagem0016.png)

Preencha as seguintes informações:
- **Name**: Insira um nome descritivo para o Security Group (ex.: AtvLinux).
- **Description**: Adicione uma descrição que ajude a identificar a finalidade desse grupo (ex.: Grupo de segurança para servidor web com HTTP e SSH liberados).
- **VPC**: Escolha a VPC em que você deseja criar o Security Group (a maioria dos usuários terá apenas uma VPC padrão).

![Imagem 0017](imagens/imagem0017.png)

Adicionar as regras de entrada da nossa Instancia

![Imagem 0018](imagens/imagem0018.png)

## Resumo das Regras Adicionadas

| Tipo | Protocolo | Faixa de Portas | Origem        |
|------|-----------|-----------------|---------------|
| SSH  | TCP       | 22              | 0.0.0.0/0     |
| Custom TCP | TCP | 111             | 0.0.0.0/0     |
| Custom UDP | UDP | 111             | 0.0.0.0/0     |
| Custom TCP | TCP | 2049            | 0.0.0.0/0     |
| Custom UDP | UDP | 2049            | 0.0.0.0/0     |
| HTTP | TCP       | 80              | 0.0.0.0/0     |
| HTTPS | TCP      | 443             | 0.0.0.0/0     |

Uma boa prática é adicionar as TAGs para manter a organização.

![Imagem 0019](imagens/imagem0019.png)
