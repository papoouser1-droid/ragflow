# Documentation: web/src/locales/id.ts

## File Metadata

- **Path**: `web/src/locales/id.ts`
- **Size**: 56794 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/id.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      indonesia: 'Indonesia',
      delete: 'Hapus',
      deleteModalTitle: 'Yakin untuk menghapus item ini?',
      ok: 'Ya',
      cancel: 'Tidak',
      total: 'Total',
      rename: 'Ubah nama',
      name: 'Nama',
      save: 'Simpan',
      namePlaceholder: 'Silakan masukkan nama',
      next: 'Lanjutkan',
      create: 'Buat',
      edit: 'Ubah',
      upload: 'Unggah',
      english: 'Inggris',
      portugueseBr: 'Portugis (Brasil)',
      chinese: 'Cina',
      traditionalChinese: 'Cina Tradisional',
      language: 'Bahasa',
      languageMessage: 'Silakan masukkan bahasa Anda!',
      languagePlaceholder: 'Pilih bahasa Anda',
      copy: 'Salin',
      copied: 'Disalin',
      comingSoon: 'Segera Hadir',
      download: 'Unduh',
      gdriveConnect: 'Sambungkan ke Google Drive',
      downloading: 'Mengunduh File',
      downloadFailed: 'Unduh Gagal',
      downloaded: 'File Terunduh',
      close: 'Tutup',
      preview: 'Pratinjau',
      move: 'Pindahkan',
      warn: 'Peringatan',
    },
    login: {
      login: 'Masuk',
      signUp: 'Daftar',
      loginDescription: 'Kami sangat senang melihat Anda kembali!',
      registerDescription: 'Senang memiliki Anda di sini!',
      emailLabel: 'Email',
      emailPlaceholder: 'Silakan masukkan email',
      passwordLabel: 'Kata Sandi',
      passwordPlaceholder: 'Silakan masukkan kata sandi',
      rememberMe: 'Ingat saya',
      signInTip: 'Belum punya akun?',
      signUpTip: 'Sudah punya akun?',
      nicknameLabel: 'Nama Panggilan',
      nicknamePlaceholder: 'Silakan masukkan nama panggilan',
      register: 'Buat akun',
      continue: 'Lanjutkan',
      title: 'Mulai membangun asisten pintar Anda.',
      description:
        'Daftar gratis untuk menjelajahi teknologi RAG teratas. Buat basis pengetahuan dan AI untuk memberdayakan bisnis Anda.',
      review: 'dari 500+ ulasan',
    },
    header: {
      knowledgeBase: 'Basis Pengetahuan',
      chat: 'Obrolan',
      register: 'Daftar',
      signin: 'Masuk',
      home: 'Beranda',
      setting: 'Pengaturan Pengguna',
      logout: 'Keluar',
      fileManager: 'Manajemen File',
      cloud: 'Unggahan Cloud',
      flow: 'Agen',
      search: 'Cari',
    },
    knowledgeList: {
      welcome: 'Selamat datang kembali',
      description: 'Basis pengetahuan mana yang akan kita gunakan hari ini?',
      createKnowledgeBase: 'Buat basis pengetahuan',
      name: 'Nama',
      namePlaceholder: 'Silakan masukkan nama!',
      doc: 'Dokumen',
      searchKnowledgePlaceholder: 'Cari',
    },
    knowledgeDetails: {
      dataset: 'Dataset',
      testing: 'Pengujian pengambilan',
      files: 'file',
      configuration: 'Konfigurasi',
      name: 'Nama',
      namePlaceholder: 'Silakan masukkan nama!',
      doc: 'Dokumen',
      datasetDescription:
        '😉 Pertanyaan dan jawaban hanya dapat dijawab setelah parsing berhasil. Perhatikan bahwa perubahan yang dilakukan di sini tidak akan disimpan secara otomatis. Jika Anda menyesuaikan pengaturan default di sini, seperti bobot kesamaan kata kunci, pastikan Anda memperbarui pengaturan terkait secara sinkron di pengaturan asisten obrolan atau pengaturan operator pemanggilan kembali.',
      addFile: 'Tambah file',
      searchFiles: 'Cari file Anda',
      localFiles: 'File lokal',
      emptyFiles: 'Buat file kosong',
      webCrawl: 'Penjelajahan Web',
      chunkNumber: 'Jumlah Potongan',
      uploadDate: 'Tanggal Unggah',
      chunkMethod: 'Metode Potongan',
      enabled: 'Aktifkan',
      disabled: 'Nonaktifkan',
      action: 'Aksi',
      parsingStatus: 'Status Parsing',
      parsingStatusTip:
        'Waktu pemrosesan dokumen bervariasi tergantung beberapa faktor. Mengaktifkan fitur seperti Knowledge Graph, RAPTOR, Ekstraksi Pertanyaan Otomatis, atau Ekstraksi Kata Kunci Otomatis akan secara signifikan menambah waktu pemrosesan. Jika bilah kemajuan macet, silakan lihat dua FAQ berikut: https://ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent.',
      processBeginAt: 'Proses Dimulai Pada',
      processDuration: 'Durasi Proses',
      progressMsg: 'Pesan Kemajuan',
      testingDescription:
        'Lakukan tes pengambilan untuk memeriksa apakah RAGFlow dapat memulihkan konten yang diinginkan untuk LLM. ',
      similarityThreshold: 'Similarity threshold',
      similarityThresholdTip:
        'Kami menggunakan skor kesamaan hibrida untuk mengevaluasi jarak antara dua baris teks. Ini adalah kesamaan kata kunci berbobot dan kesamaan kosinus vektor. Jika kesamaan antara kueri dan potongan kurang dari ambang ini, potongan akan disaring. Secara default, ambang batas diatur ke 0,2. Itu berarti hanya potongan dengan skor kemiripan hibrida 20 atau lebih tinggi yang akan diambil.',
      vectorSimilarityWeight: 'Bobot kesamaan kata kunci',
      vectorSimilarityWeightTip:
        'Kami menggunakan skor kesamaan hibrida untuk mengevaluasi jarak antara dua baris teks. Ini adalah kesamaan kata kunci berbobot dan kesamaan kosinus vektor atau skor rerank (0~1). Jumlah dari kedua bobot adalah 1.0.',
      testText: 'Teks uji',
      testTextPlaceholder: 'Silakan masukkan pertanyaan Anda!',
      testingLabel: 'Pengujian',
      similarity: 'Kesamaan Hibrida',
      termSimilarity: 'Kesamaan Istilah',
      vectorSimilarity: 'Kesamaan Vektor',
      hits: 'Hits',
      view: 'Lihat',
      filesSelected: 'File Terpilih',
      upload: 'Unggah',
      run: 'Menguraikan',
      runningStatus0: 'Belum Terselesaikan',
      runningStatus1: 'Parsing',
      runningStatus2: 'BATAL',
      runningStatus3: 'SUKSES',
      runningStatus4: 'GAGAL',
      pageRanges: 'Rentang Halaman',
      pageRangesTip:
        'rentang halaman: Tentukan rentang halaman yang perlu diparsing. Halaman yang tidak termasuk dalam rentang ini akan diabaikan.',
      fromPlaceholder: 'dari',
      fromMessage: 'Nomor halaman awal hilang',
      toPlaceholder: 'ke',
      toMessage: 'Nomor halaman akhir hilang (tidak termasuk)',
      layoutRecognize: 'Pengenalan tata letak',
      layoutRecognizeTip:
        'Gunakan model visual untuk analisis tata letak untuk lebih mengidentifikasi struktur dokumen, menemukan di mana judul, blok teks, gambar, dan tabel berada. Tanpa fitur ini, hanya teks biasa dari PDF yang dapat diperoleh. Untuk informasi lebih lanjut, lihat https://ragflow.io/docs/dev/select_pdf_parser.',
      taskPageSize: 'Ukuran halaman tugas',
      taskPageSizeMessage: 'Silakan masukkan ukuran halaman tugas Anda!',
      taskPageSizeTip: `Jika menggunakan pengenalan tata letak, file PDF akan dibagi menjadi kelompok berturut-turut. Analisis tata letak akan dilakukan secara paralel antar kelompok untuk meningkatkan kecepatan pemrosesan. 'Ukuran halaman tugas' menentukan ukuran kelompok. Semakin besar ukuran halaman, semakin kecil kemungkinan teks berkelanjutan antara halaman dibagi menjadi potongan yang berbeda.`,
      addPage: 'Tambah halaman',
      greaterThan: 'Nilai saat ini harus lebih besar dari!',
      greaterThanPrevious: 'Nilai saat ini harus lebih besar dari sebelumnya!',
      selectFiles: 'Pilih file',
      changeSpecificCategory: 'Ubah kategori spesifik',
      uploadTitle: 'Klik atau seret file ke area ini untuk mengunggah',
      uploadDescription:
        'RAGFlow mendukung pengunggahan file secara tunggal atau batch. Untuk RAGFlow yang dideploy secara lokal: batas ukuran total file per unggahan adalah 1GB, dengan batas unggahan batch sebanyak 32 file. Tidak ada batasan jumlah total file per akun. Untuk demo.ragflow.io: batas ukuran total file per unggahan adalah 10MB, dengan setiap file tidak melebihi 10MB dan maksimum 128 file per akun.',
      chunk: 'Potongan',
      bulk: 'Massal',
      cancel: 'Batal',
      rerankModel: 'Model Rerank',
      rerankPlaceholder: 'Silakan pilih',
      rerankTip: `Opsional. Jika dikosongkan, RAGFlow akan menggunakan kombinasi kesamaan kata kunci berbobot dan kesamaan kosinus vektor berbobot; jika model rerank dipilih, skor reranking berbobot akan menggantikan kesamaan kosinus vektor berbobot. Harap diperhatikan bahwa menggunakan model rerank akan secara signifikan meningkatkan waktu respons sistem. Jika Anda ingin menggunakan model rerank, pastikan menggunakan SaaS reranker; jika Anda lebih memilih model rerank yang dijalankan secara lokal, pastikan memulai RAGFlow dengan docker-compose-gpu.yml.`,
      topK: 'Top-K',
      topKTip: `Digunakan bersama dengan Rerank model, pengaturan ini menentukan jumlah potongan teks yang akan dikirim ke model reranking yang ditentukan.`,
      delimiter: `Pemisah untuk segmentasi teks`,
      html4excel: 'Excel ke HTML',
      html4excelTip: `Gunakan bersama dengan metode pemotongan General. Ketika dinonaktifkan, file spreadsheet (XLSX, XLS (Excel 97-2003)) akan dianalisis baris demi baris menjadi pasangan kunci-nilai. Ketika diaktifkan, file spreadsheet akan dianalisis menjadi tabel HTML. Jika tabel asli memiliki lebih dari 12 baris, sistem akan secara otomatis membagi menjadi beberapa tabel HTML setiap 12 baris. Untuk informasi lebih lanjut, lihat https://ragflow.io/docs/dev/enable_excel2html.`,
    },
    knowledgeConfiguration: {
      titleDescription:
        'Perbarui detail basis pengetahuan Anda terutama metode parsing di sini.',
      name: 'Nama basis pengetahuan',
      photo: 'Foto basis pengetahuan',
      description: 'Deskripsi',
      language: 'Bahasa',
      languageMessage: 'Silakan masukkan bahasa Anda!',
      languagePlaceholder: 'Silakan masukkan bahasa Anda!',
      permissions: 'Izin',
      embeddingModel: 'Model embedding',
      chunkTokenNumber: 'Ukuran potongan yang disarankan',
      chunkTokenNumberMessage: 'Jumlah token potongan diperlukan',
      embeddingModelTip:
        'Model embedding default dari basis pengetahuan. Tidak dapat diubah setelah basis pengetahuan memiliki potongan data (chunks). Untuk beralih ke model embedding default yang berbeda, Anda harus menghapu

... [Content truncated - file is 56678 bytes] ...

i API',
      country: 'Negara',
      language: 'Bahasa',
      googleScholar: 'Google Scholar',
      googleScholarDescription:
        'Komponen ini digunakan untuk mendapatkan hasil pencarian dari https://scholar.google.com/. Biasanya, ini berfungsi sebagai pelengkap basis pengetahuan. Top N menentukan jumlah hasil pencarian yang perlu Anda sesuaikan.',
      yearLow: 'Tahun terendah',
      yearHigh: 'Tahun tertinggi',
      patents: 'Paten',
      data: 'Data',
      deepL: 'DeepL',
      deepLDescription:
        'Komponen ini digunakan untuk mendapatkan terjemahan dari https://www.deepl.com/. Biasanya, ini memberikan hasil terjemahan yang lebih khusus.',
      authKey: 'Kunci otorisasi',
      sourceLang: 'Bahasa sumber',
      targetLang: 'Bahasa target',
      gitHub: 'GitHub',
      githubDescription:
        'Komponen ini digunakan untuk mencari repositori dari https://github.com/. Top N menentukan jumlah hasil pencarian yang akan disesuaikan.',
      baiduFanyi: 'BaiduFanyi',
      baiduFanyiDescription:
        'Komponen ini digunakan untuk mendapatkan terjemahan dari https://fanyi.baidu.com/. Biasanya, ini memberikan hasil terjemahan yang lebih khusus',
      appid: 'ID aplikasi',
      secretKey: 'Kunci rahasia',
      domain: 'Domain',
      transType: 'Jenis terjemahan',
      baiduSecretKeyOptions: {
        translate: 'Terjemahan umum',
        fieldtranslate: 'Terjemahan bidang',
      },
      baiduDomainOptions: {
        it: 'Teknologi informasi',
        finance: 'Keuangan dan ekonomi',
        machinery: 'Manufaktur mesin',
        senimed: 'Biomedis',
        novel: 'Sastra daring',
        academic: 'Makalah akademik',
        aerospace: 'Dirgantara',
        wiki: 'Humaniora dan ilmu sosial',
        news: 'Berita dan informasi',
        law: 'Hukum dan peraturan',
        contract: 'Kontrak',
      },
      baiduSourceLangOptions: {
        auto: 'Deteksi otomatis',
        zh: 'Cina',
        en: 'Inggris',
        yue: 'Kanton',
        wyw: 'Cina Klasik',
        jp: 'Jepang',
        kor: 'Korea',
        fra: 'Prancis',
        spa: 'Spanyol',
        th: 'Thailand',
        ara: 'Arab',
        ru: 'Rusia',
        pt: 'Portugis',
        de: 'Jerman',
        it: 'Italia',
        el: 'Yunani',
        nl: 'Belanda',
        pl: 'Polandia',
        bul: 'Bulgaria',
        est: 'Estonia',
        dan: 'Denmark',
        fin: 'Finlandia',
        cs: 'Ceko',
        rom: 'Rumania',
        slo: 'Slovenia',
        swe: 'Swedia',
        hu: 'Hungaria',
        cht: 'Cina Tradisional',
        vie: 'Vietnam',
      },
      qWeather: 'QWeather',
      qWeatherDescription:
        'Komponen ini digunakan untuk mendapatkan informasi terkait cuaca dari https://www.qweather.com/. Anda dapat mendapatkan cuaca, indeks, kualitas udara.',
      lang: 'Bahasa',
      type: 'Jenis',
      webApiKey: 'Kunci API Web',
      userType: 'Jenis pengguna',
      timePeriod: 'Periode waktu',
      qWeatherLangOptions: {
        zh: 'Cina Sederhana',
        'zh-hant': 'Cina Tradisional',
        en: 'Inggris',
        de: 'Jerman',
        es: 'Spanyol',
        fr: 'Prancis',
        it: 'Italia',
        ja: 'Jepang',
        ko: 'Korea',
        ru: 'Rusia',
        hi: 'Hindi',
        th: 'Thailand',
        ar: 'Arab',
        pt: 'Portugis',
        bn: 'Bengali',
        ms: 'Melayu',
        nl: 'Belanda',
        el: 'Yunani',
        la: 'Latin',
        sv: 'Swedia',
        id: 'Indonesia',
        pl: 'Polandia',
        tr: 'Turki',
        cs: 'Ceko',
        et: 'Estonia',
        vi: 'Vietnam',
        fil: 'Filipina',
        fi: 'Finlandia',
        he: 'Ibrani',
        is: 'Islandia',
        nb: 'Norwegia',
      },
      qWeatherTypeOptions: {
        weather: 'Prakiraan cuaca',
        indices: 'Indeks kehidupan cuaca',
        airquality: 'Kualitas udara',
      },
      qWeatherUserTypeOptions: {
        free: 'Pelanggan gratis',
        paid: 'Pelanggan berbayar',
      },
      qWeatherTimePeriodOptions: {
        now: 'Sekarang',
        '3d': '3 hari',
        '7d': '7 hari',
        '10d': '10 hari',
        '15d': '12 hari',
        '30d': '30 hari',
      },
      publish: 'API',
      exeSQL: 'ExeSQL',
      exeSQLDescription:
        'Komponen ini menanyakan hasil dari database relasional yang sesuai melalui pernyataan SQL. Mendukung MySQL, PostgreSQL, MariaDB.',
      dbType: 'Jenis Database',
      database: 'Database',
      username: 'Nama Pengguna',
      host: 'Host',
      port: 'Port',
      password: 'Kata Sandi',
      switch: 'Sakelar',
      logicalOperator: 'Operator logis',
      switchOperatorOptions: {
        equal: 'sama dengan',
        notEqual: 'tidak sama dengan',
        gt: 'Lebih besar dari',
        ge: 'Lebih besar atau sama dengan',
        lt: 'Kurang dari',
        le: 'Kurang atau sama dengan',
        contains: 'Mengandung',
        notContains: 'Tidak mengandung',
        startWith: 'Mulai dengan',
        endWith: 'Berakhir dengan',
        empty: 'Kosong',
        notEmpty: 'Tidak kosong',
      },
      switchLogicOperatorOptions: {
        and: 'Dan',
        or: 'Atau',
      },
      operator: 'Operator',
      value: 'Nilai',
      useTemplate: 'Gunakan template ini',
      wenCai: 'WenCai',
      queryType: 'Jenis kueri',
      wenCaiDescription:
        'Komponen ini dapat digunakan untuk mendapatkan informasi di berbagai bidang keuangan, termasuk tetapi tidak terbatas pada saham, dana, dll...',
      wenCaiQueryTypeOptions: {
        stock: 'saham',
        zhishu: 'indeks',
        fund: 'dana',
        hkstock: 'Saham Hong Kong',
        usstock: 'Pasar saham AS',
        threeboard: 'Pasar OTC Baru',
        conbond: 'Obligasi Konversi',
        insurance: 'asuransi',
        futures: 'futures',
        lccp: 'Pembiayaan',
        foreign_exchange: 'Mata uang asing',
      },
      akShare: 'AkShare',
      akShareDescription:
        'Komponen ini dapat digunakan untuk mendapatkan ringkasan berita keuangan dari situs web keuangan utama, membantu penelitian industri dan kuantitatif.',
      yahooFinance: 'YahooFinance',
      yahooFinanceDescription:
        'Komponen ini menanyakan informasi tentang perusahaan berdasarkan simbol ticker yang diberikan.',
      info: 'Info',
      history: 'Sejarah',
      financials: 'Keuangan',
      balanceSheet: 'Neraca',
      cashFlowStatement: 'Laporan Arus Kas',
      jin10: 'Jin10',
      jin10Description:
        'Komponen ini dapat digunakan untuk mengakses informasi di sektor keuangan dari Platform Terbuka Jin10, termasuk berita cepat, kalender, kutipan, referensi.',
      flashType: 'Jenis kilat',
      filter: 'Filter',
      contain: 'Mengandung',
      calendarType: 'Jenis kalender',
      calendarDatashape: 'Bentuk data kalender',
      symbolsDatatype: 'Jenis data simbol',
      symbolsType: 'Jenis simbol',
      jin10TypeOptions: {
        flash: 'Berita Cepat',
        calendar: 'Kalender',
        symbols: 'kutipan',
        news: 'referensi',
      },
      jin10FlashTypeOptions: {
        '1': 'Berita Pasar',
        '2': 'Berita Futures',
        '3': 'Berita AS-Hong Kong',
        '4': 'Berita Saham A',
        '5': 'Berita Komoditas & Forex',
      },
      jin10CalendarTypeOptions: {
        cj: 'Kalender Data Makroekonomi',
        qh: 'Kalender Futures',
        hk: 'Kalender Pasar Saham Hong Kong',
        us: 'Kalender Pasar Saham AS',
      },
      jin10CalendarDatashapeOptions: {
        data: 'Data',
        event: 'Acara',
        holiday: 'Liburan',
      },
      jin10SymbolsTypeOptions: {
        GOODS: 'Kutipan Komoditas',
        FOREX: 'Kutipan Forex',
        FUTURE: 'Kutipan Pasar Internasional',
        CRYPTO: 'Kutipan Cryptocurrency',
      },
      jin10SymbolsDatatypeOptions: {
        symbols: 'Daftar Komoditas',
        quotes: 'Kutipan Pasar Terbaru',
      },
      concentrator: 'Konsentrator',
      concentratorDescription:
        'Komponen yang menerima output dari komponen hulu dan meneruskannya sebagai input ke komponen hilir.',
      tuShare: 'TuShare',
      tuShareDescription:
        'Komponen ini dapat digunakan untuk mendapatkan ringkasan berita keuangan dari situs web keuangan utama, membantu penelitian industri dan kuantitatif.',
      tuShareSrcOptions: {
        sina: 'Sina',
        wallstreetcn: 'wallstreetcn',
        '10jqka': 'Flush langsung',
        eastmoney: 'Eastmoney',
        yuncaijing: 'YUNCAIJING',
        fenghuang: 'FENGHUANG',
        jinrongjie: 'JRJ',
      },
      token: 'Token',
      src: 'Sumber',
      startDate: 'Tanggal mulai',
      endDate: 'Tanggal akhir',
      keyword: 'Kata kunci',
      note: 'Catatan',
      noteDescription: 'Catatan',
      notePlaceholder: 'Silakan masukkan catatan',

      invoke: 'Permintaan HTTP',
      invokeDescription:
        'Komponen yang mampu memanggil layanan remote, menggunakan output komponen lain atau konstanta sebagai input.',
      url: 'Url',
      method: 'Metode',
      timeout: 'Waktu habis',
      headers: 'Header',
      cleanHtml: 'Bersihkan HTML',
      cleanHtmlTip:
        'Jika respons diformat HTML dan hanya ingin konten utama, aktifkan opsi ini.',
      invalidUrl:
        'Harus berupa URL yang valid atau URL dengan placeholder variabel dalam format {nama_variabel} atau {komponen@variabel}',

      prompt: 'Prompt',
      promptTip:
        'Gunakan prompt sistem untuk menjelaskan tugas untuk LLM, tentukan bagaimana harus merespons, dan menguraikan persyaratan lainnya. Prompt sistem sering digunakan bersama dengan kunci (variabel), yang berfungsi sebagai berbagai input data untuk LLM. Gunakan garis miring `/` atau tombol (x) untuk menampilkan kunci yang digunakan.',
      promptMessage: 'Prompt diperlukan',
      runningHintText: 'sedang berjalan...🕞',
    },
    footer: {
      profile: 'Semua hak dilindungi @ React',
    },
    layout: {
      file: 'file',
      knowledge: 'pengetahuan',
      chat: 'obrolan',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/id.ts` is located in the `web/src/locales` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to locales.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [config.ts](config.ts_docs.md)
- [de.ts](de.ts_docs.md)
- [en.ts](en.ts_docs.md)
- [es.ts](es.ts_docs.md)
- [fr.ts](fr.ts_docs.md)
- [ja.ts](ja.ts_docs.md)
- [pt-br.ts](pt-br.ts_docs.md)
- [ru.ts](ru.ts_docs.md)
- [until.ts](until.ts_docs.md)
- [vi.ts](vi.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
