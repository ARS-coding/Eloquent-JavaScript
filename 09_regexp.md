# Düzenli İfadeler

{{quote {author: "Jamie Zawinski", chapter: true}

Bazı insanlar, bir problemle karşılaştıklarında, 'Biliyorum, düzenli ifadeleri kullanacağım.' dediklerinden ötürü artık iki probleme sahip olurlar.

quote}}

{{index "Zawinski, Jamie"}}

{{if interactive

{{quote {author: "Master Yuan-Ma", title: "Programlama Kitabı", chapter: true}

Ahşabın damarına karşı keserseniz, çok güç gerekir. Problemin yapısına karşı programlama yaptığınızda, çok fazla kod gerekir.

quote}}

if}}

{{figure {url: "img/chapter_picture_9.jpg", alt: "Düzenli ifadelerin sözdizimsel yapısını temsil eden bir demiryolu sisteminin illüstrasyonu.", chapter: "square-framed"}}}

{{index evolution, adoption, integration}}

Programlama ((araç))ları ve teknikleri, kaotik, evrimsel bir şekilde hayatta kalır ve yayılır. Her zaman en iyi veya parlak olanlar kazanmaz, ancak doğru nişte yeterince iyi işleyenler veya tesadüfen başka başarılı bir teknoloji parçası ile entegre olanlar kazanır.

{{index "domain-specific language"}}

Bu bölümde, bu araçlardan bir tanesini tartışacağım: _((düzenli ifadeler))_. Düzenli ifadeler, dize verilerindeki ((kalıpları)) tanımlamanın bir yoludur. JavaScript'in ve birçok başka dilin ve sistemlerin bir parçası olan küçük, ayrı bir dil oluştururlar.

{{index [interface, design]}}

Düzenli ifadeler hem son derece sakar hem de son derece kullanışlıdır. Söz dizimi kriptiktir ve JavaScript'in onlar için sağladığı programlama arayüzü sakattır. Ancak, dizeleri incelemek ve işlemek için güçlü bir ((araç))tırlar. Düzenli ifadeleri doğru anlamak, sizi daha etkili bir programcı yapacaktır.

## Düzenli ifade oluşturma

{{index ["regular expression", creation], "RegExp class", "literal expression", "slash character"}}

Düzenli ifade bir tür nesnedir. `RegExp` ((constructor)) fonskiyonunu çağırarak oluşturulabilir veya bir kalıbı ileri eğik çizgi (`/`) karakterleri arasına alarak yazıp da oluşturulabilir.

```
let re1 = new RegExp("abc");
let re2 = /abc/;
```

Bu iki düzenli ifade nesnesi aynı ((kalıbı)) temsil eder: bir _a_ karakteri ardından bir _b_ ardından bir _c_.

{{index ["backslash character", "in regular expressions"], "RegExp class"}}

`RegExp` ((constructor)) fonskiyonunu kullanılırken, kalıp normal bir dize olarak yazılır, bu nedenle ters eğik çizgiler için normal kurallar geçerlidir.

{{index ["regular expression", escaping], [escaping, "in regexps"], "slash character"}}

Kalıbın eğik çizgi karakterleri arasında göründüğü ikinci notasyon durumunda, ters eğik çizgileri biraz farklı bir şekilde işler. İlk olarak, bir ileri eğik çizgi kalıbı sonlandıracağından ötürü kalıbın bir parçası olmasını istediğimiz herhangi bir ileri eğik çizgi önüne bir ters eğik çizgi koymamız gerekir. Ayrıca, özel karakter kodlarının (`\n` gibi) bir parçası olmayan ters eğik çizgiler, dize içinde olduğu gibi görmezden gelinmez ve kalıbın anlamını değiştirir. Soru işaretleri ve artı işaretleri gibi bazı karakterler, düzenli ifadelerde özel anlamlara sahiptir ve karakterin kendisini temsil etmek isteniyorsa öncesine bir ters eğik çizgi yazılmalıdır.

```
let aPlus = /A\+/;
```

## Eşleşenler için test etmek

{{index matching, "test method", ["regular expression", methods]}}

Düzenli ifade nesnelerinin bir dizi metodları vardır. En basiti `test` metodudur. Bir dize geçirirseniz, ifadenin kalıbına uyan dizeyi içerip içermediğini size söyleyen bir ((Boolean)) döndürür.

```
console.log(/abc/.test("abcde"));
// → true
console.log(/abc/.test("abxde"));
// → false
```

{{index pattern}}

Yalnızca özel olmayan karakterlerden oluşan bir düzenli ifade basit olarak, o karakter dizisini temsil eder. _abc_ dizesi, test ettiğimiz dizide (başlangıç dışında yerlerde de olacak şekilde) herhangi bir yerde bulunursa, `test` metodu `true` döndürür.

## Karakter setleri

{{index "regular expression", "indexOf method"}}

Bir dizinin _abc_ içerip içermediğini öğrenmek, `indexOf` ile de yapılabilir. Düzenli ifadeler, daha karmaşık ((kalıpları)) tanımlamamıza izin verdiği için kullanışlıdır.

Diyelim ki bir ((sayı)) eşleştirmek istiyoruz. Bir düzenli ifade içinde, karakterler arasına kare parantezler arasında bir ((küme)) karakterler yerleştirmek, ifadenin bu kısmının parantezler arasındaki karakterlerden herhangi birini eşleştirmesini sağlar.

Her iki ifade de bir ((rakam)) içeren tüm dizeleri eşleştirir:

```
console.log(/[0123456789]/.test("in 1992"));
// → true
console.log(/[0-9]/.test("in 1992"));
// → true
```

{{index "hyphen character"}}

Kare parantezler arasında, iki karakter arasındaki bir tire (-), karakterin ((Unicode)) numarası tarafından belirlenen bir karakterler ((aralığı))nı göstermek için kullanılabilir. 0'dan 9'a olan bu karakterler bu sıralamada hemen yan yanadırlar (kodlar 48 ila 57), bu nedenle `[0-9]` tümünü kapsar ve herhangi bir ((rakamı)) eşleştirir.

{{index [whitespace, matching], "alphanumeric character", "period character"}}

Birçok yaygın karakter grubunun kendi yerleşik kısayolları vardır. Rakamlar bunlardan biridir: `\d`, `[0-9]` ile aynı anlama gelir.

