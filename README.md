# 🚗 Carros Django

* Projeto de estudo prático desenvolvido com Django para simular uma aplicação web de uma loja de carros.

## 📌 Sobre o projeto

Este projeto foi desenvolvido com o objetivo de estudar e consolidar conceitos fundamentais do framework **Django**, utilizando como exemplo uma aplicação web de uma loja de veículos.

A ideia é construir, passo a passo, uma aplicação capaz de gerenciar um catálogo de carros, permitindo cadastrar, visualizar, pesquisar e futuramente editar e excluir veículos.

O projeto ainda está **em desenvolvimento**. Algumas funcionalidades já foram implementadas, enquanto outras fazem parte das próximas etapas de desenvolvimento.

O foco principal deste projeto é o aprendizado de conceitos como:

- Estrutura de projetos Django;
- Criação de aplicações;
- Models;
- Migrations;
- ORM do Django;
- Views;
- Templates;
- Django Template Language;
- Forms;
- Relacionamentos entre modelos;
- Django Admin;
- QuerySets;
- Upload de imagens;
- Requisições GET e POST;
- URLs;
- Banco de dados SQLite;
- Organização de uma aplicação web utilizando Django.

---

# 🎯 Objetivo

O objetivo do projeto é desenvolver uma aplicação web que simule uma **loja de carros**, permitindo administrar um catálogo de veículos.

A aplicação deverá evoluir para possuir funcionalidades como:

```text
                    Loja de Carros
                          │
          ┌───────────────┼───────────────┐
          │               │               │
       Listagem         Busca          Cadastro
          │               │               │
          └───────────────┼───────────────┘
                          │
                    Banco de Dados
                          │
                    SQLite / Django ORM
```
A implementação está sendo realizada de forma incremental, utilizando cada funcionalidade como oportunidade para estudar um conceito diferente do Django.

# 🛠️ Tecnologias utilizadas
* Python
* Django
* SQLite
* HTML5
* CSS3
* Pillow — utilizada para trabalhar com imagens
* Django ORM
* Django Admin

- O projeto também possui algumas dependências registradas no arquivo de requirements, incluindo Django, Pillow, Requests e OpenPyXL.

# 📂 Estrutura atual do projeto
```
Carros_django/
│
├── app/
│   ├── __pycache__/
│   ├── templates/
│   │   └── base.html
│   │
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── cars/
│   ├── __pycache__/
│   ├── migrations/
│   │
│   ├── templates/
│   │   ├── cars.html
│   │   └── new_car.html
│   │
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
│
├── media/
│   └── cars/
│       └── imagens dos veículos
│
├── venv/
│
├── .gitignore
├── db.sqlite3
├── manage.py
└── requirements.tx
```
- A aplicação cars está registrada no INSTALLED_APPS do projeto e o banco configurado atualmente é o SQLite.

# 🧱 Como o projeto foi criado
## 1. Criação do ambiente virtual

- Primeiro foi criado um ambiente virtual para manter as dependências do projeto isoladas.

No Windows:
```
python -m venv venv
```
Depois, o ambiente virtual pode ser ativado com:
```
venv\Scripts\activate
```
Quando ativado, o terminal deverá apresentar algo semelhante a:
```
(venv) C:\caminho\do\projeto>
```
## 2. Instalação do Django

Com o ambiente virtual ativado:
```
pip install django
```
Como o projeto trabalha com imagens, também é necessário o Pillow:
```
pip install pillow
```
As demais dependências podem ser instaladas posteriormente através do arquivo de requirements.

## 3. Criação do projeto Django

A estrutura principal do projeto foi criada utilizando:
```
django-admin startproject app .
```
Isso cria a estrutura:
```
app/
├── __init__.py
├── asgi.py
├── settings.py
├── urls.py
└── wsgi.py
```
E o arquivo:
```
manage.py
```
O app representa o projeto Django principal.

## 4. Criação da aplicação cars

