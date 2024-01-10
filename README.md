

                                             Kullanilabilirlik Testi

Giriş: Kullanılabilirlik testinde web sitesindeki chatbot özelliğinin kullanım kolaylığını ve etkinliğini değerlendirdik. Chatbot, yardım sağlamak ve kullanıcı sorgularını yanıtlamak için tasarlanmıştır.

Kullanıcılar: Kullanılabilirlik testi, farklı demografik özellikleri temsil eden çeşitli katılımcı gruplarını içeriyordu. İşe alınan kullanıcılar, çeşitli yaş gruplarından, eğitim geçmişlerinden ve teknolojiyle ilgili deneyim düzeylerinden bireylerden oluşuyordu.

Görevler: Kullanılabilirlik testi, chatbot özelliğinin performansını değerlendirmek için çeşitli görevler içeriyordu. Görevler, sohbet robotuyla sohbet başlatmak, web sitesinin içeriğiyle ilgili sorular sormak ve sohbet robotunun doğru ve yararlı yanıtlar verme yeteneğini değerlendirmek gibi görevleri içeriyordu.

Yöntem: Kullanılabilirlik testi kontrollü bir ortamda internet erişimi olan bir bilgisayar kullanılarak gerçekleştirilmiştir. Katılımcılara sohbet robotuyla nasıl etkileşim kuracakları konusunda net talimatlar verildi ve test başlamadan önce inceleyip imzalamaları için bir onay formu verildi. Katılımcıların etkileşimlerini ve geri bildirimlerini yakalamak için test oturumları kaydedildi.

Sonuçlar:
Görev Tamamlanma Durumu: Kullanılabilirlik testi, katılımcıların atanan görevleri chatbot'u kullanarak tamamlama becerilerini değerlendirdi. Görev tamamlanma durumu, katılımcıların chatbottan istenen bilgiyi başarıyla alıp alamadıklarına göre değerlendirildi.
Hata Oranı: Test ayrıca, test sırasında chatbot tarafından sağlanan yanlış veya yanıltıcı yanıtların sıklığını ifade eden hata oranını da ölçtü. Hata oranı, katılımcıların etkileşimleri analiz edilerek ve beklenen yanıtlar, chatbot tarafından sağlanan gerçek yanıtlarla karşılaştırılarak hesaplandı.

Kullanıcılar Arasında Görev Tamamlama Süresi Dağılımı: Test, her katılımcının atanan görevleri tamamlamak için harcadığı süreyi kaydetti. Görev tamamlama sürelerinin kullanıcılar arasındaki dağılımı, herhangi bir önemli değişiklik veya eğilimi belirlemek için analiz edildi.

Memnuniyet Anketi Sonucu: Kullanılabilirlik testini tamamladıktan sonra katılımcılardan, chatbot ile ilgili genel deneyimlerine ilişkin geri bildirim sağlamak için bir memnuniyet anketi doldurmaları istendi. Anket, kullanıcı memnuniyetini, kullanım kolaylığını ve chatbot'un algılanan kullanışlılığını değerlendirmeyi amaçladı.

Sonuç: Kullanılabilirlik testi sonuçlarına göre chatbot özelliğinde çeşitli sorunlar tespit edildi. Bu sorunlar arasında yüksek hata oranı, tutarsız yanıtlar ve belirli görevler için daha uzun görev tamamlama süreleri yer alıyordu. Memnuniyet anketinden elde edilen geri bildirimler, kullanıcıların chatbotun performansını kullanıcı dostu olma ve etkililik açısından ortalamanın altında bulduğunu da gösterdi. Bu bulgular, kullanılabilirliğini artırmak için chatbotun işlevselliğinde, doğruluğunda ve kullanıcı deneyiminde iyileştirmeler yapılması gerektiğini ortaya koyuyor.

Test Schema 
# Each top level key is a unique name for the test.
test_name:
  # Tests can optionally define these global options to their RiveScript instance
  username: "localuser"  # Override the username; this is the default one.
  utf8: true

  # The tests themselves are broken into a series of "Test Actions": it's an
  # array of tasks to be processed from top to bottom, and each action can
  # stream RiveScript source code (additive), test inputs and outputs and
  # manage user variables. See "Test Actions" below for more details.
  tests:
    # Stream in RiveScript source code to test against. Most tests will start with
    # one of these, and some tests may have multiple source statements that should
    # be streamed over top of the existing brain.
    - source: |
        // RiveScript source is included as a YAML
        // literal-style multi-line string.
        + hello bot
        - Hello human!

        + how are you
        - Good, you?
        - Alright, how are you?

        + my name is *
        - <set name=<formal>>Nice to meet you, <get name>.

        + who am i
        - Your name is <get name>, right?

    # A simple input and output
    - input: "Hello bot!"
      reply: "Hello human!"

    # You can test random outputs too, just use an array for the 'reply'
    - input: "How are you?"
      reply:
        - Good, you?
        - Alright, how are you?

    # Testing setting a user variable...
    - input: "My name is Alice"
      reply: "Nice to meet you, Alice."

    # ...and verify it was set
    - input: "Who am I?"
      reply: "Your name is Alice, right?"

    # ...by asserting the user variable is correct.
    - assert:
        name: "Alice"

    # Or you can set the user variable directly:
    - set:
        name: "Bob"

    # And verify:
    - assert:
        name: "Bob"

    # Another way to verify:
    - input: "Who am I?"
      reply: "Your name is Bob, right?"

    # And if you need to stream in additional code:
    - source: |
        + what is your name
        - I'm just a RiveScript bot.

    - input: "What is your name?"
      reply: "I'm just a RiveScript bot."
      Test Actions
The types of actions that your test runner must support are as follows:

Stream RiveScript source code. Each test should begin with a blank brain, and each stream action should stream its source code on top of the existing brain (updating the reply set it already has).

- source: |
  + hello bot
  - Hello human!
Check an input and reply (or replies):

# A one-to-one mapping
- input: "Hello bot"
  reply: "Hello human"

# A one-to-many mapping, if random replies can be given
- input: "Hello bot"
  reply:
    - "Hello human"
    - "Hi there"
Set a user variable:

# You can set as many variables as you need. Use key/value pairs
# for mapping a user variable to its value.
- set:
    name: "Alice"
    age: "5"
Assert the values of user variables:

# Similar to `set`, provide key/value pairs of variable names and
# their expected values. Multiple variables can be used.
- assert:
    name: "Alice"
    age: "5"


 API KEY ANAHTARININIZ SİZE ÖZELDİR.
 

