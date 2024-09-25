#  Análise de Dados do Oscar com MongoDB

Este projeto foi desenvolvido como parte do curso de Desenvolvimento Mobile, focando na utilização de bancos de dados NoSQL, especificamente o MongoDB. O objetivo foi aplicar operações CRUD (Criar, Ler, Atualizar e Deletar) para analisar dados relacionados ao Oscar, a famosa premiação de cinema.

## O que é MongoDB?

MongoDB é um banco de dados NoSQL que permite armazenar dados de forma flexível, usando documentos em formato JSON. Ao contrário dos bancos de dados relacionais, que utilizam tabelas, o MongoDB organiza informações de maneira que se adapte facilmente a diferentes tipos de dados.

## Estrutura do Projeto

Neste projeto, manipulamos dados do Oscar, incluindo informações sobre filmes, diretores e prêmios. As principais operações realizadas foram:

- **Criar**: Inserimos novos registros de filmes e seus respectivos dados.
- **Ler**: Consultamos informações específicas, como filmes vencedores em determinadas categorias.
- **Atualizar**: Fizemos alterações nos dados existentes, como atualizar a informação de um filme.
- **Deletar**: Removemos registros que não eram mais necessários.

## Exercícios: O Oscar vai para...

**1- Quantas vezes Natalie Portman foi indicada ao Oscar?**
```json
{
db["registros"].countDocuments({nome_do_indicado:"Natalie Portman"}) }

Resposta: 3

**2- Quantos Oscars Natalie Portman ganhou?**

```json
db["registros"].countDocuments({nome_do_indicado:"Natalie Portman",vencedor:"1"})
Resposta: 1

**3- Amy Adams já ganhou algum Oscar?**
```json
{
//por curiosidade busquei uma forma do shell me responder diretamente com sim ou não

var existe = db.registros.find({ nome_do_indicado: "Amy Adams", vencedor: "1" }).count() > 0;

if (existe) {
    print("sim");
} else {
    print("não");
}
}
resposta: não
 
 ```json
 {
 //jeito comum e prático de fazer a busca pelo banco

 
db["registros"].countDocuments({nome_do_indicado:"Amy Adams", vencedor:"1"})
 }
resposta: 0


**4- A série de filmes Toy Story ganhou um Oscar em quais anos?**
```json
{
db["registros"].find({nome_do_filme:/Toy Story/,vencedor:"1"}, {ano_cerimonia: 1, _id: 0})
}
  
  Resposta: ano_cerimonia: 2011
  ano_cerimonia: 2011
  ano_cerimonia: 2020

**5- A partir de que ano que a categoria "Actress" deixa de existir?**
```json
{

db["registros"].find( { categoria: "ACTRESS", vencedor: "1" }, { ano_cerimonia: 1, _id: 0 }
).sort({ano_cerimonia: -1 }).limit(1)
}

Resposta:  ano_cerimonia: 1976

**6- O primeiro Oscar para melhor Atriz foi para quem? Em que ano?**
```json
{
db["registros"].find({categoria:"ACTRESS",vencedor:'1'},{ano_cerimonia:1,nome_do_indicado:1,_id:0}).sort({ano_cerimonia:1}).limit(1)
}
Resposta:  ano_cerimonia: 1928,
  nome_do_indicado: 'Janet Gaynor'

**7- Na campo "Vencedor", altere todos os valores com "Sim" para 1 e todos os valores "Não" para 0.**
```json
{
//comandos usados para fazer as alterações:

db.registros.updateMany({vencedor:'false'},{$set:{vencedor:'0'}})  // vencedor:"false", alterado para 0

db.registros.updateMany({vencedor:'true'},{$set:{vencedor:'1'}})   // vencedor:"true", alterado para 1

}

**8- Em qual edição do Oscar "Crash" concorreu ao Oscar?**

```json
{

db["registros"].find({nome_do_filme:"Crash"},{ano_cerimonia:1,_id:0}).sort({ano_cerimonia:1}).limit(1)
}
Resposta: ano_cerimonia: 2006

**9- Bom... dê um Oscar para um filme que merece muito, mas não ganhou.**

Resposta: Um dos filmes indicados que mereci o Oscar, mas não o ganhou é: Rogue One: A Star Wars Story

```json
{
db["registros"].updateOne({ nome_do_filme: /Rogue One: A Star Wars Story/, vencedor: "0" },{ $set: { vencedor: '1' } })
}

**10- O filme Central Station aparece no Oscar?**
```json
{
const filme = db.registros.findOne({ nome_do_filme: "Central Station" });

if (filme) {
  print("Sim, ano da cerimônia: " + filme.ano_cerimonia);
} else {
  print("Não");
}

}
Resposta: Sim, ano da cerimônia: 1999

**11- Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser.**
```json
{
Resposta: Os filmes que eu acredito que deveriam ser indicados são: Orgulho e Preconceito, Estrelas Além do Tempo e De Repente 30.

db.registros.insertMany([
    { nome_do_filme: "Orgulho e Preconceito", categoria: "Dramas", vencedor: '1' },
    { nome_do_filme: "Estrelas Além do Tempo", categoria: "Dramas", vencedor: '1' },
    { nome_do_filme: "De Repente 30", categoria: "Comédias", vencedor: '1' }
])
}
**12 - Pensando no ano em que você nasceu: Qual foi o Oscar de melhor filme, Melhor Atriz e Melhor Diretor?**
```json
{
db.registros.find({ ano_cerimonia: 2005, vencedor: '1' }, 
  { nome_do_filme: 1, nome_do_indicado: 1, categoria: 1, _id: 0 })

//Nessa busca eu consigo ser mais específica em relação às categorias:

  db.registros.find(
  { 
    ano_cerimonia: 2005, 
    vencedor: '1', 
    $or: [
      { categoria: /ACTRESS/ },
      { categoria: "DIRECTING" }
    ]
  }, 
  { 
    nome_do_filme: 1, 
    nome_do_indicado: 1, 
    categoria: 1, 
    _id: 0 
  }
)
}
Melhor Filme: The Aviator
  
  ano_cerimonia: 2005,
  categoria: 'ACTRESS IN A SUPPORTING ROLE',
  nome_do_indicado: 'Cate Blanchett',
  nome_do_filme: 'The Aviator'

Melhor Atriz: Cate Blanchett

Melhor Diretor: Em 2005 somente o Diretor "Clint Eastwood" ganhou o Oscar na categoria Direção
 
 ano_cerimonia: 2005,
  categoria: 'DIRECTING',
  nome_do_indicado: 'Clint Eastwood',
  nome_do_filme: 'Million Dollar Baby'