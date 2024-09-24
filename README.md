# Analise de Dados

4- A série de filmes Toy Story ganhou um Oscar em quais anos?

db["registros"].find({nome_do_filme:/Toy Story/,vencedor:"1"}, {ano_cerimonia: 1, _id: 0})
  
  Resposta: ano_cerimonia: 2011
  ano_cerimonia: 2011
  ano_cerimonia: 2020

5- A partir de que ano que a categoria "Actress" deixa de existir? 

db["registros"].find( { categoria: "ACTRESS", vencedor: "1" }, { ano_cerimonia: 1, _id: 0 }
).sort({ano_cerimonia: -1 }).limit(1)

Resposta:  ano_cerimonia: 1976

6- O primeiro Oscar para melhor Atriz foi para quem? Em que ano?

db["registros"].find({categoria:"ACTRESS",vencedor:'1'},{ano_cerimonia:1,nome_do_indicado:1,_id:0}).sort({ano_cerimonia:1}).limit(1)

Resposta:  ano_cerimonia: 1928,
  nome_do_indicado: 'Janet Gaynor'

7- Na campo "Vencedor", altere todos os valores com "Sim" para 1 e todos os valores "Não" para 0.

//comandos usados para fazer as alterações:

db.registros.updateMany({vencedor:'false'},{$set:{vencedor:'0'}})  // vencedor:"false", alterado para 0

db.registros.updateMany({vencedor:'true'},{$set:{vencedor:'1'}})   // vencedor:"true", alterado para 1

8- Em qual edição do Oscar "Crash" concorreu ao Oscar?

db["registros"].find({nome_do_filme:"Crash"},{ano_cerimonia:1,_id:0}).sort({ano_cerimonia:1}).limit(1)

Resposta: ano_cerimonia: 2006

9- Bom... dê um Oscar para um filme que merece muito, mas não ganhou.




10- O filme Central do Brasil aparece no Oscar?

11- Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser. 

db.registros.insertMany([
    { nome_do_filme: "Orgulho e Preconceito", categoria: "Dramas", vencedor: '1' },
    { nome_do_filme: "Estrelas Além do Tempo", categoria: "Dramas", vencedor: '1' },
    { nome_do_filme: "De Repente 30", categoria: "Comédias", vencedor: '1' }
])

12 - Pensando no ano em que você nasceu: Qual foi o Oscar de melhor filme, Melhor Atriz e Melhor Diretor?