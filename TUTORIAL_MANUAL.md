# 📖 Panduan Lengkap: Klaim Manual Google AI Pro 5 TB (2 Tahun / 24 Bulan - US$0.00)

Tutorial ini disusun berdasarkan alur kerja dan logika `automation_engine.py`. Anda dapat mengeksekusi seluruh trik ini **secara manual** menggunakan browser Google Chrome biasa dan **Developer Tools (Console)** tanpa perlu menjalankan skrip otomatisasi Python.

---

## 📌 Mengapa Butuh DevTools Console?

Promo 2 tahun (24 bulan) ini didapatkan dengan teknik **stacking 3 lapis promo resmi Google One**:
1. **Layer 1:** Promo 12 Bulan (1 Year Free Voucher) + YouTube Premium 1M Add-on.
2. **Layer 2:** Promo 6 Bulan (Samsung Galaxy S25 Promo Token).
3. **Layer 3:** Promo 6 Bulan (Google Pixel Promo Token).

Karena promo-promo di atas tidak tampil bersamaan di menu paket standar Google One, kita menginjeksi **Offer Token** langsung ke request RPC (`Ngbm`) dan iframe checkout Google Play (`eacquire`) menggunakan JavaScript Hook di tab Console.

---

## 🛠️ Prasyarat Sebelum Memulai