Depois foi criada a aplicação responsável pelas funcionalidades relacionadas aos carros:
```
python manage.py startapp cars
```
A aplicação passou a possuir:
```
cars/
├── migrations/
├── admin.py
├── apps.py
├── forms.py
├── models.py
├── tests.py
└── views.py
```
Depois disso, cars foi adicionada ao INSTALLED_APPS:
```
INSTALLED_APPS = [
    ...
    'cars',
]
```
## 🗄️ Banco de dados

O projeto utiliza atualmente o SQLite, banco de dados padrão e bastante adequado para um projeto de estudos.

A configuração está presente em:
```
app/settings.py
```
O banco utilizado é:
```
db.sqlite3
```
O Django está configurado para utilizar:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
# 🚗 Modelagem dos dados

Uma das principais etapas já concluídas foi a criação dos modelos Brand e Car.

* Brand

Representa a marca do veículo.
```
class Brand(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=200)

    def __str__(self):
        return self.name
```
* Car

Representa o veículo cadastrado na loja.
```
class Car(models.Model):
    id = models.AutoField(primary_key=True)
    model = models.CharField(max_length=200)
    brand = models.ForeignKey(
        Brand,
        on_delete=models.PROTECT,
        related_name='brand'
    )
    factory_year = models.IntegerField(
        blank=True,
        null=True
    )
    model_year = models.IntegerField(
        blank=True,
        null=True
    )
    plate = models.CharField(
        max_length=10,
        blank=True,
        null=True
    )
    value = models.FloatField(
        blank=True,
        null=True
    )
    photo = models.ImageField(
        upload_to='cars/',
        blank=True,
        null=True
    )
```
## 🔗 Relacionamento entre Car e Brand

O modelo Car possui uma relação:
```
brand = models.ForeignKey(Brand, ...)
```
Isso significa que um carro está relacionado a uma marca.

Por exemplo:
```
Brand
│
├── BMW
│
├── Audi
│
├── Toyota
│
└── Honda
```
E os carros podem utilizar essas marcas:
```
BMW
 ├── M3
 ├── X5
 └── 320i

Toyota
 ├── Corolla
 ├── Hilux
 └── Yaris
```
Foi utilizado:
```
on_delete=models.PROTECT
```
para impedir que uma marca seja removida enquanto existirem carros associados a ela.

## 🔄 Migrations

Depois da criação ou alteração dos models, o Django utiliza migrations para transformar as alterações dos modelos em alterações no banco de dados.

Comandos utilizados:
```
python manage.py makemigrations
```
Depois:
```
python manage.py migrate
```
As migrations ficam armazenadas em:
```
cars/migrations/
```
## 👨‍💼 Django Admin

O projeto também possui integração com o Django Admin.

Os modelos Car e Brand foram registrados no arquivo:
```
cars/admin.py
```
Também foram configuradas opções para facilitar a visualização e pesquisa.

* Para Brand:
```
list_display = ('name',)
search_fields = ('name',)
```
* Para Car:
```
list_display = (
    'model',
    'brand',
    'factory_year',
    'model_year',
    'value'
)
```
# 🔐 Criando um usuário administrador

Para acessar o Django Admin, é possível criar um superusuário:
```
python manage.py createsuperuser
```
O Django solicitará:
```
Username:
Email:
Password:
Password confirmation:
```
Depois de criado, o painel pode ser acessado em:
```
http://127.0.0.1:8000/admin/
```
# 🌐 URLs

As principais URLs configuradas atualmente são:
```
/admin/
```
Painel administrativo do Django.
```
/cars/
```
Página de listagem dos carros.
```
/new_car/
```
Rota destinada ao cadastro de um novo carro.

* Atualmente, a rota /new_car/ ainda está em desenvolvimento.

## 🔎 Listagem de carros

* A aplicação já possui uma view responsável pela listagem dos veículos:
```
def cars_view(request):
    cars = Car.objects.all().order_by('model')

    search = request.GET.get('search')

    if search:
        cars = cars.filter(model__icontains=search)

    return render(
        request,
        'cars.html',
        {'cars': cars}
    )
```
Essa view já implementa duas funcionalidades importantes.

