Bu yazıda öncelikle paralel hesaplamadan bahsedip ardından sıralı hesaplamadan farkı üzerinde duracağım ve ikisiyle de yapılan hesaplamaların arasındaki zaman farkını bir örnekle göstereceğim.

Paralel hesaplama, aynı görevin (parçalara bölünmüş ve uyarlanmış), sonuçları daha hızlı elde etmek için işlemci üzerinde  eş zamanlı olarak işletilmesidir. Bu yöntem, problemlerin çözümünün ufak  görev parçalarına bölünmesi ve bunların eş zamanlı olarak koordine  edilmesine dayanır. Paralel hesaplama ile performans artar ve büyük hesaplamalar daha kısa sürede sonuçlanır. Fakat küçük işlemlerde paralel hesaplama, sıralı hesaplamadan daha yavaş sonuçlar verebilmektedir.

Paralel hesaplama ile ilgili birkaç kalıp bulunmaktadır. Bunlardan bazıları reduce, join, map, all-to-all, broadcast gibi kalıplar olarak sıralanabilir. Vereceğim örnekte bir ortalama alma fonksiyonunun hem sıralı hem paralel olarak <a href="https://chapel-lang.org/" target="_blank">Chapel programlama dili</a> ile gerçekleştirimini göstereceğim. Paralel olarak hesaplama yaptırırken reduce kalıbından faydalanacağım.

Sıralı olarak ortalama fonksiyonu yazarken aşağıdaki gibi bir yapı kullanabiliriz:

<div class="separator" style="clear: both; text-align: center;">
</div>
<div class="separator" style="clear: both; text-align: center;">
<a href="https://1.bp.blogspot.com/-MWgWnZWnSog/Xfpp2XT5fXI/AAAAAAAAIgU/KNe7ytbyVhICa8pJNJsvTesyIQW1cG4IQCLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2019-12-18%2B21-01-44.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="157" data-original-width="279" src="https://1.bp.blogspot.com/-MWgWnZWnSog/Xfpp2XT5fXI/AAAAAAAAIgU/KNe7ytbyVhICa8pJNJsvTesyIQW1cG4IQCLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2019-12-18%2B21-01-44.png" /></a></div>

Yukarıdaki kod parçasının görevi X adlı bir dizi içerisindeki bütün elemanlarını sum adlı bir değişkenle toplayıp X dizisinin eleman sayısına bölümünü döndürmektir.

Paralel olarak yazarken ise şöyle bir yapı kullanabiliriz:

<div class="separator" style="clear: both; text-align: center;">
<a href="https://1.bp.blogspot.com/-0_BLYxK5L1A/XfpqdPnLpEI/AAAAAAAAIgc/e9nMcApB38sTUQuA743CcoRSi-5zFIRSgCLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2019-12-18%2B21-04-51.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="138" data-original-width="244" src="https://1.bp.blogspot.com/-0_BLYxK5L1A/XfpqdPnLpEI/AAAAAAAAIgc/e9nMcApB38sTUQuA743CcoRSi-5zFIRSgCLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2019-12-18%2B21-04-51.png" /></a></div>
Yukarıdaki kod parçasının görevi ise reduce işlemi ile X dizisinin içindeki elemanların toplanıp sum değişkenine atıp daha sonra sum değişkenini X dizisinin eleman sayısına bölümünü döndürmektir.

Bahsettiğim reduce kalıbını Chapel programlama dilinde parantez içinde yapacağımız işlem(+, -, *, /, min, max...) boşluk reduce anahtar kelimesi tekrar boşluk ardından hesaplama yapacağımız sayı kümesi(dizi vs) şeklinde kullanmak mümkündür. Şimdi bu kodların çıktılarını inceleyelim.

3 milyon veride sıralı şekilde yapılan hesaplama işleminin çıktısı:

<div class="separator" style="clear: both; text-align: center;">
<a href="https://1.bp.blogspot.com/-fbD1erkLyNg/XfpudR0zi0I/AAAAAAAAIgo/LXW7zDDgnRswoXOFs1K-na3qqIbiHVC5QCLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2019-12-18%2B21-20-23.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="78" data-original-width="388" height="64" src="https://1.bp.blogspot.com/-fbD1erkLyNg/XfpudR0zi0I/AAAAAAAAIgo/LXW7zDDgnRswoXOFs1K-na3qqIbiHVC5QCLcBGAsYHQ/s320/Screenshot%2Bfrom%2B2019-12-18%2B21-20-23.png" width="320" /></a></div>

Aynı miktarda veri ile paralel kalıp kullanarak yaptığımız hesaplama işleminin çıktısı:

<div class="separator" style="clear: both; text-align: center;">
<a href="https://1.bp.blogspot.com/-mwfGLbEb9d8/Xfpue5zmXFI/AAAAAAAAIgs/mtLlAGG-j9QURP_V2F3Ra8QAaAJ2Q3zwgCLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2019-12-18%2B21-17-52.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="76" data-original-width="402" height="60" src="https://1.bp.blogspot.com/-mwfGLbEb9d8/Xfpue5zmXFI/AAAAAAAAIgs/mtLlAGG-j9QURP_V2F3Ra8QAaAJ2Q3zwgCLcBGAsYHQ/s320/Screenshot%2Bfrom%2B2019-12-18%2B21-17-52.png" width="320" /></a></div>


Yukarıdaki çıktılardan da görebileceğimiz üzere paralel olarak yapılan işlem yaklaşık 2.5 kat daha hızlı yapılmakta. Buradan da anlayabileceğimiz gibi, büyük programları işlemci üzerinde paralel olarak çalıştırtmak bize gayet fazla bir hız kazandırır.

<div class="separator" style="clear: both; text-align: center;">
</div>
<div class="separator" style="clear: both; text-align: center;">
</div>
<div class="separator" style="clear: both; text-align: center;">
</div>
<div class="separator" style="clear: both; text-align: center;">
</div>
<div class="separator" style="clear: both; text-align: center;">
</div>
<div class="separator" style="clear: both; text-align: center;">
</div>