1. **Browser:** Google Chrome (versi desktop).
2. **Jaringan (Wajib US IP):**
   - Gunakan VPN atau Proxy lokasi **Amerika Serikat (US)**.
   - Sangat disarankan memilih server negara bagian **Oregon** (Portland).
   - Cek IP di [whoer.net](https://whoer.net) atau [ipinfo.io](https://ipinfo.io) untuk memastikan lokasi sudah US.
3. **Kartu Pembayaran (Visa / Mastercard):**
   - Kartu Debit/Kredit/VCC internasional yang aktif.
   - Saldo minimal aman: Rp 0 - Rp 15.000 (hanya untuk verifikasi $0 atau $1 temporary hold jika kartu baru pertama kali ditambahkan ke Google Pay).
   - **Alamat Penagihan (Billing Address Bebas Pajak / Tax-Free):**
     * **Street:** `700 SW 5th Ave`
     * **City:** `Portland`
     * **State:** `OR` (Oregon)
     * **ZIP Code:** `97201`
     *(Menggunakan ZIP Oregon wajib agar nominal pajak tetap $0.00).*

---

## 🧩 Script Injeksi DevTools (Core Hook)

Setiap kali Anda berada di halaman `https://one.google.com/plans`, Anda perlu menempelkan (*paste*) kode **Core Hook** ini sekali di Console sebelum memasukkan konfigurasi layer:

```javascript
(() => {
  if (window.__HOOK_INSTALLED) return;
  window.__HOOK_INSTALLED = true;
  let currentTest = window.__INIT_TEST || null;
  window.__setDirectTest = (t) => { currentTest = t; };

  function patchTargetNode(node) {
    if (!Array.isArray(node) || !Array.isArray(currentTest?.full_items)) return false;
    let patched = false;
    if (node.length > 12 && Array.isArray(node[12])) {
      if (Array.isArray(node[7]) && Array.isArray(node[7][5]) && node[7][5].length > 0) {
        window.__capturedSubId = node[7][5][0];
      }
      node[12] = currentTest.full_items;
      const targetMode = currentTest.mode || (Array.isArray(currentTest.ear8) ? currentTest.ear8[1] : 5) || 5;
      if (currentTest.ear8) {
        node[7] = currentTest.ear8;
      } else if (Array.isArray(node[7]) && Array.isArray(node[7][5]) && node[7][5].length > 0) {
        node[7][1] = targetMode;
      } else if (window.__capturedSubId) {
        node[7] = [null, targetMode, null, null, null, [window.__capturedSubId]];
      }
      patched = true;
    }
    if (node.length > 10 && Array.isArray(node[10]) && Array.isArray(node[10][0])) {
      node[10] = [currentTest.full_items];
      if (Array.isArray(node[2]) && Array.isArray(node[2][2]) && Array.isArray(node[2][2][8])) {
        node[2][2][8] = [currentTest.full_items];
      }
      if (currentTest.ear8) {
        node[6] = currentTest.ear8;
      }
      patched = true;
    }
    return patched;
  }

  function rewriteFreq(bodyStr) {
    if (typeof bodyStr !== 'string' || !bodyStr.includes('f.req=') || !currentTest) return bodyStr;
    try {
      const parts = bodyStr.split('&');
      for (let i = 0; i < parts.length; i++) {
        if (parts[i].startsWith('f.req=')) {
          const raw = decodeURIComponent(parts[i].slice(6).replace(/\\+/g, ' '));
          const outer = JSON.parse(raw);
          let changed = false;
          if (Array.isArray(outer)) {
            for (const group of outer) {
              if (!Array.isArray(group)) continue;
              for (const call of group) {
                if (Array.isArray(call) && call[0] === 'Ngbm' && typeof call[1] === 'string') {
                  const payload = JSON.parse(call[1]);
                  if (Array.isArray(payload)) {
                    if (patchTargetNode(payload[0])) changed = true;
                    if (patchTargetNode(payload[1])) changed = true;
                  }
                  if (changed) {
                    call[1] = JSON.stringify(payload);
                  }
                }
              }
            }
          }
          if (changed) {
            parts[i] = 'f.req=' + encodeURIComponent(JSON.stringify(outer));
            return parts.join('&');
          }
        }
      }
    } catch (e) {}
    return bodyStr;
  }

  const origSend = XMLHttpRequest.prototype.send;
  XMLHttpRequest.prototype.send = function(body) {
    if (typeof body === 'string' && body.includes('Ngbm')) {
      body = rewriteFreq(body);
    }
    return origSend.call(this, body);
  };

  function rewriteUrl(val) {
    const s = String(val || '');
    if (!s.includes('/eacquire/multiline') || !currentTest) return val;
    try {
      const u = new URL(s, location.origin);
      const earStr = u.searchParams.get('ear');
      if (!earStr) return val;
      const ear = JSON.parse(earStr);
      if (Array.isArray(ear[8]) && Array.isArray(ear[8][5]) && ear[8][5].length > 0) {
        window.__capturedSubId = ear[8][5][0];
      }
      if (Array.isArray(ear[13]) && Array.isArray(currentTest.full_items)) {
        ear[13][0] = currentTest.full_items;
      }
      if (Array.isArray(ear[4]) && typeof ear[4][0] === 'string' && Array.isArray(currentTest.full_items)) {
        try {
          const inner4 = JSON.parse(atob(ear[4][0]));
          if (Array.isArray(inner4) && inner4.length > 8) {
            inner4[8] = [currentTest.full_items];
            ear[4][0] = btoa(JSON.stringify(inner4));
          }
        } catch (e4) {}
      }
      const targetMode = currentTest.mode || (Array.isArray(currentTest.ear8) ? currentTest.ear8[1] : 5) || 5;
      if (currentTest.ear8) {
        ear[8] = currentTest.ear8;
      } else if (Array.isArray(ear[8]) && Array.isArray(ear[8][5]) && ear[8][5].length > 0) {
        ear[8][1] = targetMode;
      } else if (window.__capturedSubId) {
        ear[8] = [null, targetMode, null, null, null, [window.__capturedSubId]];
      }
      ear[24] = null;
      u.searchParams.set('ear', JSON.stringify(ear));
      window.__capturedEarUrl = u.toString();
      return u.toString();
    } catch (e) {
      return val;
    }
  }

  if (window.TrustedTypePolicy && TrustedTypePolicy.prototype.createScriptURL) {
    const origCreate = TrustedTypePolicy.prototype.createScriptURL;
    TrustedTypePolicy.prototype.createScriptURL = function(input, ...args) {
      return origCreate.call(this, rewriteUrl(input), ...args);
    };
  }
  const frameProto = HTMLIFrameElement.prototype;
  const srcDesc = Object.getOwnPropertyDescriptor(frameProto, 'src');
  if (srcDesc?.set) {
    Object.defineProperty(frameProto, 'src', {
      ...srcDesc,
      set(v) {
        if (typeof v === 'string') v = rewriteUrl(v);
        return srcDesc.set.call(this, v);
      }
    });
  }
  const origSetAttr = Element.prototype.setAttribute;
  Element.prototype.setAttribute = function(name, val) {
    if (this instanceof HTMLIFrameElement && String(name).toLowerCase() === 'src') {
      val = rewriteUrl(String(val));
    }
    return origSetAttr.call(this, name, val);
  };
  console.log("✅ Core Hook Google One berhasil dipasang!");
})();
```

---

## 🚀 Langkah demi Langkah Eksekusi Manual

### 🔹 LANGKAH 1: Login & Siapkan Pembayaran Bebas Pajak
1. Aktifkan **VPN / Proxy US** di PC Anda.
2. Buka Google Chrome dan login ke akun Google yang ingin Anda klaim.
3. Kunjungi **Google Pay / Payment Methods**:
   [https://pay.google.com/gp/w/u/0/home/paymentmethods](https://pay.google.com/gp/w/u/0/home/paymentmethods)
4. Tambahkan kartu Anda (Visa/Mastercard) dan isi alamat penagihan:
   * **Country/Region:** United States (US)
   * **Address:** `700 SW 5th Ave`
   * **City:** `Portland`
   * **State:** `OR - Oregon`
   * **ZIP Code:** `97201`

---

### 🔹 LANGKAH 2: Layer 1 - Klaim 12 Bulan Pertama (US$0.00)
1. Buka halaman paket Google One:
   👉 **`https://one.google.com/plans?hl=en`**
2. Tekan tombol **F12** (atau klik kanan -> *Inspect*), lalu buka tab **Console**.
3. Salin dan tempelkan **Script Core Hook** di atas ke Console, lalu tekan **Enter**.
4. Selanjutnya, salin dan tempelkan perintah **Konfigurasi Layer 1** berikut ke Console, lalu tekan **Enter**:

```javascript
window.__setDirectTest({
  name: "Step2_Layer1_12M_YTFree_ZeroCost",
  full_items: [
    [["subs:com.google.android.apps.subscriptions.red:g1.2tb.ai.subs_variant", 21], null, null, null, 1, ["AVbCH/cspB8VdqKJ+ax/uPxUFAVuYRPX4iSjtg4Fu0PG9zUeURSM7y//LlQva1dCd/rbKVM6EJmyiG/C9BoQ95pMl8yDCHuiwCvM5CuGJME="]],
    [["subs:com.google.android.apps.subscriptions.red:g1.youtube_premium_individual_add_on", 21], null, null, null, 1, ["AVbCH/eW4Nfp8KBl0zlm7x9PhaiCvpnqwUuL2D8NFPBQgmM9+HuYS1h2xH5lLWU/BsBj775LK5GqK8UHKqoSzhDDz+PqP3paNesqLU0r9i6XCrYa"]]
  ]
});
console.log("🎯 Layer 1 Siap!");
```

5. Sekarang di halaman web Google One, klik tombol **"Get offer"**, **"Upgrade"**, atau tombol paket AI Pro paling kanan.
6. **Jendela Checkout Google Play akan muncul:**
   * ⚠️ **PENTING:** Pastikan total tagihan tertera **$0.00** (Free trial for 12 months).
   * Pilih kartu yang sudah Anda tambahkan.
   * Klik tombol **Subscribe** (Berlangganan).
7. Tunggu sekitar **45 detik** agar sistem Google Play selesai memproses paket pertama di database.

---

### 🔹 LANGKAH 3: Layer 2 - Tumpuk Promo 6 Bulan S25 (Total 18 Bulan - US$0.00)
1. Di tab yang sama, buka kembali / segarkan (*refresh*) halaman:
   👉 **`https://one.google.com/plans?hl=en`**
2. Buka **Console (F12)** kembali.
3. Tempelkan kembali **Script Core Hook** di atas (jika halaman di-refresh), tekan **Enter**.
4. Tempelkan perintah **Konfigurasi Layer 2** berikut, lalu tekan **Enter**:

```javascript
window.__setDirectTest({
  name: "Step3_Layer2_6MSS_YTFree_ZeroCost",
  full_items: [
    [["subs:com.google.android.apps.subscriptions.red:g1.2tb.ai.subs_variant", 21], null, null, null, 1, ["AVbCH/cgl4zvPT2Lqw95Gn+RYAJIVwFwi783i6uVprX7u3MCa5J4CcTXH5RABa5WjBrUoQnwf8vI10TNV7ktd26myIdxqIwGmNoJ5ofYNqpQ10RKoSvHs75wsfA="]],
    [["subs:com.google.android.apps.subscriptions.red:g1.youtube_premium_individual_add_on", 21], null, null, null, 1, ["AVbCH/eW4Nfp8KBl0zlm7x9PhaiCvpnqwUuL2D8NFPBQgmM9+HuYS1h2xH5lLWU/BsBj775LK5GqK8UHKqoSzhDDz+PqP3paNesqLU0r9i6XCrYa"]]
  ]
});
console.log("🎯 Layer 2 Siap!");
```

5. Klik tombol paket promo / "Upgrade" / "Change plan" di halaman web.
6. Popup checkout Google Play akan muncul kembali:
   * Periksa: Total biaya harus tetap **$0.00** (masa berlaku akan diperpanjang +6 bulan).
   * Klik **Subscribe**.
7. Tunggu kembali **45 detik**.

---

### 🔹 LANGKAH 4: Layer 3 - Tumpuk Promo 6 Bulan Pixel (Total 24 Bulan - US$0.00)
1. Buka kembali halaman:
   👉 **`https://one.google.com/plans?hl=en`**
2. Buka **Console (F12)**.
3. Tempelkan **Script Core Hook**, tekan **Enter**.
4. Tempelkan perintah **Konfigurasi Layer 3** berikut, lalu tekan **Enter**:

```javascript
window.__setDirectTest({
  name: "Step4_Layer3_6M_YTFree_ZeroCost",
  full_items: [
    [["subs:com.google.android.apps.subscriptions.red:g1.2tb.ai.subs_variant", 21], null, null, null, 1, ["AVbCH/fUON3w8hpfeutprA3BMQwrBwYIB6Y8CSWi1isDCDswKUUhGQ3dluj5WXsNKOvr8K5UULBGrRsf4KlW1rHLz0fF3SvxQhBFRbBOHCA="]],
    [["subs:com.google.android.apps.subscriptions.red:g1.youtube_premium_individual_add_on", 21], null, null, null, 1, ["AVbCH/eW4Nfp8KBl0zlm7x9PhaiCvpnqwUuL2D8NFPBQgmM9+HuYS1h2xH5lLWU/BsBj775LK5GqK8UHKqoSzhDDz+PqP3paNesqLU0r9i6XCrYa"]]
  ]
});
console.log("🎯 Layer 3 Siap!");
```
*(Catatan: Jika token ini tidak memicu tombol, gunakan token alternatif `AVbCH/cJ5SeHgkVRt+bdcrapx/aLSu2kbsBgMdI413zbllDNsNQoC/zVlG/Npbu55WlMzBwWPhUedIvZFDg7fw0CmCv/8DKj04IZ21pK7mrV/hh5`).*

5. Klik tombol promo / "Upgrade" di halaman web.
6. Popup checkout Google Play akan muncul dengan harga **$0.00**.
7. Klik **Subscribe**.
8. Tunggu **40 detik**.

---

### 🔹 LANGKAH 5: Verifikasi & Batalkan Perpanjangan Otomatis (Lock 2028)
Agar Anda tidak pernah ditagih secara otomatis setelah masa 2 tahun berakhir:

1. Buka halaman langganan Google Play:
   👉 **`https://play.google.com/store/account/subscriptions`**
2. Anda akan melihat langganan **Google One (Google AI Pro 5 TB)**.
3. Cek tanggal perpanjangan / jatuh tempo berikutnya:
   * Tanggal harus menunjukkan tahun **2028** (misalnya: *October 2028* atau *November 2028*).
4. Klik tombol **Manage (Kelola)** pada langganan Google One.
5. Klik **Cancel subscription (Batalkan langganan)**.
6. Jika muncul penawaran retensi ("Stay subscribed"), klik **No thanks**.
7. Pilih salah satu alasan pembatalan (misal: *Decline to answer* / Menolak menjawab), lalu klik **Continue**.
8. Konfirmasi pembatalan.

✅ **Selesai!**  
Status langganan Anda kini berstatus *"Canceled"* (Dibatalkan), namun **hak akses Google AI Pro 5 TB + Gemini Advanced tetap aktif penuh sampai akhir tahun 2028 tanpa biaya sepeser pun ($0.00)**.

---

## ⚠️ Troubleshooting & Catatan Penting

| Masalah | Solusi |
| :--- | :--- |
| **Muncul harga normal (misal $19.99)** | **JANGAN KLIK SUBSCRIBE!** Pastikan IP benar-benar US, pastikan Hook terpasang sebelum tombol diklik, dan pastikan ZIP code kartu adalah `97201` (Oregon). |
| **Popup Checkout tidak muncul** | Refresh halaman `one.google.com/plans?hl=en`, pasang ulang Core Hook di Console, set layer config, lalu klik tombol penawaran paling kanan. |
| **Error: Something went wrong** | Akun Google Anda mungkin terkena flagging risiko/fraud dari Google karena IP proxy kotor atau aktivitas mencurigakan. Gunakan residential proxy US yang bersih. |
| **Google minta verifikasi identitas / OTP kartu** | Masukkan OTP SMS / konfirmasi transaksi dari bank Anda. Transaksi tetap senilai $0.00. |