## Listagem

Os carros são recuperados através do Django ORM:
```
Car.objects.all()
```
E ordenados pelo modelo:
```
.order_by('model')
```
## 🔍 Pesquisa

Também foi implementada uma busca utilizando parâmetros GET:
```
search = request.GET.get('search')
```
Quando uma pesquisa é realizada:
```
cars = cars.filter(
    model__icontains=search
)
```
Isso permite pesquisar carros pelo nome do modelo.

Por exemplo:
```
http://127.0.0.1:8000/cars/?search=BMW
```
A aplicação retorna os carros cujo modelo contém o termo pesquisado.

## 🖥️ Templates

O projeto utiliza o sistema de templates do Django.

Atualmente existem:
```
cars/templates/
├── cars.html
└── new_car.html
```
E um template base:
```
app/templates/
└── base.html
```
O template cars.html utiliza:
```
{% extends "base.html" %}
```
Isso permite utilizar um template base e reaproveitar sua estrutura.

## 🔁 Django Template Language

A página de carros já utiliza recursos importantes da linguagem de templates do Django.

Por exemplo:
```
{% if cars %}
```
E:
```
{% for car in cars %}
```
Isso permite verificar se existem carros e percorrer os objetos enviados pela view.

Os dados do carro são exibidos através de:
```
{{ car.model }}
{{ car.brand }}
{{ car.factory_year }}
{{ car.value }}
```
## 🖼️ Upload de imagens

O modelo Car possui um campo para imagem:
```
photo = models.ImageField(
    upload_to='cars/',
    blank=True,
    null=True
)
```
As imagens são armazenadas dentro de:
```
media/cars/
```
O projeto também possui as configurações:
```
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'
```
Além disso, as URLs de mídia já estão sendo adicionadas durante o desenvolvimento:
```
urlpatterns = [
    ...
] + static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT
)
```
## 📝 Forms

Foi iniciado um formulário próprio para carros em:
```
cars/forms.py
```
Atualmente existe:
```
class CarForm(forms.Form):
    model = forms.CharField(max_length=200)
    brand = forms.ModelChoiceField(Brand.objects.all())
```
O campo brand utiliza:
```
forms.ModelChoiceField
```
permitindo selecionar uma marca existente no banco de dados.

## 🚧 Cadastro de novos carros

Essa é uma das partes que ainda não foi finalizada.

Já existe:
```
/new_car/
```
e também existe o template:
```
cars/templates/new_car.html
```
O formulário já possui:
```
<form method="post" enctype="multipart/form-data">
    {% csrf_token %}
</form>

Isso mostra que a estrutura inicial para envio de dados e arquivos já foi preparada.
```
Porém, a view ainda está assim:
```
def new_car_view(request):
    return "Novo Carro!"
```
Portanto, o cadastro ainda não está conectado ao banco de dados.

# 📊 Status atual do projeto
## ✅ Já implementado
 * Criação do projeto Django
 * Criação da aplicação cars
 * Configuração do SQLite
 * Model Brand
 * Model Car
 * Relacionamento entre Car e Brand
 * Migrations
 * Django Admin
 * Cadastro de marcas pelo Admin
 * Cadastro de carros pelo Admin
 * Configuração de upload de imagens
 * Configuração de MEDIA_ROOT
 * Configuração de MEDIA_URL
 * Template base
 * Template de listagem
 * Listagem de carros
 * Ordenação por modelo
 * Pesquisa de carros
 * Estrutura inicial do formulário
 * Rota para cadastro de carro
 * Template inicial para cadastro
## 🚧 Em desenvolvimento
* 1. Finalizar cadastro de carros

- A principal próxima etapa é conectar:
```
HTML Form
     ↓
POST
     ↓
View
     ↓
CarForm
     ↓
Django ORM
     ↓
SQLite
```
A view deverá:

