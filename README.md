* **Título do Trabalho:** Trabalho Avaliativo Sistemas Operacionais

* **Integrantes:** \- Yuri Ayres de Paula | Pedro Sena Modernel | Nicolly Torra | Thaís Lodi   
    
* **Resumo:** Este projeto tem como objetivo o acesso a serviços disponibilizados pelo docker de uma máquina virtual utilizando tecnologias Django e PostgreSQL. O sistema permite o controle por uma API backend desenvolvida com Django, que se comunica com um banco de dados PostgreSQL para armazenar informações. A interface administrativa do Django facilita a gestão dos dados contidos no banco PostgreSQL, ambos presentes dentro da máquina virtual para acesso de um host. 

**Como Executar o Projeto:** 

**1\. Pré-requisitos:** \- Docker (instalar a versão mais recente) \- Docker Compose (instalar a versão mais recente) \- Python 3.8 ou superior

**2\. Clonando o repositório**: Primeiro, clone este repositório para o seu ambiente local: ```` ```bash git clone https://github.com/YuriAyres/Docker-Sistemas-Operacionais.git ````   
`cd Docker-Sistemas-Operacionais`

### **Configurar a Rede da Máquina Virtual**

O VirtualBox tem algumas opções de configuração de rede, sendo as mais comuns as "NAT" e "Bridged". Para que o host possa acessar a VM e os serviços Docker, você deve usar a configuração "Bridged".

#### **Passos para configurar a rede "Bridged":**

* **Desligue a Máquina Virtual**: Se a máquina virtual já estiver ligada, desligue-a para fazer alterações na configuração de rede.  
* **Abra o VirtualBox** e selecione a máquina virtual que está executando o Ubuntu.  
* **Configurações de Rede**:  
  * Clique com o botão direito sobre a máquina virtual e selecione **Configurações**.  
  * Vá para a aba **Rede**.  
  * Em **Adaptador 1**, altere o tipo de rede para **Bridged Adapter** ou **Placa em modo Bridge**.

### 

**Estrutura de diretórios:**  
<img src="/img/img1.png">

### **Usar um Ambiente Virtual (**`virtualenv`**)**

Uma forma recomendada de instalar pacotes Python sem interferir com o sistema é criar um ambiente virtual. Isso cria um ambiente isolado onde você pode instalar pacotes sem afetar o sistema.

#### **Passo 1: Instalar** `virtualenv`

Se você ainda não tem o `virtualenv` instalado, faça isso:

`sudo apt install python3-venv`

#### **Passo 2: Criar um Ambiente Virtual**

Navegue até o diretório raiz do seu projeto, no nosso caso “/meu\_projeto” e crie um ambiente virtual:

`python3 -m venv env`

Isso criará um diretório chamado `env` que conterá o ambiente virtual.

#### **Passo 3: Ativar o Ambiente Virtual**

Agora, ative o ambiente virtual:

`source env/bin/activate`  
Você deve ver o nome do ambiente virtual no prompt, algo como `(env)`.

#### **Passo 4: Instalar as Dependências**

Com o ambiente virtual ativado, você pode instalar as dependências do seu projeto usando `pip`:

`pip install django`  
`pip install -r requirements.txt`

### **Firewall e Configurações de Rede**

Se por algum motivo você não conseguir acessar as portas da máquina virtual, verifique a configuração de firewall da máquina virtual e do host.

* **No Ubuntu (máquina virtual)**: Permita as portas no firewall com `ufw`:

  `sudo ufw allow 8000`

  `sudo ufw allow 5432`

**Entre no container do Django**  
`docker exec -it django_service /bin/bash`

No contêiner Django, execute os seguintes comandos:

`python manage.py makemigrations`  
`python manage.py migrate`

**Executando os Contêineres**

`docker-compose up --build`

**Inicializando o Banco de Dados do Django**

`docker-compose exec django python manage.py migrate`

**Acessando o Django Admin**  
`docker-compose exec django python manage.py createsuperuser`

**Obtenha o ip do host**

`No prompt de comando insira ipconfig e localize o Endereço ipv4`

**No arquivo settings.py localize o seguinte parâmetro**

<img src="/img/img2.png"> 
Altere para colocar os ips de onde quer acessar o serviço. Deixar ‘\*’ permite o acesso de qualquer ip, o que não é recomendado.

`ALLOWED_HOSTS = ['localhost', '127.0.0.1', '<IP_do_HOST>']`

**Obtenha o IP da Máquina Virtual**  
`ip a`

Para verificar a conectividade, no prompt do seu host insira   
`ping <IP_da_VM>`

**Teste o Acesso aos Serviços**   
No seu navegador insira:  
`http://<IP_da_VM>:8000`

**Criando um banco novo:**

**Crie um novo app Django**

Use o comando `startapp` do Django para criar um novo app. Substitua `nome_do_app` pelo nome do seu app:

`python manage.py startapp nome_do_app`

**No arquivo settings.py procure pelo seguinte parâmetro e adicione seu novo app:**  
<img src="/img/img3.png">

**Crie um Modelo para o Banco de Dados no arquivo models.py como no exemplo:**  
<img src="/img/img4.png">
**`Adicione o modelo ao admin em admin.py:`**  
<img src="/img/img5.png">

#### **Crie uma View para Exibir os Dados em views.py como no exemplo:**

<img src="/img/img6.png">

**Crie uma pasta com o nome templates e adicione `um` arquivo .html como no exemplo:**

Para criar um arquivo no ubuntu no terminal insira

`touch nomedoarquivo.extensão`

O arquivo .html deve ter o mesmo nome que foi inserido dentro do arquivo views.py

<img src="/img/img7.png">

#### **Crie uma URL para a View:**

No arquivo `urls.py` importe o arquivo views do seu app e adicione uma nova path como no exemplo:

`from segundo_app import views as segunda_views`

`path(‘carros’, segunda_views.carro_list, name=’carro_list’),`

<img src="/img/img8.png">

**Entre no container do Django**  
`docker exec -it django_service /bin/bash`

No contêiner Django, execute os seguintes comandos:

`python manage.py makemigrations`  
`python manage.py migrate`

* ## **Fontes:**

[Django Documentation](https://www.djangoproject.com/)

[PostgreSQL Documentation](https://www.postgresql.org/docs/)

[Docker Documentation](https://docs.docker.com/)
