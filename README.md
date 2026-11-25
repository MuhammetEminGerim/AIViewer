# MekaVis — 3B Dijital İkiz & Yapay Zekâ Destekli Görsel Kalite Kontrol

**MekaVis**, endüstriyel üretim hatlarındaki sac ve metal parçaların gerçek zamanlı **3B Vulkan dijital ikiz** ortamında simüle edildiği, üstten bakan sanal kamera (`CAM-01`) görüntülerinin **C# Native ONNX Runtime** üzerinde çalışan yapay zekâ modeliyle analiz edildiği ve hatalı (`NOK`) parçaların kapalı döngü pnömatik ayırma mekanizmasıyla ayrıldığı bir görsel kalite kontrol prototipidir.

> **Durum Notu:** Bu çalışma şu an 3B simülasyon ve fotoğraf testi aşamasında olan bir Ar-Ge prototipidir; doğrudan gerçek üretim hattı sertifikasyonu iddiası taşımaz. Bir sonraki aşama, gerçek fabrika numuneleri ve endüstriyel kamera/aydınlatma koşullarıyla doğrulama yapmaktır.

---

## Arayüz ve Simülasyon Görüntüleri

| Canlı 3B Hat & `PASSED · OK` | Kapalı Döngü Pnömatik Ayırma & `REJECTED · NOK` |
|---|---|
| ![Canlı 3B Hat OK](docs/images/01_live_ok.png) | ![Pnömatik Ayırma NOK](docs/images/02_live_eject.png) |

| `CAM-01 · Canlı Denetim` (Büyüteç & Kusur Kutuları) | `Hat Durumu` (Sinyal Lambası, PLC I/O & Vardiya) |
|---|---|
| ![CAM-01 Canlı Denetim](docs/images/03_cam01_zoom.png) | ![Hat Durumu Paneli](docs/images/04_hat_durumu.png) |

| 4 Kolonlu Progresif Pres & 3B Delikli Sac Besleme | `Ayarlar` (Güven Eşiği, Bant Hızı, AI Model) |
|---|---|
| ![Progresif Pres](docs/images/05_progresif_pres.png) | ![Ayarlar Paneli](docs/images/06_ayarlar.png) |

---

## Sistem Mimarisi (Kapalı Döngü)

```mermaid
flowchart LR
    P["Progresif Pres & Çıkış Oluğu<br/>3B Sac Besleme"] --> W["3B Dijital İkiz & Konveyör<br/>120 Hz Rijit-Cisim Fiziği"]
    W -->|"Optik Sensör Tetiklemesi"| CAM["CAM-01 Ortografik Kamera<br/>Canlı Görüntü Yakalama"]
    CAM -->|"Canlı Kare"| ORT["C# Native ONNX Runtime<br/>Kusur Tespiti (Çizik · Ezik · Delik Kayması)"]
    ORT -->|"OK / NOK Kararı"| UI["Operatör Arayüzü<br/>Canlı Büyüteç · Anlık KPI"]
    ORT -->|"NOK Tetikleme"| EJ["Pnömatik İtici Simülasyonu<br/>Hatalı Parça Ayırma"]
    ORT -->|"Kayıt & Rapor"| DB["SQLite Veritabanı<br/>+ CSV Vardiya Dışa Aktarımı"]
```

---

## Öne Çıkan Teknik Özellikler

- **3B Fizik Tabanlı Hat Simülasyonu:** Godot 4.7 Vulkan Forward+ ve 120 Hz Jolt fizik motoru üzerinde progresif pres, eğimli çıkış oluğu, konveyör bandı, kamera portalı, pnömatik itici ve fire kutusu.
- **Doğrudan C# Üzerinde Yapay Zekâ Çıkarımı:** Harici Python servisi gerektirmeden uygulama içi **ONNX Runtime** ile her parça için milisaniye seviyesinde kusur tespiti (`Çizik`, `Ezik`, `Delik Kayması`).
- **Gerçek Kapalı Döngü Ayırma:** İtici yalnızca yapay zekânın canlı kamera görüntüsünden verdiği `NOK` kararına göre bant hızına uyarlı zamanlamayla tetiklenir.
- **Fotoğraf Testi & Raporlama:** Harici parça fotoğrafları yüklenerek model çıktısı incelenebilir; her parça kararı yerel **SQLite** veritabanına kaydedilir ve tek tıkla **CSV** vardiya raporu alınabilir.

---

## Kaynak Kod ve İletişim

Bu depo **MekaVis** projesinin tanıtım ve dokümantasyon vitrinidir. Uygulama kaynak kodları, 3B sahne varlıkları ve eğitilmiş model ağırlıkları kamuya açık değildir (*Tüm Hakları Saklıdır*).

Canlı demo, teknik geri bildirim veya gerçek üretim parçaları üzerinde ön değerlendirme için LinkedIn üzerinden iletişime geçebilirsiniz:

- **LinkedIn:** [Muhammet Emin Gerim](https://www.linkedin.com/in/megerim/)