* Receber a requisição;
* Verificar se é POST;
* Receber os dados enviados;
* Validar o formulário;
* Criar o objeto Car;
* Salvar no banco;
* Redirecionar para a lista de carros.
## 2. Completar o CarForm

O formulário atualmente possui apenas:

* model
* brand

Ainda é necessário adicionar campos como:

* model
* brand
* factory_year
* model_year
* plate
* value
* photo

Dessa maneira, o formulário poderá representar todos os dados existentes no model Car.

## 3. Finalizar upload da imagem

O formulário já está preparado para:
```
enctype="multipart/form-data"
```
Porém, a lógica de recebimento e salvamento da imagem ainda precisa ser implementada na view.

## 4. Redirecionamento após cadastro

Depois que um carro for cadastrado, a aplicação deverá redirecionar o usuário para a página de carros.

Fluxo esperado:
```
/new_car/
     ↓
Preenchimento do formulário
     ↓
Enviar
     ↓
Validação
     ↓
Salvar Carro
     ↓
/cars/
```
## 🚀 Próximas funcionalidades planejadas

Depois de finalizar o cadastro, a aplicação pode evoluir para um CRUD completo.
```
CRUD
CREATE
Cadastrar carro
```
```
READ
Visualizar carros
```
```
UPDATE
Editar carro
```
```
DELETE
Excluir carro
Cadastro
/new_car/
Listagem
/cars/
Edição
```
* Exemplo de futura rota:
```
/cars/<id>/edit/
Exclusão
```
* Exemplo de futura rota:
```
/cars/<id>/delete/
```
# 🔎 Melhorias futuras

Após finalizar o CRUD, algumas funcionalidades podem ser adicionadas:

* Página individual de cada veículo
* Editar carros
* Excluir carros
* Confirmação antes de excluir
* Pesquisa por marca
* Filtro por faixa de preço
* Filtro por ano
* Ordenação por preço
* Paginação
* Validação dos campos
* Formatação de valores monetários
* Melhor tratamento de imagens
* Autenticação de usuários
* Controle de permissões
* Página inicial da loja
* Página de detalhes do veículo
* Responsividade
* Testes automatizados
* Deploy da aplicação
  
# ▶️ Como executar o projeto
## 1. Clonar o repositório
```
git clone https://github.com/Guigo710/Carros_django.git
```
Entre na pasta:
```
cd Carros_django
```
## 2. Criar o ambiente virtual

Caso o ambiente virtual ainda não exista:
```
python -m venv venv
```
## 3. Ativar o ambiente virtual

* No Windows:
```
venv\Scripts\activate
```
* No Linux/macOS:
```
source venv/bin/activate
```
## 4. Instalar as dependências

* O projeto possui um arquivo de dependências.

* Atualmente ele está nomeado como:
```
requirements.tx
```
* Para instalar:
```
pip install -r requirements.tx
```
Recomenda-se futuramente renomear o arquivo para requirements.txt, que é o nome convencional utilizado em projetos Python.

## 5. Aplicar as migrations
```
python manage.py migrate
```
## 6. Criar um usuário administrador
 ```  
python manage.py createsuperuser
```
## 8. Executar o servidor
```
python manage.py runserver
```
O Django deverá informar algo semelhante a:
```
Starting development server at
http://127.0.0.1:8000/
```
# 🌐 Acessando a aplicação

Lista de carros
```
http://127.0.0.1:8000/cars/
```
 Cadastro
```
http://127.0.0.1:8000/new_car/
```
 Django Admin
```
http://127.0.0.1:8000/admin/
```
# 🧪 Testando a busca

* Depois de cadastrar alguns carros pelo Django Admin, acesse:
```
http://127.0.0.1:8000/cars/
```
Utilize o campo de busca.

Por exemplo:
```
BMW
```
A aplicação utilizará:
```
model__icontains
```

para localizar modelos que contenham o texto informado.

## 🧠 Conceitos de Django estudados

* Este projeto foi desenvolvido principalmente para consolidar os seguintes conceitos:

