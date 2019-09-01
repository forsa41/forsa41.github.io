---
title: XSS without parentheses and semi-colons
categories: WebSec
image: /assets/WebSec/xsswithoutparant/0.jpg
---

<img src="{{ page.image | prepend:site.baseurl | replace:'//','/' }}" >

Bundan 4-5 sene once bir teknik kesfedildi.JavaScript, [fonksiyonlarini](http://www.thespanner.co.uk/2012/05/01/xss-technique-without-parentheses/) parantezsiz cagirabiliyordu ve bu islemi **throw** ve **onerror** sayesinde gerceklestiriyordu.

**Onerror** isleyicisini, cagirmak istedigimiz fonksiyon icin kullanirken **throw** durumunu ise fonksiyona arguman gecirmek icin kullanmaktayiz.

`<script>onerror=alert;throw 1337 </script>`

**Onerror** isleyicisi ne zaman cagrilsa, herhangi bir JavaScript exceptioni(istisna) yaratir, ve **throw** durumu ise bizim kendi istedigimiz ifadelerle exception(istisna) yaratmamiza izin verir;Cunku **throw** bir durumdur,genellikle **onerror** atamasini takip etmek icinde noktali virgulden sonra yeni bir durum baslatilarak **throw** ifadesi yerlestirilir.

Bazi sitelerde karsilasilan kadariyla parantezler ve noktali virguller filtrelenmis olabilir, ve dusununce ustte gordugumuz teknigi uyarlayarak noktali virgulsuz fonksiyon calistirabiliriz.Ilk yol gayet basit:**onerror** ifadesini suslu parantezlerle bir blok icerisine alarak kullanabilirsiniz ve blok ifadesinden sonra **throw** ifadesini noktali virgulsuz(veya yeni satir ile) kullanabilirsiniz.

`<script>{onerror=alert}throw 1337</script>`

**Onerror'u** blok icinde kullanmak gayet kullanisli ama daha cool bir alternatifte var.Ilginc olacak fakat **throw** durumu ifade kabul edebilmekte, **onerror** atamasini **throw** durumu icinde yaparsak son ifadeyi **onerror** isleyicisine yollayarak fonksiyonu cagirabiliriz. Calismasi su sekilde:

![](/assets/WebSec/xsswithoutparant/1.png)

`<script>throw onerror=alert,'some string',123,'haha'</script>`

Eger bu scripti Chrome'da calistirirsaniz goreceksiniz ki stringimizden once "Uncaught" ifadesini goreceksiniz.

![](/assets/WebSec/xsswithoutparant/2.png)

Chrome icin buna da bir cozum var."Uncaught" ifadesini degisken olarak kullanarak JavaScript calistirabiliriz.

`<script>{onerror=eval}throw'=alert\x281337\x29'</script>`

Eval ifadesine "Uncaught=alert(1337)" gonderdik.Bu olay chromeda gayet guzel calismakta.

[Kaynak](https://portswigger.net/blog/xss-without-parentheses-and-semi-colons)

Kaynaga bakarak yazinin devamini okuyabilirsiniz.Ben yaziyi yarida kestim;Cunku yazinin bu kismindan sonraki kismi bu olaylarin nasil calistigina dair ispatlarla ilgili.
