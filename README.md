<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [



  { "en": "Berfungsi sebagai tuas untuk menyalakan dan mematikan MCB secara manual, serta sebagai indikator posisi on atau off?", "id": "Tuas aktuator untuk operasional manual." },
  { "en": "Berfungsi sebagai mekanik yang secara fisik memutus aliran arus listrik ketika terjadi trip?", "id": "Mekanisme sakelar pemutus arus." },
  { "en": "Berfungsi sebagai komponen yang menyambungkan dan memutuskan aliran arus listrik utama?", "id": "Kontak utama penyambung arus listrik." },
  { "en": "Berfungsi sebagai tempat untuk menyambungkan kabel listrik masuk (input) dan keluar (output) dari MCB?", "id": "Terminal untuk koneksi kabel listrik." },
  { "en": "Berfungsi sebagai pelindung termal yang akan melengkung dan memicu pemutusan arus ketika terjadi kelebihan beban (overload)?", "id": "Strip bimetal pendeteksi beban lebih." },
  { "en": "Berfungsi sebagai baut yang digunakan oleh pabrikan untuk mengatur dan mengeset tingkat sensitivitas trip pada MCB?", "id": "Sekrup untuk kalibrasi sensitivitas trip." },
  { "en": "Berfungsi sebagai kumparan (coil) yang akan menghasilkan medan magnet untuk memicu pemutusan arus (magnetic trip) ketika terjadi hubung singkat (short circuit)?", "id": "Kumparan solenoid pendeteksi korsleting." },
  { "en": "Berfungsi untuk memadamkan atau mendinginkan busur api yang timbul saat terjadi pemutusan arus listrik?", "id": "Ruang pemadam busur api listrik." },
  { "en": "Berfungsi sebagai selubung atau cangkang luar yang melindungi semua komponen internal MCB?", "id": "Casing pelindung komponen internal." },
  { "en": "Berfungsi sebagai indikator visual yang menunjukkan bahwa MCB dalam keadaan trip (putus)?", "id": "Penanda visual status trip MCB." },
  { "en": "Berfungsi untuk memutus sirkuit listrik ketika terjadi arus berlebih yang dapat merusak peralatan?", "id": "Unit pemutus sirkuit arus berlebih." },
  { "en": "Berfungsi sebagai mekanisme pegas yang membantu mempercepat pemisahan kontak saat MCB trip?", "id": "Pegas pendorong pemisahan kontak cepat." },
  { "en": "Berfungsi untuk melindungi sirkuit dari arus yang melebihi kapasitas normal dalam waktu tertentu?", "id": "Fitur proteksi terhadap beban lebih." },
  { "en": "Berfungsi untuk memutus sirkuit dengan sangat cepat ketika terjadi korsleting atau hubungan langsung antara fasa dan netral?", "id": "Fitur proteksi terhadap hubung singkat." },
  { "en": "Berfungsi untuk melindungi kabel dan peralatan listrik dari kerusakan akibat panas berlebih?", "id": "Fungsi utama proteksi termal." },
  { "en": "Berfungsi untuk mendeteksi lonjakan arus yang sangat tinggi dan memutus sirkuit dalam hitungan milidetik?", "id": "Fungsi utama proteksi magnetik." },
  { "en": "Berfungsi untuk memungkinkan pengguna mematikan aliran listrik secara manual untuk perbaikan atau pemeliharaan?", "id": "Fungsi sebagai sakelar manual." },
  { "en": "Komponen yang bereaksi terhadap panas yang dihasilkan oleh arus berlebih?", "id": "Elemen termal strip bimetal." },
  { "en": "Bagian yang ditarik oleh medan magnet dari solenoid untuk memicu mekanisme trip?", "id": "Plunger penarik mekanisme trip." },
  { "en": "Bagian dari unit trip yang merespon terhadap kondisi hubung singkat?", "id": "Pemicu magnetik untuk korsleting." },
  { "en": "Mekanisme yang menahan kontak tetap tertutup dalam kondisi normal dan melepaskannya saat trip?", "id": "Sistem pengunci dan pelepas kontak." },
  { "en": "Saluran di dalam pemadam busur api yang berfungsi untuk memecah dan mendinginkan busur api?", "id": "Jalur pemecah busur api." },
  { "en": "Bagian terminal yang digunakan untuk mengencangkan kabel pada MCB?", "id": "Baut pengencang kabel di terminal." },
  { "en": "Pelindung tambahan yang sering dipasang di luar MCB untuk melindunginya dari debu, air, dan sentuhan langsung?", "id": "Kotak pelindung eksternal MCB." },
  { "en": "Kontak tambahan pada beberapa MCB yang dapat digunakan untuk fungsi pensinyalan atau kontrol eksternal?", "id": "Kontak sinyal atau kontrol tambahan." },
  { "en": "Sistem yang bekerja berdasarkan prinsip pemuaian dua jenis logam yang berbeda ketika dipanaskan?", "id": "Prinsip kerja termal bimetal." },
  { "en": "Sistem yang bekerja berdasarkan prinsip elektromagnetik untuk mendeteksi arus hubung singkat?", "id": "Prinsip kerja magnetik solenoid." },
  { "en": "Bagian MCB yang secara langsung berinteraksi dengan pengguna untuk pengoperasian manual?", "id": "Tuas operasi manual pengguna." },
  { "en": "Jalur yang dilalui arus listrik di dalam MCB dari terminal input ke terminal output?", "id": "Lintasan utama arus listrik internal." },
  { "en": "Mekanisme internal yang menerjemahkan gerakan dari bimetal atau solenoid menjadi pemutusan kontak?", "id": "Mekanisme penerjemah sinyal trip." },
  { "en": "Menjaga agar tidak terjadi kebakaran akibat aliran listrik yang berlebihan?", "id": "Fungsi utama pencegahan kebakaran listrik." },
  { "en": "Memudahkan identifikasi masalah pada sirkuit ketika MCB trip?", "id": "Sebagai indikator adanya gangguan listrik." },
  { "en": "Melindungi peralatan elektronik yang sensitif dari lonjakan arus?", "id": "Fungsi melindungi peralatan sensitif." },
  { "en": "Secara otomatis memutus aliran listrik jika penggunaan daya melebihi batas kontrak dengan PLN?", "id": "Sebagai pembatas daya listrik otomatis." },
  { "en": "Menjadi sakelar utama untuk memutus seluruh aliran listrik di dalam rumah?", "id": "Sebagai sakelar pemutus utama." },
  { "en": "Komponen yang memastikan pemutusan sirkuit terjadi dengan cepat untuk meminimalkan kerusakan?", "id": "Mekanisme untuk pemutusan cepat." },
  { "en": "Bagian yang menahan busur api agar tetap berada di dalam area yang aman untuk dipadamkan?", "id": "Ruang penahan busur api." },
  { "en": "Bagian dari housing yang terbuat dari bahan isolasi untuk mencegah sengatan listrik?", "id": "Bahan isolator pelindung sengatan." },
  { "en": "Komponen yang memberikan gaya dorong untuk memisahkan kontak saat dilepaskan oleh mekanisme pengunci?", "id": "Pegas pendorong pemutus kontak." },
  { "en": "Tuas yang juga berfungsi menunjukkan status MCB, apakah 'ON', 'OFF', atau 'TRIP'?", "id": "Indikator status posisi tuas." },
  { "en": "Prinsip kerja yang digunakan untuk memutus arus karena beban lebih (overheat)?", "id": "Prinsip pemicu termal (panas)." },
  { "en": "Prinsip kerja yang digunakan untuk memutus arus karena hubungan singkat?", "id": "Prinsip pemicu magnetik (korsleting)." },
  { "en": "Bagian yang menghubungkan tuas aktuator dengan switch mekanis?", "id": "Batang penghubung tuas dan sakelar." },
  { "en": "Area pada MCB tempat kabel masukan (fasa) disambungkan?", "id": "Terminal masukan dari sumber listrik." },
  { "en": "Area pada MCB tempat kabel keluaran (beban) disambungkan?", "id": "Terminal keluaran menuju beban." },
  { "en": "Struktur internal yang memisahkan dan mendinginkan gas panas dari busur api?", "id": "Pelat pendingin gas busur api." },
  { "en": "Fungsi yang melindungi kabel instalasi dari pelelehan akibat arus yang terlalu tinggi?", "id": "Sebagai pelindung utama kabel instalasi." },
  { "en": "Berperan sebagai pengaman utama pada panel distribusi listrik?", "id": "Fungsi pengaman di panel distribusi." },
  { "en": "Mekanisme yang memungkinkan MCB direset kembali ke posisi 'ON' setelah gangguan diatasi?", "id": "Mekanisme untuk reset setelah trip." },
  { "en": "Bagian dari housing yang memiliki klip untuk pemasangan pada rel DIN (DIN Rail)?", "id": "Klip pengunci untuk rel DIN." },
  { "en": "Menjaga suhu MCB tetap stabil selama beroperasi pada kondisi normal?", "id": "Fungsi menjaga stabilitas suhu." },
  { "en": "Kumparan yang menciptakan medan magnet ketika dialiri arus hubung singkat yang sangat besar?", "id": "Kumparan pemicu trip magnetik." },
  { "en": "Batang logam kecil yang bergerak karena gaya magnet untuk menekan tuas trip?", "id": "Armatur pendorong tuas trip." },
  { "en": "Bagian yang secara langsung mengalami gaya dari bimetal yang melengkung?", "id": "Tuas pemicu dari elemen termal." },
  { "en": "Bagian yang secara langsung mengalami gaya dari plunger/armatur magnetik?", "id": "Tuas pemicu dari elemen magnetik." },
  { "en": "Kontak yang bergerak untuk membuka dan menutup sirkuit?", "id": "Bagian kontak yang bisa bergerak." },
  { "en": "Kontak yang tetap pada posisinya saat sirkuit dibuka atau ditutup?", "id": "Bagian kontak yang diam." },
  { "en": "Menyediakan pemutusan sirkuit yang andal dan dapat digunakan kembali setelah trip, tidak seperti sekring?", "id": "Sebagai pengaman yang dapat direset." },
  { "en": "Melindungi dari bahaya sengatan listrik dengan memutus arus jika terjadi kebocoran ke ground (untuk tipe tertentu)?", "id": "Sebagai proteksi terhadap arus bocor." },
  { "en": "Bagian yang memastikan semua kutub pada MCB multi-kutub (2P/3P) trip secara bersamaan?", "id": "Mekanisme trip untuk semua kutub." },
  { "en": "Berfungsi untuk mendistribusikan energi listrik dari sumber ke berbagai sirkuit beban?", "id": "Sebagai pendistribusi energi listrik." },
  { "en": "Menyediakan proteksi yang sangat sensitif dan cepat untuk peralatan elektronik rentan (Tipe Z/K)?", "id": "Proteksi untuk peralatan sangat sensitif." },
  { "en": "Menyediakan proteksi untuk beban dengan lonjakan arus awal yang tinggi seperti motor (Tipe C/D)?", "id": "Proteksi untuk beban induktif motor." },
  { "en": "Menyediakan proteksi umum untuk sirkuit pencahayaan dan stop kontak di perumahan (Tipe B)?", "id": "Proteksi umum untuk perumahan." },
  { "en": "Berfungsi sebagai pemutus sirkuit miniatur yang dipasang pada panel listrik?", "id": "Definisi dari Miniature Circuit Breaker." },
  { "en": "Komponen yang membedakan respon MCB terhadap beban lebih dan hubung singkat?", "id": "Mekanisme trip ganda (termal-magnetik)." },
  { "en": "Bagian yang memastikan kontak terpisah dengan jarak yang cukup untuk mencegah loncatan api kembali?", "id": "Celah pemisah antar kontak." },
  { "en": "Bahan yang digunakan pada housing MCB yang tahan panas dan tidak mudah terbakar?", "id": "Material plastik tahan api." },
  { "en": "Tulisan pada MCB yang menunjukkan batas arus normal yang dapat dilewatkan secara terus menerus?", "id": "Nilai rating arus nominal." },
  { "en": "Tulisan pada MCB yang menunjukkan kemampuan maksimum memutus arus hubung singkat tanpa rusak?", "id": "Nilai kapasitas pemutusan arus." },
  { "en": "Kode pada MCB (B, C, D) yang menentukan karakteristik waktu-arus trip magnetiknya?", "id": "Kode kurva karakteristik trip." },
  { "en": "Titik pada MCB tempat kabel dari sumber listrik PLN disambungkan?", "id": "Terminal untuk kabel fasa masuk." },
  { "en": "Titik pada MCB tempat kabel yang menuju ke beban/peralatan listrik disambungkan?", "id": "Terminal untuk kabel beban keluar." },
  { "en": "Fungsi yang mencegah MCB di-ON-kan secara paksa saat masih ada gangguan (trip-free)?", "id": "Mekanisme anti-paksa saat trip." },
  { "en": "Bagian yang memberikan informasi visual tentang merek, tipe, dan spesifikasi MCB?", "id": "Label informasi dan spesifikasi produk." },
  { "en": "Mekanisme yang mengurangi energi busur api saat pemutusan?", "id": "Sistem untuk pembatasan arus." },
  { "en": "Komponen utama dari sistem proteksi termal?", "id": "Elemen utama deteksi panas." },
  { "en": "Komponen utama dari sistem proteksi magnetik?", "id": "Elemen utama deteksi korsleting." },
  { "en": "Bagian yang menahan semua komponen internal MCB pada tempatnya?", "id": "Rangka penahan komponen internal." },
  { "en": "Melindungi pengguna dari kontak tidak sengaja dengan bagian bertegangan?", "id": "Fungsi isolasi dan proteksi sentuh." },
  { "en": "Struktur yang mengarahkan gas panas hasil busur api keluar dari MCB?", "id": "Saluran pembuangan gas panas." },
  { "en": "Bertugas memutus arus saat beban 3-5 kali lebih besar dari arus maksimalnya?", "id": "Karakteristik dari MCB Tipe B." },
  { "en": "Bertugas memutus arus saat beban 5-10 kali lebih besar dari arus maksimalnya?", "id": "Karakteristik dari MCB Tipe C." },
  { "en": "Bertugas memutus arus saat beban 10-25 kali lebih besar dari arus maksimalnya, cocok untuk mesin industri?", "id": "Karakteristik dari MCB Tipe D." },
  { "en": "Bertugas memutus arus dengan sangat cepat saat beban 2-3 kali lebih tinggi, untuk perangkat sangat sensitif?", "id": "Karakteristik dari MCB Tipe Z." },
  { "en": "Berfungsi sebagai saklar pembatas arus pada KWH meter milik PLN?", "id": "Sebagai pembatas daya dari PLN." },
  { "en": "Berfungsi sebagai saklar pengaman instalasi di dalam rumah milik pelanggan?", "id": "Sebagai pengaman instalasi milik pelanggan." },
  { "en": "Bagian yang berfungsi menghubungkan dan memutus satu jalur fasa saja?", "id": "MCB untuk satu fasa." },
  { "en": "Bagian yang berfungsi melindungi sirkuit tiga fasa (R, S, T)?", "id": "MCB untuk tiga fasa." },
  { "en": "Fungsi yang memastikan sirkuit benar-benar terputus saat tuas berada di posisi OFF?", "id": "Indikator pemutusan sirkuit positif." },
  { "en": "Seluruh unit yang bertanggung jawab untuk mendeteksi dan bereaksi terhadap gangguan listrik?", "id": "Sistem proteksi MCB terpadu." },
  { "en": "Komponen yang menahan energi mekanis saat pegas trip terlepas?", "id": "Mekanisme untuk menahan energi mekanis." },
  { "en": "Proses dimana bimetal menjadi panas dan membengkok karena arus berlebih?", "id": "Proses pelengkungan bimetal karena panas." },
  { "en": "Proses dimana solenoid menghasilkan gaya magnet yang kuat akibat arus hubung singkat?", "id": "Proses aktivasi elektromagnetik solenoid." },
  { "en": "Lapisan pada kontak untuk meningkatkan konduktivitas dan ketahanan terhadap busur api?", "id": "Lapisan pelindung kontak (perak)." },
  { "en": "Mekanisme yang memungkinkan pemasangan dan pelepasan MCB dari DIN rail dengan mudah?", "id": "Sistem pemasangan cepat pada rel." },
  { "en": "Bagian dari housing yang dirancang untuk menahan tekanan internal dari gas busur api?", "id": "Dinding penahan tekanan gas internal." },
  { "en": "Memastikan pemutusan yang andal bahkan setelah lama tidak dioperasikan?", "id": "Menjamin keandalan jangka panjang." },
  { "en": "Melindungi dari korsleting antara kabel fasa dengan kabel netral?", "id": "Sebagai proteksi korsleting fasa-netral." },
  { "en": "Melindungi dari korsleting antara kabel fasa dengan kabel ground/tanah?", "id": "Sebagai proteksi korsleting fasa-tanah." },
  { "en": "Berfungsi sebagai penopang utama tempat semua komponen internal MCB dirakit?", "id": "Sasis sebagai penopang utama." },
  { "en": "Lapisan pada strip bimetal yang memiliki koefisien muai panas lebih tinggi?", "id": "Lapisan bimetal yang aktif." },
  { "en": "Lapisan pada strip bimetal yang memiliki koefisien muai panas lebih rendah untuk menciptakan efek lengkungan?", "id": "Lapisan bimetal yang pasif." },
  { "en": "Mekanisme yang memastikan trip terjadi secara independen dari tekanan pada tuas aktuator?", "id": "Mekanisme operasi yang independen." },
  { "en": "Energi yang dilepaskan dalam bentuk panas dan cahaya saat pemutusan arus?", "id": "Energi dari busur api." },
  { "en": "Fungsi yang membatasi jumlah energi (I²t) yang dapat melewati MCB saat terjadi hubung singkat?", "id": "Sebagai fungsi pembatas energi." },
  { "en": "Bagian dari kumparan solenoid yang berfungsi sebagai inti untuk memperkuat medan magnet?", "id": "Inti besi penguat solenoid." },
  { "en": "Pegas yang berfungsi mengembalikan plunger atau armatur solenoid ke posisi semula setelah trip?", "id": "Pegas untuk mereset plunger." },
  { "en": "Ruang di dalam MCB yang dirancang untuk menahan tekanan gas akibat busur api?", "id": "Ruang untuk ekspansi gas." },
  { "en": "Proses pendinginan dan pemadaman busur api dengan membaginya menjadi beberapa busur api kecil?", "id": "Prinsip pemadaman busur api." },
  { "en": "Material yang digunakan pada ujung kontak untuk menahan erosi akibat busur api?", "id": "Paduan perak-tungsten anti erosi." },
  { "en": "Bagian yang menghubungkan beberapa kutub MCB (misal 3-pole) untuk trip bersamaan?", "id": "Batang penghubung untuk trip bersamaan." },
  { "en": "Simbol pada MCB yang menunjukkan bahwa alat tersebut memiliki fungsi isolasi yang aman?", "id": "Simbol untuk fungsi isolasi." },
  { "en": "Informasi pada MCB yang menunjukkan standar keamanan internasional yang dipenuhinya (misal: IEC 60898)?", "id": "Informasi standar kepatuhan produk." },
  { "en": "Bagian dari terminal yang dirancang untuk menjepit kabel dengan kuat dan merata?", "id": "Klem penjepit kabel di terminal." },
  { "en": "Fungsi yang melindungi dari bahaya arus berlebih yang berlangsung lama namun tidak cukup besar untuk trip magnetik?", "id": "Deteksi termal untuk beban lebih." },
  { "en": "Fungsi yang memberikan reaksi pemutusan dalam hitungan milidetik terhadap lonjakan arus sangat tinggi?", "id": "Deteksi magnetik untuk hubung singkat." },
  { "en": "Kemampuan MCB untuk memutus arus hubung singkat maksimum tanpa menyebabkan bahaya?", "id": "Kapasitas pemutusan ultimit (Icu)." },
  { "en": "Kemampuan MCB untuk memutus arus hubung singkat dan tetap dapat beroperasi kembali setelahnya?", "id": "Kapasitas pemutusan servis (Ics)." },
  { "en": "Komponen yang berfungsi sebagai engsel untuk mekanisme pengunci (latching)?", "id": "Poros engsel mekanisme pengunci." },
  { "en": "Tekanan kontak yang dihasilkan oleh pegas untuk memastikan koneksi listrik yang baik saat ON?", "id": "Gaya pegas untuk menekan kontak." },
  { "en": "Proses dimana busur api 'ditarik' masuk ke dalam ruang pemadam api oleh efek magnetik?", "id": "Proses peniupan magnetik busur api." },
  { "en": "Bahan keramik atau polimer khusus yang digunakan dalam arc chute untuk menahan panas tinggi?", "id": "Material isolasi tahan panas tinggi." },
  { "en": "Struktur pada housing yang berfungsi untuk melepaskan gas panas secara aman dari dalam MCB?", "id": "Saluran ventilasi pembuangan gas." },
  { "en": "Bagian dari mekanisme trip yang berfungsi seperti 'pelatuk' pada pistol?", "id": "Mekanisme pelepas seperti pelatuk." },
  { "en": "Fungsi yang mencegah busur api menyambar ke bagian konduktif lain di dalam panel?", "id": "Sebagai fungsi penahanan busur api." },
  { "en": "Kondisi dimana kontak mulai terpisah dan arus mulai dibatasi sebelum mencapai puncak hubung singkat?", "id": "Kondisi pra-pemutusan arus." },
  { "en": "Mekanisme yang memastikan kecepatan pemisahan kontak tidak bergantung pada kecepatan gerakan tuas?", "id": "Mekanisme pemutusan cepat (snap-action)." },
  { "en": "Bagian yang memberikan penandaan warna pada tuas aktuator untuk menunjukkan rating arusnya?", "id": "Kode warna rating arus." },
  { "en": "Bantalan atau pin yang berfungsi mengurangi gesekan pada bagian yang bergerak di dalam mekanisme?", "id": "Bantalan untuk mengurangi gesekan." },
  { "en": "Fungsi yang melindungi motor listrik dari kerusakan akibat stall atau kegagalan start?", "id": "Proteksi untuk kondisi rotor terkunci." },
  { "en": "Bagian dari sirkuit magnetik yang melengkapi jalur fluks magnet dari solenoid?", "id": "Jangkar pelengkap sirkuit magnetik." },
  { "en": "Kondisi MCB yang tidak dapat direset segera setelah trip karena bimetal masih panas?", "id": "Waktu tunggu pendinginan bimetal." },
  { "en": "Proses penyesuaian sekrup kalibrasi di pabrik untuk memastikan akurasi trip?", "id": "Proses untuk penyetelan kalibrasi." },
  { "en": "Bagian pada housing yang berfungsi sebagai penghalang antar kutub pada MCB multi-kutub?", "id": "Sekat pemisah antar kutub." },
  { "en": "Fungsi yang memungkinkan penambahan modul seperti kontak bantu atau shunt trip?", "id": "Sebagai perangkat dengan kapabilitas modular." },
  { "en": "Komponen tambahan yang memungkinkan MCB ditripkan dari jarak jauh menggunakan sinyal tegangan?", "id": "Kumparan untuk trip jarak jauh." },
  { "en": "Komponen tambahan yang akan mentripkan MCB jika tegangan suplai turun di bawah level tertentu?", "id": "Pelepas karena tegangan rendah." },
  { "en": "Permukaan pada kontak yang dirancang untuk menahan pengelasan saat terjadi arus tinggi?", "id": "Permukaan kontak anti-las." },
  { "en": "Bagian dari aktuator yang dirancang agar nyaman dan tidak licin saat dioperasikan?", "id": "Gagang tuas yang ergonomis." },
  { "en": "Struktur internal yang menjaga jarak aman antar komponen bertegangan?", "id": "Jarak rambat dan jarak bebas." },
  { "en": "Fungsi yang menjamin bahwa MCB akan trip pada tingkat arus yang ditentukan dalam standar?", "id": "Menjamin akurasi dari trip." },
  { "en": "Proses pengujian di pabrik untuk memverifikasi setiap fungsi MCB?", "id": "Proses pengujian rutin pabrikan." },
  { "en": "Bagian dari mekanisme yang mengubah gerakan linier dari plunger solenoid menjadi gerakan rotasi untuk melepas pengunci?", "id": "Tuas pengubah arah gerak." },
  { "en": "Titik pivot tempat strip bimetal terpasang pada rangka MCB?", "id": "Engsel untuk strip bimetal." },
  { "en": "Fungsi yang membatasi kenaikan suhu pada terminal MCB saat dialiri arus nominal?", "id": "Fungsi manajemen panas terminal." },
  { "en": "Mekanisme pengunci yang menggunakan pin atau kait untuk menahan kontak tertutup?", "id": "Kait penahan mekanisme pengunci." },
  { "en": "Mekanisme yang dilepaskan oleh kait pengunci saat terjadi trip?", "id": "Batang pelepas mekanisme pengunci." },
  { "en": "Fungsi yang memastikan MCB tetap kokoh terpasang di DIN rail meski ada getaran?", "id": "Sistem pengunci pada rel DIN." },
  { "en": "Label pada MCB yang menunjukkan diagram koneksi atau sirkuit internalnya?", "id": "Label diagram skematik sirkuit." },
  { "en": "Nomor unik pada MCB untuk keperluan pelacakan produksi dan garansi?", "id": "Nomor seri untuk pelacakan." },
  { "en": "Struktur pada housing yang mencegah masuknya debu dan kelembaban ke bagian internal?", "id": "Segel pelindung dari debu." },
  { "en": "Kemampuan material housing untuk menahan benturan mekanis tanpa pecah?", "id": "Tingkat ketahanan benturan mekanis." },
  { "en": "Fungsi yang memungkinkan pemasangan gembok pada tuas untuk mencegah pengoperasian?", "id": "Fitur untuk penguncian (lockout)." },
  { "en": "Jalur konduktif fleksibel yang menghubungkan kontak bergerak ke terminal?", "id": "Konektor fleksibel penghubung kontak." },
  { "en": "Mekanisme yang memastikan gaya pegas selalu siap untuk memisahkan kontak dengan cepat?", "id": "Mekanisme penyimpan energi potensial." },
  { "en": "Bagian dari unit trip yang menentukan karakteristik kurva B, C, atau D?", "id": "Unit penentu kalibrasi magnetik." },
  { "en": "Bagian dari unit trip yang menentukan karakteristik waktu tunda pada beban lebih?", "id": "Unit penentu kalibrasi termal." },
  { "en": "Fungsi yang memungkinkan beberapa MCB dihubungkan bersamaan dengan satu plat konduktor?", "id": "Fitur kompatibilitas dengan busbar." },
  { "en": "Plat konduktor yang digunakan untuk mendistribusikan daya ke beberapa MCB secara bersamaan?", "id": "Busbar tipe sisir penghubung." },
  { "en": "Bagian dari terminal yang dirancang untuk menerima busbar sisir?", "id": "Slot untuk pemasangan busbar." },
  { "en": "Kemampuan MCB untuk beroperasi dengan andal dalam rentang suhu lingkungan tertentu?", "id": "Rentang suhu untuk beroperasi." },
  { "en": "Fungsi yang melindungi terhadap arus berlebih akibat penambahan banyak peralatan pada satu sirkuit?", "id": "Proteksi terhadap akumulasi beban." },
  { "en": "Kondisi dimana arus mengalir melalui jalur yang tidak semestinya dengan resistansi sangat rendah?", "id": "Definisi dari kondisi hubung singkat." },
  { "en": "Kondisi dimana arus melebihi rating normal tetapi masih mengalir melalui jalur yang benar?", "id": "Definisi dari kondisi beban lebih." },
  { "en": "Seluruh komponen bergerak yang terlibat dari pelepasan pengunci hingga pemisahan kontak?", "id": "Rantai kinematik dari mekanisme trip." },
  { "en": "Titik di dalam mekanisme dimana gaya kecil dari bimetal/solenoid diperkuat untuk melepas pegas yang kuat?", "id": "Titik untuk penggandaan gaya." },
  { "en": "Bahan yang digunakan pada housing yang tidak akan rusak oleh paparan sinar UV (untuk pemasangan outdoor)?", "id": "Material plastik tahan sinar UV." },
  { "en": "Fungsi yang memastikan kontak utama terbuka sepenuhnya sebelum kontak bantu memberikan sinyal?", "id": "Urutan kerja operasi kontak." },
  { "en": "Mekanisme yang mencegah getaran atau guncangan ringan menyebabkan trip yang tidak diinginkan?", "id": "Fitur stabilitas terhadap getaran." },
  { "en": "Bagian dari arc chute yang menangkap partikel logam panas dari busur api?", "id": "Perangkap untuk partikel logam panas." },
  { "en": "Fungsi yang melindungi transformator dari arus magnetisasi awal yang tinggi (inrush current)?", "id": "Proteksi untuk arus awal transformator." },
  { "en": "Fungsi yang melindungi sirkuit semikonduktor yang sangat rentan terhadap energi berlebih?", "id": "Proteksi untuk sirkuit semikonduktor." },
  { "en": "Batas maksimum tegangan yang dapat ditahan oleh isolasi MCB tanpa rusak?", "id": "Tegangan impuls ketahanan isolasi." },
  { "en": "Fungsi yang memungkinkan MCB dipasang dalam berbagai orientasi (vertikal/horizontal) tanpa mempengaruhi kinerja?", "id": "Independensi dari posisi pemasangan." },
  { "en": "Lapisan pada komponen logam internal untuk mencegah korosi?", "id": "Lapisan pelindung anti-korosi." },
  { "en": "Bagian yang memastikan tuas tetap di posisi tengah setelah trip, sebagai indikasi visual yang jelas?", "id": "Posisi tuas indikator trip." },
  { "en": "Mekanisme yang memberikan 'rasa' klik yang tegas saat MCB di-ON-kan atau di-OFF-kan?", "id": "Mekanisme umpan balik taktil." },
  { "en": "Komponen yang menyerap energi mekanis saat kontak berhenti bergerak untuk mencegah pantulan?", "id": "Komponen peredam energi mekanis." },
  { "en": "Fungsi yang memisahkan sirkuit yang terganggu dari sisa sistem kelistrikan?", "id": "Fungsi sebagai isolasi gangguan." },
  { "en": "Kemampuan MCB untuk membedakan antara gangguan nyata dan gangguan sesaat (transient)?", "id": "Kemampuan diskriminasi gangguan (selektivitas)." },
  { "en": "Bagian dari housing yang dirancang untuk memfasilitasi aliran udara pendingin di sekitar MCB?", "id": "Sirip untuk pendinginan housing." },
  { "en": "Proses penuaan komponen (misal: pegas, bimetal) yang dapat mempengaruhi kinerja MCB seiring waktu?", "id": "Proses degradasi pada komponen." },
  { "en": "Bagian yang memberikan informasi tentang torsi pengencangan yang benar untuk sekrup terminal?", "id": "Informasi indikasi torsi pengencangan." },
  { "en": "Fungsi yang memastikan semua spesifikasi pada label tetap terbaca sepanjang masa pakai MCB?", "id": "Fitur ketahanan pada label." },
  { "en": "Mekanisme yang memungkinkan penggabungan MCB dengan perangkat proteksi arus sisa (RCD)?", "id": "Sistem untuk kombinasi RCBO." },
  { "en": "Sensor tambahan pada RCBO yang mendeteksi perbedaan arus antara fasa dan netral?", "id": "Transformator arus toroidal internal." },
  { "en": "Bagian dari housing yang terbuat dari material transparan untuk inspeksi visual internal (pada model tertentu)?", "id": "Jendela untuk inspeksi visual." },
  { "en": "Proses manufaktur yang memastikan konsistensi dan kualitas tinggi di setiap unit MCB?", "id": "Proses kontrol kualitas produksi." },
  { "en": "Fungsi yang menjamin MCB tidak akan gagal secara katastropik bahkan saat memutus arus di atas kapasitasnya?", "id": "Fitur keamanan anti-gagal." },
  { "en": "Desain internal yang meminimalkan resistansi listrik untuk mengurangi panas saat beroperasi normal?", "id": "Desain untuk kerugian daya rendah." },
  { "en": "Pegas yang berfungsi menekan kontak bergerak ke kontak tetap dengan kuat saat posisi ON?", "id": "Pegas utama untuk menekan kontak." },
  { "en": "Bagian yang menjadi titik tumpu bagi mekanisme trip untuk berotasi?", "id": "Titik tumpu dari mekanisme trip." },
  { "en": "Seluruh sistem proteksi yang menggabungkan elemen termal dan magnetik dalam satu unit?", "id": "Unit trip termal-magnetik terpadu." },
  { "en": "Pin atau penanda yang menunjukkan apakah MCB telah mengalami trip karena hubung singkat?", "id": "Indikator gangguan akibat korsleting." },
  { "en": "Struktur internal yang menjaga agar busur api tidak merusak komponen mekanis di dekatnya?", "id": "Pelindung internal dari busur api." },
  { "en": "Fungsi yang memastikan isolasi yang memadai antara sirkuit utama dan sirkuit kontrol (kontak bantu)?", "id": "Fitur isolasi galvanik sirkuit." },
  { "en": "Bagian yang memastikan keselarasan yang tepat antara kontak bergerak dan kontak tetap?", "id": "Pemandu untuk keselarasan kontak." },
  { "en": "Kemampuan MCB untuk menahan arus hubung singkat untuk waktu yang singkat tanpa trip, untuk koordinasi (selektivitas)?", "id": "Ketahanan arus singkat waktu tunda." },
  { "en": "Proses dimana busur api ditarik memanjang di sepanjang elektroda di dalam arc chute?", "id": "Proses gerakan busur api." },
  { "en": "Fungsi yang mengharuskan penyesuaian (penurunan) rating arus MCB saat dipasang pada suhu lingkungan yang tinggi?", "id": "Fungsi penyesuaian rating suhu." },
  { "en": "Fungsi yang mengharuskan penyesuaian (penurunan) rating arus MCB saat dioperasikan di ketinggian (misal: pegunungan)?", "id": "Fungsi penyesuaian rating ketinggian." },
  { "en": "Proses pengujian di pabrik untuk menstabilkan komponen mekanis dan termal sebelum kalibrasi akhir?", "id": "Proses penuaan buatan komponen." },
  { "en": "Resistansi internal MCB itu sendiri yang menyebabkan panas saat dialiri arus?", "id": "Resistansi internal konduktor MCB." },
  { "en": "Daya (dalam Watt) yang hilang menjadi panas oleh MCB saat beroperasi pada arus nominalnya?", "id": "Jumlah disipasi daya (panas)." },
  { "en": "Kemampuan MCB untuk bekerja pada jaringan listrik dengan frekuensi non-standar (misal: bukan 50/60Hz)?", "id": "Kapabilitas untuk frekuensi operasi." },
  { "en": "Desain jalur arus internal yang berbentuk 'U' untuk menciptakan medan magnet yang membantu mendorong busur api?", "id": "Efek dari loop elektrodinamik." },
  { "en": "Dinamika gas di dalam MCB saat busur api memanaskan dan menekan udara untuk membantu memadamkan api?", "id": "Efek dari tekanan gas termal." },
  { "en": "Gas yang dihasilkan dari material polimer di sekitar busur api yang membantu proses deionisasi?", "id": "Gas dari ablasi polimer." },
  { "en": "Pengujian untuk memastikan MCB tidak trip secara tidak sengaja akibat getaran atau guncangan mekanis?", "id": "Uji ketahanan terhadap getaran." },
  { "en": "Batas jumlah operasi (buka-tutup) mekanis yang dapat dilakukan MCB sebelum aus?", "id": "Batas daya tahan mekanis." },
  { "en": "Batas jumlah operasi pemutusan arus (trip) pada beban nominal yang dapat dilakukan MCB?", "id": "Batas daya tahan listrik." },
  { "en": "Kegagalan dimana kontak MCB menempel/menyatu secara permanen setelah terjadi hubung singkat?", "id": "Kegagalan karena pengelasan kontak." },
  { "en": "Kegagalan dimana isolasi internal MCB rusak atau tembus akibat tegangan berlebih?", "id": "Kegagalan tembus dielektrik isolasi." },
  { "en": "Kondisi dimana pegas trip kehilangan kekuatannya seiring waktu dan siklus operasi?", "id": "Kondisi kelelahan material pegas." },
  { "en": "Sistem proteksi dimana hanya MCB terdekat dari gangguan yang trip (selektivitas)?", "id": "Sistem koordinasi proteksi (selektivitas)." },
  { "en": "Selektivitas dimana MCB hilir selalu trip lebih dulu dari MCB hulu hingga batas kapasitas pemutusan?", "id": "Jenis selektivitas total." },
  { "en": "Selektivitas dimana MCB hilir trip lebih dulu hanya sampai tingkat arus gangguan tertentu?", "id": "Jenis selektivitas parsial." },
  { "en": "Fungsi MCB hulu yang mendukung MCB hilir dengan membatasi energi hubung singkat yang melaluinya?", "id": "Fungsi proteksi cadangan (backup)." },
  { "en": "Angka pada MCB yang menunjukkan tingkat proteksi casing terhadap debu dan air?", "id": "Kode tingkat proteksi (IP)." },
  { "en": "Bahan yang digunakan pada tuas aktuator yang tidak menghantarkan listrik?", "id": "Material isolasi pada tuas." },
  { "en": "Proses dimana permukaan kontak terkikis sedikit demi sedikit setiap kali terjadi busur api?", "id": "Proses erosi pada permukaan kontak." },
  { "en": "Bentuk khusus pada permukaan kontak untuk memastikan koneksi yang baik meski ada sedikit erosi?", "id": "Bentuk geometri dari kontak." },
  { "en": "Gaya tolak-menolak antar kontak akibat arus hubung singkat yang sangat tinggi, yang membantu pemisahan awal?", "id": "Gaya tolak elektrodinamik kontak." },
  { "en": "Bagian dari terminal yang dirancang untuk menerima kabel serabut (stranded) maupun pejal (solid)?", "id": "Desain terminal yang universal." },
  { "en": "Mekanisme yang memungkinkan pemasangan aksesori (seperti kontak bantu) di sisi MCB?", "id": "Slot untuk pemasangan samping." },
  { "en": "Pengujian yang dilakukan dengan memberikan tegangan tinggi antara bagian aktif dan casing MCB?", "id": "Uji tegangan tembus (Hi-pot)." },
  { "en": "Efek dari arus distorsi harmonik yang dapat menyebabkan pemanasan berlebih pada komponen netral dan termal?", "id": "Efek pemanasan akibat harmonik." },
  { "en": "Kemampuan MCB untuk beroperasi di sirkuit DC, dimana pemadaman busur api lebih sulit?", "id": "Kemampuan aplikasi arus searah (DC)." },
  { "en": "Mekanisme pemadaman busur DC yang menggunakan magnet permanen untuk meniup busur api?", "id": "Pemadam busur dengan magnet permanen." },
  { "en": "Petunjuk polaritas (+/-) pada MCB khusus DC yang harus diikuti saat instalasi?", "id": "Penandaan polaritas untuk sirkuit DC." },
  { "en": "Fungsi yang memastikan MCB aman disentuh bahkan jika ada kegagalan internal (misal: double insulation)?", "id": "Fitur proteksi Kelas II." },
  { "en": "Bagian yang dirancang untuk pecah atau meleleh secara terkendali jika terjadi kegagalan katastropik?", "id": "Elemen sekering internal pengaman." },
  { "en": "Celah udara minimum yang dijaga antara bagian bertegangan dan bagian non-tegangan?", "id": "Jarak bebas udara (clearance)." },
  { "en": "Jarak terpendek di sepanjang permukaan isolator antara dua bagian konduktif?", "id": "Jarak rambat pada isolator." },
  { "en": "Struktur bergaris atau bergelombang pada permukaan isolator untuk memperpanjang jarak rambat?", "id": "Sirip untuk perpanjangan jarak rambat." },
  { "en": "Fungsi yang melindungi sirkuit dari lonjakan tegangan sesaat yang disebabkan oleh petir atau switching?", "id": "Ketahanan terhadap lonjakan tegangan." },
  { "en": "Penggunaan beberapa MCB 1-kutub yang dihubungkan tuasnya dengan aksesori eksternal?", "id": "Aksesori penghubung tuas (handle tie)." },
  { "en": "Bagian dari housing yang memastikan keselarasan saat dipasang berdampingan di DIN rail?", "id": "Profil pengunci antar housing." },
  { "en": "Mekanisme pengunci klip DIN rail yang dapat dioperasikan dengan obeng?", "id": "Mekanisme pelepas klip pengunci." },
  { "en": "Warna housing MCB (misal: abu-abu muda) yang sesuai standar industri?", "id": "Standar warna industri (RAL)." },
  { "en": "Proses pencetakan informasi pada housing yang tahan terhadap abrasi dan bahan kimia?", "id": "Proses pencetakan laser/pad." },
  { "en": "Simbol pada MCB yang menunjukkan kesesuaian dengan standar lingkungan (misal: RoHS)?", "id": "Sertifikasi kepatuhan standar lingkungan." },
  { "en": "Fungsi yang mencegah benda asing (misal: ujung obeng) menyentuh bagian aktif di dalam terminal?", "id": "Proteksi sentuh pada terminal." },
  { "en": "Desain terminal yang memungkinkan koneksi dua kabel dengan ukuran berbeda secara aman?", "id": "Desain terminal ganda (dual)." },
  { "en": "Kemampuan MCB untuk memutus arus kapasitif dari bank kapasitor tanpa restrike?", "id": "Kemampuan pemutusan arus kapasitif." },
  { "en": "Kemampuan MCB untuk memutus arus induktif dari beban motor atau transformator?", "id": "Kemampuan pemutusan arus induktif." },
  { "en": "Kondisi dimana busur api menyala kembali setelah pemadaman awal karena tegangan pemulihan?", "id": "Fenomena penyalaan ulang busur api." },
  { "en": "Tegangan yang muncul di antara kontak MCB sesaat setelah arus terputus?", "id": "Tegangan pemulihan transien (TRV)." },
  { "en": "Bahan yang digunakan sebagai pelumas kering pada mekanisme internal untuk mengurangi keausan?", "id": "Bahan pelumas padat internal." },
  { "en": "Bagian dari mekanisme yang berfungsi menyerap getaran dan suara saat MCB trip?", "id": "Bagian peredam kejut dan suara." },
  { "en": "Proses pembentukan lapisan oksida pada permukaan kontak yang dapat meningkatkan resistansi?", "id": "Proses oksidasi pada permukaan kontak." },
  { "en": "Fungsi dari sedikit gerakan menggesek (wiping) saat kontak menutup untuk membersihkan lapisan oksida?", "id": "Aksi pembersihan permukaan kontak." },
  { "en": "Desain tuas yang mencegah pemasangan kembali penutup panel jika MCB dalam keadaan trip?", "id": "Fitur interlock dengan penutup panel." },
  { "en": "Bagian dari housing yang dirancang untuk dipasang pada panel dengan sekrup, bukan DIN rail?", "id": "Telinga untuk pemasangan dengan sekrup." },
  { "en": "Kemampuan MCB untuk beroperasi pada sirkuit frekuensi variabel (VFD)?", "id": "Kompatibilitas dengan drive frekuensi variabel." },
  { "en": "Pengujian yang memverifikasi waktu trip MCB pada berbagai kelipatan arus nominal?", "id": "Pengujian verifikasi kurva trip." },
  { "en": "Fungsi MCB yang juga melindungi dari risiko kebakaran akibat sambungan longgar di terminalnya?", "id": "Proteksi terhadap koneksi longgar." },
  { "en": "Sensor pada tipe AFDD yang mendeteksi karakteristik unik dari busur api serial atau paralel?", "id": "Sensor pendeteksi busur api." },
  { "en": "Bagian dari housing yang dirancang untuk menahan busur api internal agar tidak keluar dari casing?", "id": "Fitur penahanan busur api internal." },
  { "en": "Karakteristik MCB yang tidak berubah secara signifikan meskipun telah disimpan dalam waktu lama?", "id": "Stabilitas produk jangka panjang." },
  { "en": "Kondisi dimana housing MCB berubah warna akibat panas berlebih dari terminal yang longgar?", "id": "Indikasi visual panas berlebih." },
  { "en": "Fungsi yang memungkinkan identifikasi sirkuit dengan menempelkan label pada MCB?", "id": "Jendela untuk label identifikasi." },
  { "en": "Bagian dari mekanisme yang memastikan kontak bergerak dengan kecepatan yang telah ditentukan?", "id": "Bagian pengatur kecepatan kontak." },
  { "en": "Desain internal yang meminimalkan efek medan magnet eksternal terhadap kinerja MCB?", "id": "Fitur imunitas medan magnet." },
  { "en": "Perbedaan waktu trip antara dua MCB yang disusun seri untuk mencapai selektivitas?", "id": "Interval waktu untuk diskriminasi." },
  { "en": "Bahan pada kontak yang memiliki titik leleh tinggi untuk menahan suhu busur api?", "id": "Material kontak tahan panas." },
  { "en": "Proses dimana ion-ion dalam plasma busur api direkombinasi menjadi partikel netral?", "id": "Proses rekombinasi ion plasma." },
  { "en": "Desain ruang pemadam api yang memaksimalkan interaksi antara busur api dan pelat pemadam?", "id": "Optimalisasi desain ruang pemadam." },
  { "en": "Mekanisme yang mencegah kontak memantul kembali dan menutup setelah trip (contact bounce)?", "id": "Mekanisme peredam pantulan kontak." },
  { "en": "Fungsi yang memastikan kinerja MCB konsisten di seluruh batch produksi?", "id": "Menjamin konsistensi proses manufaktur." },
  { "en": "Analisis yang dilakukan untuk memprediksi dan mencegah potensi kegagalan MCB?", "id": "Analisis mode dan efek kegagalan." },
  { "en": "Teknologi yang memungkinkan pemantauan status MCB dari jarak jauh secara digital?", "id": "Kapabilitas untuk komunikasi digital." },
  { "en": "Bagian dari housing yang memberikan kekuatan struktural dan ketahanan terhadap deformasi?", "id": "Tulang penguat struktur internal." },
  { "en": "Fungsi yang melindungi sirkuit pencahayaan LED yang memiliki arus awal tinggi?", "id": "Sebagai proteksi untuk beban LED." },
  { "en": "Karakteristik dimana resistansi isolasi menurun seiring dengan meningkatnya kelembaban?", "id": "Karakteristik sensitivitas terhadap kelembaban." },
  { "en": "Pengujian yang mensimulasikan kondisi lingkungan korosif (misalnya, kabut garam)?", "id": "Uji ketahanan terhadap korosi." },
  { "en": "Bagian terkecil dari mekanisme trip yang berfungsi sebagai pemicu akhir?", "id": "Pin pemicu mekanisme trip." },
  { "en": "Bagian dari plunger solenoid yang dirancang untuk memberikan gaya tumbukan maksimum?", "id": "Ujung penumbuk pada plunger." },
  { "en": "Desain yang memastikan MCB dapat didaur ulang dengan mudah di akhir masa pakainya?", "id": "Desain yang ramah daur ulang." },
  { "en": "Gaya magnet sisa pada inti solenoid setelah arus gangguan hilang?", "id": "Gaya remanensi magnetik sisa." },
  { "en": "Bahan dengan remanensi magnetik rendah yang digunakan pada inti solenoid agar cepat mereset?", "id": "Bahan besi lunak (soft iron)." },
  { "en": "Mekanisme yang secara aktif mendinginkan busur api menggunakan aliran gas?", "id": "Mekanisme pemadaman aliran gas." },
  { "en": "Sistem pada terminal yang menunjukkan jika kabel sudah terpasang dengan benar?", "id": "Indikator untuk koneksi positif." },
  { "en": "Fungsi yang memastikan MCB tidak rusak oleh arus gangguan yang lebih tinggi dari ratingnya (dengan backup)?", "id": "Ketahanan terhadap arus lewat." },
  { "en": "Desain plat pemadam api yang berbentuk 'V' untuk memecah busur api secara efisien?", "id": "Desain plat bentuk V." },
  { "en": "Kondisi dimana arus gangguan tidak cukup besar untuk trip magnetik dan terlalu singkat untuk trip termal?", "id": "Zona buta pada proteksi." },
  { "en": "Fungsi khusus MCB yang dirancang untuk melindungi sirkuit panel surya (PV)?", "id": "Aplikasi untuk fotovoltaik (PV)." },
  { "en": "Fitur yang mencegah pelepasan gas panas ke arah depan (ke arah operator)?", "id": "Fitur pengarah ventilasi aman." },
  { "en": "Proses pengelasan komponen internal menggunakan laser untuk presisi tinggi?", "id": "Proses pengelasan laser internal." },
  { "en": "Bagian dari aktuator yang terhubung dengan mekanisme common trip pada MCB multi-kutub?", "id": "Jembatan penghubung pada aktuator." },
  { "en": "Fungsi yang menjaga agar karakteristik trip tidak berubah setelah ribuan siklus operasi?", "id": "Stabilitas karakteristik jangka panjang." },
  { "en": "Kemampuan MCB untuk menahan efek penuaan akibat paparan panas dan siklus listrik?", "id": "Ketahanan terhadap penuaan termal." },
  { "en": "Mekanisme yang dapat dikunci dalam posisi OFF untuk menjamin keamanan saat pemeliharaan?", "id": "Fitur penguncian bawaan (built-in)." },
  { "en": "Rongga di dalam housing yang dirancang untuk menampung aksesori internal?", "id": "Rongga untuk aksesori internal." },
  { "en": "Fungsi yang memastikan MCB trip sebelum kabel instalasi mencapai suhu yang berbahaya?", "id": "Fungsi koordinasi energi kabel." },
  { "en": "Pengujian untuk memastikan MCB tidak terpengaruh oleh medan elektromagnetik dari perangkat lain?", "id": "Uji kompatibilitas elektromagnetik (EMC)." },
  { "en": "Bahan peredam di dalam MCB untuk mengurangi getaran suara saat trip kecepatan tinggi?", "id": "Material peredam getaran akustik." },
  { "en": "Fungsi yang memungkinkan MCB untuk digunakan sebagai saklar pemutus utama dalam kondisi tertentu?", "id": "Kesesuaian sebagai saklar pemutus." },
  { "en": "Fungsi komponen tambahan yang menyerap energi dari lonjakan tegangan untuk melindungi mekanisme internal?", "id": "Komponen penyerap lonjakan tegangan." },
  { "en": "Prinsip fisika dimana tekanan gas yang meningkat pesat akibat panas busur api membantu mendorong dan memadamkannya?", "id": "Prinsip peniupan termal busur api." },
  { "en": "Fungsi utama yang membedakan perangkat RCBO (Residual Current Circuit Breaker with Overcurrent Protection) dari MCB biasa?", "id": "Fungsi proteksi arus bocor tanah." },
  { "en": "Fungsi utama yang membedakan MCCB (Molded Case Circuit Breaker) dari MCB dalam hal pengaturan trip?", "id": "Fitur trip yang dapat diatur." },
  { "en": "Komponen pada MCB DC yang menggunakan magnet permanen untuk 'meniup' busur api ke satu arah?", "id": "Magnet permanen pemadam busur." },
  { "en": "Fungsi dari jalur busur api yang lebih panjang dan rumit pada MCB untuk aplikasi DC?", "id": "Kompensasi ketiadaan titik nol." },
  { "en": "Senyawa kimia korosif yang dapat terbentuk di dalam ruang pemadam api akibat interaksi busur dengan udara?", "id": "Senyawa oksida nitrogen (NOx)." },
  { "en": "Proses pengujian yang menempatkan MCB dalam siklus suhu ekstrem untuk menguji ketahanannya?", "id": "Uji siklus suhu ekstrem." },
  { "en": "Karakteristik yang memungkinkan MCB menahan arus masuk (inrush current) yang tinggi dari kapasitor tanpa trip?", "id": "Ketahanan terhadap arus masuk kapasitor." },
  { "en": "Desain tuas yang membutuhkan gerakan ke posisi OFF penuh sebelum bisa di-reset ke posisi ON setelah trip?", "id": "Mekanisme reset tuas positif." },
  { "en": "Fungsi pada MCB khusus PV (Photovoltaic) yang mencegah arus mengalir balik dari panel surya di malam hari?", "id": "Fungsi pemblokiran arus balik." },
  { "en": "Bagian dari standar (misal: IEC 60898-1) yang mendefinisikan MCB untuk penggunaan rumah tangga dan sejenisnya?", "id": "Standar untuk aplikasi domestik." },
  { "en": "Bagian dari standar (misal: IEC 60947-2) yang mendefinisikan pemutus sirkuit untuk penggunaan industri?", "id": "Standar untuk aplikasi industri." },
  { "en": "Tanda pada MCB yang menunjukkan bahwa ia dapat digunakan dengan aman sebagai saklar pemisah (isolator)?", "id": "Simbol sebagai saklar pemisah." },
  { "en": "Fungsi penanda visual (misalnya, pin merah) yang hanya muncul saat MCB trip karena hubung singkat, bukan beban lebih?", "id": "Indikator trip gangguan berat." },
  { "en": "Proses dimana material isolator di dekat busur api menguap, menghasilkan gas hidrogen yang membantu pendinginan?", "id": "Proses ablasi dinding isolator." },
  { "en": "Fungsi housing yang dirancang untuk menahan tekanan ledakan internal tanpa pecah atau terlepas dari DIN rail?", "id": "Integritas struktural saat gangguan." },
  { "en": "Desain internal yang meminimalkan induktansi untuk mengurangi energi yang tersimpan di sirkuit?", "id": "Desain dengan induktansi rendah." },
  { "en": "Fungsi yang memastikan resistansi kontak tetap rendah setelah ribuan siklus operasi?", "id": "Stabilitas dari resistansi kontak." },
  { "en": "Pengujian yang mengukur berapa lama MCB dapat menahan arus hubung singkat rendah tanpa rusak (untuk selektivitas)?", "id": "Uji Icw (arus tahan singkat)." },
  { "en": "Kemampuan MCB untuk membatasi arus puncak hubung singkat kurang dari yang seharusnya terjadi jika tanpa MCB?", "id": "Karakteristik sebagai pembatas arus." },
  { "en": "Desain terminal yang menggunakan pegas untuk menjepit kabel secara konstan, tidak memerlukan pengencangan ulang?", "id": "Desain terminal dengan pegas." },
  { "en": "Bahan yang digunakan pada housing yang tidak akan melepaskan gas beracun saat terbakar?", "id": "Bahan bebas dari halogen." },
  { "en": "Fungsi yang melindungi sirkuit elektronik dari gangguan frekuensi radio yang dihasilkan saat switching?", "id": "Filter internal untuk EMI/RFI." },
  { "en": "Bagian dari mekanisme yang dirancang untuk gagal pada titik tertentu untuk melindungi bagian yang lebih mahal?", "id": "Titik kegagalan yang terencana." },
  { "en": "Fungsi yang memungkinkan beberapa tuas MCB dioperasikan secara bersamaan menggunakan satu tuas putar eksternal?", "id": "Adaptor untuk tuas putar." },
  { "en": "Proses pembersihan kontak secara otomatis saat MCB dioperasikan secara manual?", "id": "Sistem kontak pembersihan mandiri." },
  { "en": "Kemampuan material isolasi untuk tidak membentuk jalur konduktif (carbon tracking) saat terkena busur api?", "id": "Ketahanan terhadap jejak karbon." },
  { "en": "Desain yang memastikan keseragaman medan magnet di sekitar bimetal untuk trip yang konsisten?", "id": "Optimalisasi jalur fluks magnetik." },
  { "en": "Fungsi yang memungkinkan MCB untuk trip saat mendeteksi frekuensi yang salah pada sirkuit?", "id": "Proteksi terhadap frekuensi abnormal." },
  { "en": "Bagian dari housing yang memiliki permukaan kasar untuk meningkatkan pelepasan panas ke udara?", "id": "Permukaan sebagai radiator panas." },
  { "en": "Mekanisme internal yang memberikan waktu tunda singkat sebelum trip magnetik untuk menghindari trip palsu?", "id": "Peredam hidrolik-magnetik internal." },
  { "en": "Fungsi yang memastikan MCB tidak dapat dipasang terbalik pada DIN rail?", "id": "Fitur pemasangan anti-terbalik." },
  { "en": "Kode QR pada MCB modern yang terhubung ke lembar data teknis atau manual instalasi?", "id": "Tautan data teknis digital." },
  { "en": "Lapisan pada komponen internal yang tahan terhadap jamur dan kelembaban untuk aplikasi tropis?", "id": "Lapisan pelindung anti-jamur." },
  { "en": "Fungsi yang memastikan MCB tetap beroperasi dalam lingkungan dengan tingkat salinitas tinggi (aplikasi kelautan)?", "id": "Ketahanan terhadap korosi garam." },
  { "en": "Desain terminal yang memungkinkan pengujian tegangan tanpa melepas kabel?", "id": "Port untuk uji tegangan." },
  { "en": "Proses produksi dimana setiap MCB diuji secara otomatis oleh sistem visi komputer untuk kelengkapan?", "id": "Inspeksi visual otomatis (AVI)." },
  { "en": "Bagian dari mekanisme pengunci yang memiliki permukaan yang dikeraskan untuk mengurangi keausan?", "id": "Permukaan kontak mekanisme pengunci." },
  { "en": "Karakteristik dimana bimetal dirancang untuk mengkompensasi perubahan suhu lingkungan?", "id": "Bimetal dengan kompensasi suhu." },
  { "en": "Fungsi yang melindungi dari bahaya arus lebih yang disebabkan oleh kegagalan dioda pada penyearah?", "id": "Proteksi untuk sirkuit penyearah." },
  { "en": "Kemampuan MCB untuk menahan getaran sinusoidal dan acak tanpa mengalami kerusakan mekanis atau trip palsu?", "id": "Ketahanan getaran lebar-spektrum." },
  { "en": "Desain ventilasi gas yang meminimalkan emisi partikel panas yang dapat menyulut material di sekitarnya?", "id": "Sistem ventilasi tanpa api." },
  { "en": "Fungsi dari rongga udara yang dirancang khusus di dalam MCB untuk meredam gelombang tekanan?", "id": "Rongga sebagai peredam tekanan." },
  { "en": "Bagian dari kontak yang dirancang untuk menangani pembentukan busur api, melindungi area kontak utama?", "id": "Ujung kontak untuk busur api." },
  { "en": "Area pada kontak yang menghantarkan arus dalam kondisi tertutup normal dengan resistansi terendah?", "id": "Area kontak utama penghantar." },
  { "en": "Urutan dimana ujung kontak busur terhubung terakhir dan terputus pertama untuk melindungi kontak utama?", "id": "Prinsip kerja kontak utama-busur." },
  { "en": "Kemampuan MCB untuk dipasang berdekatan tanpa saling mempengaruhi kinerja termalnya?", "id": "Independensi pemasangan secara berdampingan." },
  { "en": "Fungsi yang memastikan arus hubung singkat diputus dalam waktu kurang dari setengah siklus gelombang AC?", "id": "Pemutusan dalam setengah siklus." },
  { "en": "Proses dimana busur api 'dijepit' secara magnetis di antara dua pelat untuk memadamkannya?", "id": "Proses penjepitan magnetik busur api." },
  { "en": "Bahan dengan konduktivitas termal tinggi yang digunakan untuk menyebarkan panas dari titik panas lokal?", "id": "Komponen penyebar panas (heat spreader)." },
  { "en": "Fungsi yang memungkinkan MCB dipasang pada berbagai jenis DIN rail (misal: tipe G atau C)?", "id": "Kompatibilitas dengan multi-DIN rail." },
  { "en": "Desain casing yang meminimalkan celah untuk mencegah masuknya serangga kecil?", "id": "Desain casing anti-serangga." },
  { "en": "Bagian dari mekanisme yang dirancang untuk memiliki momen inersia rendah agar dapat bergerak cepat?", "id": "Komponen bergerak yang ringan." },
  { "en": "Fungsi yang memastikan MCB akan trip bahkan jika mekanisme reset macet atau rusak?", "id": "Mekanisme anti-gagal (fail-safe)." },
  { "en": "Penggunaan material berbeda pada tuas dan housing untuk memberikan kontras visual yang jelas?", "id": "Indikasi status dengan kontras warna." },
  { "en": "Karakteristik dimana level trip magnetik sedikit meningkat seiring meningkatnya suhu untuk stabilitas?", "id": "Kompensasi suhu untuk trip magnetik." },
  { "en": "Fungsi yang melindungi peralatan medis sensitif dari gangguan listrik sekecil apa pun?", "id": "Aplikasi untuk kelas medis." },
  { "en": "Desain terminal yang menyalurkan panas dari kabel ke badan MCB untuk didisipasikan?", "id": "Terminal sebagai penyerap panas." },
  { "en": "Proses kalibrasi yang menggunakan laser untuk memotong sebagian kecil dari bimetal demi presisi tinggi?", "id": "Proses kalibrasi dengan laser." },
  { "en": "Fungsi yang memastikan MCB tidak akan trip oleh arus start motor yang normal sesuai kurvanya?", "id": "Diskriminasi terhadap arus start motor." },
  { "en": "Bagian dari solenoid yang dirancang untuk menghasilkan gaya maksimum pada langkah awal gerakannya?", "id": "Optimalisasi gaya pada plunger." },
  { "en": "Karakteristik dimana MCB akan trip lebih cepat pada frekuensi yang lebih tinggi karena efek kulit (skin effect)?", "id": "Karakteristik sensitivitas terhadap frekuensi." },
  { "en": "Kemampuan MCB untuk menahan arus puncak yang sangat tinggi dalam durasi sangat singkat (mikrodetik)?", "id": "Ketahanan terhadap arus puncak (Ipeak)." },
  { "en": "Fungsi dari lapisan pernis pada kumparan solenoid untuk mencegah korosi dan hubung singkat antar lilitan?", "id": "Enamel sebagai isolasi kawat." },
  { "en": "Bagian dari housing yang memiliki nomor cetakan untuk melacak batch produksi material plastik?", "id": "Penanda cetakan untuk pelacakan." },
  { "en": "Fungsi yang memastikan kekuatan penguncian klip DIN rail cukup kuat untuk menahan gaya saat trip?", "id": "Kekuatan retensi pada DIN rail." },
  { "en": "Desain internal yang memastikan jarak aman dari percikan api ke dinding panel logam?", "id": "Jarak aman pelepasan gas." },
  { "en": "Kemampuan MCB untuk membatasi tegangan busur api di bawah level yang dapat merusak isolasi kabel?", "id": "Kemampuan mengontrol tegangan busur." },
  { "en": "Fungsi yang memungkinkan beberapa MCB 1-kutub digunakan dalam sirkuit 3-fasa tanpa netral?", "id": "Aplikasi multi-fasa tanpa netral." },
  { "en": "Pengujian yang mensimulasikan seumur hidup produk dalam waktu yang dipercepat?", "id": "Uji kehidupan produk dipercepat." },
  { "en": "Bagian dari mekanisme yang berfungsi sebagai 'memori' mekanis untuk posisi trip?", "id": "Kait memori posisi trip." },
  { "en": "Desain yang meminimalkan jumlah komponen bergerak untuk meningkatkan keandalan?", "id": "Desain mekanis yang sederhana." },
  { "en": "Fungsi yang mencegah akumulasi kelembaban di dalam MCB melalui siklus pernapasan (breathing)?", "id": "Sistem ventilasi anti-kondensasi." },
  { "en": "Bahan penyerap energi di sekitar mekanisme untuk mengurangi guncangan mekanis saat trip?", "id": "Bantalan peredam dari elastomer." },
  { "en": "Karakteristik dimana MCB tipe tertentu dapat trip oleh arus DC yang berdenyut (pulsating DC)?", "id": "Sensitivitas terhadap arus DC berdenyut." },
  { "en": "Fungsi yang melindungi dari arus berlebih pada sirkuit kontrol atau sinyal berdaya rendah?", "id": "Proteksi untuk sirkuit tambahan." },
  { "en": "Desain yang memastikan keseragaman aliran udara pendingin ketika beberapa MCB dipasang dalam satu baris?", "id": "Manajemen aliran udara grup." },
  { "en": "Fungsi dari lubang kalibrasi yang disegel dengan resin atau lak untuk mencegah perubahan setelan?", "id": "Penyegelan permanen lubang kalibrasi." },
  { "en": "Kemampuan MCB untuk berfungsi dalam lingkungan vakum atau bertekanan rendah (aplikasi luar angkasa/penerbangan)?", "id": "Kapabilitas operasi tekanan rendah." },
  { "en": "Fungsi yang memungkinkan pengujian mekanisme trip secara manual tanpa mengalirkan arus?", "id": "Tombol tes untuk mekanisme." },
  { "en": "Bagian dari housing yang memiliki permukaan halus untuk mencegah penumpukan kotoran?", "id": "Finishing permukaan anti-kotoran." },
  { "en": "Struktur internal yang bertindak sebagai pemandu gelombang tekanan untuk mengarahkannya ke ventilasi?", "id": "Saluran pemandu gelombang kejut." },
  { "en": "Fungsi yang melindungi dari pemanasan berlebih akibat koneksi terminal yang tidak sempurna (resistansi tinggi)?", "id": "Deteksi panas pada terminal." },
  { "en": "Desain kontak yang sedikit berputar saat menutup untuk memecah lapisan kontaminasi tipis?", "id": "Gerakan putar pembersihan kontak." },
  { "en": "Fungsi yang memastikan MCB dapat menahan arus hubung singkat dari dua arah (atas dan bawah)?", "id": "Kapabilitas line/load bolak-balik." },
  { "en": "Proses dimana kekuatan medan magnet yang dibutuhkan untuk trip berkurang seiring dengan penutupan celah udara di solenoid?", "id": "Karakteristik gaya-langkah invers." },
  { "en": "Fungsi yang memastikan bahwa trip karena gangguan tidak dapat di-reset jika gangguan masih ada?", "id": "Fungsi anti-reset saat gangguan." },
  { "en": "Bagian dari arc chute yang terbuat dari material yang menyerap energi panas dari busur api?", "id": "Bahan penyerap energi termal." },
  { "en": "Fungsi yang memungkinkan MCB untuk berkoordinasi dengan sekring (fuse) di hulu atau hilir?", "id": "Fitur koordinasi dengan sekring." },
  { "en": "Desain mekanisme yang memastikan waktu trip total (dari deteksi hingga padam) sangat singkat?", "id": "Optimalisasi waktu pemutusan total." },
  { "en": "Kemampuan MCB untuk menahan paparan bahan kimia industri tanpa merusak housing atau labelnya?", "id": "Fitur ketahanan terhadap bahan kimia." },
  { "en": "Fungsi yang melindungi dari kondisi dimana satu fasa dari sistem tiga fasa hilang?", "id": "Proteksi dari kehilangan fasa." },
  { "en": "Desain kontak bantu yang memiliki aksi 'break-before-make' untuk aplikasi kontrol yang aman?", "id": "Logika kontak bantu (break-before-make)." },
  { "en": "Fungsi dari penanda torsi pada terminal yang berubah warna atau bentuk saat torsi yang benar tercapai?", "id": "Indikator visual untuk torsi." },
  { "en": "Bagian dari mekanisme yang dirancang untuk dapat diproduksi secara massal dengan toleransi yang sangat ketat?", "id": "Komponen cetakan presisi tinggi." },
  { "en": "Fungsi yang memastikan bahwa bahkan kegagalan satu komponen kecil tidak akan menghalangi fungsi trip yang aman?", "id": "Redundansi pada mekanisme keselamatan." },
  { "en": "Fungsi dari pemilihan material polikarbonat atau PBT untuk housing, yang berhubungan dengan kekuatan dan stabilitas dimensi?", "id": "Memberikan ketahanan mekanis dan termal." },
  { "en": "Fungsi dari desain 'double break' pada beberapa MCB, dimana sirkuit diputus di dua titik secara bersamaan?", "id": "Membagi tegangan busur api." },
  { "en": "Apa fungsi dari celah udara yang presisi antara inti solenoid dan plunger dalam kondisi normal?", "id": "Menentukan sensitivitas trip magnetik." },
  { "en": "Fungsi dari parameter I²t (Let-through energy) yang tertera pada datasheet MCB?", "id": "Untuk koordinasi proteksi dan kabel." },
  { "en": "Mengapa plat-plat pada pemadam busur api (arc chute) harus terisolasi satu sama lain?", "id": "Menaikkan total tegangan busur api." },
  { "en": "Fungsi dari bentuk kontak yang agak membulat (cembung)?", "id": "Memastikan koneksi resistansi rendah." },
  { "en": "Fungsi dari sifat 'trip-free' yang secara sengaja memisahkan tuas dari mekanisme pengunci internal?", "id": "Menjamin sirkuit tetap terproteksi." },
  { "en": "Fungsi dari adanya sedikit 'overtravel' atau gerakan lebih pada tuas setelah kontak menutup?", "id": "Memberikan gaya kontak yang stabil." },
  { "en": "Fungsi dari proses riveting atau pengelasan presisi untuk menghubungkan strip bimetal ke konduktor?", "id": "Memastikan transfer panas yang efisien." },
  { "en": "Fungsi dari karakteristik 'waktu-invers' pada elemen termal (bimetal)?", "id": "Memberikan toleransi arus start motor." },
  { "en": "Apa fungsi dari pemilihan tembaga atau paduan tembaga sebagai material konduktor internal?", "id": "Meminimalkan kerugian daya internal." },
  { "en": "Fungsi dari pegas yang kuat pada mekanisme trip untuk menciptakan pemisahan kontak yang cepat?", "id": "Mengurangi durasi busur api." },
  { "en": "Apa fungsi dari standarisasi ukuran housing (misal: lebar per kutub 17.5mm)?", "id": "Memastikan interoperabilitas antar merek." },
  { "en": "Fungsi dari adanya sedikit jeda waktu (inherent delay) pada trip magnetik?", "id": "Mencegah trip palsu sesaat." },
  { "en": "Fungsi dari bahan keramik pada beberapa bagian di dekat busur api?", "id": "Sebagai isolator tahan suhu tinggi." },
  { "en": "Mengapa trip magnetik harus bereaksi jauh lebih cepat daripada trip termal?", "id": "Karena bahaya korsleting lebih besar." },
  { "en": "Fungasi dari 'tegangan busur' (arc voltage) yang sengaja dinaikkan oleh arc chute?", "id": "Menghentikan aliran arus listrik." },
  { "en": "Apa fungsi dari desain terminal yang menjepit kabel pada dua sisi?", "id": "Memberikan koneksi yang lebih aman." },
  { "en": "Fungsi dari casing yang disegel (sealed) setelah perakitan di pabrik?", "id": "Mencegah manipulasi kalibrasi internal." },
  { "en": "Apa fungsi dari kalibrasi pada suhu referensi tertentu (misal: 30°C)?", "id": "Memastikan kinerja trip konsisten." },
  { "en": "Fungsi dari lubang ventilasi yang dirancang untuk mengarahkan gas panas menjauh dari komponen sensitif?", "id": "Mencegah kerusakan internal akibat panas." },
  { "en": "Apa fungsi dari pengujian 'short-circuit making capacity' (Icm)?", "id": "Memastikan penutupan sirkuit yang aman." },
  { "en": "Fungsi dari pemilihan material dengan koefisien gesek rendah pada titik pivot mekanisme?", "id": "Memastikan mekanisme bergerak lancar." },
  { "en": "Mengapa MCB untuk aplikasi motor (Tipe D/K) memiliki ambang trip magnetik yang lebih tinggi?", "id": "Menahan arus start motor tinggi." },
  { "en": "Fungsi dari penambahan lapisan perak atau paduannya pada permukaan kontak?", "id": "Meningkatkan konduktivitas dan ketahanan." },
  { "en": "Apa fungsi dari desain MCB multi-kutub yang memastikan semua kutub trip bersamaan (common trip)?", "id": "Mencegah kondisi fasa tidak seimbang." },
  { "en": "Fungsi dari adanya ruang kosong yang cukup di dalam casing MCB?", "id": "Sebagai ruang ekspansi gas panas." },
  { "en": "Fungsi dari kekakuan (rigidity) struktur rangka internal MCB?", "id": "Menahan gaya elektromagnetik kuat." },
  { "en": "Apa fungsi dari pengujian ketahanan terhadap kelembaban (humidity test)?", "id": "Memastikan kinerja isolasi tetap baik." },
  { "en": "Fungsi dari mekanisme 'snap action' yang memastikan kontak membuka atau menutup dengan cepat, terlepas dari kecepatan gerakan tuas?", "id": "Meminimalkan pembentukan busur api." },
  { "en": "Mengapa MCB untuk sirkuit elektronik (Tipe B/Z) memiliki ambang trip magnetik yang sangat rendah?", "id": "Memberikan proteksi sangat cepat." },
  { "en": "Fungsi dari kurva trip yang dipublikasikan oleh pabrikan?", "id": "Sebagai alat bantu desain sistem." },
  { "en": "Fungsi dari perlakuan panas (heat treatment) pada komponen logam seperti pegas dan kait pengunci?", "id": "Meningkatkan kekuatan dan ketahanan material." },
  { "en": "Apa fungsi dari desain yang meminimalkan jumlah total bagian (part count)?", "id": "Meningkatkan keandalan dan efisiensi produksi." },
  { "en": "Fungsi dari penandaan yang jelas antara terminal Line (masuk) dan Load (keluar) pada beberapa tipe MCB?", "id": "Memastikan instalasi yang benar." },
  { "en": "Fungsi dari penggunaan material non-magnetik di dekat mekanisme trip termal?", "id": "Mencegah interferensi medan magnet." },
  { "en": "Apa fungsi dari pengujian daya tahan listrik (electrical endurance test) pada arus nominal?", "id": "Memverifikasi fungsi saklar berulang." },
  { "en": "Fungsi dari desain terminal yang dapat menerima baik busbar maupun kabel?", "id": "Memberikan fleksibilitas saat instalasi." },
  { "en": "Mengapa bentuk plunger solenoid dirancang untuk memiliki massa tertentu?", "id": "Memberikan momentum tumbukan yang cukup." },
  { "en": "Fungsi dari celah isolasi yang dijaga antara kutub-kutub pada MCB multi-kutub?", "id": "Mencegah loncatan api antar fasa." },
  { "en": "Fungsi dari pemilihan material tuas yang tahan terhadap sinar UV (untuk aplikasi outdoor)?", "id": "Mencegah material menjadi rapuh." },
  { "en": "Fungsi dari tekstur pada permukaan housing?", "id": "Menyembunyikan goresan dan estetika." },
  { "en": "Apa fungsi dari 'waktu pra-busur' (pre-arcing time) dalam proses trip?", "id": "Waktu reaksi internal mekanisme." },
  { "en": "Apa fungsi dari 'waktu busur' (arcing time) dalam proses trip?", "id": "Durasi nyala busur api." },
  { "en": "Fungsi dari total waktu pemutusan (total clearing time) yang merupakan penjumlahan waktu pra-busur dan waktu busur?", "id": "Menentukan total durasi gangguan." },
  { "en": "Fungsi dari sifat material housing yang 'self-extinguishing'?", "id": "Mencegah penyebaran api internal." },
  { "en": "Fungsi dari penandaan rating tegangan (misal: 230/400V)?", "id": "Menunjukkan batas tegangan operasi aman." },
  { "en": "Fungsi dari pengujian kekuatan dielektrik pada frekuensi daya (power-frequency withstand test)?", "id": "Memastikan isolasi tahan tegangan lebih." },
  { "en": "Fungsi dari pengujian ketahanan tegangan impuls (impulse withstand test)?", "id": "Memverifikasi ketahanan terhadap sambaran petir." },
  { "en": "Fungsi dari penggunaan baut atau sekrup non-ferrous (seperti kuningan) pada beberapa bagian internal?", "id": "Menghindari pemanasan akibat arus eddy." },
  { "en": "Fungsi dari desain yang memastikan pendinginan yang efisien saat MCB dipasang secara vertikal?", "id": "Memanfaatkan aliran udara konveksi." },
  { "en": "Mengapa beberapa MCB industri memiliki badan yang lebih besar dari MCB domestik dengan rating yang sama?", "id": "Untuk kapasitas pemutusan lebih tinggi." },
  { "en": "Fungsi dari penggunaan pelumas khusus pada mekanisme perakitan?", "id": "Memastikan operasi mulus seumur hidup." },
  { "en": "Apa fungsi dari desain 'anti-tamper' pada baut kalibrasi?", "id": "Mencegah perubahan pengaturan trip." },
  { "en": "Fungsi dari ketebalan dinding housing yang seragam?", "id": "Memastikan kekuatan mekanis merata." },
  { "en": "Fungsi dari penggunaan paduan khusus pada strip bimetal yang memiliki resistivitas yang stabil?", "id": "Memastikan waktu trip yang konsisten." },
  { "en": "Apa fungsi dari 'gaya kontak balik' (contact bounce) yang harus diminimalkan?", "id": "Mencegah pengelasan kontak dini." },
  { "en": "Fungsi dari desain yang memungkinkan aliran gas hasil busur api keluar tanpa mengganggu MCB di sebelahnya?", "id": "Menjaga integritas operasional panel." },
  { "en": "Apa fungsi dari pengujian penandaan (marking test) menggunakan bahan kimia?", "id": "Memastikan label tetap terbaca." },
  { "en": "Fungsi dari adanya pin atau tonjolan pemandu (guiding pin) pada perakitan internal?", "id": "Memastikan posisi komponen akurat." },
  { "en": "Mengapa kumparan solenoid memiliki jumlah lilitan yang spesifik?", "id": "Menentukan kekuatan medan magnet." },
  { "en": "Fungsi dari bentuk 'kandang' pada terminal (box lug)?", "id": "Melindungi untaian kabel serabut." },
  { "en": "Fungsi dari warna tuas yang berbeda-beda pada beberapa merek (misal: hijau untuk ON, merah untuk OFF)?", "id": "Memberikan indikasi status cepat." },
  { "en": "Fungsi dari pengujian siklus hidup mekanis tanpa beban listrik?", "id": "Memverifikasi ketahanan komponen mekanis." },
  { "en": "Apa fungsi dari kekuatan penahan (holding force) pada mekanisme pengunci?", "id": "Menahan kontak saat ada getaran." },
  { "en": "Fungsi dari desain yang memungkinkan aksesori dipasang atau dilepas di lapangan?", "id": "Memberikan fleksibilitas dan kemampuan upgrade." },
  { "en": "Fungsi dari penggunaan rivet berongga pada beberapa titik perakitan?", "id": "Sebagai titik pivot yang kuat." },
  { "en": "Fungsi dari adanya sedikit lengkungan pada batang penghubung trip (trip bar)?", "id": "Mengkompensasi toleransi manufaktur kecil." },
  { "en": "Apa fungsi dari pengujian resistansi isolasi (insulation resistance test)?", "id": "Memastikan tidak ada arus bocor." },
  { "en": "Fungsi dari pemilihan bahan isolasi dengan Comparative Tracking Index (CTI) yang tinggi?", "id": "Menunjukkan ketahanan terhadap jejak karbon." },
  { "en": "Mengapa beberapa bagian internal dilapisi timah (tin-plated)?", "id": "Meningkatkan kemampuan solder dan korosi." },
  { "en": "Fungsi dari bentuk plunger solenoid yang meruncing (tapered)?", "id": "Memberikan aktivasi yang lebih terkontrol." },
  { "en": "Fungsi dari penambahan 'magnetic shunt' pada beberapa desain trip termal?", "id": "Mencegahnya mempengaruhi kinerja bimetal." },
  { "en": "Apa fungsi dari 'gaya lepas' (release force) yang dibutuhkan oleh mekanisme pengunci?", "id": "Menentukan sensitivitas trip MCB." },
  { "en": "Fungsi dari penggunaan perakitan otomatis (robotic assembly) dalam produksi MCB?", "id": "Mencapai konsistensi dan presisi tinggi." },
  { "en": "Apa fungsi dari 'koordinasi energi' antara MCB dan perangkat hilir?", "id": "Memastikan perangkat hilir tidak rusak." },
  { "en": "Fungsi dari penggunaan plastik termoset (thermoset) dibandingkan termoplastik (thermoplastic) pada beberapa MCB industri?", "id": "Memberikan stabilitas suhu superior." },
  { "en": "Apa fungsi dari desain terminal yang memiliki penghalang antara sekrup dan kabel?", "id": "Mencegah kerusakan untaian kabel." },
  { "en": "Fungsi dari pengujian torsi maksimum pada terminal?", "id": "Memastikan terminal tidak rusak." },
  { "en": "Fungsi dari desain kontak bantu (auxiliary contact) yang terisolasi secara galvanik dari sirkuit utama?", "id": "Memungkinkan pensinyalan atau kontrol aman." },
  { "en": "Fungsi dari adanya nomor paten yang tertera pada housing MCB?", "id": "Melindungi inovasi desain unik." },
  { "en": "Apa fungsi dari 'efek kulit' (skin effect) yang menjadi relevan pada frekuensi tinggi?", "id": "Meningkatkan resistansi efektif konduktor." },
  { "en": "Fungsi dari desain yang memastikan tidak ada bagian logam yang terekspos saat MCB terpasang di panel?", "id": "Memberikan proteksi sentuh lengkap." },
  { "en": "Fungsi dari penggunaan material komposit pada beberapa komponen mekanis?", "id": "Mendapatkan kombinasi sifat terbaik." },
  { "en": "Apa fungsi dari 'reset time' setelah trip termal?", "id": "Mencegah sirkuit langsung dihidupkan." },
  { "en": "Fungsi dari desain yang memungkinkan udara bersirkulasi di antara MCB yang dipasang berdampingan?", "id": "Membantu pembuangan panas berlebih." },
  { "en": "Fungsi dari penggunaan pegas daun (leaf spring) dibandingkan pegas koil (coil spring) pada beberapa mekanisme?", "id": "Memberikan profil gaya lebih spesifik." },
  { "en": "Apa fungsi dari 'tegangan kerja kontinu maksimum' (Uc)?", "id": "Menunjukkan batas tegangan operasi kontinu." },
  { "en": "Fungsi dari penandaan kelas pembatas energi (misal: Kelas 3)?", "id": "Menunjukkan efektivitas pembatasan energi." },
  { "en": "Fungsi dari desain simetris internal pada beberapa MCB?", "id": "Menyederhanakan proses perakitan otomatis." },
  { "en": "Fungsi dari pengujian 'glow wire test' pada material plastik?", "id": "Memastikan plastik tidak mudah terbakar." },
  { "en": "Apa fungsi dari 'gaya sisa' (residual force) pada pegas kontak saat MCB dalam posisi ON?", "id": "Memastikan kontak tetap tertekan kuat." },
  { "en": "Fungsi dari desain yang menempatkan mekanisme trip jauh dari sumber panas utama (kontak dan terminal)?", "id": "Meminimalkan pengaruh panas eksternal." },
  { "en": "Fungsi dari toleransi manufaktur yang sangat ketat pada celah busur api (arc gap)?", "id": "Memastikan busur api padam konsisten." },
  { "en": "Fungsi dari pengujian akhir 'end-of-line' yang memeriksa 100% produk?", "id": "Menangkap setiap cacat produksi." },
  { "en": "Apa fungsi dari desain modular yang memungkinkan kombinasi MCB dan RCD menjadi satu unit RCBO?", "id": "Menghemat ruang dan menyederhanakan pengkabelan." },
  { "en": "Fungsi dari analisis elemen hingga (Finite Element Analysis - FEA) dalam proses desain MCB?", "id": "Mensimulasikan dan mengoptimalkan desain." },
  { "en": "Fungsi dari 'lutut' (knee) pada kurva trip yang menunjukkan titik transisi antara operasi termal dan magnetik?", "id": "Mendefinisikan transisi proteksi termal-magnetik." },
  { "en": "Fungsi dari toleransi 'band' atau pita pada kurva trip yang ditentukan oleh standar?", "id": "Memberikan rentang kinerja yang diizinkan." },
  { "en": "Apa fungsi dari standar IEC 60898 yang secara spesifik membatasi MCB untuk penggunaan oleh orang awam (non-profesional)?", "id": "Menjamin keamanan untuk pengguna awam." },
  { "en": "Fungsi dari penggunaan material dengan ketahanan 'arc tracking' yang tinggi pada isolator internal?", "id": "Mencegah pembentukan jejak karbon." },
  { "en": "Fungsi dari pengujian 'power-on-hours' (jam menyala) dalam validasi desain?", "id": "Mensimulasikan penuaan komponen elektronik." },
  { "en": "Apa fungsi dari 'koordinasi selektivitas' berdasarkan energi (I²t) dibandingkan hanya berdasarkan arus?", "id": "Memberikan diskriminasi yang lebih akurat." },
  { "en": "Fungsi dari desain terminal 'tunnel' atau 'elevator'?", "id": "Memastikan kabel terjepit dengan merata." },
  { "en": "Fungsi dari penambahan sirkuit 'snubber' pada beberapa MCB untuk aplikasi switching beban induktif?", "id": "Menekan lonjakan tegangan induktif." },
  { "en": "Apa fungsi dari penggunaan 'magnetic latching' pada beberapa tipe relay atau pemutus sirkuit?", "id": "Menjaga posisi kontak tanpa daya." },
  { "en": "Fungsi dari 'pigtail' fleksibel yang menghubungkan kontak bergerak ke terminal?", "id": "Memungkinkan kontak bergerak bebas." },
  { "en": "Mengapa celah udara (air gap) pada kontak MCB DC harus lebih besar daripada MCB AC?", "id": "Membantu pemadaman busur api DC." },
  { "en": "Fungsi dari pengujian 'salt mist' (kabut garam) untuk MCB aplikasi kelautan (marine)?", "id": "Memverifikasi ketahanan terhadap korosi garam." },
  { "en": "Fungsi dari penggunaan sekrup terminal dengan kepala kombinasi (slot dan phillips)?", "id": "Memberikan fleksibilitas penggunaan obeng." },
  { "en": "Apa fungsi dari 'waktu tunda tetap' (fixed time delay) yang sengaja ditambahkan pada beberapa pemutus sirkuit?", "id": "Memungkinkan koordinasi proteksi lebih baik." },
  { "en": "Fungsi dari analisis statistik (seperti Cpk atau Ppk) dalam kontrol kualitas produksi MCB?", "id": "Memastikan proses produksi konsisten." },
  { "en": "Fungsi dari 'derating' untuk pemasangan berkelompok (group installation)?", "id": "Mengkompensasi kenaikan suhu gabungan." },
  { "en": "Apa fungsi dari kode lacak 'batch' atau 'lot' pada setiap unit MCB?", "id": "Memungkinkan penelusuran riwayat produksi." },
  { "en": "Fungsi dari desain yang memastikan terminal input dan output memiliki kapasitas panas yang serupa?", "id": "Mencegah ketidakseimbangan termal internal." },
  { "en": "Fungsi dari penggunaan 'kontak perak-grafit' (silver-graphite) pada beberapa aplikasi?", "id": "Memberikan pelumasan dan konduktivitas." },
  { "en": "Mengapa mekanisme trip magnetik harus independen dari orientasi pemasangan MCB?", "id": "Memastikan gravitasi tidak mempengaruhi kinerja." },
  { "en": "Fungsi dari pengujian 'mechanical shock' yang mensimulasikan benturan keras?", "id": "Memastikan MCB tidak trip palsu." },
  { "en": "Apa fungsi dari 'tegangan pemulihan' (recovery voltage) sebagai parameter desain?", "id": "Mencegah busur api menyala kembali." },
  { "en": "Fungsi dari penggunaan bahan peredam getaran (damping material) pada pemasangan MCB untuk aplikasi kereta api?", "id": "Mengisolasi MCB dari getaran konstan." },
  { "en": "Fungsi dari sertifikasi ATEX untuk MCB yang digunakan di area berbahaya (misal: tambang, kilang minyak)?", "id": "Mencegah MCB menjadi sumber ledakan." },
  { "en": "Apa fungsi dari 'arus non-tripping' (non-tripping current) yang ditentukan dalam standar?", "id": "Menjamin stabilitas operasi normal." },
  { "en": "Fungsi dari pengujian 'electromagnetic compatibility' (EMC)?", "id": "Memastikan MCB tidak saling mengganggu." },
  { "en": "Mengapa casing MCB seringkali dibuat dari bahan polimer daripada logam?", "id": "Memberikan isolasi listrik yang melekat." },
  { "en": "Fungsi dari 'zona trip seketika' (instantaneous trip zone) yang datar pada kurva?", "id": "Menunjukkan waktu trip mekanis tercepat." },
  { "en": "Fungsi dari penggunaan pengikat kabel (cable tie) atau partisi pada panel untuk memisahkan kabel daya dan sinyal?", "id": "Mencegah interferensi sinyal (crosstalk)." },
  { "en": "Fungsi dari 'faktor daya' (power factor) sirkuit uji saat memverifikasi kapasitas pemutusan?", "id": "Mensimulasikan kondisi kasus terburuk." },
  { "en": "Apa fungsi dari desain tuas yang tersembunyi (recessed) pada beberapa tipe?", "id": "Mencegah pengoperasian tidak disengaja." },
  { "en": "Fungsi dari 'kontak bantu' yang memiliki logika 'changeover' (CO)?", "id": "Memberikan sinyal NO dan NC." },
  { "en": "Fungsi dari pengujian 'overvoltage withstand' (ketahanan tegangan lebih) untuk komponen elektronik di RCBO?", "id": "Memastikan sirkuit elektronik tidak rusak." },
  { "en": "Fungsi dari desain yang memungkinkan penggantian kontak (pada pemutus sirkuit yang lebih besar)?", "id": "Memperpanjang masa pakai perangkat." },
  { "en": "Apa fungsi dari 'kalibrasi trip' yang dilakukan pada beberapa titik arus yang berbeda?", "id": "Memastikan seluruh kurva trip akurat." },
  { "en": "Fungsi dari penandaan 'suhu kalibrasi' pada badan MCB?", "id": "Memberitahu suhu referensi kinerja." },
  { "en": "Fungsi dari penggunaan 'busbar' berbentuk sisir (comb busbar) yang terisolasi?", "id": "Menyederhanakan dan merapikan pengkabelan." },
  { "en": "Mengapa bagian dalam arc chute terkadang dibuat bergerigi atau tidak rata?", "id": "Menciptakan turbulensi gas panas." },
  { "en": "Fungsi dari adanya 'jendela' kecil yang menunjukkan status kontak secara mekanis (merah/hijau)?", "id": "Memberikan indikasi status kontak langsung." },
  { "en": "Apa fungsi dari 'shunt trip' yang memungkinkan MCB ditripkan dari jarak jauh?", "id": "Untuk aplikasi pemutusan darurat." },
  { "en": "Fungsi dari penggunaan beberapa pegas dengan kekuatan berbeda dalam satu mekanisme?", "id": "Mengoptimalkan profil gaya operasi." },
  { "en": "Fungsi dari 'analisis toleransi' dalam tahap desain mekanis?", "id": "Mencegah mekanisme macet atau gagal." },
  { "en": "Mengapa beberapa terminal MCB dirancang untuk dikencangkan dengan kunci torsi?", "id": "Menjamin koneksi listrik yang optimal." },
  { "en": "Fungsi dari lapisan 'passivation' pada komponen baja atau tembaga?", "id": "Meningkatkan ketahanan terhadap oksidasi." },
  { "en": "Fungsi dari 'under-voltage release' (UVR) yang mentripkan MCB saat tegangan hilang?", "id": "Mencegah mesin menyala kembali otomatis." },
  { "en": "Apa fungsi dari 'kondisioning' komponen (misal: beberapa kali siklus trip) sebelum kalibrasi akhir?", "id": "Menstabilkan kinerja komponen mekanis." },
  { "en": "Fungsi dari penggunaan 'circlips' atau 'retaining rings' dalam perakitan?", "id": "Menahan komponen pada porosnya." },
  { "en": "Fungsi dari 'derating factor' untuk frekuensi operasi yang berbeda dari 50/60Hz?", "id": "Mengkompensasi pemanasan akibat frekuensi." },
  { "en": "Apa fungsi dari standar yang mengatur warna kabel untuk sistem 3 fasa?", "id": "Memastikan urutan fasa benar." },
  { "en": "Fungsi dari 'blocking mechanism' yang mencegah pemasangan aksesori yang salah?", "id": "Memastikan hanya aksesori kompatibel." },
  { "en": "Fungsi dari 'final assembly torque check' pada sekrup-sekrup casing?", "id": "Memastikan housing tertutup rapat." },
  { "en": "Mengapa resistansi isolasi diukur menggunakan tegangan DC tinggi?", "id": "Mendeteksi kelemahan jalur isolasi." },
  { "en": "Fungsi dari penggunaan 'heat sink' mini pada beberapa komponen elektronik di dalam MCB?", "id": "Membuang panas dari komponen elektronik." },
  { "en": "Apa fungsi dari 'anti-counterfeiting features' seperti hologram atau kode unik pada MCB merek ternama?", "id": "Melindungi konsumen dari produk palsu." },
  { "en": "Fungsi dari 'kelas koordinasi' Tipe 1 atau Tipe 2 menurut IEC?", "id": "Mendefinisikan tingkat kerusakan yang diizinkan." },
  { "en": "Fungsi dari 'gasket' atau segel karet di antara bagian-bagian housing?", "id": "Mencapai rating proteksi IP tinggi." },
  { "en": "Mengapa beberapa MCB memiliki rating ganda, misal 10kA menurut IEC dan 5kA menurut UL?", "id": "Karena metode pengujian yang berbeda." },
  { "en": "Fungsi dari 'wiper' atau sikat kecil pada beberapa mekanisme putar?", "id": "Menjaga permukaan kontak tetap bersih." },
  { "en": "Apa fungsi dari 'test report' yang menyertai MCB untuk proyek-proyek besar?", "id": "Sebagai bukti dokumenter kelulusan uji." },
  { "en": "Fungsi dari 'molded-in' terminal markings (cetakan langsung) dibandingkan label kertas?", "id": "Tanda menjadi permanen dan awet." },
  { "en": "Fungsi dari desain 'bebas silikon' (silicon-free) untuk MCB yang digunakan di industri cat?", "id": "Mencegah kontaminasi pada proses pengecatan." },
  { "en": "Fungsi dari 'sudut trip' pada mekanisme pengunci?", "id": "Sudut kritis pelepasan mekanisme." },
  { "en": "Apa fungsi dari 'trip flag' mekanis berwarna cerah?", "id": "Memberikan indikasi visual sangat jelas." },
  { "en": "Fungsi dari 'phase barrier' atau sekat pemisah fasa yang dipasang di antara MCB?", "id": "Mencegah loncatan api antar kutub." },
  { "en": "Fungsi dari 'computational fluid dynamics' (CFD) dalam desain MCB?", "id": "Mensimulasikan aliran gas panas." },
  { "en": "Mengapa beberapa MCB khusus menggunakan kontak berlapis emas (gold-plated)?", "id": "Untuk aplikasi sinyal arus rendah." },
  { "en": "Fungsi dari 'wear indicator' pada beberapa pemutus sirkuit besar?", "id": "Memberikan indikasi tingkat keausan kontak." },
  { "en": "Fungsi dari 'energy balance' dalam desain termal?", "id": "Mencapai suhu operasi yang stabil." },
  { "en": "Apa fungsi dari 'type test' yang dilakukan oleh laboratorium independen?", "id": "Memverifikasi desain produk sesuai standar." },
  { "en": "Fungsi dari penggunaan bahan 'amorf' atau 'nanokristalin' pada inti transformator di RCBO?", "id": "Mendeteksi arus bocor sangat kecil." },
  { "en": "Fungsi dari 'anti-tamper plug' untuk menutupi sekrup kalibrasi?", "id": "Sebagai segel fisik bukti manipulasi." },
  { "en": "Mengapa mekanisme trip-free sering disebut sebagai 'positive break operation'?", "id": "Memberikan jaminan pemutusan yang pasti." },
  { "en": "Fungsi dari 'magnetic gap' yang dapat diatur pada beberapa pemutus sirkuit industri?", "id": "Memungkinkan penyesuaian ambang trip magnetik." },
  { "en": "Fungsi dari pengujian 'cold start' pada suhu di bawah nol?", "id": "Memastikan mekanisme tidak macet." },
  { "en": "Apa fungsi dari 'rating siklus tugas' (duty cycle rating)?", "id": "Menentukan frekuensi operasi yang aman." },
  { "en": "Fungsi dari 'lubang pembuangan tekanan' yang dilengkapi dengan filter logam?", "id": "Melepaskan tekanan gas dengan aman." },
  { "en": "Fungsi dari pengujian 'accelerated aging' menggunakan paparan UV dan kelembaban?", "id": "Mensimulasikan efek penuaan akibat cuaca." },
  { "en": "Mengapa beberapa bagian dari mekanisme trip dilapisi dengan Nikel atau Krom?", "id": "Memberikan permukaan tahan aus." },
  { "en": "Fungsi dari 'hysteresis' pada strip bimetal?", "id": "Mencegah MCB trip-reset berulang." },
  { "en": "Fungsi dari 'time-current characteristic' (TCC) yang sangat curam?", "id": "Memungkinkan koordinasi yang lebih presisi." },
  { "en": "Apa fungsi dari 'pull test' pada koneksi kabel di terminal?", "id": "Memverifikasi kekuatan mekanis penjepit." },
  { "en": "Fungsi dari penggunaan 'O-ring' pada poros tuas putar eksternal?", "id": "Menjaga rating IP panel listrik." },
  { "en": "Fungsi dari 'sertifikasi seismik' untuk peralatan yang dipasang di zona gempa?", "id": "Menjamin fungsi setelah gempa." },
  { "en": "Mengapa penting untuk mengetahui 'impedansi sumber' saat memilih kapasitas pemutusan MCB?", "id": "Menentukan besar arus hubung singkat." },
  { "en": "Fungsi dari 'arus cut-off' pada MCB pembatas arus?", "id": "Membatasi puncak arus gangguan." },
  { "en": "Fungsi dari 'faktor K' yang digunakan untuk derating akibat arus harmonik?", "id": "Memperhitungkan pemanasan tambahan harmonik." },
  { "en": "Fungsi dari 'terminal sadel' (saddle clamp)?", "id": "Memberikan tekanan penjepitan merata." },
  { "en": "Apa fungsi dari 'bilah pengaduk magnetik' (magnetic stirring) pada busur api?", "id": "Mencegah satu titik kontak panas." },
  { "en": "Fungsi dari 'sistem interlock' mekanis antara dua pemutus sirkuit?", "id": "Mencegah dua sumber terhubung bersamaan." },
  { "en": "Fungsi dari penggunaan 'sekrup tahan getaran' (vibration-proof screw) pada terminal?", "id": "Mencegah sekrup melonggar seiring waktu." },
  { "en": "Fungsi dari 'diagram satu garis' (single-line diagram) dalam desain panel?", "id": "Membantu analisis dan pemeliharaan sistem." },
  { "en": "Apa fungsi dari 'endurance test' yang dilakukan pada suhu ekstrem (tinggi dan rendah)?", "id": "Memastikan keandalan pada suhu ekstrem." },
  { "en": "Fungsi dari 'ground fault protection' yang terintegrasi pada beberapa pemutus sirkuit industri?", "id": "Mendeteksi gangguan fasa-ke-tanah." },
  { "en": "Fungsi dari 'return spring' pada plunger solenoid?", "id": "Mengembalikan plunger ke posisi siaga." },
  { "en": "Apa fungsi dari desain kontak yang 'non-welding'?", "id": "Meminimalkan kemungkinan kontak menempel." },
  { "en": "Fungsi dari penggunaan 'busbar riser' di panel bertingkat?", "id": "Mendistribusikan daya secara vertikal." },
  { "en": "Fungsi dari pengujian 'terminal temperature rise' (kenaikan suhu terminal)?", "id": "Memastikan terminal tidak merusak kabel." },
  { "en": "Fungsi dari 'grup fungsional' komponen dalam desain modular?", "id": "Menyederhanakan manufaktur dan perbaikan." },
  { "en": "Apa fungsi dari 'ROHS compliance' (kepatuhan ROHS)?", "id": "Menyatakan produk bebas bahan berbahaya." },
  { "en": "Fungsi dari 'lekukan' pada sisi housing MCB?", "id": "Meningkatkan pendinginan saat dipasang berdampingan." },
  { "en": "Fungsi dari penggunaan material 'zamak' pada beberapa komponen mekanis?", "id": "Memberikan kekuatan dan presisi dimensi." },
  { "en": "Apa fungsi dari 'waktu leleh' (melting time) pada kurva I²t?", "id": "Waktu hingga elemen mulai membatasi." },
  { "en": "Fungsi dari 'busur api stasioner' vs 'busur api bergerak'?", "id": "Menentukan metode dan efektivitas pemadaman." },
  { "en": "Mengapa beberapa kontak dilapisi Cadmium?", "id": "Meningkatkan ketahanan terhadap pengelasan kontak." },
  { "en": "Fungsi dari 'gaya kontak dinamis' saat terjadi gangguan?", "id": "Gaya tolak yang membantu pemisahan." },
  { "en": "Fungsi dari 'mekanisme toggle' dengan titik mati (dead point)?", "id": "Memastikan aksi sakelar cepat (snap)." },
  { "en": "Apa fungsi dari 'konstanta pegas' pada mekanisme trip?", "id": "Menentukan gaya dan kecepatan pemutusan." },
  { "en": "Fungsi dari proses 'deburring' setelah pencetakan komponen plastik?", "id": "Menghilangkan sisa plastik tajam." },
  { "en": "Fungsi dari 'karakteristik penuaan' bimetal yang harus diketahui?", "id": "Memprediksi perubahan kinerja seiring waktu." },
  { "en": "Apa fungsi dari 'jalur arus eddy' yang diminimalkan dalam desain?", "id": "Mengurangi pemanasan internal yang tidak perlu." },
  { "en": "Fungsi dari 'pegas anti-pantul' pada mekanisme kontak?", "id": "Mencegah kontak menutup kembali sesaat." },
  { "en": "Fungsi dari standarisasi warna tuas untuk rating arus yang berbeda?", "id": "Memudahkan identifikasi rating arus visual." },
  { "en": "Apa fungsi dari 'arus puncak prospektif' dalam pengujian?", "id": "Arus maksimal jika MCB gagal." },
  { "en": "Fungsi dari bahan isolasi dengan suhu distorsi panas tinggi?", "id": "Menjaga bentuk saat suhu tinggi." },
  { "en": "Mengapa trip termal memiliki toleransi yang lebih lebar daripada magnetik?", "id": "Karena sifat pemanasan lebih lambat." },
  { "en": "Fungsi dari 'deionisasi' ruang pemadam api setelah trip?", "id": "Menghilangkan partikel konduktif di udara." },
  { "en": "Apa fungsi dari desain terminal dengan 'proteksi kabel'?", "id": "Mencegah sekrup merusak untaian kabel." },
  { "en": "Fungsi dari penggunaan 'ultrasonic welding' untuk menyatukan housing?", "id": "Menciptakan segel yang kuat permanen." },
  { "en": "Apa fungsi dari 'kalibrasi ulang' setelah perubahan desain minor?", "id": "Memastikan kinerja tetap sesuai standar." },
  { "en": "Fungsi dari lubang ventilasi yang menghadap ke atas atau bawah?", "id": "Mengarahkan gas panas dengan aman." },
  { "en": "Apa fungsi dari pengujian 'making and breaking capacity'?", "id": "Memastikan kemampuan menyambung dan memutus." },
  { "en": "Fungsi dari penggunaan 'bushing' polimer pada titik pivot?", "id": "Mengurangi gesekan dan keausan mekanis." },
  { "en": "Mengapa MCB Tipe K memiliki sensitivitas antara Tipe C dan D?", "id": "Melindungi sirkuit dengan arus start sedang." },
  { "en": "Fungsi dari penambahan tembaga pada paduan perak di kontak?", "id": "Meningkatkan kekerasan dan ketahanan aus." },
  { "en": "Apa fungsi dari 'batang isolasi' yang menghubungkan tuas-tuas?", "id": "Memastikan trip bersamaan secara terisolasi." },
  { "en": "Fungsi dari 'labirin' di dalam jalur ventilasi gas?", "id": "Mendinginkan gas sebelum keluar casing." },
  { "en": "Fungsi dari kekasaran permukaan (surface roughness) pada kontak?", "id": "Mempengaruhi resistansi dan potensi pengelasan." },
  { "en": "Apa fungsi dari pengujian ketahanan terhadap jamur (fungus test)?", "id": "Memastikan keandalan di iklim tropis." },
  { "en": "Fungsi dari mekanisme 'aksi ganda' pada beberapa tuas?", "id": "Memerlukan dua gerakan untuk reset." },
  { "en": "Mengapa MCB untuk kereta api harus tahan getaran ekstrem?", "id": "Mencegah kegagalan akibat guncangan konstan." },
  { "en": "Fungsi dari 'diagram Venn' dalam analisis kegagalan?", "id": "Mengidentifikasi tumpang tindih penyebab kegagalan." },
  { "en": "Fungsi dari perlakuan 'anodizing' pada komponen aluminium?", "id": "Menciptakan lapisan pelindung tahan korosi." },
  { "en": "Apa fungsi dari desain yang 'bebas kadmium'?", "id": "Memenuhi standar lingkungan dan kesehatan." },
  { "en": "Fungsi dari penandaan 'posisi pemasangan' yang direkomendasikan?", "id": "Untuk pendinginan dan kinerja optimal." },
  { "en": "Fungsi dari penggunaan 'sekrup self-tapping' dalam perakitan?", "id": "Menyederhanakan dan mempercepat proses perakitan." },
  { "en": "Apa fungsi dari pengujian 'short-time withstand current' (Icw)?", "id": "Menguji ketahanan untuk koordinasi selektivitas." },
  { "en": "Fungsi dari 'terminal anti-rotasi' untuk kabel besar?", "id": "Mencegah kabel memutar saat dikencangkan." },
  { "en": "Fungsi dari 'penyerap kelembaban' (desiccant) dalam kemasan?", "id": "Melindungi MCB selama penyimpanan dan transportasi." },
  { "en": "Apa fungsi dari 'gaya magnet permanen' pada mekanisme latching?", "id": "Membantu menahan kontak tanpa daya." },
  { "en": "Fungsi dari 'analisis modal' untuk ketahanan getaran?", "id": "Mengidentifikasi frekuensi resonansi berbahaya." },
  { "en": "Fungsi dari 'selektivitas zona' dalam sistem distribusi besar?", "id": "Mengisolasi gangguan pada zona terdekat." },
  { "en": "Mengapa beberapa kontak memiliki bentuk seperti mahkota (crown)?", "id": "Memberikan beberapa titik kontak simultan." },
  { "en": "Fungsi dari 'tegangan busur balik' (arc restrike voltage)?", "id": "Parameter kritis dalam pemadaman busur." },
  { "en": "Fungsi dari 'sertifikasi kelautan' (misal: DNV, BV)?", "id": "Menjamin keandalan di lingkungan kapal." },
  { "en": "Apa fungsi dari 'arus trip konvensional' dalam standar?", "id": "Arus yang dijamin mentripkan MCB." },
  { "en": "Fungsi dari 'filter ferit' pada kabel di dekat MCB?", "id": "Menekan gangguan frekuensi tinggi (EMI)." },
  { "en": "Fungsi dari 'desain anti-debu' pada mekanisme internal?", "id": "Mencegah partikel mengganggu gerakan presisi." },
  { "en": "Fungsi dari 'overload release' yang dapat diatur pada MCCB?", "id": "Menyesuaikan proteksi dengan beban spesifik." },
  { "en": "Fungsi dari 'penandaan laser' dibandingkan cetak tinta?", "id": "Memberikan penandaan permanen yang presisi." },
  { "en": "Mengapa beberapa MCB memiliki 'rating ganda' tegangan (AC/DC)?", "id": "Menunjukkan kapabilitas pada sistem berbeda." },
  { "en": "Fungsi dari 'pin anti-rotasi' pada pemasangan aksesori?", "id": "Memastikan orientasi pemasangan yang benar." },
  { "en": "Fungsi dari 'pengujian torsi lepas' (stripping torque) pada terminal?", "id": "Memastikan ulir tidak rusak saat dikencangkan." },
  { "en": "Fungsi dari 'sirkuit elektronik' pada AFDD untuk menganalisis gelombang?", "id": "Mendeteksi tanda-tanda unik busur api." },
  { "en": "Fungsi dari 'housing yang diperkuat serat kaca' (glass-reinforced)?", "id": "Meningkatkan kekakuan dan kekuatan mekanis." },
  { "en": "Fungsi dari 'selektivitas kronometrik'?", "id": "Koordinasi berdasarkan perbedaan waktu tunda." },
  { "en": "Fungsi dari 'kontak bantu berlapis emas' (gold-flashed)?", "id": "Menjamin keandalan sinyal arus rendah." },
  { "en": "Mengapa 'faktor daya' sirkuit DC dianggap satu?", "id": "Karena tidak ada pergeseran fasa." },
  { "en": "Fungsi dari 'mekanisme pembersih' pada kontak putar?", "id": "Mengikis lapisan oksida saat dioperasikan." },
  { "en": "Fungsi dari 'nomor revisi perangkat keras' pada label?", "id": "Melacak perubahan desain internal produk." },
  { "en": "Fungsi dari 'desain ramah lingkungan' (eco-design)?", "id": "Meminimalkan dampak lingkungan seumur hidup." },
  { "en": "Fungsi dari 'sudut kontak' saat penyambungan?", "id": "Mempengaruhi dinamika pantulan dan pengelasan." },
  { "en": "Fungsi dari 'indikator torsi' yang dapat diklik?", "id": "Memberikan umpan balik saat torsi tercapai." },
  { "en": "Fungsi dari 'analisis termal' menggunakan kamera inframerah?", "id": "Mengidentifikasi titik panas dalam desain." },
  { "en": "Mengapa 'arus bocor' pada RCBO diukur dalam miliampere?", "id": "Karena arus sekecil itu berbahaya." },
  { "en": "Fungsi dari 'tombol tes' pada RCBO?", "id": "Memverifikasi fungsi mekanisme proteksi bocor." },
  { "en": "Fungsi dari 'inti ferit' pada transformator arus RCBO?", "id": "Memusatkan fluks magnet arus bocor." },
  { "en": "Fungsi dari 'sirkuit penguat' pada RCBO?", "id": "Memperkuat sinyal arus bocor lemah." },
  { "en": "Fungsi dari 'rating arus sisa' (IΔn) pada RCBO?", "id": "Menentukan sensitivitas deteksi arus bocor." },
  { "en": "Mengapa RCBO tipe A lebih baik dari tipe AC?", "id": "Mendeteksi arus bocor AC dan DC." },
  { "en": "Fungsi dari 'ketahanan terhadap arus sisa surja'?", "id": "Mencegah trip palsu akibat petir." },
  { "en": "Fungsi dari 'waktu tunda' pada RCBO tipe S (Selektif)?", "id": "Memberikan koordinasi dengan RCBO hilir." },
  { "en": "Fungsi dari 'pigtail netral' pada beberapa RCBO?", "id": "Sebagai koneksi referensi sirkuit netral." },
  { "en": "Fungsi dari 'analisis spektrum frekuensi' pada AFDD?", "id": "Membedakan derau normal dari busur api." },
  { "en": "Mengapa AFDD direkomendasikan untuk kamar tidur?", "id": "Melindungi dari kebakaran saat tidur." },
  { "en": "Fungsi dari 'self-test' otomatis pada beberapa AFDD?", "id": "Memastikan perangkat selalu berfungsi benar." },
  { "en": "Fungsi dari 'indikator LED' pada AFDD?", "id": "Memberikan kode diagnostik penyebab trip." },
  { "en": "Fungsi dari 'busur api seri' yang dideteksi AFDD?", "id": "Gangguan akibat kabel putus sebagian." },
  { "en": "Fungsi dari 'busur api paralel' yang dideteksi AFDD?", "id": "Gangguan akibat kerusakan isolasi kabel." },
  { "en": "Fungsi dari 'kapasitas pemutusan terkondisi' (Icc)?", "id": "Rating saat dilindungi perangkat hulu." },
  { "en": "Fungsi dari 'motor-drive' eksternal untuk MCB?", "id": "Memungkinkan pengoperasian MCB dari jauh." },
  { "en": "Mengapa 'pelapisan seng-nikel' lebih unggul dari seng saja?", "id": "Memberikan ketahanan korosi jauh lebih baik." },
  { "en": "Fungsi dari 'mekanisme kompensasi keausan kontak'?", "id": "Menjaga tekanan kontak tetap konstan." },
  { "en": "Fungsi dari 'desain terminal ganda' (twin terminal)?", "id": "Memungkinkan koneksi dua kabel terpisah." },
  { "en": "Fungsi dari 'rating suhu ambien' -25°C?", "id": "Menjamin operasi di lingkungan sangat dingin." },
  { "en": "Fungsi dari 'kelas isolasi' menurut standar UL?", "id": "Mendefinisikan ketahanan suhu material isolasi." },
  { "en": "Fungsi dari 'rating interupsi' yang berbeda pada tegangan berbeda?", "id": "Karena memutus tegangan tinggi lebih sulit." },
  { "en": "Fungsi dari 'sertifikasi HACR' untuk aplikasi pendingin udara?", "id": "Menunjukkan kesesuaian untuk beban AC." },
  { "en": "Fungsi dari 'rating SWD' (Switching Duty) pada saklar?", "id": "Menunjukkan kesesuaian untuk beban lampu neon." },
  { "en": "Fungsi dari 'desain kutub tersegel' (sealed pole)?", "id": "Meningkatkan keandalan di lingkungan korosif." },
  { "en": "Fungsi dari 'mekanisme bebas pantul' (bounce-free)?", "id": "Menggunakan mekanisme terpisah untuk meminimalkan pantulan." },
  { "en": "Fungsi dari 'pemindaian 3D' dalam kontrol kualitas?", "id": "Memverifikasi dimensi komponen sangat presisi." },
  { "en": "Fungsi dari 'desain tahan lembab' (damp-proof)?", "id": "Mencegah penyerapan kelembaban oleh material." },
  { "en": "Fungsi dari 'kontak bantu dengan aksi cepat' (snap-action)?", "id": "Memberikan sinyal yang bersih dan tegas." },
  { "en": "Fungsi dari 'rating frekuensi switching' maksimum?", "id": "Menentukan batas operasi berulang aman." },
  { "en": "Fungsi dari 'desain terminal anti-salah' (mispulling-proof)?", "id": "Mencegah kabel terlepas saat ditarik." },
  { "en": "Fungsi dari 'penandaan CE' pada produk?", "id": "Menyatakan kesesuaian dengan standar Eropa." },
  { "en": "Fungsi dari 'sertifikasi UL' untuk pasar Amerika Utara?", "id": "Menyatakan kesesuaian dengan standar keselamatan UL." },
  { "en": "Fungsi dari 'desain global' yang memenuhi banyak standar?", "id": "Memungkinkan satu produk dijual mendunia." },
  { "en": "Fungsi dari 'kode tanggal produksi' yang tercetak pada housing?", "id": "Melacak umur dan batch produk." },
  { "en": "Fungsi dari 'analisis Weibull' dalam pengujian keandalan MCB?", "id": "Memprediksi tingkat kegagalan produk." },
  { "en": "Apa fungsi dari 'efek pinch' pada plasma busur api?", "id": "Menjepit dan menekan kolom busur." },
  { "en": "Fungsi dari 'kontak perak-oksida timah' (AgSnO2)?", "id": "Ketahanan superior terhadap pengelasan kontak." },
  { "en": "Mengapa 'kekerasan' material kait pengunci sangat penting?", "id": "Mencegah keausan pada mekanisme trip." },
  { "en": "Fungsi dari 'gaya elektrostatis' di antara kontak?", "id": "Mempengaruhi dinamika celah busur api." },
  { "en": "Fungsi dari 'mekanisme penunda waktu' berbasis fluida silikon?", "id": "Memberikan waktu tunda trip hidrolik." },
  { "en": "Apa fungsi dari 'konstanta dielektrik' material isolator?", "id": "Menentukan kemampuan menyimpan energi listrik." },
  { "en": "Fungsi dari proses 'annealing' pada komponen tembaga?", "id": "Menghilangkan stres mekanis material." },
  { "en": "Fungsi dari 'karakteristik V-I' busur api?", "id": "Menjelaskan hubungan tegangan dan arus." },
  { "en": "Apa fungsi dari 'jalur pelepasan muatan statis' (ESD)?", "id": "Melindungi sirkuit elektronik internal." },
  { "en": "Fungsi dari 'pegas torsi' pada beberapa mekanisme tuas?", "id": "Memberikan gaya balik secara rotasional." },
  { "en": "Fungsi dari standarisasi 'profil DIN rail' (misal: TH35)?", "id": "Memastikan kecocokan pemasangan universal." },
  { "en": "Apa fungsi dari 'arus puncak terpotong' (peak let-through)?", "id": "Menunjukkan efektivitas pembatasan arus." },
  { "en": "Fungsi dari bahan isolasi dengan 'indeks oksigen' tinggi?", "id": "Menunjukkan material sulit terbakar." },
  { "en": "Mengapa 'konduktivitas termal' housing penting?", "id": "Membantu pembuangan panas ke lingkungan." },
  { "en": "Fungsi dari 'ionisasi termal' pada gas di arc chute?", "id": "Menciptakan plasma konduktif busur api." },
  { "en": "Apa fungsi dari desain terminal 'bebas getaran'?", "id": "Mencegah koneksi longgar seiring waktu." },
  { "en": "Fungsi dari penggunaan 'X-ray inspection' pada produksi?", "id": "Memeriksa cacat internal tak terlihat." },
  { "en": "Apa fungsi dari 'kalibrasi otomatis' menggunakan aktuator?", "id": "Meningkatkan presisi dan kecepatan kalibrasi." },
  { "en": "Fungsi dari 'desain ventilasi silang' (cross-ventilation)?", "id": "Meningkatkan efisiensi pendinginan konveksi." },
  { "en": "Apa fungsi dari pengujian 'kapasitas hubung singkat terkondisi'?", "id": "Memverifikasi keamanan dengan proteksi backup." },
  { "en": "Fungsi dari penggunaan 'pin dowel' dalam perakitan presisi?", "id": "Menjamin keselarasan antar komponen." },
  { "en": "Mengapa MCB tipe F dikembangkan untuk beberapa aplikasi?", "id": "Proteksi sirkuit dengan karakteristik khusus." },
  { "en": "Fungsi dari penambahan 'Bismuth' pada kontak perak?", "id": "Mengurangi kemungkinan pengelasan kontak." },
  { "en": "Apa fungsi dari 'mekanisme trip independen manual'?", "id": "Memastikan trip meski tuas macet." },
  { "en": "Fungsi dari 'saluran pendingin' di dalam housing?", "id": "Mengarahkan aliran udara pendingin internal." },
  { "en": "Fungsi dari 'tegangan tembus' (breakdown voltage) gas?", "id": "Tegangan pemicu terbentuknya busur api." },
  { "en": "Apa fungsi dari pengujian 'ketahanan terhadap pelarut industri'?", "id": "Memastikan label dan housing awet." },
  { "en": "Fungsi dari 'mekanisme reset' yang memerlukan gaya lebih?", "id": "Mencegah reset yang tidak disengaja." },
  { "en": "Mengapa MCB aplikasi surya (PV) butuh rating tegangan DC tinggi?", "id": "Karena panel surya menghasilkan tegangan DC." },
  { "en": "Fungsi dari 'analisis pohon kegagalan' (Fault Tree Analysis)?", "id": "Menganalisis akar penyebab kegagalan sistem." },
  { "en": "Fungsi dari perlakuan 'shot peening' pada pegas?", "id": "Meningkatkan ketahanan terhadap kelelahan material." },
  { "en": "Apa fungsi dari 'desain kemasan anti-statis'?", "id": "Melindungi MCB dengan komponen elektronik." },
  { "en": "Fungsi dari penandaan 'arah aliran arus' pada MCB DC?", "id": "Kritis untuk fungsi pemadaman busur." },
  { "en": "Fungsi dari penggunaan 'sekrup dengan washer terintegrasi'?", "id": "Menyederhanakan instalasi, mencegah kehilangan washer." },
  { "en": "Apa fungsi dari pengujian 'siklus daya' (power cycling)?", "id": "Mensimulasikan efek pemuaian dan penyusutan." },
  { "en": "Fungsi dari 'terminal plug-in' pada beberapa sistem modular?", "id": "Memungkinkan penggantian cepat tanpa melepas kabel." },
  { "en": "Fungsi dari 'kemasan blister' untuk penjualan ritel?", "id": "Melindungi produk dan memberikan visibilitas." },
  { "en": "Apa fungsi dari 'gaya gesek statis' pada mekanisme?", "id": "Gaya awal yang harus diatasi." },
  { "en": "Fungsi dari 'analisis aliran cetakan' (mold flow analysis)?", "id": "Mengoptimalkan proses pencetakan komponen plastik." },
  { "en": "Fungsi dari 'selektivitas logika' pada pemutus sirkuit pintar?", "id": "Koordinasi berbasis komunikasi antar perangkat." },
  { "en": "Mengapa beberapa kontak dibuat dari 'perak-nikel' (AgNi)?", "id": "Ketahanan baik terhadap erosi busur." },
  { "en": "Fungsi dari 'panjang busur api' sebagai parameter desain?", "id": "Mempengaruhi tegangan dan energi busur." },
  { "en": "Fungsi dari 'sertifikasi VDE' di pasar Jerman/Eropa?", "id": "Menunjukkan kepatuhan standar keselamatan tinggi." },
  { "en": "Apa fungsi dari 'arus referensi dingin' untuk kalibrasi?", "id": "Arus trip tanpa pengaruh pemanasan." },
  { "en": "Fungsi dari 'pelindung terminal' (terminal cover) tambahan?", "id": "Meningkatkan proteksi sentuh (rating IP)." },
  { "en": "Fungsi dari 'desain ergonomis' pada tuas aktuator?", "id": "Memberikan kenyamanan dan kemudahan operasi." },
  { "en": "Fungsi dari 'kemampuan trip pada arus sisa DC'?", "id": "Proteksi terhadap arus bocor DC." },
  { "en": "Fungsi dari 'penandaan laser 2D barcode' (Data Matrix)?", "id": "Menyimpan banyak informasi dalam ruang kecil." },
  { "en": "Mengapa 'stabilitas termal' material plastik penting?", "id": "Menjaga sifat mekanis pada suhu tinggi." },
  { "en": "Fungsi dari 'pin ejektor' pada cetakan injeksi plastik?", "id": "Mendorong produk keluar dari cetakan." },
  { "en": "Fungsi dari 'pengujian torsi' pada tuas aktuator?", "id": "Memastikan kekuatan mekanis tuas itu sendiri." },
  { "en": "Fungsi dari 'sirkuit deteksi titik nol' (zero-crossing)?", "id": "Mengoptimalkan waktu switching pada beban AC." },
  { "en": "Fungsi dari 'housing ultrasonik tersegel'?", "id": "Membuat produk tahan rusak dan disegel." },
  { "en": "Fungsi dari 'selektivitas energi' (I²t) parsial?", "id": "Koordinasi yang hanya efektif hingga arus tertentu." },
  { "en": "Fungsi dari 'kontak perak-karbon' (AgC)?", "id": "Mencegah pengelasan pada arus tinggi." },
  { "en": "Mengapa 'kecepatan pemisahan kontak' sangat penting?", "id": "Membantu memanjangkan dan mendinginkan busur." },
  { "en": "Fungsi dari 'mekanisme penyeimbang tekanan' pada housing?", "id": "Mencegah deformasi saat trip internal." },
  { "en": "Fungsi dari 'nomor EAN/UPC' pada kemasan?", "id": "Untuk identifikasi produk dalam sistem ritel." },
  { "en": "Fungsi dari 'desain rendah asap nol halogen' (LSZH)?", "id": "Keamanan kebakaran di ruang tertutup." },
  { "en": "Fungsi dari 'sudut kontak gesek' (wiping angle)?", "id": "Menentukan efektivitas pembersihan kontak." },
  { "en": "Fungsi dari 'indikator keausan' pada MCCB?", "id": "Memberi tahu kapan harus mengganti kontak." },
  { "en": "Fungsi dari 'analisis statistik' pada data pengujian?", "id": "Memahami variabilitas dan kapabilitas proses." },
  { "en": "Mengapa RCBO tipe B/B+ sangat mahal?", "id": "Mendeteksi semua jenis arus bocor." },
  { "en": "Fungsi dari 'tombol reset' yang berbeda dari tuas?", "id": "Untuk mereset sirkuit elektronik internal." },
  { "en": "Fungsi dari 'kumparan sekunder' pada transformator RCBO?", "id": "Mendeteksi fluks magnet sisa." },
  { "en": "Fungsi dari 'ambang batas kebisingan' pada sirkuit AFDD?", "id": "Mengabaikan derau listrik yang normal." },
  { "en": "Fungsi dari 'algoritma' pada prosesor sinyal AFDD?", "id": "Menganalisis dan mengidentifikasi pola busur." },
  { "en": "Mengapa AFDD memerlukan sumber daya internal?", "id": "Untuk memberi daya pada sirkuit elektronik." },
  { "en": "Fungsi dari 'memori kesalahan' pada perangkat pintar?", "id": "Menyimpan informasi penyebab trip terakhir." },
  { "en": "Fungsi dari 'busur api akibat paku' (nail penetration)?", "id": "Simulasi umum penyebab busur api paralel." },
  { "en": "Fungsi dari 'rating tegangan isolasi' (Ui)?", "id": "Menentukan ketahanan isolasi jangka panjang." },
  { "en": "Fungsi dari 'konektor CAGE CLAMP®' pada terminal?", "id": "Memberikan koneksi pegas bebas perawatan." },
  { "en": "Mengapa 'pelapisan kromium' digunakan pada beberapa mekanisme?", "id": "Memberikan permukaan sangat keras dan licin." },
  { "en": "Fungsi dari 'mekanisme energi tersimpan' dengan pegas?", "id": "Memastikan operasi independen dari operator." },
  { "en": "Fungsi dari 'desain terminal ganda sejajar'?", "id": "Memudahkan koneksi loop-through." },
  { "en": "Fungsi dari 'rating suhu penyimpanan'?", "id": "Menentukan batas suhu aman penyimpanan." },
  { "en": "Fungsi dari 'kelas penggunaan' (Utilization Category) AC-22?", "id": "Menunjukkan kesesuaian untuk beban campuran." },
  { "en": "Fungsi dari 'rating HID' untuk lampu pelepasan intensitas tinggi?", "id": "Menunjukkan kesesuaian untuk beban lampu HID." },
  { "en": "Fungsi dari 'rating F' untuk pemasangan di atas kayu?", "id": "Menunjukkan batas suhu permukaan aman." },
  { "en": "Fungsi dari 'desain kutub berventilasi'?", "id": "Membantu pendinginan di dalam kutub." },
  { "en": "Fungsi dari 'mekanisme trip palu' (hammer trip)?", "id": "Menggunakan tumbukan untuk melepaskan pengunci." },
  { "en": "Fungsi dari 'pemindaian laser' untuk verifikasi profil?", "id": "Memastikan bentuk komponen sesuai desain." },
  { "en": "Fungsi dari 'desain tahan iklim' (climate-proof)?", "id": "Menjamin operasi di berbagai suhu/kelembaban." },
  { "en": "Fungsi dari 'kontak bantu dengan aksi lambat' (slow-make)?", "id": "Untuk aplikasi yang tidak kritis waktu." },
  { "en": "Fungsi dari 'rating frekuensi operasi' minimum?", "id": "Batas bawah frekuensi untuk kinerja." },
  { "en": "Fungsi dari 'desain terminal dengan pemandu kabel'?", "id": "Memudahkan memasukkan kabel dengan benar." },
  { "en": "Fungsi dari 'penandaan UKCA' untuk pasar Inggris Raya?", "id": "Menyatakan kesesuaian standar Inggris." },
  { "en": "Fungsi dari 'sertifikasi CCC' untuk pasar Tiongkok?", "id": "Menyatakan kesesuaian standar Tiongkok." },
  { "en": "Fungsi dari 'desain modular' dengan lebar setengah modul?", "id": "Menghemat ruang panel secara signifikan." },
  { "en": "Fungsi dari 'pengujian ketahanan balistik' pada housing?", "id": "Memastikan penahanan ledakan internal kuat." },
  { "en": "Fungsi dari 'kontak bantu dengan kontak berlapis silang'?", "id": "Meningkatkan keandalan sinyal sangat rendah." },
  { "en": "Fungsi dari 'rating daya terbuang' (watt loss)?", "id": "Informasi untuk perhitungan panas panel." },
  { "en": "Fungsi dari 'desain terminal atas-bawah' yang dapat dibalik?", "id": "Memberikan fleksibilitas pengkabelan maksimum." }

        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
