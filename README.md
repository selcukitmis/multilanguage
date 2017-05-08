# Asp.net Mvc ile Multi Language - Globalization and Localization kullanımı

Bu örnek çoklu dilli web siteleri geliştirmek amaçlı yapılmıştır.

Çok dilli web siteleri geliştirmek için yapmanız gerekenleri aşağıdadır

Çok dilli uygulamaları Resource dosyaları ile sağlanmaktadır ve sadece siteyi görüntüleyen kişinin bilgisayarında sağlanır. 
Multi language için yapacağımız örnek sadece arayüzler için geçerli olacaktır. 

Dolayısıyla hangi dilin görüntülenmesi gerektiği bilgisini cookie den okuyup **System.Threading.Thread.CurrentThread.CurrentCulture** bölümüne seçili dili set edeceğiz. 

Görüntülenen verilerin de farklı dillerde olmasını istiyorsanız veri tabanı yapınıza buna göre konfigüre etmeniz gerekmektedir. 
Örneğimiz üç farklı dil (Türkçe, İngilizce, Almanca) destekleyecektir. 

Üç farklı dilin çalışabilmesi için projemize 4 adet Resource dosyası ekleyeceğiz.

* Example.resx (Default Dil)

* Example.tr-TR.resx (Türkçe)

* Example.en-US.resx (İngilizce)

* Example.de-DE.resx (Almanca)

Bunun sebebi; uygulamamız ilk çalıştığında, herhangi bir dil seçilmediğinde görüntülenecek olan "Default" dilin görüntülenmesi içindir.
Resource dosyalarının çalışma prensibi Key - Value mantığındadır. 

Her kelime / cümle için bir anahtar belirlenir ve bu anahtara karşılık gelecek anlam yazılır.

Resource Dosyalarını ekleyiniz. 

Resource dosyalarının isimleri aynı olmalıdır. Örneğin; Example.resx. Bu dosya default dil için oluşturulur. 

Sonuna ekleyeceğiniz uzantı ile hangi dilde çalışacağını belirtmiş oluyoruz.

Resource dosyalarının içine giriniz ve üst bölümde bulunan Access Modifier bölümünü Public olarak işaretleyiniz

Her resource dosyalarının içinde sol üst bölümde bulunan Add Resource düğmesine tıklayarak yeni bir name-value oluşturunuz. .

* Example.resx dosyasında; Name: HelloWorld, Value: Merhaba Dünya

* Example.tr-TR.resx dosyasında; Name: HelloWorld, Value: Merhaba Dünya

* Example.en-US.resx dosyasında; Name: HelloWorld, Value: Hello World

* Example.de-DE.resx dosyasında; Name: HelloWorld, Value: Hallo Welt

Web.config dosyası içerisindeki System.Web böümünün içerisine `<globalization uiCulture='auto' culture='auto' />` etiketini ekleyin.

Global.asax dosyasına

`protected void Application_BeginRequest(object sender, EventArgs e)`

methodunu ekleyin ve aşağıdaki kodları bu methodun içerisine yapıştırın 

`var lang = "tr-TR"; // Default dil`

`var cookie = Request.Cookies["MultiLanguageExample"];`

`if (cookie != null && cookie.Value != null)`

`lang = cookie.Value;`

`System.Threading.Thread.CurrentThread.CurrentUICulture = System.Globalization.CultureInfo.GetCultureInfo(lang);`

`System.Threading.Thread.CurrentThread.CurrentCulture = System.Globalization.CultureInfo.CreateSpecificCulture(lang);`

Web Sayfanız içerisinde hangi anahtarın görüntülenmesini istiyorsanız bunu belirtin.
Örneğin; @Example.HelloWorld
Farklı bir dil görüntülemek için /Home/ChangeLanguage actionresult bölümünü call edeceğiz. 

Parametre olarak görüntülemek istediğimiz dilin uzantısını göndermemiz gerekiyor.

ChangeLanguage ActionResult bölümünün içerisindeki kodlar aşağıdaki gibi olmalıdır.

`HttpCookie cookie;`

`cookie = new HttpCookie("MultiLanguageExample");`

`Thread.CurrentThread.CurrentCulture =new CultureInfo(language);`

`cookie.Value = language;`

`Response.SetCookie(cookie);`

`if (Request.UrlReferrer != null) return Redirect(Request.UrlReferrer.LocalPath);`

`return Redirect("/Home/Index");`