# Django Project

* Estrutura principal responsável pelas configurações da aplicação.
```
app/
Django App
```
* Aplicação responsável por uma funcionalidade específica.
```
cars/
Models
```
* Representação das entidades do sistema.
```
Brand
Car
ORM
```
* Interação com o banco de dados através de Python:
```
Car.objects.all()
QuerySet
```
* Consulta e manipulação dos dados:
```
Car.objects.all().order_by('model')
Filters
```
* Pesquisa utilizando o ORM:
```
cars.filter(model__icontains=search)
Views
```
* Responsáveis pela lógica das requisições:
```
def cars_view(request):
Templates
```
* Responsáveis pela apresentação dos dados:
```
{% for car in cars %}
Forms
```
* Responsáveis pela criação e validação de formulários:
```
class CarForm(forms.Form):
URLs
```
* Responsáveis pelo direcionamento das requisições:
```
path('cars/', cars_view, name='cars_list')
Admin
```
* Interface administrativa fornecida pelo Django.
```
Migrations
```
* Controle das alterações realizadas na estrutura do banco.
```
Media
```
* Gerenciamento de arquivos enviados pelo usuário, como imagens.

## 🔄 Arquitetura atual

O funcionamento básico da aplicação pode ser representado da seguinte maneira:
```
                 NAVEGADOR
                     │
                     ▼
                  URLs
                     │
                     ▼
                  Views
                     │
              ┌──────┴──────┐
              │             │
              ▼             ▼
            Forms        Templates
              │             │
              ▼             │
            Models ◄─────────┘
              │
              ▼
           Django ORM
              │
              ▼
          SQLite Database
```

No caso da listagem:
```
Usuário
   │
   │ GET /cars/
   ▼
cars_view()
   │
   ▼
Car.objects.all()
   │
   ▼
SQLite
   │
   ▼
QuerySet
   │
   ▼
cars.html
   │
   ▼
Lista de veículos
```
## 📈 Evolução planejada

A evolução do projeto está sendo feita em etapas:
```

[1] Estrutura Django
        ↓
[2] Models
        ↓
[3] Banco de dados
        ↓
[4] Admin
        ↓
[5] Listagem
        ↓
[6] Pesquisa
        ↓
[7] Forms
        ↓
[8] Cadastro          ← etapa atual
        ↓
[9] Edição
        ↓
[10] Exclusão
        ↓
[11] Detalhes
        ↓
[12] Autenticação
        ↓
[13] Melhorias
        ↓
[14] Deploy
```
# ⚠️ Status do projeto

## 🚧 Projeto em desenvolvimento

* Este repositório representa o estado atual dos meus estudos em Django.

* A aplicação ainda não deve ser considerada um sistema completo de loja de veículos. Algumas funcionalidades já estão implementadas e funcionais, enquanto outras estão sendo desenvolvidas como parte do processo de aprendizado.

* O objetivo é continuar evoluindo o projeto conforme novos conceitos de Django forem estudados.

# 🔐 Observação sobre configuração

* O arquivo settings.py atualmente possui DEBUG = True e uma SECRET_KEY diretamente no código.

* Essa configuração é adequada para um ambiente local de estudos, mas não deve ser utilizada dessa forma em produção.

* Em uma futura versão, a ideia é utilizar variáveis de ambiente para informações sensíveis, por exemplo:
```
SECRET_KEY
DEBUG
DATABASE_URL
```
* Também será necessário revisar o ALLOWED_HOSTS antes de realizar um deploy.

# 📚 Objetivo de aprendizado

* Este projeto faz parte dos meus estudos de desenvolvimento Back-end com Python e Django.

* A proposta não é apenas construir uma aplicação de loja de carros, mas utilizar o projeto como uma forma prática de compreender como uma aplicação web é estruturada utilizando Django.

* Cada funcionalidade representa uma etapa de aprendizado, desde a criação dos models e banco de dados até a construção de um CRUD completo.
