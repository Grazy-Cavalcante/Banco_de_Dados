# Momento 

Contém a base de indicados da empresa Momento para treinar consultas complexas no MongoDB.

Vamos fazer algumas perguntas para brincar de análise exploratória de dados com MongoDB.

* Quantos funcionarios da empresa Momento trabalham no departamento de vendas?
```js
db.funcionarios.countDocuments({departamento: ObjectId('85992103f9b3e0b3b3c1fe71')})
```
## Resposta: 10 funcionários.

* Inclua suas próprias informações no departamento de Tecnologia da empresa.
```js
db.funcionarios.insertOne({nome:"Grazielly Cavalcante", telefone:"11.97859.2758",email:"grazioliveira2705@gmail.com",dataAdmissao:"2024-09-30",cargo:"Desenvolvedora BackEnd",salario:12000, departamento:ObjectId('85992103f9b3e0b3b3c1fe74') })
```
## Resposta: acknowledged: true,
  insertedId: ObjectId('66fb3b04bbf5ad2bbe71375f')

* Agora diga, quantos funcionários temos ao total na empresa?
```js
db.funcionarios.countDocuments()
```
## Resposta: 24 funcionários 
* E quanto ao Departamento de Tecnologia?
```js
db.funcionarios.countDocuments({departamento:ObjectId('85992103f9b3e0b3b3c1fe74')})
```
## Resposta: 6 funcionários 
* Qual a média salarial do departamento de tecnologia?
```js
db.funcionarios.aggregate([
  {
    $match: { departamento: ObjectId('85992103f9b3e0b3b3c1fe74') }
  },
  {
    $group: {
      _id: null,
      mediasalarios: { $avg: "$salario" }
    }
  },
  {
    $project: {
      _id: 0, 
      mediasalarios: 1 
    }
  },
  {
    $limit: 1 
  }
])
```
## Resposta: mediasalarios: 5800
* Quanto o departamento de Vendas gasta em salários?
```js
db.funcionarios.aggregate([
  {
    $match: { departamento: ObjectId('85992103f9b3e0b3b3c1fe71') }
  },
  {
    $group: {
      _id: null,
      totalSalario: { $sum: "$salario" }
    }
  },
  {
    $project: {
      _id: 0, 
      totalSalario: 1 
    }
  }
])
```
## Resposta:  totalSalario: 95100
* Um novo departamento foi criado. O departamento de Inovações. 
Ele será locado no Brasil. Por favor, adicione-o no banco de dados da empresa colocando quaisquer informações que você achar relevantes.
```js
// Primeiramente foi criado um escritório para alocar o departamento de inovações
db.escritorios.insertOne({
  nome: "Death Star",
  endereco: "Geonosis, Scarif e Yavin, 155",
  telefone: "1195566-7788", 
  pais: "BR",
  suprimentos: [
    {
      produto: "Executor",
      quantidade: 5,
      precoUnitario: 1400000 
    },
    {
      produto: "Home One",
      quantidade: 10,
      precoUnitario: 2000000 
    },
    {
      produto: "Millennium Falcon",
      quantidade: 2,
      precoUnitario: 1200000 
    },
    {
      produto: "TIE Bomber",
      quantidade: 200,
      precoUnitario: 100000 
    },
    {
      produto: "TIE Fighters",
      quantidade: 250,
      precoUnitario: 200000 
    }
  ]
})

// Após esse procedimento, foi criado o departamento de Inovações

db.departamentos.insertOne({nome:"Inovações",escritorio:ObjectId('66fb51d74a81596c03e8aa28')})

```
* O departamento de Inovações está sem funcionários. Inclua alguns colegas de turma nesse departamento.  
```js
db.funcionarios.insertMany([{nome:"Gabriel Augusto", 
telefone:"1191234-5678",
email:"gabrielaugusto@gmail.com", 
Cargo:"UX / UI Designer", 
salario: "10000", 
departamento:ObjectId('66fb51d74a81596c03e8aa28') 
},
{
nome:"Mariana Paiva", 
telefone:"11.93569.8897",
email:"mariapaiva@gmail.com",dataAdmissao:"2024-09-30",
cargo:"Desenvolvedora BackEnd",
salario:12000, 
departamento:ObjectId('66fb51d74a81596c03e8aa28')
},
{
nome: "Heloisa Mendes",
 telefone: "1190028.8922",
email: "helomendes@gmail.com",
dataAdmissao: '2022-09-30',
cargo: "Full-Stack Developer",
salario: 12000,
departamento:ObjectId('66fb51d74a81596c03e8aa28')
},
{
nome: "Anakin skywalker",
telefone: "1190028.8023",
email: "anakinsky@gmail.com",
dataAdmissao: '2022-09-30',
cargo: "scrum master",
salario: 18000,
departamento:ObjectId('66fb51d74a81596c03e8aa28')
},
{
nome: "Han Solo",
telefone: "1190028.8023",
email: "hans@gmail.com",
dataAdmissao: '2022-09-30',
cargo: "Desenvolvedor Frontend",
salario: 10000,
departamento:ObjectId('66fb51d74a81596c03e8aa28')
},
{
nome: "Leia Organa",
telefone: "1190028.8023",
email: "organaleia@gmail.com",
dataAdmissao: '2022-09-30',
cargo: "Product Owner",
salario: 20000,
departamento:ObjectId('66fb51d74a81596c03e8aa28')

}])
```
* Quantos funcionarios a empresa Momento tem agora?

* Quantos funcionários da empresa Momento possuem conjuges?

* Qual a média salarial dos funcionários da empresa Momento, excluindo-se o CEO?

* Qual a média salarial do departamento de tecnologia? 

* Qual o departamento com a maior média salarial?

* Qual o departamento com o menor número de funcionários?

* Pensando na relação quantidade e valor unitario, qual o produto mais valioso da empresa?

* Qual o produto mais vendido da empresa?

* Qual o produto menos vendido da empresa?