{{index "newline character", [whitespace, matching]}}

{{table {cols: [1, 5]}}}

| `\d`    | Herhangi bir ((rakam)) karakter
| `\w`    | Herhangi bir alfanümerik ("((kelime karakteri))")
| `\s`    | Herhangi bir boşluk karakteri (boşluk, tab, yeni satır, vb.)
| `\D`    | Rakam _olmayan_ bir karakter
| `\W`    | Alfanümerik olmayan bir karakter
| `\S`    | Boşluk olmayan bir karakter
| `.`     | Yeni satır dışında herhangi bir karakter

Bu nedenle, 01-30-2003 15:20 gibi bir ((tarih)) ve ((zaman)) biçimini şu şekilde eşleştirebilirsiniz:

```
let dateTime = /\d\d-\d\d-\d\d\d\d \d\d:\d\d/;
console.log(dateTime.test("01-30-2003 15:20"));
// → true
console.log(dateTime.test("30-jan-2003 15:20"));
// → false
```

{{index ["backslash character", "in regular expressions"]}}

Bu tamamen berbat görünüyor, değil mi? Yarısından fazlası ters eğik çizgiler ve gerçekten ((kalıp)) ifade edilen kısmı bulmayı zorlaştıran bir arka plan gürültüsü oluşturuyor. Bu ifadenin [daha sonra](regexp#date_regexp_counted) biraz geliştirilmiş bir sürümünü göreceğiz.

{{index [escaping, "in regexps"], "regular expression", set}}

Bu ters eğik çizgi kodları aynı zamanda ((kare parantezler)) içinde de kullanılabilir. Örneğin, `[\d.]`, herhangi bir rakam veya bir nokta karakterini ifade eder. Ancak nokta kendisi, kare parantezler arasında, özel anlamını kaybeder. `+` işareti gibi diğer özel karakterler için de aynı şey geçerlidir.

{{index "square brackets", inversion, "caret character"}}

Bir karakterler kümesini tersine çevirmek - yani, kümedeki karakterlerin dışındakileri eşleştirmek istediğinizi belirtmek - için bir üst simge (`^`) karakterini açılış parantezinin hemen ardından yazabilirsiniz.

```
let nonBinary = /[^01]/;
console.log(nonBinary.test("1100100010100110"));
// → false
console.log(nonBinary.test("0111010112101001"));
// → true
```

## Uluslararası karakterler

{{index internationalization, Unicode, ["regular expression", internationalization]}}

JavaScript'in başlangıçtaki basit implementasyonu ve bu basit yaklaşımın daha sonra ((standart)) davranış olarak belirlenmesi nedeniyle, JavaScript'in düzenli ifadeleri, İngilizce dilinde bulunmayan karakterler hakkında oldukça gariptir. Örneğin, JavaScript'in düzenli ifadelerine göre, bir (("kelime karakteri")), yalnızca Latin alfabesinin 26 karakterinden biridir (büyük veya küçük harf), ondalık basamaklar ve, nedense alt çizgi karakteri. _é_ veya _β_ gibi kesinlikle kelime karakterleri olan şeyler, `\w` eşleşirken büyük harfli `\W` olmayan kelime kategorisine eşleşmeyecektir.

{{index [whitespace, matching]}}

Tuhaf bir tarihsel kazadan dolayı, `\s` (boşluk) bu sorunu yaşamaz ve Unicode standardı tarafından boşluk olarak kabul edilen tüm karakterleri eşleştirir, bunlar arasında ((boşluk karakteri)) ve ((Moğol ünlü ayırıcı)) gibi şeyler de vardır.

{{index "character category", [Unicode, property]}}

Bir düzenli ifade içinde `\p` kullanarak Unicode standartında belirli bir özellik verilen tüm karakterlerle eşleştirebilmek mümkündür. Bu, harfleri daha kozmopolit bir şekilde eşleştirmemize olanak tanır. Ancak, yine de orijinal dil standartlarıyla uyumluluktan dolayı, bunlar yalnızca düzenli ifadenin sonuna ((Unicode)) için bir `u` karakteri eklediğinizde tanınır.

{{table {cols: [1, 5]}}}

| `\p{L}`             | Herhangi bir harf
| `\p{N}`             | Herhangi bir sayısal karakter
| `\p{P}`             | Herhangi bir noktalama işareti karakteri
| `\P{L}`             | Harf olmayan herhangi bir şey (büyük harf P tersine çevrilir)
| `\p{Script=Hangul}` | Verilen alfabe dosyasındaki herhangi bir karakter ([Bölüm ?](higher_order#scripts))

Düzenli ifadeleri `\w` aracılığıyla metin işleme amaçları için kullanmak, İngilizce olmayan metinlerde (hatta İngilizce olan ancak “cliché” gibi ödünç alınmış kelimeleri kullanan metinlerde) “é” gibi karakterleri harfler olarak işlemeyeceğinden bir dezavantajdır. İçerikleri biraz daha uzun olsa da, `\p` özellik grupları daha sağlamdır.

```{test: never}
console.log(/\p{L}/u.test("α"));
// → true
console.log(/\p{L}/u.test("!"));
// → false
console.log(/\p{Script=Greek}/u.test("α"));
// → true
console.log(/\p{Script=Arabic}/u.test("α"));
// → false
```

{{index "Number function"}}

Öte yandan, bir şeyler yapmak için sayıları eşleştirecekse, genellikle onlara yapılacak şeyler için `\d`'yi kullanmak istersiniz çünkü `Number` gibi bir fonksiyonun sizin için sayı atanmış rastgele herhangi bir karakterleri JavaScript sayısına dönüştürmesi mümkün değildir.

## Bir desenin tekrarlanan bölümleri

{{index ["regular expression", repetition]}}

Şimdi tek bir basamağı eşleştirmeyi nasıl yapacağımızı biliyoruz. Bir tam sayıyı - bir veya daha fazla ((rakam))ın bir ((dizi))sini eşleştirmek istersek ne yapmalıyız?

{{index "plus character", repetition, "+ operator"}}

Düzenli ifadede bir artı işareti (`+`) bir şeyin birden fazla kez tekrarlanabileceğini gösterir. Dolayısıyla, `/\d+/`, bir veya daha fazla rakam karakteri eşleştirir.

```
console.log(/'\d+'/.test("'123'"));
// → true
console.log(/'\d+'/.test("''"));
// → false
console.log(/'\d*'/.test("'123'"));
// → true
console.log(/'\d*'/.test("''"));
// → true
```

{{index "* operator", asterisk}}

Yıldız (`*`) benzer bir anlama sahiptir, ancak aynı zamanda kalıbın sıfır kez eşleşmesine izin verir. Bir şeyin sonunda yıldız olan bir şey asla bir kalıbın eşleşmesini engellemez - uygun metni bulamazsa sadece sıfır örnekler eşleştirir.

{{index "British English", "American English", "question mark"}}

Bir soru işareti, kalıbın bir parçasını _((isteğe bağlı))_ yapar, yani sıfır veya bir kez olabilir. Aşağıdaki örnekte, _u_ karakterinin olması izin verilir, ancak kalıp aynı zamanda eksik olduğunda da eşleşir.

```
let neighbor = /neighbou?r/;
console.log(neighbor.test("neighbour"));
// → true
console.log(neighbor.test("neighbor"));
// → true
```

{{index repetition, [braces, "in regular expression"]}}

Bir kalıbın belirli bir sayı kadar gerçekleşmesi gerektiğini belirtmek için süslü parantezler kullanın. Örneğin, bir öğeden sonra `{4}` yazmak, onun tam olarak dört kez gerçekleşmesini gerektirir. Ayrıca bu şekilde bir ((aralık)) belirlemek de mümkündür: `{2,4}` öğenin en az iki kez ve en fazla dört kez gerçekleşmesini gerektirir.

{{id date_regexp_counted}}

İşte hem tek hem de çift ((rakam)) günleri, ayları ve saatleri olan ((tarih)) ve ((zaman)) kalıbının başka bir versiyonu. Ayrıca anlaması da biraz daha kolaydır.

```
let dateTime = /\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2}/;
console.log(dateTime.test("1-30-2003 8:45"));
// → true
```

Süslü parantezleri kullanırken, bir sayıdan sonraki virgülü atlayarak açık uçlu ((aralık))lar belirleyebilirsiniz. Bu nedenle, `{5,}` beş veya daha fazla kez anlamına gelir.

## Alt ifadeleri gruplandırma

{{index ["regular expression", grouping], grouping, [parentheses, "in regular expressions"]}}

`*` veya `+` gibi bir operatörü aynı anda birden fazla öğe üzerinde kullanmak istiyorsanız, parantezleri kullanmanız gerekir. Parantezlerle çevrili bir düzenli ifade parçası, onu takip eden operatörler açısından tek bir öğe olarak sayılır.

```
let cartoonCrying = /boo+(hoo+)+/i;
console.log(cartoonCrying.test("Boohoooohoohooo"));
// → true
```

{{index crying}}

İlk ve ikinci `+` karakteri, sırasıyla _boo_ ve _hoo_'daki ikinci _o_ için uygulanır. Üçüncü `+`, `(hoo+)` grubuna tamamen uygulanır, bir veya daha fazla benzer dizilerle eşleşir.

{{index "case sensitivity", capitalization, ["regular expression", flags]}}

Örnekteki ifadenin sonundaki `i`, bu düzenli ifadeyi harf büyüklüğüne duyarsız hale getirir, böylece model kendisi tamamen küçük harfken giriş dizgisindeki büyük harf _B_ ile de eşleşir.

## Maçlar ve gruplar

{{index ["regular expression", grouping], "exec method", [array, "RegExp match"]}}

`test` metodu, bir düzenli ifadeyi eşlemek için mutlak en basit yoldur. Sadece eşleşip eşleşmediğini söyler ve başka hiçbir şey söylemez. Düzenli ifadelerin ayrıca `exec` (çalıştır) adlı bir metodu vardır; bu metot, eşleşme bulunamazsa `null` döndürür ve aksi takdirde eşleşme hakkında bilgi içeren bir nesne döndürür.

```
let match = /\d+/.exec("one two 100");
console.log(match);
// → ["100"]
console.log(match.index);
// → 8
```

{{index "index property", [string, indexing]}}

`exec` tarafından döndürülen bir nesne, başarılı eşleşmenin dizede _nerede_ başladığını belirten bir `index` özelliğine sahiptir. Bunun dışında, nesne eşleşen dizgeyi içeren bir dize dizisi gibi görünür ki zaten öyledir de. Önceki örnekte, bu aradığımız ((rakam)) dizisidir.

{{index [string, methods], "match method"}}

Dize değerleri benzer şekilde davranan bir `match` metodu içerir.

```
console.log("one two 100".match(/\d+/));
// → ["100"]
```

{{index grouping, "capture group", "exec method"}}

Düzenli ifade, parantezlerle gruplandırılmış alt ifadeler içeriyorsa, bu gruplarla eşleşen metinlerin de dizi içinde görünür. Tüm eşleşme her zaman ilk öğedir. Bir sonraki öğe, ilk grup tarafından eşleştirilen kısım (ifadenin içindeki açılış parantezi önce gelen grup), ardından ikinci grup ve sonrasıdır.

```
let quotedText = /'([^']*)'/;
console.log(quotedText.exec("she said 'hello'"));
// → ["'hello'", "hello"]
```

{{index "capture group"}}

Bir grup hiç eşleşmediğinde (örneğin, bir soru işareti tarafından izlendiğinde), çıktı dizisindeki konumu `undefined` değerini tutar. Ve bir grup birden fazla kez eşleşirse (örneğin, bir `+` tarafından izlendiğinde), yalnızca son eşleşme diziye girer.

```
console.log(/bad(ly)?/.exec("bad"));
// → ["bad", undefined]
console.log(/(\d)+/.exec("123"));
// → ["123", "3"]
```

Eşleşmelerin yalnızca gruplama amacıyla kullanılmasını istiyor ve döndürülen dizide gösterilmesini istemiyorsanız, açılış parantezinden sonra `?:` ekleyebilirsiniz.

```
console.log(/(?:na)+/.exec("banana"));
// → ["nana"]
```

{{index "exec method", ["regular expression", methods], extraction}}

Gruplar, bir dizgenin parçalarını çıkarmak için yararlı olabilir. Bir dizgenin yalnızca bir ((tarih)) içerip içermediğini kontrol etmekle kalmayıp aynı zamanda bunu çıkarmak ve bunu temsil eden bir nesne oluşturmak istiyorsak, parantezleri sayı desenlerinin etrafına sarabilir ve `exec` fonksiyonunun sonucundan tarihi seçebiliriz.

Ancak önce, JavaScript'te tarih ve ((zaman)) değerlerini temsil etmenin yerleşik yolunu tartışacağımız kısa bir yolculuğa çıkacağız.

## The Date class

{{index constructor, "Date class"}}

JavaScript has a standard class for representing ((date))s—or, rather, points in ((time)). It is called `Date`. If you simply create a date object using `new`, you get the current date and time.

```{test: no}
console.log(new Date());
// → Fri Feb 02 2024 18:03:06 GMT+0100 (CET)
```

{{index "Date class"}}

You can also create an object for a specific time.

```
console.log(new Date(2009, 11, 9));
// → Wed Dec 09 2009 00:00:00 GMT+0100 (CET)
console.log(new Date(2009, 11, 9, 12, 59, 59, 999));
// → Wed Dec 09 2009 12:59:59 GMT+0100 (CET)
```

{{index "zero-based counting", [interface, design]}}

JavaScript uses a convention where month numbers start at zero (so December is 11), yet day numbers start at one. This is confusing and silly. Be careful.

The last four arguments (hours, minutes, seconds, and milliseconds) are optional and taken to be zero when not given.

{{index "getTime method", timestamp}}

Timestamps are stored as the number of milliseconds since the start of 1970, in the UTC ((time zone)). This follows a convention set by "((Unix time))", which was invented around that time. You can use negative numbers for times before 1970. The `getTime` method on a date object returns this number. It is big, as you can imagine.

```
console.log(new Date(2013, 11, 19).getTime());
// → 1387407600000
console.log(new Date(1387407600000));
// → Thu Dec 19 2013 00:00:00 GMT+0100 (CET)
```

{{index "Date.now function", "Date class"}}

If you give the `Date` constructor a single argument, that argument is treated as such a millisecond count. You can get the current millisecond count by creating a new `Date` object and calling `getTime` on it or by calling the `Date.now` function.

{{index "getFullYear method", "getMonth method", "getDate method", "getHours method", "getMinutes method", "getSeconds method", "getYear method"}}

Date objects provide methods such as `getFullYear`, `getMonth`, `getDate`, `getHours`, `getMinutes`, and `getSeconds` to extract their components. Besides `getFullYear` there's also `getYear`, which gives you the year minus 1900 (`98` or `119`) and is mostly useless.

{{index "capture group", "getDate method", [parentheses, "in regular expressions"]}}

Putting parentheses around the parts of the expression that we are interested in, we can now create a date object from a string.

```
function getDate(string) {
  let [_, month, day, year] =
    /(\d{1,2})-(\d{1,2})-(\d{4})/.exec(string);
  return new Date(year, month - 1, day);
}
console.log(getDate("1-30-2003"));
// → Thu Jan 30 2003 00:00:00 GMT+0100 (CET)
```

{{index destructuring, "underscore character"}}

The `_` (underscore) binding is ignored and used only to skip the full match element in the array returned by `exec`.

## Boundaries and look-ahead

{{index matching, ["regular expression", boundary]}}

Unfortunately, `getDate` will also happily extract a date from the string `"100-1-30000"`. A match may happen anywhere in the string, so in this case, it'll just start at the second character and end at the second-to-last character.

{{index boundary, "caret character", "dollar sign"}}

If we want to enforce that the match must span the whole string, we can add the markers `^` and `$`. The caret matches the start of the input string, whereas the dollar sign matches the end. So, `/^\d+$/` matches a string consisting entirely of one or more digits, `/^!/` matches any string that starts with an exclamation mark, and `/x^/` does not match any string (there cannot be an _x_ before the start of the string).

{{index "word boundary", "word character"}}

There is also a `\b` marker, which matches “word boundaries”, positions that have a word character one side, and a non-word character on the other. Unfortunately, these use the same simplistic concept of word characters as `\w`, and are therefore not very reliable.

Note that these markers don't match any actual characters. They just enforces that a given condition holds at the place where they appears in the pattern.

{{index "look-ahead"}}

_Look-ahead_ tests do something similar. They provide a pattern, and will make the match fail if the input doesn't match that pattern, but don't actually move the match position forward. They are written between `(?=` and `)`.

```
console.log(/a(?=e)/.exec("braeburn"));
// → ["a"]
console.log(/a(?! )/.exec("a b"));
// → null
```

Note how the `e` in the first example is necessary to match, but is not part of the matched string. The `(?! )` notation expresses a _negative_ look-ahead. This only matches if the pattern in the parentheses _doesn't_ match, causing the second example to only match “a” characters that don't have a space after them.

## Choice patterns

{{index branching, ["regular expression", alternatives], "farm example"}}

Say we want to know whether a piece of text contains not only a number but a number followed by one of the words _pig_, _cow_, or _chicken_, or any of their plural forms.

We could write three regular expressions and test them in turn, but there is a nicer way. The ((pipe character)) (`|`) denotes a ((choice)) between the pattern to its left and the pattern to its right. So I can say this:

```
let animalCount = /\d+ (pig|cow|chicken)s?/;
console.log(animalCount.test("15 pigs"));
// → true
console.log(animalCount.test("15 pugs"));
// → false
```

{{index [parentheses, "in regular expressions"]}}

Parentheses can be used to limit the part of the pattern that the pipe operator applies to, and you can put multiple such operators next to each other to express a choice between more than two alternatives.

## The mechanics of matching

{{index ["regular expression", matching], [matching, algorithm], "search problem"}}

Conceptually, when you use `exec` or `test`, the regular expression engine looks for a match in your string by trying to match the expression first from the start of the string, then from the second character, and so on, until it finds a match or reaches the end of the string. It'll either return the first match that can be found or fail to find any match at all.

{{index ["regular expression", matching], [matching, algorithm]}}

To do the actual matching, the engine treats a regular expression something like a ((flow diagram)). This is the diagram for the livestock expression in the previous example:

{{figure {url: "img/re_pigchickens.svg", alt: "Railroad diagram that first passes through a box labeled 'digit', which has a loop going back from after it to before it, and then a box for a space character. After that, the railroad splits in three, going through boxes for 'pig', 'cow', and 'chicken'. After those it rejoins, and goes through a box labeled 's', which, being optional, also has a railroad that passes it by. Finally, the line reaches the accepting state."}}}

{{index traversal}}

Our expression matches if we can find a path from the left side of the diagram to the right side. We keep a current position in the string, and every time we move through a box, we verify that the part of the string after our current position matches that box.

{{id backtracking}}

## Backtracking

{{index ["regular expression", backtracking], "binary number", "decimal number", "hexadecimal number", "flow diagram", [matching, algorithm], backtracking}}

The regular expression `/^([01]+b|[\da-f]+h|\d+)$/` matches either a binary number followed by a _b_, a hexadecimal number (that is, base 16, with the letters _a_ to _f_ standing for the digits 10 to 15) followed by an _h_, or a regular decimal number with no suffix character. This is the corresponding diagram:

{{figure {url: "img/re_number.svg", alt: "Railroad diagram for the regular expression '^([01]+b|\\d+|[\\da-f]+h)$'"}}}

{{index branching}}

When matching this expression, it will often happen that the top (binary) branch is entered even though the input does not actually contain a binary number. When matching the string `"103"`, for example, it becomes clear only at the 3 that we are in the wrong branch. The string _does_ match the expression, just not the branch we are currently in.

{{index backtracking, "search problem"}}

So the matcher _backtracks_. When entering a branch, it remembers its current position (in this case, at the start of the string, just past the first boundary box in the diagram) so that it can go back and try another branch if the current one does not work out. For the string `"103"`, after encountering the 3 character, it will start trying the branch for hexadecimal numbers, which fails again because there is no _h_ after the number. So it tries the decimal number branch. This one fits, and a match is reported after all.

{{index [matching, algorithm]}}

The matcher stops as soon as it finds a full match. This means that if multiple branches could potentially match a string, only the first one (ordered by where the branches appear in the regular expression) is used.

Backtracking also happens for ((repetition)) operators like `+` and `*`. If you match `/^.*x/` against `"abcxe"`, the `.*` part will first try to consume the whole string. The engine will then realize that it needs an _x_ to match the pattern. Since there is no _x_ past the end of the string, the star operator tries to match one character less. But the matcher doesn't find an _x_ after `abcx` either, so it backtracks again, matching the star operator to just `abc`. _Now_ it finds an _x_ where it needs it and reports a successful match from positions 0 to 4.

{{index performance, complexity}}

It is possible to write regular expressions that will do a _lot_ of backtracking. This problem occurs when a pattern can match a piece of input in many different ways. For example, if we get confused while writing a binary-number regular expression, we might accidentally write something like `/([01]+)+b/`.

{{figure {url: "img/re_slow.svg", alt: "Railroad diagram for the regular expression '([01]+)+b'",width: "6cm"}}}

{{index "inner loop", [nesting, "in regexps"]}}

If that tries to match some long series of zeros and ones with no trailing _b_ character, the matcher first goes through the inner loop until it runs out of digits. Then it notices there is no _b_, so it backtracks one position, goes through the outer loop once, and gives up again, trying to backtrack out of the inner loop once more. It will continue to try every possible route through these two loops. This means the amount of work _doubles_ with each additional character. For even just a few dozen characters, the resulting match will take practically forever.

## The replace method

{{index "replace method", "regular expression"}}

String values have a `replace` method that can be used to replace part of the string with another string.

```
console.log("papa".replace("p", "m"));
// → mapa
```

{{index ["regular expression", flags], ["regular expression", global]}}

The first argument can also be a regular expression, in which case the first match of the regular expression is replaced. When a `g` option (for _global_) is added after the regular expression, _all_ matches in the string will be replaced, not just the first.

```
console.log("Borobudur".replace(/[ou]/, "a"));
// → Barobudur
console.log("Borobudur".replace(/[ou]/g, "a"));
// → Barabadar
```

{{index grouping, "capture group", "dollar sign", "replace method", ["regular expression", grouping]}}

The real power of using regular expressions with `replace` comes from the fact that we can refer to matched groups in the replacement string. For example, say we have a big string containing the names of people, one name per line, in the format `Lastname, Firstname`. If we want to swap these names and remove the comma to get a `Firstname Lastname` format, we can use the following code:

```
console.log(
  "Liskov, Barbara\nMcCarthy, John\nMilner, Robin"
    .replace(/(\p{L}+), (\p{L}+)/gu, "$2 $1"));
// → Barbara Liskov
//   John McCarthy
//   Robin Milner
```

The `$1` and `$2` in the replacement string refer to the parenthesized groups in the pattern. `$1` is replaced by the text that matched against the first group, `$2` by the second, and so on, up to `$9`. The whole match can be referred to with `$&`.

{{index [function, "higher-order"], grouping, "capture group"}}

It is possible to pass a function—rather than a string—as the second argument to `replace`. For each replacement, the function will be called with the matched groups (as well as the whole match) as arguments, and its return value will be inserted into the new string.

Here's an example:

```
let stock = "1 lemon, 2 cabbages, and 101 eggs";
function minusOne(match, amount, unit) {
  amount = Number(amount) - 1;
  if (amount == 1) { // only one left, remove the 's'
    unit = unit.slice(0, unit.length - 1);
  } else if (amount == 0) {
    amount = "no";
  }
  return amount + " " + unit;
}
console.log(stock.replace(/(\d+) (\p{L}+)/gu, minusOne));
// → no lemon, 1 cabbage, and 100 eggs
```

This takes a string, finds all occurrences of a number followed by an alphanumeric word, and returns a string that has one less of every such quantity.

The `(\d+)` group ends up as the `amount` argument to the function, and the `(\p{L}+)` group gets bound to `unit`. The function converts `amount` to a number—which always works since it matched `\d+`—and makes some adjustments in case there is only one or zero left.

## Greed

{{index greed, "regular expression"}}

It is possible to use `replace` to write a function that removes all ((comment))s from a piece of JavaScript ((code)). Here is a first attempt:

```{test: wrap}
function stripComments(code) {
  return code.replace(/\/\/.*|\/\*[^]*\*\//g, "");
}
console.log(stripComments("1 + /* 2 */3"));
// → 1 + 3
console.log(stripComments("x = 10;// ten!"));
// → x = 10;
console.log(stripComments("1 /* a */+/* b */ 1"));
// → 1  1
```

{{index "period character", "slash character", "newline character", "empty set", "block comment", "line comment"}}

The part before the _or_ operator matches two slash characters followed by any number of non-newline characters. The part for multiline comments is more involved. We use `[^]` (any character that is not in the empty set of characters) as a way to match any character. We cannot just use a period here because block comments can continue on a new line, and the period character does not match newline characters.

But the output for the last line appears to have gone wrong. Why?

{{index backtracking, greed, "regular expression"}}

The `[^]*` part of the expression, as I described in the section on backtracking, will first match as much as it can. If that causes the next part of the pattern to fail, the matcher moves back one character and tries again from there. In the example, the matcher first tries to match the whole rest of the string and then moves back from there. It will find an occurrence of `*/` after going back four characters and match that. This is not what we wanted—the intention was to match a single comment, not to go all the way to the end of the code and find the end of the last block comment.

Because of this behavior, we say the repetition operators (`+`, `*`, `?`, and `{}`) are _((greed))y_, meaning they match as much as they can and backtrack from there. If you put a ((question mark)) after them (`+?`, `*?`, `??`, `{}?`), they become nongreedy and start by matching as little as possible, matching more only when the remaining pattern does not fit the smaller match.

And that is exactly what we want in this case. By having the star match the smallest stretch of characters that brings us to a `*/`, we consume one block comment and nothing more.

```{test: wrap}
function stripComments(code) {
  return code.replace(/\/\/.*|\/\*[^]*?\*\//g, "");
}
console.log(stripComments("1 /* a */+/* b */ 1"));
// → 1 + 1
```

A lot of ((bug))s in ((regular expression)) programs can be traced to unintentionally using a greedy operator where a nongreedy one would work better. When using a ((repetition)) operator, prefer the nongreedy variant.

## Dynamically creating RegExp objects

{{index ["regular expression", creation], "underscore character", "RegExp class"}}

There are cases where you might not know the exact ((pattern)) you need to match against when you are writing your code. Say you want to test for the user's name in a piece of text. You can build up a string and use the `RegExp` ((constructor)) on that. Here's an example:

```
let name = "harry";
let regexp = new RegExp("(^|\\s)" + name + "($|\\s)", "gi");
console.log(regexp.test("Harry is a dodgy character."));
// → true
```

{{index ["regular expression", flags], ["backslash character", "in regular expressions"]}}

When creating the `\s` part of the string, we have to use two backslashes because we are writing them in a normal string, not a slash-enclosed regular expression. The second argument to the `RegExp` constructor contains the options for the regular expression—in this case, `"gi"` for global and case insensitive.

But what if the name is `"dea+hl[]rd"` because our user is a ((nerd))y teenager? That would result in a nonsensical regular expression that won't actually match the user's name.

{{index ["backslash character", "in regular expressions"], [escaping, "in regexps"], ["regular expression", escaping]}}

To work around this, we can add backslashes before any character that has a special meaning.

```
let name = "dea+hl[]rd";
let escaped = name.replace(/[\\[.+*?(){|^$]/g, "\\$&");
let regexp = new RegExp("(^|\\s)" + escaped + "($|\\s)",
                        "gi");
let text = "This dea+hl[]rd guy is super annoying.";
console.log(regexp.test(text));
// → true
```

## The search method

{{index ["regular expression", methods], "indexOf method", "search method"}}

The `indexOf` method on strings cannot be called with a regular expression. But there is another method, `search`, that does expect a regular expression. Like `indexOf`, it returns the first index on which the expression was found, or -1 when it wasn't found.

```
console.log("  word".search(/\S/));
// → 2
console.log("    ".search(/\S/));
// → -1
```

Unfortunately, there is no way to indicate that the match should start at a given offset (like we can with the second argument to `indexOf`), which would often be useful.

## The lastIndex property

{{index "exec method", "regular expression"}}

The `exec` method similarly does not provide a convenient way to start searching from a given position in the string. But it does provide an *in*convenient way.

{{index ["regular expression", matching], matching, "source property", "lastIndex property"}}

Regular expression objects have properties. One such property is `source`, which contains the string that expression was created from. Another property is `lastIndex`, which controls, in some limited circumstances, where the next match will start.

{{index [interface, design], "exec method", ["regular expression", global]}}

Those circumstances are that the regular expression must have the global (`g`) or sticky (`y`) option enabled, and the match must happen through the `exec` method. Again, a less confusing solution would have been to just allow an extra argument to be passed to `exec`, but confusion is an essential feature of JavaScript's regular expression interface.

```
let pattern = /y/g;
pattern.lastIndex = 3;
let match = pattern.exec("xyzzy");
console.log(match.index);
// → 4
console.log(pattern.lastIndex);
// → 5
```

{{index "side effect", "lastIndex property"}}

If the match was successful, the call to `exec` automatically updates the `lastIndex` property to point after the match. If no match was found, `lastIndex` is set back to zero, which is also the value it has in a newly constructed regular expression object.

The difference between the global and the sticky options is that, when sticky is enabled, the match will succeed only if it starts directly at `lastIndex`, whereas with global, it will search ahead for a position where a match can start.

```
let global = /abc/g;
console.log(global.exec("xyz abc"));
// → ["abc"]
let sticky = /abc/y;
console.log(sticky.exec("xyz abc"));
// → null
```

{{index bug}}

When using a shared regular expression value for multiple `exec` calls, these automatic updates to the `lastIndex` property can cause problems. Your regular expression might be accidentally starting at an index that was left over from a previous call.

```
let digit = /\d/g;
console.log(digit.exec("here it is: 1"));
// → ["1"]
console.log(digit.exec("and now: 1"));
// → null
```

{{index ["regular expression", global], "match method"}}

Another interesting effect of the global option is that it changes the way the `match` method on strings works. When called with a global expression, instead of returning an array similar to that returned by `exec`, `match` will find _all_ matches of the pattern in the string and return an array containing the matched strings.

```
console.log("Banana".match(/an/g));
// → ["an", "an"]
```

So be cautious with global regular expressions. The cases where they are necessary—calls to `replace` and places where you want to explicitly use `lastIndex`—are typically the only places where you want to use them.

### Getting all matches

{{index "lastIndex property", "exec method", loop}}

A common thing to do is to find all the matches of a regular expression in a string. We can do this by using the `matchAll` method.

```
let input = "A string with 3 numbers in it... 42 and 88.";
let matches = input.matchAll(/\d+/g);
for (let match of matches) {
  console.log("Found", match[0], "at", match.index);
}
// → Found 3 at 14
//   Found 42 at 33
//   Found 88 at 40
```

{{index ["regular expression", global]}}

This method returns an array of match arrays. The regular expression given it _must_ have `g` enabled.

{{id ini}}
## Parsing an INI file

{{index comment, "file format", "enemies example", "INI file"}}

To conclude the chapter, we'll look at a problem that calls for ((regular expression))s. Imagine we are writing a program to automatically collect information about our enemies from the ((Internet)). (We will not actually write that program here, just the part that reads the ((configuration)) file. Sorry.) The configuration file looks like this:

```{lang: "null"}
searchengine=https://duckduckgo.com/?q=$1
spitefulness=9.7

; comments are preceded by a semicolon...
; each section concerns an individual enemy
[larry]
fullname=Larry Doe
type=kindergarten bully
website=http://www.geocities.com/CapeCanaveral/11451

[davaeorn]
fullname=Davaeorn
type=evil wizard
outputdir=/home/marijn/enemies/davaeorn
```

{{index grammar}}

The exact rules for this format (which is a widely used format, usually called an _INI_ file) are as follows:

- Blank lines and lines starting with semicolons are ignored.

- Lines wrapped in `[` and `]` start a new ((section)).

- Lines containing an alphanumeric identifier followed by an `=`   character add a setting to the current section.

- Anything else is invalid.

Our task is to convert a string like this into an object whose properties hold strings for settings written before the first section header and subobjects for sections, with those subobjects holding the section's settings.

{{index "carriage return", "line break", "newline character"}}

Since the format has to be processed ((line)) by line, splitting up the file into separate lines is a good start. We saw the `split` method in [Chapter ?](data#split). Some operating systems, however, use not just a newline character to separate lines but a carriage return character followed by a newline (`"\r\n"`). Given that the `split` method also allows a regular expression as its argument, we can use a regular expression like `/\r?\n/` to split in a way that allows both `"\n"` and `"\r\n"` between lines.

```{startCode: true}
function parseINI(string) {
  // Start with an object to hold the top-level fields
  let result = {};
  let section = result;
  for (let line of string.split(/\r?\n/)) {
    let match;
    if (match = line.match(/^(\w+)=(.*)$/)) {
      section[match[1]] = match[2];
    } else if (match = line.match(/^\[(.*)\]$/)) {
      section = result[match[1]] = {};
    } else if (!/^\s*(;|$)/.test(line)) {
      throw new Error("Line '" + line + "' is not valid.");
    }
  };
  return result;
}

console.log(parseINI(`
name=Vasilis
[address]
city=Tessaloniki`));
// → {name: "Vasilis", address: {city: "Tessaloniki"}}
```

{{index "parseINI function", parsing}}

The code goes over the file's lines and builds up an object. Properties at the top are stored directly into that object, whereas properties found in sections are stored in a separate section object. The `section` binding points at the object for the current section.

There are two kinds of significant lines—section headers or property lines. When a line is a regular property, it is stored in the current section. When it is a section header, a new section object is created, and `section` is set to point at it.

{{index "caret character", "dollar sign", boundary}}

Note the recurring use of `^` and `$` to make sure the expression matches the whole line, not just part of it. Leaving these out results in code that mostly works but behaves strangely for some input, which can be a difficult bug to track down.

{{index "if keyword", assignment, ["= operator", "as expression"]}}

The pattern `if (match = string.match(...))` makes use of the fact that the value of an ((assignment)) expression (`=`) is the assigned value. You often aren't sure that your call to `match` will succeed, so you can access the resulting object only inside an `if` statement that tests for this. To not break the pleasant chain of `else if` forms, we assign the result of the match to a binding and immediately use that assignment as the test for the `if` statement.

{{index [parentheses, "in regular expressions"]}}

If a line is not a section header or a property, the function checks whether it is a comment or an empty line using the expression `/^\s*(;|$)/` to match lines that either contain only space, or space followed by a semicolon (making the rest of the line a comment). When a line doesn't match any of the expected forms, the function throws an exception.

## Code units and characters

Another design mistake that's been standardized, in JavaScript regular expressions, is that by default, operator like `.` or `?` work on code units, as discussed in [Chapter ?](higher_order#code_units), not actual characters. This means characters that are composed of two code units behave strangely.

```
console.log(/🍎{3}/.test("🍎🍎🍎"));
// → false
console.log(/<.>/.test("<🌹>"));
// → false
console.log(/<.>/u.test("<🌹>"));
// → true
```

The problem is that the 🍎 in the first line is treated as two code units, and the `{3}` part is applied only to the second one. Similarly, the dot matches a single code unit, not the two that make up the rose ((emoji)).

You must add the `u` (Unicode) option to your regular expression to make it treat such characters properly.

```
console.log(/🍎{3}/u.test("🍎🍎🍎"));
// → true
```

{{id summary_regexp}}

## Summary

Regular expressions are objects that represent patterns in strings. They use their own language to express these patterns.

{{table {cols: [1, 5]}}}

| `/abc/`     | A sequence of characters
| `/[abc]/`   | Any character from a set of characters
| `/[^abc]/`  | Any character _not_ in a set of characters
| `/[0-9]/`   | Any character in a range of characters
| `/x+/`      | One or more occurrences of the pattern `x`
| `/x+?/`     | One or more occurrences, nongreedy
| `/x*/`      | Zero or more occurrences
| `/x?/`      | Zero or one occurrence
| `/x{2,4}/`  | Two to four occurrences
| `/(abc)/`   | A group
| `/a|b|c/`   | Any one of several patterns
| `/\d/`      | Any digit character
| `/\w/`      | An alphanumeric character ("word character")
| `/\s/`      | Any whitespace character
| `/./`       | Any character except newlines
| `/\p{L}/u`  | Any letter character
| `/^/`       | Start of input
| `/$/`       | End of input
| `/(?=a)/`   | A look-ahead test

A regular expression has a method `test` to test whether a given string matches it. It also has a method `exec` that, when a match is found, returns an array containing all matched groups. Such an array has an `index` property that indicates where the match started.

Strings have a `match` method to match them against a regular expression and a `search` method to search for one, returning only the starting position of the match. Their `replace` method can replace matches of a pattern with a replacement string or function.

Regular expressions can have options, which are written after the closing slash. The `i` option makes the match case insensitive. The `g` option makes the expression _global_, which, among other things, causes the `replace` method to replace all instances instead of just the first. The `y` option makes it sticky, which means that it will not search ahead and skip part of the string when looking for a match. The `u` option turns on Unicode mode, which enables `\p` syntax and fixes a number of problems around the handling of characters that take up two code units.

Regular expressions are a sharp ((tool)) with an awkward handle. They simplify some tasks tremendously but can quickly become unmanageable when applied to complex problems. Part of knowing how to use them is resisting the urge to try to shoehorn things that they cannot cleanly express into them.

## Exercises

{{index debugging, bug}}

It is almost unavoidable that, in the course of working on these exercises, you will get confused and frustrated by some regular expression's inexplicable ((behavior)). Sometimes it helps to enter your expression into an online tool like [_debuggex.com_](https://www.debuggex.com/) to see whether its visualization corresponds to what you intended and to ((experiment)) with the way it responds to various input strings.

### Regexp golf

{{index "program size", "code golf", "regexp golf (exercise)"}}

_Code golf_ is a term used for the game of trying to express a particular program in as few characters as possible. Similarly, _regexp golf_ is the practice of writing as tiny a regular expression as possible to match a given pattern, and _only_ that pattern.

{{index boundary, matching}}

For each of the following items, write a ((regular expression)) to test whether the given pattern occurs in a string. The regular expression should match only strings containing the pattern. When your expression works, see whether you can make it any smaller.

 1. _car_ and _cat_
 2. _pop_ and _prop_
 3. _ferret_, _ferry_, and _ferrari_
 4. Any word ending in _ious_
 5. A whitespace character followed by a period, comma, colon, or semicolon
 6. A word longer than six letters
 7. A word without the letter _e_ (or _E_)

Refer to the table in the [chapter summary](regexp#summary_regexp) for help. Test each solution with a few test strings.

{{if interactive
```
// Fill in the regular expressions

verify(/.../,
       ["my car", "bad cats"],
       ["camper", "high art"]);

verify(/.../,
       ["pop culture", "mad props"],
       ["plop", "prrrop"]);

verify(/.../,
       ["ferret", "ferry", "ferrari"],
       ["ferrum", "transfer A"]);

verify(/.../,
       ["how delicious", "spacious room"],
       ["ruinous", "consciousness"]);

verify(/.../,
       ["bad punctuation ."],
       ["escape the period"]);

verify(/.../,
       ["Siebentausenddreihundertzweiundzwanzig"],
       ["no", "three small words"]);

verify(/.../,
       ["red platypus", "wobbling nest"],
       ["earth bed", "bedrøvet abe", "BEET"]);


function verify(regexp, yes, no) {
  // Ignore unfinished exercises
  if (regexp.source == "...") return;
  for (let str of yes) if (!regexp.test(str)) {
    console.log(`Failure to match '${str}'`);
  }
  for (let str of no) if (regexp.test(str)) {
    console.log(`Unexpected match for '${str}'`);
  }
}
```

if}}

### Quoting style

{{index "quoting style (exercise)", "single-quote character", "double-quote character"}}

Imagine you have written a story and used single ((quotation mark))s throughout to mark pieces of dialogue. Now you want to replace all the dialogue quotes with double quotes, while keeping the single quotes used in contractions like _aren't_.

{{index "replace method"}}

Think of a pattern that distinguishes these two kinds of quote usage and craft a call to the `replace` method that does the proper replacement.

{{if interactive
```{test: no}
let text = "'I'm the cook,' he said, 'it's my job.'";
// Change this call.
console.log(text.replace(/A/g, "B"));
// → "I'm the cook," he said, "it's my job."
```
if}}

{{hint

{{index "quoting style (exercise)", boundary}}

The most obvious solution is to replace only quotes with a nonletter character on at least one side—something like `/\P{L}'|'\P{L}/`. But you also have to take the start and end of the line into account.

{{index grouping, "replace method", [parentheses, "in regular expressions"]}}

In addition, you must ensure that the replacement also includes the characters that were matched by the `\P{L}` pattern so that those are not dropped. This can be done by wrapping them in parentheses and including their groups in the replacement string (`$1`, `$2`). Groups that are not matched will be replaced by nothing.

hint}}

### Numbers again

{{index sign, "fractional number", [syntax, number], minus, "plus character", exponent, "scientific notation", "period character"}}

Write an expression that matches only JavaScript-style ((number))s. It must support an optional minus _or_ plus sign in front of the number, the decimal dot, and exponent notation—`5e-3` or `1E10`—again with an optional sign in front of the exponent. Also note that it is not necessary for there to be digits in front of or after the dot, but the number cannot be a dot alone. That is, `.5` and `5.` are valid JavaScript numbers, but a lone dot _isn't_.

{{if interactive
```{test: no}
// Fill in this regular expression.
let number = /^...$/;

// Tests:
for (let str of ["1", "-1", "+15", "1.55", ".5", "5.",
                 "1.3e2", "1E-4", "1e+12"]) {
  if (!number.test(str)) {
    console.log(`Failed to match '${str}'`);
  }
}
for (let str of ["1a", "+-1", "1.2.3", "1+1", "1e4.5",
                 ".5.", "1f5", "."]) {
  if (number.test(str)) {
    console.log(`Incorrectly accepted '${str}'`);
  }
}
```

if}}

{{hint

{{index ["regular expression", escaping], ["backslash character", "in regular expressions"]}}

First, do not forget the backslash in front of the period.

Matching the optional ((sign)) in front of the ((number)), as well as in front of the ((exponent)), can be done with `[+\-]?` or `(\+|-|)` (plus, minus, or nothing).

{{index "pipe character"}}

The more complicated part of the exercise is the problem of matching both `"5."` and `".5"` without also matching `"."`. For this, a good solution is to use the `|` operator to separate the two cases—either one or more digits optionally followed by a dot and zero or more digits _or_ a dot followed by one or more digits.

{{index exponent, "case sensitivity", ["regular expression", flags]}}

Finally, to make the _e_ case insensitive, either add an `i` option to the regular expression or use `[eE]`.

hint}}
