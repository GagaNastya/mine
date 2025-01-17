## Описать применение терминов
➡ **Aggregation** - выполнение сложных операций анализа данных \
Включает в себя группировку, сортировку, фильтрацию и вычисление статистических показателей, вычисление агрегатных функций и другие операции, которые позволяют извлекать полезные сведения из больших объемов данных или подготавливать данные для дальнейшего анализа. \
Например вычисление среднего возраста: \
db.users.aggregate([
  { $group: { _id: null, averageAge: { $avg: "$age" } } }
]) 


➡ **Text search** - полнотекстовый поиск по строкам полей \
При этом также позволяет искать не только точные совпадения, но и частичные совпадения, синонимы, а также проводить другие операции, такие как игнорирование регистра и логические операции. \
Полезно использовать для поиска текстовых данных в полях документов. \
Например поиск документов содержащих слово "mongodb" в поле "content": db.articles.find({ $text: { $search: "mongodb" } })
 
➡ **MapReduce** - обработка больших объемов данных \
Позволяет распараллеливать и выполнять сложные операции над большим объемом данных, разделяя задачу на шаги маппинга (Map) и редукции (Reduce).


➡ **Geospatial queries** - поиск по гео \
Позволяет выполнять операции, такие как поиск ближайших объектов, определение географической области и другие операции для поиска и анализа данных на основе географических координат. 

Например: Поиск ближайших ресторанов к определенным координатам:
db.restaurants.find({
  location: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [longitude, latitude]
      },
      $maxDistance: 1000
    }
  }
})

 
➡ **Transactions** - согласованность в сложных операциях в нескольких документах
Позволяют выполнять набор операций в рамках единой транзакционной сессии, гарантируя, что либо все операции будут успешно выполнены, либо ни одна из них не будет применена, что очень полезно при необходимости согласованности между несколькими документами или коллекциями.