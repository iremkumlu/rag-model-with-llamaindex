# RAG Model with LLamaIndex

## Proje Açıklaması

Bu proje, LLamaIndex kütüphanesini kullanarak Retrieval-Augmented Generation (RAG) modelini geliştirmeyi amaçlamaktadır. Model, Groq API üzerinden LLama 3.3-70B Versatile modeliyle çalışarak sorgulara metin tabanlı yanıtlar üretir. Yanıtların doğruluğu, Cosine Similarity metriği ile değerlendirilmiştir. Bu proje, Türkçe belgeler üzerinde RAG modelinin nasıl optimize edilebileceğini ve farklı parametrelerin model performansı üzerindeki etkilerini keşfetmektedir.

## Kullanılan Teknolojiler

- **LLM (Language Model)**: Groq API - LLama 3.3-70B Versatile
- **Embedding Modeli**: Data-Lab/multilingual-e5-large-instruct-embedder-tgd
- **Veritabanı**: ChromaDB
- **Chunking**: SentenceSplitter (chunk_size=1024, chunk_overlap=128)
- **Retrieval Parametreleri**: Top_k ve Similarity Cutoff
- **Değerlendirme**: Cosine Similarity
- **Kullanıcı Değerlendirmesi**: Modelin ürettiği yanıtlar gerçek cevaplarla karşılaştırılarak değerlendirilmiştir.

## Kullanılan Veri

Bu projede "yapayz.pdf" adlı Türkçe bir belge kullanılmıştır. Bu belge, modelin eğitimi ve test edilmesi için kullanılan ana veri kaynağını oluşturur. Belge, yapay zekânın gelişimi, üretken yapay zekâ ve ayırt edici yapay zekâ arasındaki farkları ele alır.

### Belge İçeriği

**yapayz.pdf** belgesi, yapay zekânın çeşitli alanlarda nasıl şekillendiğini ve insanlık kapasitesini nasıl yeniden keşfettiğini vurgular. Ayrıca, üretken yapay zekâ ve ayırt edici yapay zekâ arasındaki farkları açıklayarak bu teknolojilerin gelecekteki etkilerini tartışmaktadır.

## Model Geliştirme Süreci

### Veri İşleme ve Chunking

Veriler, `SentenceSplitter` kullanılarak belirlenen `chunk_size` ve `chunk_overlap` parametreleri ile bölünmüştür. Farklı boyutlardaki chunk'ların modelin yanıt kalitesi üzerindeki etkileri test edilmiştir.

### Vektör Veritabanı ve Indexleme

Veriler, `Data-Lab/multilingual-e5-large-instruct-embedder-tgd` embedding modeli ile vektörlere dönüştürülmüş ve ChromaDB üzerinde saklanmıştır.

### Sorgu ve Yanıt Üretme

Groq API üzerinden LLama 3.3-70B Versatile modeli kullanılarak sorgulara yanıt üretilmiş ve retrieval sürecinde `top_k` ve `similarity_cutoff` parametreleri ile en uygun yanıtlar belirlenmiştir.

## Parametre Testleri ve Sonuçlar

Farklı parametreler ile testler gerçekleştirilmiş ve model yanıtları Cosine Similarity metriği ile değerlendirilmiştir. Aşağıdaki tablo, test edilen parametreler ve elde edilen benzerlik skorlarını özetlemektedir:

| Chunk Size | Chunk Overlap | Top_k | Similarity Cutoff | Temperature | Cosine Similarity |
| ---------- | ------------- | ----- | ----------------- | ----------- | ----------------- |
| 1024       | 128           | 5     | 0.95              | 0.7         | 0.7190            |
| 2048       | 256           | 10    | 0.75              | 0.8         | 0.7200            |
| 512        | 64            | 15    | 0.85              | 1.0         | 0.6702            |

## Başarı Sonucu: Rate Limit Öncesi En Yüksek Cosine Similarity Skoru

Rate limit hatası almadan önce yapılan testler sonucunda elde edilen en yüksek Cosine Similarity skoru 0.86 olmuştur. Bu skor, `chunk_size=1024`, `chunk_overlap=128`, `top_k=5`, `temperature=0.7`, ve `similarity_cutoff=0.90` parametreleri ile elde edilmiştir. Bu yüksek performanslı skor, modelin parametrelerin optimize edilmesiyle elde edilebilecek maksimum doğruluk seviyesinin bir göstergesidir.

## Karşılaşılan Sorunlar ve Çözümler

### Groq API Rate Limit Problemi

Groq API kullanımı sırasında belirli bir süre sonra `rate limit error` hatası alınmıştır. Bu durum, modelin bazı verileri eksik işlemesine ve Cosine Similarity hesaplamasının objektifliğini etkileyebilecek bir duruma yol açmıştır. Bu sorunun çözülmesi için API çağrıları arasına bekleme süresi eklemek, sorguları batch olarak işlemek veya daha düşük `top_k` ve `chunk_size` değerleri kullanarak API limitini daha verimli kullanmak gibi önlemler önerilmiştir.

## Sonuç ve Değerlendirme

Bu çalışma, Retrieval-Augmented Generation (RAG) modelinin farklı parametre kombinasyonları ile nasıl optimize edilebileceğini ve çeşitli parametrelerin model performansı üzerindeki etkilerini değerlendirmiştir. Elde edilen bulgular, benzer çalışmalar için bir rehber niteliği taşımakta olup, gelecekteki optimizasyon süreçlerinde faydalı olabilecek önemli çıkarımlar sunmaktadır.

## Kurulum ve Çalıştırma

### Gerekli Kütüphaneler

Bu projeyi çalıştırabilmek için aşağıdaki Python kütüphanelerine ihtiyacınız olacaktır:

- `llama_index`
- `groq_api`
- `chromadb`
- `numpy`
- `scikit-learn`

### Kurulum Adımları

1. Gereksinimleri yüklemek için aşağıdaki komutu çalıştırın:

   ```bash
   pip install -r requirements.txt
   ```
