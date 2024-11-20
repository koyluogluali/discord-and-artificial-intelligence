

import discord
from discord.ext import commands
from bot_token import bot_token
from keras_code import detected_valorant_agents
import os

# Discord intents ve bot ayarları
intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)

# Bot hazır olduğunda bir mesaj yaz
@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

# Basit bir merhaba komutu
@bot.command()
async def merhaba(ctx):
    await ctx.send(f'selam ben bir botum {bot.user}!')

# "he" komutu
@bot.command()
async def heh(ctx, count_heh=5):
    await ctx.send("he" * count_heh)

# Valorant ajanlarını algılama komutu
@bot.command()
async def detect(ctx):
    if ctx.message.attachments:
        await ctx.send(f'Algılama başladı!')

        for attachment in ctx.message.attachments:
            file_name = attachment.filename
            file_path = f"images/{file_name}"

            # Fotoğraf kaydetme
            await attachment.save(file_path)

            # Dosya formatı kontrolü (sadece resimler)
            if not file_name.endswith(('png', 'jpg', 'jpeg', 'gif')):
                await ctx.send(f"Geçersiz dosya formatı! Lütfen bir resim dosyası yükleyin.")

            # Model ile tahmin yapma
            classname, score = detected_valorant_agents(
            file_path, 
            "valorant-neon-pnc.png",  # Eğer modelin referans dosyası varsa
            "converted_keras/keras_model.h5", 
            "converted_keras/labels.txt"
            )
            await ctx.send(f'Tahmin edilen ajan: {classname} (Güven Skoru: {score:.2f})')
            
            

            # Dosya kaydedildi mesajı
            await ctx.send(f'Dosya kaydedildi ve işleme alındı!')

    else:
        await ctx.send(f'Bu komutla birlikte bir fotoğraf da yüklemelisiniz!')
        if classname == 'neon':
         await ctx.send("""Hız Şeridi yeteneği için oyun içi açıklama şöyledir: Önüne doğru, kısa veya bir yüzeye çarpana kadar ilerleyen iki enerji hattı çıkarmak için ATEŞ ET. Bu enerji hatları, görüşü engelleyen ve içinden geçen düşmanlara hasar veren statik elektrik duvarlarına dönüşür." Hız Şeridi, temel olarak her iki tarafta duvarlar oluşturarak dar bir yol oluşturan bir görüş engelleme yeteneğidir. Düşman görüşünü engellerken, Neon'un gerektiğinde dövüşleri izole etmesine de olanak tanır. Oyuncuların belirli bir alana nişan alması ve yukarıda bahsedilen yolu oluşturacak olan C tuşuna basması gerekir. Oluşturulan duvarlardan girmeye veya çıkmaya çalışan düşmanlara bir miktar hasar verilecektir. Sonuç olarak, Hız Şeridi Neon'un rakiplerini tedirgin eder ve her zaman her yerde çarpışmaya hazır olmalarını ister. Hız Şeridi Elektro Ok ile birlikte kullanarak belirli bölgeleri sersemletmek ve ardından dövüşleri izole etmek için kullanılan bir kombinasyondur. Bu yetenek 300 krediye mal olur ve 1 saniyelik bir etki süresine sahiptir.
    Q - ELEKTRO OK
    VALORANT'ta Elektro Ok yeteneği için oyun içi açıklama şöyledir: "ANINDA bir enerji oku fırlat. Ok bir kere seker. Ok bir yüzeye çarptığında sersemletici bir patlamayla altındaki zemine elektrik akımı verir." Kısaca açıklamak gerekirse, Ajan Neon'un Elektro Ok'u, nasıl ve nereye sıçradığına bağlı olarak aynı anda iki farklı alanı sersemletebilen bir sersemletme yeteneğidir. Yetenek, Q tuşuna basıldığında oyuncunun nişangâhının hedeflendiği yere iner, yüzeyden (varsa) seker ve nereye giderse gitsin başka bir sersemletme uygular. Oyuncular iki farklı alanı veya bir sıkışma noktasını sersemletmeyi seçebilir. Diğer bir kullanım yolu ise aynı alanın etrafında davranışsal olarak büyük bir sersemletmeye dönüştürmek için yeteneği çok kısa mesafeler arasında sektirmektir. Oyuncular, belirli bir açıyı veya rakibi gözetleyerek avantajlı bir dövüş elde etmek için Elektro Ok'u YÜKSEK DEVİR ile ideal bir şekilde birleştirebilirler. Her turda en fazla iki Elektro Ok şarjı satın alınabilir ve her biri 200 krediye mal olur.
    İMZA YETENEĞİ - E - YÜKSEK DEVİR
    VALORANT'taki oyun içi açıklamaya göre, Yüksek Vites "Neon'u hızlandırmak için ANINDA gücünü odakla. Yükü dolduğunda, ALT. ATIŞ'la elektrikli kayma hareketini etkinleştir. Her iki skorda bir kayma hareketinin yükü dolar." Yüksek Vites E tuşuna basılarak etkinleştirilir. Neon oyuncularının gerektiğinde hızlanmasını sağlar. ALT ATEŞ tuşuna basmayı seçtiklerinde ek bir kayma seçeneği mevcuttur. Kayma ile birlikte hız değişikliği, Neon oyuncularının korunan açıları verimli bir şekilde gözetlemesine ve alanlarda çok daha kolay gezinmesine olanak tanır. Koşma ve Kayma yeteneği, iki uçlu olması nedeniyle Zayıflatma öncesi Jett atlaması ile biraz karşılaştırılabilir. Yüksek Devir yeteneği,Viper'da olduğu gibi bir yakıt göstergesine sahiptir ve yalnızca etkinleştirilmediği sürece yenilenir. Kayma seçeneği kullanıldığında, söz konusu turda iki öldürmeden sonra tekrar yenilenir. Neon kullanıcıları, süper kayma efekti oluşturmak için bazı haritalardaki eğimli yüzeylerde kayma yeteneğini de kullanabilir,ULTİ - X - ENERJİ PATLAMASI
    Enerji Patlaması resmi açıklaması şöyle: "Neon'un tüm gücünü ve hızını kısa bir süreliğine etkinleştir. Gücünü, hareket ederken isabetliliği yüksek olan ölümcül bir ışın saldırısına dönüştürmek için ATEŞ ET. Aldığın her skorla birlikte bekleme süresi sıfırlanır." Enerji Patlaması, Neon'un ultisinin doğasını mükemmel bir şekilde açıklar. Bu yetenek 7 Ulti küresine mal olur ve X tuşuna basılarak etkinleştirilebilir. Etkinleştirildiğinde, Neon pasif bir Yüksek Devir artışı kazanırken, aynı zamanda parmakları aracılığıyla, hedeflemeyi başardığı her saniye için düşmanlara hasar veren elektrik ışınları yönlendirir. Basitçe söylemek gerekirse, Neon oyuncuları X tuşuna her bastığında, Neon Yüksek Devir'e geçer ve belirli bir süre boyunca parmak uçlarından hasar verebilir. Bunu, hasar veren bir yeteneği olan Raze'inkiyle karşılaştırılabilecek bir Ultimate yeteneği olarak düşünün, ancak zaman içinde hasar vermek yerine çarpma anında hasar verir. Aynı zamanda bu yeteneği sınırsız kullanmanız mümkün değildir. Bu da etkinleştirme zamanlamasını çok önemli hale getirir.""")

        elif  classname == 'yoru':
            await ctx.send("""C Yeteneği – Sahte (Fakeout) 
    Gerçek bir oyuncunu sahte ayak sesleri çıkararak diğer düşman ajanların dikkatlerini dağıtabilir, ani bir saldırı yaparak takımını öne geçirebilir.
    E Yeteneği - Ağ Çöküşü (Gatecrash)
    İstediğiniz yere doğru hareket etmek için bir portal geçiti açın ve düşmanlarınızın etrafında dolaşın.
    Q Yeteneği - Kör Taraf (Blindside)
    Etrafınızdaki rakip ajanları kör etmek için hareketli flaşı kullanın. Flaş duvara çarptığında aktif olur ve size avantaj sağlar.
    X Yeteneği - Boyutsal Kayma (Dimensional Drift)
    Boyutlar arasında gezebilen bir maske takın ve Yoru'nun boyutunda dolaşın, düşmanlarınızın nerede saklandığını görün takımınıza haber verin. Yeteneğinizi kullandığınızda düşmanlarınızın kurşunları size işlemez.""")
        elif  classname == 'chamber':
            await ctx.send(""" C - Kartvizit
    Düşmanları tarayan bir tuzak yerleştirin. Bir düşman tarama menziline girdiğinde tuzak geri sayıma başlar ve yakalanan düşmanları yavaşlatan bir alan oluşturur.
    Q - Kelle Avcısı
    Ağır bir tabanca kuşanın, sağ tıklayarak nişan alıp alternatif atış moduna geçin.
    E - Buluşma
    iki adet ışınlanma noktası yerleştirin. menzil içerisindeyken tekrardan etkinleştirerek diğer noktaya hızlı ışınlanın. ışınlanma noktaları yerden alınabilir ve tekrar yerleştirilebilir
    X - Keskin Tarz
    Kullandığınızda rakiplerinizi tek vuruşta öldüren güçlü bir keskin nişancı tüfeği çıkartın. Bir düşmanı bu silahla öldürdüğünüzde etrafındaki diğer düşmanları yavaşlatan bir alan oluşturur.
    Valorant yeni 18. ajanı Chamber, 3.10 yama notları ile 16 Kasım’da tanıtılacak. Bölüm 3 Kısım 3 Savaş Bileti ise 2 Kasım’da geliyor. Daha fazla Valorant haberi için takipte kalın!""")
        elif  classname == 'cypher':
            await ctx.send("""Tuzak Teli, yüzeyde oldukça basit bir yetenek. İki duvar arasına kurabileceğiniz bir teldir. Ne yazık ki, içinden geçen düşmanlar komedi şakaları yapmaz. Bunun yerine, kısıtlanırlar ve oldukça kolay bir hedefe dönüşürler. Bu, tek bir düşmanı kendi yollarında durdurmak için oldukça etkilidir.

    Düşmanlar tuzağı yok edebilir, bu yüzden kolay bir öldürme sağlayacağını varsayarak onu öylece ortalıkta bırakamazsınız. Ancak tuzak yok edilse bile düşmanların nerede olduğuna dair iyi bir fikir edinirsiniz.
    Bir tuzak teli kanatlarınızı korumak için harika bir cihazdır.""")
# Botu başlatma
bot.run(bot_token)
