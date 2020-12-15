# Yazılım Mimarisinin Üç R'ı

![software architecture pyramid](public/software-architecture-pyramid.png)

## Yazılım Mimarisi

50 yılı aşkın yazılım mühendisliğinin varlığından sonra, yazılım mimarisinin tam bir tanımına varmadık. Sonuçta, bilgisayar bilimindeki sanattır - onu tanımlamak için en kararlı çabalarımızdan tam altını doldurmuyor. Yine de, endüstrimizin ve uygulamalarımızın yapısı için o kadar hayati ki, görmezden gelmek imkansızdır.

Fikir birliği tam anlamıyla olmamasına rağmen, yazılım mimarisinin resmileştirilmesine yaklaşmamıza yardımcı olabilecek birçok tanım var. Belki de en dikkat çekeni IEEE'den geliyor:

> "Mimarlık bileşenlerini , bileşenlerin birbirleriyle ilişkilerini ve bileşenlerin çevresiyle ilişkisini içeren ve tasarım ve gelişimine rehberlik eden bir sistemin temel organizasyonudur." [IEEE 1471]

Bu tanım ve diğerleri mimariyi oluşturan unsurlara açıklık getirebilse de, uygulamalarımızı geliştirirken kullanmamız için zihinsel bir model vermiyor. Ancak bu proje sadece bunu vermeyi amaçlamaktadır. 3 özel ["ility"](https://en.wiktionary.org/wiki/ility) (okunabilirlik`readability`, tekrar kullanılabilirlik `reusability` ve yeniden düzenlenebilirlik `refactorability`) bakarak, sistemimizin kodu ve mimarisini düşünmek için bize bir çerçeve oluşturabilecek bir mimari özellikler hiyerarşisi oluşturabiliriz. Size bir mimari vermeyecektir, ancak uygulamanız için hangi mimarinin çalıştığını düşünmenizde size yol gösterecektir.

## Bu Proje Nedir?

Bu proje, yazılım mimarisinin 3 “ility”si ile (okunabilirlik, tekrar kullanılabilirlik ve yeniden düzenlenebilirlik) analiz etmeye çalışan ve bu kavramları hiyerarşik olarak düşünerek nasıl daha iyi kod oluşturabileceğimizi gösteren bir rehberdir. Bu proje herhangi bir beceri seviyesindeki herhangi bir geliştirici içindir, ancak yeni başlıyorsanız, bu konuda deneyimli bir uygulayıcıdan daha fazla değer bulacaksınız.

Bakacağımız kod, JavaScript'te yazılmış, ekosistemdeki iki büyük kütüphaneyi kullanan (React ve Redux) çok basit bir alışveriş sepeti uygulamasıdır JavaScript ve yukarıda belirtilen araçlar hiçbir şekilde belirli bir uygulamayı yapılandırmanın tek yolu değildir. Sektöre birçok yeni gelen ve birçok uzman tarafından da kullanılıyorlar, bu nedenle kullanım sıklıkları onları kod kalitesini tartışmak için harika bir ortak dil haline getiriyor. Uygulamamızı her seferinde bir parça geliştireceğiz ve `3 R` hiyerarşisindeki her adımda Kötü ve İyi sürümlerine bakacağız. Tüm kodu `src` dizininde bulabilirsiniz ve bunun nasıl oluşturulacağı ve geliştirileceği ile ilgili talimatlar bu README'nin altındadır.

Tekrarlanması gereken bir şey daha var: bu proje yazılıma bakmanın tek yolu değildir ve kesinlikle size bir mimari veremez, ancak bu benimkini yönlendirdiği gibi sizin de düşüncelerinizi yönlendirebilecek bir rehberdir.

Daha fazla uzatmadan başlayalım!

## 1. Readability (Okunabilirlik)

Okunabilirlik, kod kalitesini değerlendirmenin en basit yoludur ve düzeltilmesi en kolay yöntemdir. Bir kod parçasını açtığınızda gördüğünüz en bariz şeydir ve genellikle aşağıdakilerden oluşur:

- Biçimlendirme
- Değişken isimleri
- Fonksiyon adları
- Fonksiyon argümanlarının miktarı
- Fonksiyon uzunluğu (satır sayısı)
- İç içe geçme seviyeleri

Dikkate alınması gereken tek şey bunlar değil, ama kırmızı bayraklar kaldırmaya en yakın olanları. Neyse ki, yukarıdaki sorunlarla ilgili sorunları çözmek için izlenmesi gereken birkaç kolay kural vardır:

- Otomatik bir biçimlendiriciye yatırım yapın. Ekibinizin üzerinde anlaştığı bir tanesini bulun ve oluşturma sürecinize entegre edin. Kod incelemeleri sırasında argümanları biçimlendirmekten daha fazla zaman ve para harcayan hiçbir şey yoktur. Bir biçimlendirici alın ve asla geriye bakmayın! Bu projede Prettier'i kullanacağız.
- Anlamlı ve belirgin değişken / fonksiyon adları kullanın. Kod insanlar içindir ve yalnızca arada bir bilgisayarlar içindir. Adlandırma, kodunuzun arkasındaki anlamı bildiren en büyük şeydir.
- Fonksiyon argümanlarınızı 1-3 arasında sınırlandırın. 0 argüman, durumu mutasyona(değiştirmeye) uğrattığınızı veya arayandan başka bir yerden gelen duruma güvendiğinizi gösterir. 3'ten fazla argümanın okunması ve yeniden düzenlenmesi oldukça zordur zor olmasının sebebi fonksiyonunuzun sahip olduğu daha fazla argümanı alabileceği birçok yol var.
- Bir işlev için belirlenmiş bir satır sınırı yoktur, çünkü bu kodladığınız belirli bir dile bağlıdır. Ana nokta, işlevinizin bir şey ve yalnızca bir şey yapmasıdır. Vergiden sonra bir öğenin fiyatını hesaplayan işleviniz önce veritabanına bağlanmak, öğeye bakmak, vergi verilerini almak ve sonra hesaplamayı yapmak zorundaysa, o zaman açıkça birden fazla şey yapar. Uzun fonksiyonlar tipik olarak çok fazla şey olduğunu gösterir.
- İkiden fazla iç içe yerleştirme düzeyi düşük performansa (bir döngüde) işaret edebilir ve uzun şartlarda okumak özellikle zor olabilir. İç içe mantığı ayrı fonksiyonlara ayırmayı göz önünde bulundurun.

Kötü okunabilirliğin neye benzediğini görmek için alışveriş sepeti uygulamamızın bu ilk parçasına bir göz atalım:

```javascript
// src/1-readable/bad/index.js
import React, { Component } from "react";

// Inventory
class inv extends Component {
  constructor() {
    super();

    // State
    this.state = {
      c: "usd", // currency
      i: [
        // inventory
        {
          product: "Flashlight",
          img: "/flashlight.jpg",
          desc: "A really great flashlight",
          price: 100,
          id: 1,
          c: "usd",
        },
        {
          product: "Tin can",
          img: "/tin_can.jpg",
          desc: "Pretty much what you would expect from a tin can",
          price: 32,
          id: 2,
          c: "usd",
        },
        {
          product: "Cardboard Box",
          img: "/cardboard_box.png",
          desc: "It holds things",
          price: 5,
          id: 3,
          c: "usd",
        },
      ],
    };
  }

  render() {
    return (
      <table style={{ width: "100%" }}>
        <tbody>
          <tr>
            <th>Product</th>

            <th>Image</th>

            <th>Description</th>

            <th>Price</th>
          </tr>
          {this.state.i.map(function (i, idx) {
            return (
              <tr key={idx}>
                <td>{i.product}</td>

                <td>
                  <img src={i.img} alt="" />
                </td>

                <td>{i.desc}</td>

                <td>{i.price}</td>
              </tr>
            );
          })}
        </tbody>
      </table>
    );
  }
}

export default inv;
```

Hemen görebildiğimiz bir takım sorunlar var:

- Tutarsız ve hoş olmayan biçimlendirme
- Kötü adlandırılmış değişkenler
- Dağınık veri yapıları (envanter kimlikler tarafından anahtarlanmamış)
- Gereksiz olan ya da iyi bir değişken isminin ne işe yaradığına dair yorumlar

Bunu nasıl geliştirebileceğimize bir göz atalım:

```javascript
// src/1-readable/good/index.js
import React, { Component } from "react";

export default class Inventory extends Component {
  constructor() {
    super();
    this.state = {
      localCurrency: "usd",
      inventory: {
        1: {
          product: "Flashlight",
          img: "/flashlight.jpg",
          desc: "A really great flashlight",
          price: 100,
          currency: "usd",
        },
        2: {
          product: "Tin can",
          img: "/tin_can.jpg",
          desc: "Pretty much what you would expect from a tin can",
          price: 32,
          currency: "usd",
        },
        3: {
          product: "Cardboard Box",
          img: "/cardboard_box.png",
          desc: "It holds things",
          price: 5,
          currency: "usd",
        },
      },
    };
  }

  render() {
    return (
      <table style={{ width: "100%" }}>
        <tbody>
          <tr>
            <th>Product</th>

            <th>Image</th>

            <th>Description</th>

            <th>Price</th>
          </tr>
          {Object.keys(this.state.inventory).map((itemId) => (
            <tr key={itemId}>
              <td>{this.state.inventory[itemId].product}</td>

              <td>
                <img src={this.state.inventory[itemId].img} alt="" />
              </td>

              <td>{this.state.inventory[itemId].desc}</td>

              <td>{this.state.inventory[itemId].price}</td>
            </tr>
          ))}
        </tbody>
      </table>
    );
  }
}
```

Bu geliştirilmiş kod artık aşağıdaki özellikleri göstermektedir:

- Otomatik biçimlendirici Prettier kullanılarak sürekli biçimlendirilir
- İsimler çok daha açıklayıcı
- Veri yapıları uygun şekilde düzenlenmiştir. Bu durumda, Envanter kimliğe göre anahtarlanır. Kötü okunabilirlik kötü performans anlamına gelebilir. Kötü kod örneğimizde envanterimizden bir öğe almak isteseydik, bir O (n) arama zamanımız olurdu, ancak Kimliğe göre anahtarlanmış Envanter ile, büyük envanterlerle ÇOK daha hızlı olan bir O (1) araması alırız.
- Yorumlara artık gerek yoktur çünkü iyi adlandırma, kodun anlamını netleştirmeye yarar. İş mantığı karmaşık olduğunda ve dokümantasyon gerektiğinde yorumlar gereklidir.

## 2. Reusability (Tekrar Kullanılabilirlik)

Yeniden kullanılabilirlik, bu kodu okuyabilmenizin, yabancılarla çevrimiçi iletişim kurabilmenizin ve hatta program yapabilmenizin tek nedenidir. Yeniden kullanılabilirlik, yeni fikirleri geçmişin küçük parçalarıyla ifade etmemizi sağlar.

Bu nedenle yeniden kullanılabilirlik, yazılım mimarinize rehberlik etmesi gereken çok önemli bir kavramdır. Yeniden kullanılabilirliği genellikle DRY (Don't Repeat Yourself) açısından düşünürüz. Bu, işin bir yönü - doğru şekilde soyutlayabiliyorsanız(abstraction), yinelenen koda sahip olmayın. Yeniden kullanılabilirlik bunun da ötesine geçer. Bu, geliştirici arkadaşlarınızın "Evet, bunun ne işe yaradığını tam olarak biliyorum!" Demesini sağlayan temiz, basit API'ler yapmakla ilgilidir! Yeniden kullanılabilirlik, kodunuzu birlikte çalışmayı bir zevk haline getirir ve özellikleri daha hızlı gönderebileceğiniz anlamına gelir.

Önceki örneğimize bakacağız ve envanterimizin birden çok ülkedeki fiyatlandırmasını işlemek için bir para birimi dönüştürücü ekleyerek bunu genişleteceğiz:

```javascript
// src/2-reusable/bad/index.js
import React, { Component } from "react";

export default class Inventory extends Component {
  constructor() {
    super();
    this.state = {
      localCurrency: "usd",
      inventory: {
        1: {
          product: "Flashlight",
          img: "/flashlight.jpg",
          desc: "A really great flashlight",
          price: 100,
          currency: "usd",
        },
        2: {
          product: "Tin can",
          img: "/tin_can.jpg",
          desc: "Pretty much what you would expect from a tin can",
          price: 32,
          currency: "usd",
        },
        3: {
          product: "Cardboard Box",
          img: "/cardboard_box.png",
          desc: "It holds things",
          price: 5,
          currency: "usd",
        },
      },
    };

    this.currencyConversions = {
      usd: {
        rupee: 66.78,
        yuan: 6.87,
        usd: 1,
      },
    };

    this.currencySymbols = {
      usd: "$",
      rupee: "₹",
      yuan: "元",
    };
  }

  onSelectCurrency(e) {
    this.setState({
      localCurrency: e.target.value,
    });
  }

  convertCurrency(amount, fromCurrency, toCurrency) {
    const convertedCurrency =
      amount * this.currencyConversions[fromCurrency][toCurrency];
    return this.currencySymbols[toCurrency] + convertedCurrency;
  }

  render() {
    return (
      <div>
        <label htmlFor="currencySelector">Currency:</label>
        <select
          className="u-full-width"
          id="currencySelector"
          onChange={this.onSelectCurrency.bind(this)}
          value={this.state.localCurrency}
        >
          <option value="usd">USD</option>
          <option value="rupee">Rupee</option>
          <option value="yuan">Yuan</option>
        </select>
        <table style={{ width: "100%" }}>
          <tbody>
            <tr>
              <th>Product</th>

              <th>Image</th>

              <th>Description</th>

              <th>Price</th>
            </tr>

            {Object.keys(this.state.inventory).map((itemId) => (
              <tr key={itemId}>
                <td>{this.state.inventory[itemId].product}</td>

                <td>
                  <img src={this.state.inventory[itemId].img} alt="" />
                </td>

                <td>{this.state.inventory[itemId].desc}</td>

                <td>
                  {this.convertCurrency(
                    this.state.inventory[itemId].price,
                    this.state.inventory[itemId].currency,
                    this.state.localCurrency
                  )}
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    );
  }
}
```

Bu kod çalışır, ancak yalnızca çalışmak kodun amacı değildir. Bu yüzden buna, işe yarayıp yaramadığını ve okunabilir olup olmadığını analiz etmekten daha güçlü bir mercekle bakmamız gerekiyor. Yeniden kullanılabilir olup olmadığına bakmalıyız. Herhangi bir sorun fark ettiniz mi?

Biraz düşün!

Pekala, yukarıdaki kodda 3 ana sorun var:

- Para Birimi Seçici, Envanter bileşenine bağlıdır
- Para Birimi Dönüştürücü, Envanter bileşenine bağlanır
- Envanter verileri, Envanter bileşeninde açıkça tanımlanır ve bu bir API'den gelmemektedir.

Her işlev ve modül sadece bir şey yapmalıdır, aksi takdirde kaynak koda baktığınızda neler olduğunu anlamak çok zor olabilir. Envanter bileşeni, para birimlerini dönüştürmek ve seçmek için değil, yalnızca bir envanteri görüntülemek için olmalıdır. Modüllerin ve işlevlerin bir şeyi yapmasının yararı, test edilmelerinin daha kolay olması ve yeniden kullanılmasının daha kolay olmasıdır. Döviz Çeviricimizi uygulamanın başka bir bölümünde kullanmak isteseydik, Envanter bileşeninin tamamını dahil etmemiz gerekirdi. Sadece para birimini çevirmemiz gerekiyorsa, bu mantıklı değil.

Daha fazla yeniden kullanılabilir bileşenle bunun neye benzediğini görelim:

```javascript
// src/2-reusable/good/currency-converter.js
export default class CurrencyConverter {
  constructor(currencyConversions) {
    this.currencyConversions = currencyConversions;
    this.currencySymbols = {
      usd: "$",
      rupee: "₹",
      yuan: "元",
    };
  }

  convert(amount, fromCurrency, toCurrency) {
    const convertedCurreny =
      amount * this.currencyConversions[fromCurrency][toCurrency];
    return this.currencySymbols[toCurrency] + convertedCurreny;
  }
}
```

```javascript
// src/2-reusable/good/currency-selector.js

import React, { Component } from "react";

class CurrencySelector extends Component {
  constructor(props) {
    super();
    this.props = props;
    this.state = {
      localCurrency: props.localCurrency.currency,
    };

    this.setGlobalCurrency = props.setGlobalCurrency;
  }

  onSelectCurrency(e) {
    const currency = e.target.value;

    this.setGlobalCurrency(currency);

    this.setState({
      localCurrency: currency,
    });
  }

  render() {
    return (
      <div>
        <label htmlFor="currencySelector">Currency:</label>
        <select
          className="u-full-width"
          id="currencySelector"
          onChange={this.onSelectCurrency.bind(this)}
          value={this.state.localCurrency}
        >
          <option value="usd">USD</option>
          <option value="rupee">Rupee</option>
          <option value="yuan">Yuan</option>
        </select>
      </div>
    );
  }
}

CurrencySelector.propTypes = {
  setGlobalCurrency: React.PropTypes.func.isRequired,
  localCurrency: React.PropTypes.string.isRequired,
};

export default CurrencySelector;
```

```javascript
// src/2-reusable/good/inventory.js

import React, { Component } from "react";

class Inventory extends Component {
  constructor(props) {
    super();
    this.state = {
      localCurrency: props.localCurrency,
      inventory: props.inventory,
    };

    this.CurrencyConverter = props.currencyConverter;
  }

  componentWillReceiveProps(nextProps) {
    this.setState({
      localCurrency: nextProps.localCurrency,
    });
  }

  render() {
    return (
      <div>
        <table style={{ width: "100%" }}>
          <tbody>
            <tr>
              <th>Product</th>

              <th>Image</th>

              <th>Description</th>

              <th>Price</th>
            </tr>

            {Object.keys(this.state.inventory).map((itemId) => (
              <tr key={itemId}>
                <td>{this.state.inventory[itemId].product}</td>

                <td>
                  <img src={this.state.inventory[itemId].img} alt="" />
                </td>

                <td>{this.state.inventory[itemId].desc}</td>

                <td>
                  {this.CurrencyConverter.convert(
                    this.state.inventory[itemId].price,
                    this.state.inventory[itemId].currency,
                    this.state.localCurrency
                  )}
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    );
  }
}

Inventory.propTypes = {
  inventory: React.PropTypes.object.isRequired,
  currencyConverter: React.PropTypes.object.isRequired,
  localCurrency: React.PropTypes.string.isRequired,
};

export default Inventory;
```

```javascript
// src/2-reusable/good/index.js

import React, { Component } from "react";
import CurrencyConverter from "./currency-converter";
import Inventory from "./inventory";
import CurrencySelector from "./currency-selector";

export default class ReusableGood extends Component {
  constructor() {
    super();

    this.inventory = {
      1: {
        product: "Flashlight",
        img: "/flashlight.jpg",
        desc: "A really great flashlight",
        price: 100,
        currency: "usd",
      },
      2: {
        product: "Tin can",
        img: "/tin_can.jpg",
        desc: "Pretty much what you would expect from a tin can",
        price: 32,
        currency: "usd",
      },
      3: {
        product: "Cardboard Box",
        img: "/cardboard_box.png",
        desc: "It holds things",
        price: 5,
        currency: "usd",
      },
    };

    // Most likely we would fetch this from an external source if this were a real app
    this.currencyConversions = {
      usd: {
        rupee: 66.78,
        yuan: 6.87,
        usd: 1,
      },
    };

    this.state = {
      localCurrency: "usd",
    };

    this.setGlobalCurrency = (currency) => {
      this.setState({
        localCurrency: currency,
      });
    };
  }

  render() {
    return (
      <div>
        <CurrencySelector
          setGlobalCurrency={this.setGlobalCurrency}
          localCurrency={this.state.localCurrency}
        />
        <Inventory
          inventory={this.inventory}
          currencyConverter={new CurrencyConverter(this.currencyConversions)}
          localCurrency={this.state.localCurrency}
        />
      </div>
    );
  }
}
```

Bu kod baya gelişti. Artık para birimi seçimi ve dönüştürme için ayrı modüllerimiz var. Dahası, artık envanter verilerini Envanter bileşenimize sağlayabiliriz. Bu, örneğin envanter verilerini indirebileceğimiz ve kaynak kodunu değiştirmeden Envanter bileşenine sağlayabileceğimiz anlamına gelir. Bu ayrıştırma, Bağımlılığı Tersine Çevirme İlkesidir(Dependency Inversion Principle) ve yeniden kullanılabilir kod oluşturmanın güçlü bir yoludur.

Şimdi biraz dikkatli olma zamanı. Derine dalmadan ve her şeyi yeniden kullanılabilir hale getirmeden önce, yeniden kullanılabilirliğin başkalarının kullanması için iyi bir API'ye sahip olmanız gerektirdiğini anlamak önemlidir. Bunu yapmazsanız, güncellemeye gittiğinizde API'nizi kullanan kişi için çok iyi bir yol olmayabilir çünkü yeterince iyi düşünülmediğini anlarsınız. Öyleyse, kod ne zaman yeniden KULLANILAMAMALIDIR?

- Henüz iyi bir API tanımlayamıyorsanız, ayrı bir modül oluşturmayın. Çoğaltma, kötü bir temelden daha iyidir.
- Yakın gelecekte işlevinizi veya modülünüzü yeniden kullanmayı beklemiyorsanız.

## 3. Refactorability (Yeniden düzenlenebilirlik)

Yeniden düzenlenebilen kod, korkmadan değiştirebileceğiniz koddur.
Bir Cuma gecesi canlıya alabileceğiniz ve Pazartesi sabahı, kullanıcılarınızın herhangi bir hatayla karşılaştığına dair herhangi bir endişe duymadan geri gelebileceğiniz koddur.

Yeniden düzenlenebilirlik, bir bütün olarak sistemle ilgilidir. Yeniden kullanılabilir modüllerinizin LEGO parçaları gibi birbirine nasıl bağlandığıyla ilgili. Çalışan modülünüzü değiştirirseniz ve Raporlama modülünüzü bir şekilde bozarsanız, bazı yeniden düzenleme sorunlarınız olduğunu anlarsınız. Yeniden düzenlenebilirlik, 3 R hiyerarşisinin en yüksek parçasıdır ve elde edilmesi ve sürdürülmesi en zor olanıdır. İnsanin dahil olduğu sistemde mutlaka hatalar olacaktır ve kodlar bundan farklı değildir. Ancak, kodumuzu yeniden düzenlenebilir hale getirmek için yapabileceğimiz şeyler var. Peki bunlar nedir?

- İzole yan etkiler
- Testler
- Statik tipler

JavaScript kullanıyoruz ve TypeScript gibi türleri olan bir alternatif değil bu yüzden statik türlerin yardımlarını tam göremeyeceğiz. Kodunuzun aşağıda gördüğünüz gibi türleri olduğunda, hiç kimsenin kodunuza yanlış değerler geçiremeyeceğini ve bu da uygulamanızın yaşayabileceği olası hataların sayısını sınırladığını söylemek yeterlidir:

```javascript
// We can't get passed arrays, strings, objects, or any type other than a number
function add(a: number, b: number) {
  return a + b;
}
```

Büyük uygulamalar için JavaScript'e statik olarak yazılmış bir alternatif kullanmanızı şiddetle tavsiye ederim. Tipler, testlerinizin sağlayabileceğinin ötesinde size ekstra güven verir. Ancak türler her şeye yardımcı olmaz, yine de yan etkilerinizi izole etmeniz ve kodunuzu test etmeniz gerekir.

Yan etkilerinizi izole etmek ne anlama geliyor diye merak ediyor olabilirsiniz. Ve hatta yan etkilerin ne olduğunu soruyor olabilirsiniz?

Bir _yan etki_, işlevinizin veya modülünüzün kendi kapsamı dışındaki verileri değiştirmesidir. Bir diske veri yazıyorsanız, global bir değişkeni değiştiriyorsanız veya hatta terminale bir şey yazdırıyorsanız, bir yan etkiniz vardır. Programınızın hiç yan etkisi yoksa, o zaman bir kara kutudur(Black box). Programlar, bir bilgisayarın yürüttüğü, verileri alıp veri üreten talimatlardır. Dışarı çıkan bir veri yoksa, o program işe yarayan bir program değildir. Ancak, bir programın veri üretebilmesi için kendi dışındaki dünyada bir şeyi değiştirmesi gerekir. Bu nedenle yan etkilere ihtiyacımız var ama onları da izole etmemiz gerekiyor.

Yan etkileri neden izole etmeliyiz?

- Yan etkiler kodumuzun test edilmesini zorlaştırır, çünkü bir işlevin çalışması başka bir işlevin bağlı olduğu bazı verileri değiştirirse, bir işlevin her zaman aynı girdiyle aynı çıktıyı vereceğinden emin olamayız.
- Yan etkiler, aksi takdirde yeniden kullanılabilir modüller arasında bağlantı sağlar. Modül A, modül B'nin bağlı olduğu bazı genel durumu değiştirirse, A'nın B'den önce çalıştırılması gerekir.
- Yan etkiler sistemimizi öngörülemez hale getirir. Herhangi bir işlev veya modül uygulamanın durumunu değiştirebiliyorsa, bir modülü güncellememizin tüm sistemi nasıl etkileyeceğinden emin olamayız.

Yan etkileri nasıl izole ederiz? Cevap, uygulamamızın global durumunu güncellemek için tek bir merkezi yer oluşturmaktır. İstemci tarafı JavaScript uygulaması için bunu yapmanın birçok harika yolu vardır, ancak bu proje için Redux kullanacağız.

Bir alışveriş sepeti eklemek için mevcut kodumuzu değiştireceğiz. Bu yeni koda bir göz atalım ve neden yeniden düzenleneMEdiğini görelim:

```javascript
// src/3-refactorable/bad/inventory.js
import React, { Component } from "react";

class Inventory extends Component {
  constructor(props) {
    super();
    this.state = {
      localCurrency: props.localCurrency,
      inventory: props.inventory,
    };

    this.CurrencyConverter = props.currencyConverter;
    this.cart = window.cart;
  }

  componentWillReceiveProps(nextProps) {
    this.setState({
      localCurrency: nextProps.localCurrency,
    });
  }

  onAddToCart(itemId) {
    this.cart.push(itemId);
  }

  render() {
    return (
      <div>
        <table style={{ width: "100%" }}>
          <tbody>
            <tr>
              <th>Product</th>

              <th>Image</th>

              <th>Description</th>

              <th>Price</th>

              <th>Cart</th>
            </tr>

            {Object.keys(this.state.inventory).map((itemId) => (
              <tr key={itemId}>
                <td>{this.state.inventory[itemId].product}</td>

                <td>
                  <img src={this.state.inventory[itemId].img} alt="" />
                </td>

                <td>{this.state.inventory[itemId].desc}</td>

                <td>
                  {this.CurrencyConverter.convert(
                    this.state.inventory[itemId].price,
                    this.state.inventory[itemId].currency,
                    this.state.localCurrency
                  )}
                </td>

                <td>
                  <button onClick={() => this.onAddToCart(itemId)}>
                    Add to Cart
                  </button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    );
  }
}

Inventory.propTypes = {
  inventory: React.PropTypes.object.isRequired,
  localCurrency: React.PropTypes.string.isRequired,
  currencyConverter: React.PropTypes.object.isRequired,
};

export default Inventory;
```

```javascript
// src/3-refactorable/bad/cart.js
import React, { Component } from "react";

class Cart extends Component {
  constructor(props) {
    super();
    this.state = {
      cart: window.cart,
      localCurrency: props.localCurrency,
      inventory: props.inventory,
    };

    // Repeatedly sync global cart to local cart
    this.watcher = window.setInterval(() => {
      this.setState({
        cart: window.cart,
      });
    }, 1000);
    this.CurrencyConverter = props.currencyConverter;
  }

  componentWillReceiveProps(nextProps) {
    this.setState({
      localCurrency: nextProps.localCurrency,
    });
  }

  componentWillUnmount() {
    window.clearInterval(this.watcher);
  }

  render() {
    return (
      <div>
        <h2>Cart</h2>
        {this.state.cart.length === 0 ? (
          <p>Nothing in the cart</p>
        ) : (
          <table style={{ width: "100%" }}>
            <tbody>
              <tr>
                <th>Product</th>

                <th>Price</th>
              </tr>
              {this.state.cart.map((itemId, idx) => (
                <tr key={idx}>
                  <td>{this.state.inventory[itemId].product}</td>

                  <td>
                    {this.CurrencyConverter.convert(
                      this.state.inventory[itemId].price,
                      this.state.inventory[itemId].currency,
                      this.state.localCurrency
                    )}
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        )}
      </div>
    );
  }
}

Cart.propTypes = {
  inventory: React.PropTypes.object.isRequired,
  localCurrency: React.PropTypes.string.isRequired,
  currencyConverter: React.PropTypes.object.isRequired,
};

export default Cart;
```

Burada, şu anda alışveriş sepetinde bulunan envanter öğelerini gösteren yeni bir alışveriş sepeti modülümüz var. Bu kodda çok sorunlu iki şey var, bunlar nedir?

Biraz düşünelim!

Yukarıdaki kodla ilgili iki ana sorun şunlardır:

- Alışveriş sepeti global bir değişkene yazılır: `window.cart`
- Sepet, global `window.cart`tan sürekli okunarak güncellenir ve bu zamanlamaya bir bağlılık sağlar.

Modüllerimiz yeniden kullanılabilir ve okunabilir olsa da, küresel(global) değişkenlere yazarak genel sistemimizi çok kırılgan hale getiriyoruz. Eklenen herhangi bir kütüphane(third-party library) bizim kullandığımız `window.cart` üzerine yazma işlemi yapabilir ve bir şeyleri bozabilir. Ayrıca, yazdığımız herhangi bir modül, herhangi bir güvenlik önlemi veya merkezi güncelleme yolu olmadan ona erişebilir ve onu değiştirebilir.

"Evet, evet, uygulamamı ilk etapta asla böyle yapılandırmam" diyor olabilirsiniz. Bu harika! Yine de, bu abartılı olsa da, asıl mesele, alışveriş sepetinin güncellenme ve okunma şeklinin merkezileştirilmemiş olmasıdır.Global değişkenler ve `setInterval` kullanmak yerine bir mesaj iletme modülü kullanıyor olsaydınız, bu durum kodunuzun anlaşılmasını ve geniş ölçekte yeniden düzenlenmesini de zorlaştırabilir çünkü durumu izole etmek ve bir modülün diğerini nasıl etkileyebileceğini anlamak zor olabilir.

We will centralize our state management using Redux. If you haven't used Redux before, [check out the tutorial](http://redux.js.org/docs/basics/).

Redux kullanarak durum yönetimimizi(state management) merkezileştireceğiz. Daha önce Redux kullanmadıysanız.[Dokumantasyona göz atın](http://redux.js.org/docs/basics/).

Bakalım bu daha yeniden işlenebilir kod neye benziyor:

```javascript
// src/3-refactorable/good/containers/inventory.js
import React from "react";
import { connect } from "react-redux";
import { addToCartAction } from "../actions";

import CurrencyConverter from "../lib/currency-converter";
import Inventory from "../components/inventory";

const InventoryContainer = ({
  inventory,
  currencies,
  localCurrency,
  addToCart,
}) => (
  <Inventory
    currencyConverter={new CurrencyConverter(currencies)}
    inventory={inventory}
    localCurrency={localCurrency}
    addToCart={(productId) => addToCart(productId)}
  />
);

InventoryContainer.propTypes = {
  currencies: React.PropTypes.object.isRequired,
  inventory: React.PropTypes.object.isRequired,
  localCurrency: React.PropTypes.string.isRequired,
  addToCart: React.PropTypes.func.isRequired,
};

const mapStateToProps = (state) => ({
  inventory: state.inventory,
  currencies: state.currencies,
  localCurrency: state.localCurrency,
});

export default connect(mapStateToProps, {
  addToCart: addToCartAction,
})(InventoryContainer);
```

```javascript
// src/3-refactorable/good/containers/cart.js
import React from "react";
import { connect } from "react-redux";

import CurrencyConverter from "../lib/currency-converter";
import Cart from "../components/cart";

const CartContainer = ({ cart, inventory, currencies, localCurrency }) => (
  <Cart
    cart={cart}
    currencyConverter={new CurrencyConverter(currencies)}
    localCurrency={localCurrency}
    inventory={inventory}
  />
);

CartContainer.propTypes = {
  currencies: React.PropTypes.object.isRequired,
  cart: React.PropTypes.array.isRequired,
  inventory: React.PropTypes.object.isRequired,
  localCurrency: React.PropTypes.string.isRequired,
};

const mapStateToProps = (state) => ({
  cart: state.cart,
  inventory: state.inventory,
  currencies: state.currencies,
  localCurrency: state.localCurrency,
});

export default connect(mapStateToProps, {})(CartContainer);
```

```javascript
// src/3-refactorable/good/actions/index.js
import * as types from "../constants/action-types";

export const addToCartAction = (productId) => ({
  type: types.ADD_TO_CART,
  productId,
});
```

```javascript
// src/3-refactorable/good/reducers/cart.js
import { ADD_TO_CART } from "../constants/action-types";

const initialState = [];

export default (state = initialState, action) => {
  switch (action.type) {
    case ADD_TO_CART:
      return [...state, parseInt(action.productId, 10)];
    default:
      return state;
  }
};
```

Bu geliştirilmiş kod, yan etkilerimizi, `productId`'yi barındıran bir `action` ve oradan da `reducer`'e iletilen şekilde ürün eklemeyi yapan şekilde merkezleştirilmiş oldu. Bu yeni alışveriş sepeti `store` yerleştirilir ve bu gerçekleştiğinde, durumlarını `store` belirli parçalarından alan tüm bileşenlerimiz, yeni verilerin `react-redux` tarafından bilgilendirilir ve bunların durumlarını(state) günceller. React, güncellenen her bileşeni akıllıca yeniden(re-render) oluşturacak ve işte bu kadar!

Bu akış, uygulamanızın durumunun yalnızca tek bir şekilde güncellenebileceğinden emin olmanızı mümkün kılar ve bu, _action_ -> _reducer_ -> _store_ -> _component_ pipeline yoluyla olur .Değiştirilecek küresel bir durum, iletilecek ve izlenecek mesaj yok ve modüllerimizin üretebileceği kontrolsüz yan etkiler yok. En iyi yanı, uygulamamızın tüm durumunu takip edebilmemizdir, böylece hata ayıklama ve QA çok daha kolay hale gelebilir, çünkü tüm uygulamamızın tam bir anlık görüntüsüne sahibiz.

Unutulmaması gereken bir uyarı: Bu projenin örnek uygulamasında Redux'a ihtiyacınız olmayabilir, ancak bu kodu genişletecek olsaydık, Redux'u her şeyi üst düzey denetleyiciye `index.js` ye koymak yerine durum yönetimi çözümü olarak kullanmak daha kolay hale gelirdi.

Orada uygulamamızın durumunu izole edebilir ve uygun veri değiştirme eylem işlevlerini her modülden geçirebilirdik. Bununla ilgili sorun, ölçeğe göre, devasa bir `index.js` denetleyicisinde yaşayacak çok fazla veriye ve aktarılacak çok sayıda işlemimizin olması. Durumun uygun şekilde merkezileştirilmesini erkenden uyguladığımızda, uygulamamız geliştikçe çok fazla değişiklik yapmamıza gerek kalmayacak.

Bakmamız gereken son şey testler. Testler, bir modülü değiştirebileceğimize dair bize güven verir ve yine de yapılması planlanan şeyi yapar. Alışveriş Sepeti ve Envanter bileşenleri için testlere bakacağız:

```javascript
// src/test/cart.test.js
import React from "react";
import { shallow } from "enzyme";
import Cart from "../src/3-refactorable/good/components/cart";

const props = {
  localCurrency: "usd",
  cart: [1, 1],
  inventory: {
    1: {
      product: "Flashlight",
      img: "/flashlight.jpg",
      desc: "A really great flashlight",
      price: 100,
      currency: "usd",
    },
  },
  currencyConverter: {
    convert: jest.fn(),
  },
};

it("should render Cart without crashing", () => {
  const cartComponent = shallow(<Cart {...props} />);
  expect(cartComponent);
});

it("should show all cart data in cart table", () => {
  props.currencyConverter.convert = function () {
    return `$${props.inventory[1].price}`;
  };

  const cartComponent = shallow(<Cart {...props} />);
  let tr = cartComponent.find("tr");
  expect(tr.length).toEqual(3);

  props.cart.forEach((item, idx) => {
    let td = cartComponent.find("td");
    let product = td.at(2 * idx);
    let price = td.at(2 * idx + 1);

    expect(product.text()).toEqual(props.inventory[item].product);
    expect(price.text()).toEqual(props.currencyConverter.convert());
  });
});
```

```javascript
// src/test/inventory.test.js
import React from "react";
import { shallow } from "enzyme";
import Inventory from "../src/3-refactorable/good/components/inventory";

const props = {
  localCurrency: "usd",
  inventory: {
    1: {
      product: "Flashlight",
      img: "/flashlight.jpg",
      desc: "A really great flashlight",
      price: 100,
      currency: "usd",
    },
  },
  addToCart: jest.fn(),
  changeCurrency: jest.fn(),
  currencyConverter: {
    convert: jest.fn(),
  },
};

it("should render Inventory without crashing", () => {
  const inventoryComponent = shallow(<Inventory {...props} />);
  expect(inventoryComponent);
});

it("should show all inventory data in table", () => {
  props.currencyConverter.convert = function () {
    return `$${props.inventory[1].price}`;
  };

  const inventoryComponent = shallow(<Inventory {...props} />);
  let tr = inventoryComponent.find("tr");
  expect(tr.length).toEqual(2);

  let td = inventoryComponent.find("td");
  let product = td.at(0);
  let image = td.at(1);
  let desc = td.at(2);
  let price = td.at(3);

  expect(product.text()).toEqual("Flashlight");
  expect(image.html()).toEqual('<td><img src="/flashlight.jpg" alt=""/></td>');
  expect(desc.text()).toEqual("A really great flashlight");
  expect(price.text()).toEqual("$100");
});

it("should have Add to Cart button work", () => {
  const inventoryComponent = shallow(<Inventory {...props} />);
  let addToCartBtn = inventoryComponent.find("button").first();
  addToCartBtn.simulate("click");
  expect(props.addToCart).toBeCalled();
});
```

Bu testler, Alışveriş Sepeti ve Envanter bileşenleri iin bize doğru çalıştıını garantiler:

- Gösterilmesi gereken dataların olduğunu
- Tutarlı bir API olduğunu
- Belirli bir eylem işlevini çağırarak durumu değiştirebilir olduğunu.

## Son düşünceler

Yazılım mimarisi, değiştirilmesi zor olan şeydir, bu nedenle okunabilir, yeniden kullanılabilir ve yeniden yapılandırılabilir bir temele erken yatırım yapın. Sonradan değiştirmek gerçekten zordur. 3 R'yi takip ederek, kullanıcılarınız size teşekkür edecek, geliştiricileriniz size teşekkür edecek ve siz de kendinize teşekkür edeceksiniz.

---

## Geliştirme

Bunu okuduğunuz için teşekkürler! Bu projeyi genişletmek veya katkıda bulunmak istiyorsanız, ihtiyacınız olan her şeyi yüklemek için aşağıdaki komutları çalıştırın:

```
npm install -g create-react-app
npm install
npm run start
```

Bir tarayıcı açın ve şurada çalışan uygulamayı görün: `http://localhost:3000/`

## Katkı

Katkılarınız için teşekkür ederiz!

Dostça bir Çekme İsteği açmadan önce, aşağıdakileri çalıştırdığınızdan ve linter tarafından kaydedilen tüm hataları çözdüğünüzden emin olun:

```
npm run fmt
npm run lint
```

Son olarak, değişikliklerinizi yansıtmak için bu `README.md` deki ilgili kod örneklerini değiştirin.

## Lisans

MIT
