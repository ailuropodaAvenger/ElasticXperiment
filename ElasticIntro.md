Indexing single video :
PUT /videos/video/1?pretty
```json

{
  "id": 1,
  "title": "Mr & Mrs",
  "description": "Mr & Mrs (মিস্টার এন্ড মিসেস) is a bengali drama released in ntv in this Eid Ul Azha 2016. This drama is telecast by International Television Channel Ltd (NTV). Actor and actress on this drama are Tahsan & Mithila, Alif, Khalid, Flora and so on.",
  "duration": "00:44:56",
  "link": "https://www.youtube.com/watch?v=KwWbgLD7YCY",
  "categories":{"id": 1,"name": "Teleflim"},
  "role": {
    "Production": [{"id": 1,"name": "NTV","logo": ""}],
    "Actor": [
                {"id": 1,"name": "Tahsan Khan","image": "","gender": "male"},
                {"id": 2,"name": "Mithila","image": "","gender": "female"}
              ]
            }
  }

{
  "id": 2,
  "title": "Rupkotha Ekhon Ar Hoy Na",
  "description": "Rupkotha Ekhon Ar Hoy Na (রূপকথা এখন আর হয় না) is a bengali drama released in ntv on this eid festive 2016. This drama is telecast by International Television Channel Ltd (NTV). Actor and actress on this drama are Momo & Tahsan. ",
  "duration": "00:43:38",
  "link": "https://www.youtube.com/watch?v=WPqwre_TX-w",
  "categories":{"id": 1,"name": "Teleflim"},
  "role": {
    "Production": [{"id": 1,"name": "NTV","logo": ""}],
    "Actor": [
              {"id": 1,"name": "Tahsan Khan","image": "","gender": "male"},
              {"id": 3,"name": "Zakia Bari Mamo","image": "","gender": "female"}
              ]
            }
}

{
  "id": 3,
  "title": "Tomay Vebe Lekha",
  "description": "Tomay Vebe Lekha Bangla Natok Download Website for Valentines Day Natok (2016). Tahsan & Tisha fans for New Drama Tomay Vebe Lekha Bangla Natok 2016 .",
  "duration": "00:42:10",
  "link": "https://www.youtube.com/watch?v=dfJOaA076k0",
  "categories":{"id": 1,"name": "Teleflim"},
  "role": {
    "Production": [{"id": 1,"name": "NTV","logo": ""}],
    "Actor": [
              {"id": 1,"name": "Tahsan Khan","image": "","gender": "male"},
              {"id": 4,"name": "Tisha","image": "","gender": "female"}
              ]
            }
 }

Indexing with bulk API:
must put new line/enter after 1st line
if got json parse error... sense auto indent
POST /videos/video/_bulk?pretty
{"index":{"_id":"4"}}
{"id":4,"title":"Prem o Protishodh","description":"Mosharraf Karim & Tisha Natok Prem o Protishodh | প্রেম ও প্রতিশোধ. [HD]Exclusively brought to you by Mosharraf Karim HD.","duration":"00:43:25","link":"https://www.youtube.com/watch?v=ZwTSwpwB0e8","categories":{"id":1,"name":"Teleflim"},"role":{"Production":[{"id":1,"name":"NTV","logo":""}],"Actor":[{"id":5,"name":"Mosharraf Karim","image":"","gender":"male"},{"id":4,"name":"Tisha","image":"","gender":"female"}]}}
{"index":{"_id":"5"}}
{"id":5,"title":"Tomar Amar","description":".","duration":"00:4:11","link":"https://www.youtube.com/watch?v=VdTSv6u6iP0","categories":{"id":2,"name":"Music"},"role":{"Production":[{"id":1,"name":"NTV","logo":""}],"Actor":[{"id":1,"name":"Tahsan","image":"","gender":"male"},{"id":2,"name":"Mithila","image":"","gender":"female"}]}}
{"index":{"_id":"6"}}
{"id":6,"title":"Sporsher Baire Tumi","description":"","duration":"00:03:39","link":"https://www.youtube.com/watch?v=Vw1qLsdIVtw","categories":{"id":2,"name":"Music"},"role":{"Production":[{"id":1,"name":"NTV","logo":""}],"Actor":[{"id":1,"name":"Tahsan","image":"","gender":"male"},{"id":6,"name":"Elita","image":"","gender":"female"}]}}

```
