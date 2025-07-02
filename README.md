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


  { "en": "Kepanjangan HT?", "id": "Handy Talkie." },
  { "en": "Fungsi utama repeater?", "id": "Memperluas jangkauan sinyal radio." },
  { "en": "Sandi Taruna?", "id": "Berita atau informasi." },
  { "en": "Frekuensi komunikasi Polri?", "id": "VHF dan UHF." },
  { "en": "Perangkat radio di mobil?", "id": "Radio rig atau mobile station." },
  { "en": "Tujuan utama komunikasi radio?", "id": "Koordinasi tugas kepolisian cepat." },
  { "en": "Sebutkan mode operasi radio?", "id": "Simplex dan duplex." },
  { "en": "Definisi mode simplex?", "id": "Komunikasi satu arah secara bergantian." },
  { "en": "Definisi mode duplex?", "id": "Komunikasi dua arah bersamaan." },
  { "en": "Arti sandi Ganti?", "id": "Selesai bicara, menunggu balasan." },
  { "en": "Fungsi tombol squelch?", "id": "Menghilangkan noise saat hening." },
  { "en": "Jenis antena pada HT?", "id": "Stik, helical, dan teleskopik." },
  { "en": "Arti kata Roger?", "id": "Pesan diterima dan dimengerti." },
  { "en": "Fungsi tombol PTT?", "id": "Tombol memulai transmisi bicara." },
  { "en": "Perbedaan VHF dan UHF?", "id": "Panjang gelombang dan daya tembus." },
  { "en": "Keunggulan frekuensi VHF?", "id": "Jangkauan luas di area terbuka." },
  { "en": "Keunggulan frekuensi UHF?", "id": "Daya tembus baik di perkotaan." },
  { "en": "Arti sandi Standby?", "id": "Siap siaga menerima perintah." },
  { "en": "Nama lain radio rig?", "id": "Radio mobile atau base station." },
  { "en": "Definisi kanal radio?", "id": "Saluran frekuensi untuk komunikasi." },
  { "en": "Fungsi enkripsi pada radio?", "id": "Mengamankan percakapan dari penyadap." },
  { "en": "Arti sandi Wilco?", "id": "Will comply, akan dilaksanakan." },
  { "en": "Apa itu callback?", "id": "Melakukan panggilan kembali ke stasiun." },
  { "en": "Definisi sistem trunking?", "id": "Penggunaan kanal efisien secara otomatis." },
  { "en": "Arti kegiatan monitoring?", "id": "Mendengarkan kanal tanpa transmisi." },
  { "en": "Arti sandi 86?", "id": "Perintah dimengerti dan dilaksanakan." },
  { "en": "Sumber daya utama HT?", "id": "Baterai yang dapat diisi ulang." },
  { "en": "Penyebab suara brebet?", "id": "Sinyal lemah atau adanya interferensi." },
  { "en": "Arti kata Copy?", "id": "Pesan diterima dengan sangat jelas." },
  { "en": "Satuan frekuensi radio?", "id": "Hertz, MegaHertz, GigaHertz." },
  { "en": "Apa itu panggilan umum?", "id": "Panggilan ke semua stasiun radio." },
  { "en": "Apa itu panggilan selektif?", "id": "Panggilan ke stasiun radio tertentu." },
  { "en": "Sandi untuk kecelakaan lalu-lintas?", "id": "Laka Lantas atau TKA." },
  { "en": "Fungsi utama microphone?", "id": "Mengubah suara menjadi sinyal listrik." },
  { "en": "Arti sandi 813?", "id": "Komunikasi ingin dimonitor pimpinan." },
  { "en": "Apa itu dead spot?", "id": "Area tanpa jangkauan sinyal." },
  { "en": "Fungsi kode CTCSS/DCS?", "id": "Mengurangi gangguan dari pengguna lain." },
  { "en": "Arti sandi Halong?", "id": "Tugas dibubarkan atau selesai." },
  { "en": "Definisi frekuensi input repeater?", "id": "Frekuensi mengirim ke repeater." },
  { "en": "Definisi frekuensi output repeater?", "id": "Frekuensi menerima dari repeater." },
  { "en": "Pentingnya etika di radio?", "id": "Menjaga disiplin dan ketertiban komunikasi." },
  { "en": "Arti sandi Negatif?", "id": "Berita tidak benar atau ditolak." },
  { "en": "Arti sandi Positif?", "id": "Berita benar atau disetujui." },
  { "en": "Kegunaan radio saat bencana?", "id": "Koordinasi evakuasi dan bantuan darurat." },
  { "en": "Apa itu key up?", "id": "Menekan PTT tanpa berbicara." },
  { "en": "Kepanjangan TKP?", "id": "Tempat Kejadian Perkara." },
  { "en": "Perawatan dasar perangkat HT?", "id": "Jaga kebersihan, isi daya benar." },
  { "en": "Sandi meminta bantuan medis?", "id": "Ambulans segera ke TKP." },
  { "en": "Definisi interferensi?", "id": "Gangguan sinyal dari sumber eksternal." },
  { "en": "Kepanjangan Pamen?", "id": "Perwira Menengah." },
  { "en": "Kepanjangan Pama?", "id": "Perwira Pertama." },
  { "en": "Kepanjangan BKO?", "id": "Bawah Kendali Operasi." },
  { "en": "Sandi untuk fungsi Intelijen?", "id": "Intel atau Ik." },
  { "en": "Sandi untuk fungsi Sabhara?", "id": "Samapta atau Sabhara." },
  { "en": "Sandi untuk fungsi Lantas?", "id": "Lalu Lintas atau Lantas." },
  { "en": "Sandi untuk fungsi Reskrim?", "id": "Reserse atau Reskrim." },
  { "en": "Fungsi utama antena radio?", "id": "Memancarkan dan menerima gelombang." },
  { "en": "Apa itu callsign?", "id": "Tanda panggil unik stasiun radio." },
  { "en": "Arti sandi 87?", "id": "Laporan situasi terkini di lapangan." },
  { "en": "Fungsi tombol Scan radio?", "id": "Memindai semua kanal aktif otomatis." },
  { "en": "Sandi petugas mengalami kecelakaan?", "id": "Sandi 91." },
  { "en": "Sandi petugas diserang?", "id": "Sandi 92." },
  { "en": "Alfabet fonetik untuk A?", "id": "Alpha." },
  { "en": "Alfabet fonetik untuk B?", "id": "Bravo." },
  { "en": "Alfabet fonetik untuk C?", "id": "Charlie." },
  { "en": "Alfabet fonetik untuk D?", "id": "Delta." },
  { "en": "Sandi untuk kegiatan patroli?", "id": "Patroli atau Turjawali." },
  { "en": "Istilah interupsi berita penting?", "id": "Break atau Interupsi." },
  { "en": "Sandi untuk markas komando?", "id": "Jaya 60 atau Kantor Pusat." },
  { "en": "Sandi kendaraan patroli mobil?", "id": "Timor atau Panther." },
  { "en": "Sandi untuk pimpinan?", "id": "Kijang atau Komandan." },
  { "en": "Sandi untuk petugas Provos?", "id": "Panther atau Provos." },
  { "en": "Sandi untuk petugas Reserse?", "id": "Walet atau Buser." },
  { "en": "Arti sandi plot?", "id": "Memetakan posisi atau lokasi target." },
  { "en": "Kepanjangan Turjawali?", "id": "Pengaturan, Penjagaan, Pengawalan, Patroli." },
  { "en": "Arti perintah Amankan?", "id": "Lakukan penangkapan atau pengamanan." },
  { "en": "Arti perintah Monitor?", "id": "Awasi atau pantau suatu sasaran." },
  { "en": "Kepanjangan APP?", "id": "Arahan Pimpinan Pelaksana." },
  { "en": "Sandi untuk tahanan?", "id": "Tralis atau orang ditahan." },
  { "en": "Sandi untuk pelajar?", "id": "Semut atau anak sekolah." },
  { "en": "Sandi untuk ojek?", "id": "Lalat atau pengemudi ojek." },
  { "en": "Sandi pasukan tingkat peleton?", "id": "Pleton." },
  { "en": "Sandi pasukan tingkat kompi?", "id": "Kompi." },
  { "en": "Sandi mutu berita jelek?", "id": "Sandi 33." },
  { "en": "Sandi mutu berita baik?", "id": "Sandi 54." },
  { "en": "Sandi 10-2?", "id": "Dimana posisi, minta laporan." },
  { "en": "Sandi 10-4?", "id": "Diterima dan dimengerti." },
  { "en": "Sandi 10-8?", "id": "Menuju ke suatu tempat." },
  { "en": "Sandi 815?", "id": "Berita terlalu cepat, mohon ulangi." },
  { "en": "Sandi 811?", "id": "Kembali mengudara atau standby." },
  { "en": "Sandi 812?", "id": "Berita ini untuk disampaikan ke..." },
  { "en": "Larangan saat komunikasi radio?", "id": "Bercanda, berkata kasar, musik." },
  { "en": "Sandi untuk frekuensi?", "id": "Gelombang atau kanal." },
  { "en": "Sandi untuk truk besar?", "id": "Gajah." },
  { "en": "Istilah memeriksa kualitas suara?", "id": "Cek Sound atau Tes Radio." },
  { "en": "Fungsi repeater tone?", "id": "Mencegah interferensi stasiun lain." },
  { "en": "Kepanjangan Jarkom?", "id": "Jaringan Komunikasi." },
  { "en": "Apa itu overmodulated?", "id": "Suara transmisi pecah atau sember." },
  { "en": "Sandi untuk Polda?", "id": "Jaya 1 atau nama sandi." },
  { "en": "Sandi untuk Polres?", "id": "Jaya 2 atau nama sandi." },
  { "en": "Sandi untuk Polsek?", "id": "Jaya 3 atau nama sandi." },
  { "en": "Sebutkan jenis modulasi radio?", "id": "AM, FM, dan PM." },
  { "en": "Penyebab HT tidak bisa transmit?", "id": "Baterai lemah atau antena rusak." },
  { "en": "Sandi untuk patroli motor besar?", "id": "Puma atau patroli BM." },
  { "en": "Sandi untuk unit pengamat?", "id": "Elang atau mata-mata." },
  { "en": "Apa itu delay repeater?", "id": "Jeda waktu setelah PTT dilepas." },
  { "en": "Kepanjangan Patra?", "id": "Patroli Roda Dua." },
  { "en": "Fungsi keypad lock?", "id": "Mencegah tombol tertekan tidak sengaja." },
  { "en": "Sandi situasi aman terkendali?", "id": "Sandi 81-1." },
  { "en": "Sandi situasi merah?", "id": "Kondisi sangat darurat atau berbahaya." },
  { "en": "Penyebab desensitization repeater?", "id": "Penerima repeater kurang sensitif." },
  { "en": "Kepanjangan Brimob?", "id": "Brigade Mobil." },
  { "en": "Sandi unit penjinak bom?", "id": "Gegana atau Jibom." },
  { "en": "Kepanjangan Polairud?", "id": "Polisi Perairan dan Udara." },
  { "en": "Fungsi GPS pada HT modern?", "id": "Melaporkan posisi otomatis (APRS)." },
  { "en": "Arti perintah Lidik?", "id": "Melakukan kegiatan penyelidikan." },
  { "en": "Sandi untuk tersangka?", "id": "TSK atau orang diduga pelaku." },
  { "en": "Sandi untuk barang bukti?", "id": "BB atau barang bukti." },
  { "en": "Definisi bandwidth kanal?", "id": "Lebar pita frekuensi sebuah kanal." },
  { "en": "Sandi Operasi Zebra?", "id": "Operasi penertiban lalu lintas." },
  { "en": "Sandi Operasi Lilin?", "id": "Operasi pengamanan Natal Tahun Baru." },
  { "en": "Sandi Operasi Ketupat?", "id": "Operasi pengamanan hari raya Lebaran." },
  { "en": "Komunikasi Densus 88?", "id": "Menggunakan kode khusus sangat terenkripsi." },
  { "en": "Apa itu cross-band repeat?", "id": "Repeater antara band VHF-UHF." },
  { "en": "Kepanjangan Jatanras?", "id": "Kejahatan dan Kekerasan." },
  { "en": "Sandi satuan narkotika?", "id": "Sat Narkoba atau Tindak Narkoba." },
  { "en": "Sandi satuan korupsi?", "id": "Tipikor atau Tindak Pidana Korupsi." },
  { "en": "Cara mengatasi audio feedback?", "id": "Jauhkan HT dari perangkat lain." },
  { "en": "Sandi untuk Pos Komando?", "id": "Posko atau pusat kendali." },
  { "en": "Definisi sinyal RSSI?", "id": "Indikator kekuatan sinyal diterima." },
  { "en": "Arti sandi Pagar Betis?", "id": "Melakukan pengepungan suatu area." },
  { "en": "Sandi untuk pemeriksaan kendaraan?", "id": "Razia atau pemeriksaan selektif." },
  { "en": "Sandi untuk Markas Besar?", "id": "Mabes atau Jaya 65." },
  { "en": "Kepanjangan SPKT?", "id": "Sentra Pelayanan Kepolisian Terpadu." },
  { "en": "Pentingnya kerahasiaan komunikasi?", "id": "Mencegah bocornya informasi operasi." },
  { "en": "Sandi pembubaran massa?", "id": "Dorong atau halau massa." },
  { "en": "Kepanjangan PHH?", "id": "Pasukan Anti Huru-hara." },
  { "en": "Sandi untuk pengendalian massa?", "id": "Dalmas." },
  { "en": "Kepanjangan RoIP?", "id": "Radio over Internet Protocol." },
  { "en": "Keuntungan sistem RoIP?", "id": "Menghubungkan radio via internet." },
  { "en": "Sandi unit sidik jari?", "id": "Inafis atau identifikasi." },
  { "en": "Sandi untuk Laboratorium Forensik?", "id": "Labfor." },
  { "en": "Sandi untuk Dokkes?", "id": "Kedokteran dan Kesehatan Kepolisian." },
  { "en": "Kepanjangan SAR?", "id": "Search and Rescue." },
  { "en": "Definisi duty cycle radio?", "id": "Batas waktu transmisi terus menerus." },
  { "en": "Sandi untuk pengawalan VIP?", "id": "Walpri atau pengawalan pejabat." },
  { "en": "Sandi tahanan melarikan diri?", "id": "Tahanan kabur atau escape." },
  { "en": "Sandi untuk pengejaran?", "id": "Melakukan pengejaran terhadap target." },
  { "en": "Sandi untuk penyanderaan?", "id": "Situasi sandera atau penyanderaan." },
  { "en": "Panggilan darurat mengancam nyawa?", "id": "Mayday, Mayday, Mayday." },
  { "en": "Panggilan darurat non-nyawa?", "id": "Pan-pan, Pan-pan, Pan-pan." },
  { "en": "Sandi untuk arus lalu lintas?", "id": "Arus lalin atau situasi lalin." },
  { "en": "Sandi rekayasa lalu lintas?", "id": "Contraflow atau pengalihan arus." },
  { "en": "Sandi untuk SPBU?", "id": "Pom bensin atau SPBU." },
  { "en": "Sandi untuk mesin ATM?", "id": "Anjungan Tunai Mandiri atau ATM." },
  { "en": "Sandi pengamanan gereja?", "id": "Pam gereja atau obyek ibadah." },
  { "en": "Sandi pengamanan masjid?", "id": "Pam masjid atau obyek ibadah." },
  { "en": "Definisi simplex repeater?", "id": "Repeater simpan dan rekam suara." },
  { "en": "Kepanjangan PJR?", "id": "Patroli Jalan Raya." },
  { "en": "Sandi untuk objek vital?", "id": "Obvit atau obyek vital nasional." },
  { "en": "Sandi untuk kantor konsulat?", "id": "Konsulat atau perwakilan negara." },
  { "en": "Sandi untuk kantor kedutaan?", "id": "Kedutaan besar atau kedubes." },
  { "en": "Sandi untuk unjuk rasa?", "id": "Unras atau demonstrasi massa." },
  { "en": "Sandi untuk perkelahian massal?", "id": "Tawuran atau bentrok warga." },
  { "en": "Kepanjangan Curanmor?", "id": "Pencurian Kendaraan Bermotor." },
  { "en": "Kepanjangan Curas?", "id": "Pencurian dengan Kekerasan." },
  { "en": "Kepanjangan Curat?", "id": "Pencurian dengan Pemberatan." },
  { "en": "Sandi orang mabuk?", "id": "Dalam pengaruh miras atau mabuk." },
  { "en": "Sandi untuk senjata api?", "id": "Senpi." },
  { "en": "Sandi untuk senjata tajam?", "id": "Sajam." },
  { "en": "Sandi untuk bahan peledak?", "id": "Handak." },
  { "en": "Sandi pengamanan Pemilu?", "id": "Pam Pemilu." },
  { "en": "Sandi pengamanan Pilkada?", "id": "Pam Pilkada." },
  { "en": "Sandi untuk Kapolda?", "id": "Jaya 1 atau Pimpinan." },
  { "en": "Sandi untuk Kapolres?", "id": "Jaya 2 atau Pimpinan." },
  { "en": "Sandi untuk Kapolsek?", "id": "Jaya 3 atau Pimpinan." },
  { "en": "Sandi untuk Wakapolda?", "id": "Jaya 1-2 atau Wakil Pimpinan." },
  { "en": "Peran repeater controller?", "id": "Otak pengendali semua fungsi repeater." },
  { "en": "Sandi untuk sweeping?", "id": "Pemeriksaan menyeluruh suatu lokasi." },
  { "en": "Sandi untuk pemakaman?", "id": "Prosesi atau lokasi pemakaman." },
  { "en": "Sandi untuk rumah sakit?", "id": "RS atau objek rumah sakit." },
  { "en": "Sandi untuk pelabuhan?", "id": "Objek vital pelabuhan." },
  { "en": "Sandi untuk bandara?", "id": "Objek vital bandar udara." },
  { "en": "Sandi untuk stasiun KA?", "id": "Objek vital stasiun kereta." },
  { "en": "Sandi untuk terminal bus?", "id": "Objek vital terminal." },
  { "en": "Definisi gain antena?", "id": "Kemampuan antena memfokuskan sinyal." },
  { "en": "Sandi meminta bantuan personel?", "id": "Minta back up perkuatan." },
  { "en": "Sandi untuk luka ringan?", "id": "Korban luka ringan." },
  { "en": "Sandi untuk luka berat?", "id": "Korban luka berat." },
  { "en": "Sandi untuk meninggal dunia?", "id": "MD atau korban jiwa." },
  { "en": "Sandi untuk kasus penculikan?", "id": "Kasus 1.1 atau penculikan." },
  { "en": "Sandi untuk kasus pemerasan?", "id": "Kasus 1.2 atau pemerasan." },
  { "en": "Sandi untuk kasus penipuan?", "id": "Kasus 1.3 atau penipuan." },
  { "en": "Sandi jalur tidak resmi?", "id": "Jalan tikus." },
  { "en": "Kepanjangan DMR pada radio?", "id": "Digital Mobile Radio." },
  { "en": "Keunggulan utama radio digital?", "id": "Suara lebih jernih, fitur canggih." },
  { "en": "Apa itu P25?", "id": "Standar radio digital untuk publik." },
  { "en": "Fungsi SWR meter?", "id": "Mengukur performa antena dan kabel." },
  { "en": "Sandi untuk DPO?", "id": "Daftar Pencarian Orang." },
  { "en": "Apa itu 'talkgroup'?", "id": "Grup percakapan virtual di radio." },
  { "en": "Fungsi 'Emergency Button' HT?", "id": "Mengirim sinyal darurat ke pusat." },
  { "en": "Sandi 'silent monitoring'?", "id": "Mendengar tanpa terdeteksi oleh target." },
  { "en": "Peran operator stasiun pusat?", "id": "Mengatur dan memonitor alur komunikasi." },
  { "en": "Sandi 'status zero'?", "id": "Anggota gugur dalam tugas." },
  { "en": "Definisi 'radio stun'?", "id": "Mematikan radio target dari jarak jauh." },
  { "en": "Definisi 'radio kill'?", "id": "Menonaktifkan permanen radio yang hilang." },
  { "en": "Sandi 'geledah'?", "id": "Lakukan penggeledahan badan atau tempat." },
  { "en": "Sandi 'rekayasa lalin'?", "id": "Melakukan perubahan arus lalu lintas." },
  { "en": "Apa itu 'ISSI' radio?", "id": "Nomor identitas unik radio digital." },
  { "en": "Fungsi 'site trunking'?", "id": "Menjaga komunikasi saat koneksi gagal." },
  { "en": "Sandi 'laporan 1x24 jam'?", "id": "Kewajiban pelaporan setelah kejadian." },
  { "en": "Sandi 'pembuntutan'?", "id": "Melakukan pengintaian atau pembuntutan." },
  { "en": "Apa itu 'audio feedback'?", "id": "Suara melengking akibat loop audio." },
  { "en": "Sandi 'Tim Jaguar'?", "id": "Tim khusus anti kejahatan jalanan." },
  { "en": "Sandi 'Tim Cobra'?", "id": "Tim khusus penindak kejahatan." },
  { "en": "Sandi 'Tim Rajawali'?", "id": "Tim khusus pengurai massa." },
  { "en": "Fungsi duplexer di repeater?", "id": "Memisah sinyal pancar dan terima." },
  { "en": "Definisi 'intermodulation'?", "id": "Gangguan dari pencampuran beberapa sinyal." },
  { "en": "Sandi 'jalur rawan'?", "id": "Rute atau area rawan kejahatan." },
  { "en": "Sandi 'jam rawan'?", "id": "Waktu sering terjadinya gangguan keamanan." },
  { "en": "SOP memulai komunikasi?", "id": "Sebut callsign tujuan dan callsign sendiri." },
  { "en": "SOP mengakhiri komunikasi?", "id": "Ucapkan sandi 'clear' atau 'out'." },
  { "en": "Sandi 'penyadapan'?", "id": "Melakukan penyadapan target terotorisasi." },
  { "en": "Sandi 'undercover'?", "id": "Petugas melakukan penyamaran." },
  { "en": "Apa itu 'power amplifier'?", "id": "Perangkat penguat daya pancar radio." },
  { "en": "Sandi 'penyergapan'?", "id": "Melakukan penyergapan di lokasi target." },
  { "en": "Sandi 'sterilisasi'?", "id": "Mengamankan lokasi dari ancaman bom." },
  { "en": "Sandi 'disposal'?", "id": "Proses pemusnahan bahan peledak." },
  { "en": "Perbedaan 'clear' dan 'out'?", "id": "Clear menunggu jawaban, out tidak." },
  { "en": "Sandi 'pencarian orang hilang'?", "id": "Melakukan pencarian orang hilang." },
  { "en": "Sandi 'pemalsuan dokumen'?", "id": "Kasus terkait pemalsuan surat." },
  { "en": "Sandi 'uang palsu'?", "id": "Kasus peredaran uang palsu." },
  { "en": "Sandi 'perjudian'?", "id": "Kasus terkait tindak pidana perjudian." },
  { "en": "Sandi 'ilegal logging'?", "id": "Kasus pembalakan liar." },
  { "en": "Sandi 'ilegal fishing'?", "id": "Kasus penangkapan ikan ilegal." },
  { "en": "Sandi 'human trafficking'?", "id": "Kasus perdagangan manusia." },
  { "en": "Sandi 'KDRT'?", "id": "Kekerasan Dalam Rumah Tangga." },
  { "en": "Sandi 'perlindungan anak'?", "id": "Kasus terkait perlindungan anak." },
  { "en": "Sandi 'cybercrime'?", "id": "Kasus kejahatan dunia maya." },
  { "en": "Fungsi 'voting' pada radio?", "id": "Memilih sinyal repeater terbaik otomatis." },
  { "en": "Sandi 'status quo'?", "id": "Situasi kembali ke keadaan semula." },
  { "en": "Sandi 'evakuasi'?", "id": "Memindahkan orang dari lokasi bahaya." },
  { "en": "Sandi 'bencana alam'?", "id": "Kejadian bencana alam di wilayah." },
  { "en": "Sandi 'banjir'?", "id": "Bencana banjir, mohon bantuan." },
  { "en": "Sandi 'gempa bumi'?", "id": "Bencana gempa bumi." },
  { "en": "Sandi 'tanah longsor'?", "id": "Bencana tanah longsor." },
  { "en": "Sandi 'gunung meletus'?", "id": "Erupsi gunung berapi." },
  { "en": "Sandi 'kebakaran hutan'?", "id": "Karhutla atau kebakaran hutan." },
  { "en": "Apa itu 'VOX'?", "id": "Transmisi radio diaktifkan oleh suara." },
  { "en": "Kapan VOX digunakan?", "id": "Saat tangan tidak bisa menekan PTT." },
  { "en": "Sandi 'konsentrasi massa'?", "id": "Adanya perkumpulan massa dalam jumlah besar." },
  { "en": "Sandi 'blokade jalan'?", "id": "Penutupan jalan oleh massa." },
  { "en": "Sandi 'negosiasi'?", "id": "Melakukan negosiasi dengan pelaku." },
  { "en": "Sandi 'mediasi'?", "id": "Melakukan mediasi antar pihak bertikai." },
  { "en": "Sandi 'Damai'?", "id": "Situasi telah damai dan kondusif." },
  { "en": "Sandi 'Tuntas'?", "id": "Tugas atau kasus telah selesai." },
  { "en": "Sandi 'rolling'?", "id": "Patroli bergerak dari satu titik." },
  { "en": "Sandi 'stasioner'?", "id": "Patroli diam di satu titik." },
  { "en": "Sandi 'lalu lintas padat'?", "id": "Arus lalin padat merayap." },
  { "en": "Sandi 'lalu lintas lancar'?", "id": "Arus lalin lancar terkendali." },
  { "en": "Sandi 'kecelakaan beruntun'?", "id": "Kecelakaan melibatkan banyak kendaraan." },
  { "en": "Sandi 'korban material'?", "id": "Hanya ada kerugian benda." },
  { "en": "Sandi 'korban jiwa'?", "id": "Ada korban meninggal dunia." },
  { "en": "Sandi 'Titik Kumpul'?", "id": "Lokasi berkumpulnya personel." },
  { "en": "Sandi 'Ring 1'?", "id": "Area pengamanan paling dekat target." },
  { "en": "Sandi 'Ring 2'?", "id": "Area pengamanan lapisan kedua." },
  { "en": "Sandi 'escape route'?", "id": "Jalur evakuasi atau melarikan diri." },
  { "en": "Sandi 'strong point'?", "id": "Titik pertahanan atau penjagaan kuat." },
  { "en": "Sandi 'jam pimpinan'?", "id": "Kegiatan arahan dari pimpinan." },
  { "en": "Sandi 'apel'?", "id": "Kegiatan apel pagi atau sore." },
  { "en": "Sandi 'serah terima'?", "id": "Serah terima tugas jaga." },
  { "en": "Sandi 'piket'?", "id": "Petugas yang sedang dinas jaga." },
  { "en": "Sandi 'pawas'?", "id": "Perwira pengawas." },
  { "en": "Sandi 'padal'?", "id": "Perwira pengendali." },
  { "en": "Sandi 'provokator'?", "id": "Orang yang memprovokasi kerusuhan." },
  { "en": "Sandi 'sweeping sajam'?", "id": "Razia pemeriksaan senjata tajam." },
  { "en": "Sandi 'sweeping miras'?", "id": "Razia minuman keras ilegal." },
  { "en": "Sandi 'DPO tertangkap'?", "id": "Daftar Pencarian Orang tertangkap." },
  { "en": "Sandi 'TO'?", "id": "Target Operasi." },
  { "en": "Sandi 'eksekusi'?", "id": "Pelaksanaan perintah atau putusan." },
  { "en": "Sandi 'penggerebekan'?", "id": "Melakukan penggerebekan lokasi target." },
  { "en": "Sandi 'kantor pemerintah'?", "id": "Objek vital kantor pemerintah." },
  { "en": "Sandi 'sekolah'?", "id": "Objek vital area sekolah." },
  { "en": "Sandi 'universitas'?", "id": "Objek vital area kampus." },
  { "en": "Sandi 'pasar'?", "id": "Objek vital area pasar tradisional." },
  { "en": "Sandi 'pusat perbelanjaan'?", "id": "Objek vital mal atau plaza." },
  { "en": "Sandi 'kawasan industri'?", "id": "Objek vital kawasan pabrik." },
  { "en": "Sandi 'jalur pipa gas'?", "id": "Objek vital jalur pipa gas." },
  { "en": "Sandi 'gardu listrik'?", "id": "Objek vital gardu listrik PLN." },
  { "en": "Fungsi 'Color Code' DMR?", "id": "Membedakan sistem radio yang sama." },
  { "en": "Apa itu 'Time Slot'?", "id": "Membagi satu frekuensi jadi dua." },
  { "en": "Definisi 'Private Call'?", "id": "Panggilan pribadi antar dua radio." },
  { "en": "Definisi 'Group Call'?", "id": "Panggilan ke semua anggota talkgroup." },
  { "en": "Definisi 'All Call'?", "id": "Panggilan ke semua radio sistem." },
  { "en": "Eja nopol 'B 1234 CD'?", "id": "Bravo satu dua tiga empat Charlie Delta." },
  { "en": "Protokol radio dengan TNI?", "id": "Gunakan frekuensi gabungan yang ditentukan." },
  { "en": "Protokol radio dengan Damkar?", "id": "Gunakan kanal darurat bersama." },
  { "en": "Protokol radio dengan Basarnas?", "id": "Gunakan kanal frekuensi operasi SAR." },
  { "en": "SOP radio check pagi?", "id": "Laporkan callsign, kekuatan sinyal, baterai." },
  { "en": "SOP jika HT hilang?", "id": "Segera lapor pimpinan untuk radio kill." },
  { "en": "Sandi 'perawatan radio'?", "id": "Lakukan pemeliharaan dan perawatan." },
  { "en": "Siapa penanggung jawab radio?", "id": "Setiap personel yang memegang." },
  { "en": "Sandi 'update situasi'?", "id": "Laporkan perkembangan situasi terkini." },
  { "en": "Sandi 'tersangka tidak kooperatif'?", "id": "Target melawan atau tidak patuh." },
  { "en": "Sandi 'TKP sudah steril'?", "id": "Lokasi kejadian sudah aman." },
  { "en": "Fungsi antena Yagi?", "id": "Antena directional untuk jarak jauh." },
  { "en": "Fungsi antena Omnidirectional?", "id": "Memancarkan sinyal ke segala arah." },
  { "en": "Apa itu 'radio profile'?", "id": "Konfigurasi talkgroup dan fitur radio." },
  { "en": "Sandi 'BAP'?", "id": "Berita Acara Pemeriksaan." },
  { "en": "Sandi 'saksi mata'?", "id": "Orang yang melihat langsung kejadian." },
  { "en": "Sandi 'ahli waris'?", "id": "Keluarga dari korban meninggal dunia." },
  { "en": "Fungsi 'repeater kerodong'?", "id": "Repeater portabel untuk operasi khusus." },
  { "en": "Sandi 'pembajakan'?", "id": "Kasus pembajakan kendaraan atau kapal." },
  { "en": "Sandi 'penyelundupan'?", "id": "Kasus penyelundupan barang ilegal." },
  { "en": "Apa itu 'deviation' FM?", "id": "Ukuran pergeseran frekuensi modulasi." },
  { "en": "Sandi 'uji balistik'?", "id": "Pemeriksaan proyektil atau senjata api." },
  { "en": "Sandi 'uji DNA'?", "id": "Pemeriksaan sampel DNA forensik." },
  { "en": "Sandi 'visum'?", "id": "Pemeriksaan medis untuk bukti hukum." },
  { "en": "Sandi 'otopsi'?", "id": "Pemeriksaan jenazah untuk sebab kematian." },
  { "en": "Sandi 'surat perintah'?", "id": "Sprint atau surat perintah tugas." },
  { "en": "Sandi 'gelar perkara'?", "id": "Presentasi kasus di hadapan pimpinan." },
  { "en": "Sandi 'P21'?", "id": "Berkas perkara dinyatakan lengkap." },
  { "en": "Sandi 'tersangka menyerahkan diri'?", "id": "TSK menyerahkan diri secara sukarela." },
  { "en": "Sandi 'Sispamkota'?", "id": "Sistem Pengamanan Kota." },
  { "en": "Sandi 'kontijensi'?", "id": "Rencana untuk situasi tak terduga." },
  { "en": "Sandi 'latihan gabungan'?", "id": "Latgab atau latihan bersama instansi." },
  { "en": "Definisi 'radio discipline'?", "id": "Kepatuhan pada aturan komunikasi radio." },
  { "en": "Sandi 'arus pendek listrik'?", "id": "Penyebab kebakaran akibat korsleting." },
  { "en": "Sandi 'gas bocor'?", "id": "Laporan kebocoran gas berbahaya." },
  { "en": "Sandi 'gedung runtuh'?", "id": "Laporan bangunan atau gedung runtuh." },
  { "en": "Sandi 'jembatan putus'?", "id": "Laporan jembatan runtuh atau putus." },
  { "en": "Sandi 'jalur macet total'?", "id": "Lalin terkunci, tidak bergerak." },
  { "en": "Sandi 'lalin dialihkan'?", "id": "Arus lalu lintas dialihkan sementara." },
  { "en": "Sandi 'parkir liar'?", "id": "Penertiban kendaraan parkir sembarangan." },
  { "en": "Sandi 'balap liar'?", "id": "Penindakan aksi balap liar." },
  { "en": "Sandi 'premanisme'?", "id": "Penindakan aksi premanisme." },
  { "en": "Sandi 'pesta miras'?", "id": "Pembubaran kegiatan pesta minuman keras." },
  { "en": "Sandi 'pabrik narkoba'?", "id": "Penggerebekan lokasi produksi narkoba." },
  { "en": "Sandi 'bandar narkoba'?", "id": "Target operasi adalah bandar narkoba." },
  { "en": "Sandi 'kurir narkoba'?", "id": "Penangkapan kurir narkoba." },
  { "en": "Sandi 'pengguna narkoba'?", "id": "Pengamanan pengguna narkotika." },
  { "en": "Sandi 'overdosis'?", "id": "Korban akibat overdosis obat." },
  { "en": "Sandi 'penemuan mayat'?", "id": "Ditemukan mayat tanpa identitas." },
  { "en": "Sandi 'bayi dibuang'?", "id": "Penemuan bayi yang dibuang." },
  { "en": "Sandi 'kekerasan seksual'?", "id": "Kasus terkait kekerasan seksual." },
  { "en": "Sandi 'bullying'?", "id": "Kasus perundungan di sekolah." },
  { "en": "Sandi 'hoax'?", "id": "Penyebaran berita bohong." },
  { "en": "Sandi 'hate speech'?", "id": "Kasus ujaran kebencian." },
  { "en": "Sandi 'pencemaran nama baik'?", "id": "Kasus ITE pencemaran nama baik." },
  { "en": "Sandi 'peretasan'?", "id": "Kasus peretasan sistem elektronik." },
  { "en": "Sandi 'carding'?", "id": "Kasus penyalahgunaan kartu kredit." },
  { "en": "Sandi 'pinjol ilegal'?", "id": "Kasus pinjaman online ilegal." },
  { "en": "Sandi 'investasi bodong'?", "id": "Kasus penipuan berkedok investasi." },
  { "en": "Sandi 'geng motor'?", "id": "Kelompok motor yang meresahkan." },
  { "en": "Sandi 'sajam di sekolah'?", "id": "Laporan siswa membawa senjata tajam." },
  { "en": "Sandi 'kebakaran kendaraan'?", "id": "Laporan kendaraan roda 2/4 terbakar." },
  { "en": "Sandi 'pohon tumbang'?", "id": "Pohon tumbang menutup jalan." },
  { "en": "Sandi 'tiang listrik tumbang'?", "id": "Tiang listrik roboh berbahaya." },
  { "en": "Sandi 'lalu lintas normal'?", "id": "Situasi lalu lintas kembali normal." },
  { "en": "Sandi 'tersangka bersenjata api'?", "id": "TSK membawa Senpi, harap waspada." },
  { "en": "Sandi 'tersangka lari ke pemukiman'?", "id": "TSK masuk ke perkampungan warga." },
  { "en": "Sandi 'tersangka menyandera warga'?", "id": "TSK melakukan aksi penyanderaan." },
  { "en": "Sandi 'minta anjing pelacak'?", "id": "Butuh bantuan unit K9." },
  { "en": "Sandi 'minta tim negosiator'?", "id": "Butuh bantuan tim negosiator." },
  { "en": "Sandi 'minta tim gegana'?", "id": "Butuh bantuan unit Gegana." },
  { "en": "Sandi 'minta water cannon'?", "id": "Butuh bantuan AWC atau water cannon." },
  { "en": "Sandi 'minta barakuda'?", "id": "Butuh bantuan kendaraan lapis baja." },
  { "en": "Sandi 'minta pemadam kebakaran'?", "id": "Butuh bantuan unit Damkar." },
  { "en": "Sandi 'pos pengamanan'?", "id": "Pospam atau pos pengamanan." },
  { "en": "Sandi 'pos pelayanan'?", "id": "Posyan atau pos pelayanan." },
  { "en": "Sandi 'pos terpadu'?", "id": "Pos terpadu gabungan instansi." },
  { "en": "Sandi 'arus mudik'?", "id": "Pemantauan arus mudik lebaran." },
  { "en": "Sandi 'arus balik'?", "id": "Pemantauan arus balik lebaran." },
  { "en": "Sandi 'one way'?", "id": "Pemberlakuan rekayasa lalin satu arah." },
  { "en": "Sandi 'ganjil genap'?", "id": "Pemberlakuan sistem ganjil genap." },
  { "en": "Sandi 'area steril'?", "id": "Area terlarang untuk umum." },
  { "en": "Sandi 'garis polisi'?", "id": "Pasang police line di TKP." },
  { "en": "Sandi 'press conference'?", "id": "Kegiatan konferensi pers." },
  { "en": "Sandi 'TKP gelap'?", "id": "TKP minim penerangan." },
  { "en": "Sandi 'TKP ramai'?", "id": "TKP dipenuhi kerumunan warga." },
  { "en": "Sandi 'hujan deras'?", "id": "Kondisi cuaca hujan sangat deras." },
  { "en": "Sandi 'angin kencang'?", "id": "Kondisi cuaca angin sangat kencang." },
  { "en": "Sandi 'kabut tebal'?", "id": "Jarak pandang terbatas akibat kabut." },
  { "en": "Definisi 'co-channel interference'?", "id": "Gangguan dari frekuensi yang sama." },
  { "en": "Definisi 'adjacent channel interference'?", "id": "Gangguan dari frekuensi di sebelahnya." },
  { "en": "Fungsi antena ATEX/IS?", "id": "Aman digunakan di area mudah terbakar." },
  { "en": "Apa itu 'receiver sensitivity'?", "id": "Kemampuan radio menerima sinyal lemah." },
  { "en": "Polarisasi antena radio Polri?", "id": "Umumnya menggunakan polarisasi vertikal." },
  { "en": "Definisi 'carrier wave'?", "id": "Gelombang pembawa sinyal informasi." },
  { "en": "Definisi 'sideband'?", "id": "Komponen frekuensi di samping carrier." },
  { "en": "Apa itu 'signal-to-noise ratio'?", "id": "Rasio kekuatan sinyal terhadap noise." },
  { "en": "Sandi untuk 'BAST'?", "id": "Berita Acara Serah Terima." },
  { "en": "SOP pelaporan radio rusak?", "id": "Lapor ke teknisi via jalur lain." },
  { "en": "Sandi untuk rapat internal?", "id": "Ada giat rapat di ruangan." },
  { "en": "Sandi untuk logistik kurang?", "id": "Kekurangan suplai atau logistik." },
  { "en": "Sandi personel butuh istirahat?", "id": "Personel mengalami kelelahan, butuh jeda." },
  { "en": "SOP verifikasi perintah sensitif?", "id": "Ulangi perintah, minta konfirmasi pimpinan." },
  { "en": "Protokol channel macet total?", "id": "Pindah ke kanal cadangan darurat." },
  { "en": "Sandi pengamanan wisuda?", "id": "Pam giat wisuda di kampus." },
  { "en": "Sandi 'check point'?", "id": "Titik pemeriksaan atau penyekatan." },
  { "en": "Sandi 'buddy system'?", "id": "Sistem patroli berpasangan." },
  { "en": "Sandi 'maping lokasi'?", "id": "Melakukan pemetaan detail suatu area." },
  { "en": "Sandi 'analisa target'?", "id": "Menganalisa kebiasaan dan profil target." },
  { "en": "Sandi 'surveillance'?", "id": "Pengamatan rahasia jangka panjang." },
  { "en": "Sandi 'dilepas'?", "id": "Target dilepas dari pantauan." },
  { "en": "Sandi 'diambil alih'?", "id": "Pemantauan diambil alih tim lain." },
  { "en": "Sandi 'clean area'?", "id": "Lokasi aman dari penyadapan." },
  { "en": "Sandi 'debriefing'?", "id": "Pembahasan setelah selesai operasi." },
  { "en": "Sandi 'Anev'?", "id": "Analisa dan Evaluasi kegiatan." },
  { "en": "Sandi 'Latpraops'?", "id": "Latihan Pra Operasi." },
  { "en": "Sandi 'TWG'?", "id": "Tactical Wall Game." },
  { "en": "Sandi 'simulasi'?", "id": "Latihan simulasi penanganan situasi." },
  { "en": "Sandi 'force majeure'?", "id": "Keadaan kahar di luar kendali." },
  { "en": "Sandi 'ambulans udara'?", "id": "Minta bantuan helikopter medis." },
  { "en": "Sandi 'jalur udara ditutup'?", "id": "Wilayah udara ditutup untuk penerbangan." },
  { "en": "Sandi 'jalur laut ditutup'?", "id": "Wilayah laut ditutup untuk pelayaran." },
  { "en": "Sandi 'razia KTP'?", "id": "Pemeriksaan identitas penduduk." },
  { "en": "Sandi 'WNA'?", "id": "Warga Negara Asing." },
  { "en": "Sandi 'imigrasi'?", "id": "Terkait keimigrasian atau orang asing." },
  { "en": "Sandi 'bea cukai'?", "id": "Terkait kepabeanan dan cukai." },
  { "en": "Sandi 'karantina'?", "id": "Terkait karantina kesehatan atau hewan." },
  { "en": "Sandi 'tersangka positif narkoba'?", "id": "Hasil tes urin TSK positif." },
  { "en": "Sandi 'DPO lintas negara'?", "id": "DPO diduga kabur ke luar negeri." },
  { "en": "Sandi 'Red Notice' Interpol?", "id": "Permintaan penangkapan buronan internasional." },
  { "en": "Sandi 'Blue Notice' Interpol?", "id": "Mencari informasi tambahan orang." },
  { "en": "Sandi 'jalur tikus perbatasan'?", "id": "Jalur ilegal di perbatasan negara." },
  { "en": "Sandi 'patroli perbatasan'?", "id": "Melaksanakan patroli di garis batas." },
  { "en": "Sandi 'pos lintas batas'?", "id": "PLBN atau Pos Lintas Batas Negara." },
  { "en": "Sandi 'penjara'?", "id": "Lapas atau lembaga pemasyarakatan." },
  { "en": "Sandi 'sidang pengadilan'?", "id": "Pengamanan giat sidang di pengadilan." },
  { "en": "Sandi 'saksi kunci'?", "id": "Saksi utama dalam sebuah kasus." },
  { "en": "Sandi 'perlindungan saksi'?", "id": "Memberikan pengamanan pada saksi." },
  { "en": "Sandi 'LPSK'?", "id": "Lembaga Perlindungan Saksi dan Korban." },
  { "en": "Sandi 'ekshumasi'?", "id": "Pembongkaran makam untuk otopsi ulang." },
  { "en": "Sandi 'rekonstruksi'?", "id": "Rekonstruksi adegan di TKP." },
  { "en": "Sandi 'pra-rekonstruksi'?", "id": "Persiapan sebelum rekonstruksi TKP." },
  { "en": "Sandi 'olah TKP'?", "id": "Melakukan proses olah TKP." },
  { "en": "Sandi 'status siaga 1'?", "id": "Kesiapan tertinggi seluruh personel." },
  { "en": "Sandi 'status siaga 2'?", "id": "Kesiapan ditingkatkan sebagian personel." },
  { "en": "Sandi 'jam malam'?", "id": "Pemberlakuan pembatasan kegiatan malam." },
  { "en": "Sandi 'PSBB'?", "id": "Pembatasan Sosial Berskala Besar." },
  { "en": "Sandi 'PPKM'?", "id": "Pemberlakuan Pembatasan Kegiatan Masyarakat." },
  { "en": "Sandi 'pelanggaran prokes'?", "id": "Pelanggaran protokol kesehatan." },
  { "en": "Sandi 'razia masker'?", "id": "Penertiban penggunaan masker." },
  { "en": "Sandi 'tes acak'?", "id": "Pemeriksaan acak di suatu lokasi." },
  { "en": "Sandi 'penyekatan jalur'?", "id": "Melakukan penyekatan di jalur mudik." },
  { "en": "Sandi 'pos pantau'?", "id": "Pos pemantauan situasi." },
  { "en": "Sandi 'CCTV'?", "id": "Closed-Circuit Television." },
  { "en": "Sandi 'pantau CCTV'?", "id": "Monitor pergerakan melalui kamera CCTV." },
  { "en": "Sandi 'CCTV rusak'?", "id": "Kamera CCTV di lokasi mati." },
  { "en": "Sandi 'ETLE'?", "id": "Electronic Traffic Law Enforcement." },
  { "en": "Sandi 'tilang elektronik'?", "id": "Penindakan pelanggaran via ETLE." },
  { "en": "Sandi 'command center'?", "id": "Pusat komando dan kendali." },
  { "en": "Sandi 'drone'?", "id": "Menggunakan pesawat tanpa awak." },
  { "en": "Sandi 'pantau dari udara'?", "id": "Melakukan pemantauan via drone." },
  { "en": "Sandi 'pencarian panas tubuh'?", "id": "Gunakan thermal camera atau drone." },
  { "en": "Sandi 'kemacetan parah'?", "id": "Kondisi lalu lintas stuck." },
  { "en": "Sandi 'lalin terurai'?", "id": "Kemacetan mulai bisa diurai." },
  { "en": "Sandi 'contraflow berhasil'?", "id": "Rekayasa lalin contraflow efektif." },
  { "en": "Sandi 'buka tutup jalur'?", "id": "Pemberlakuan buka tutup jalur." },
  { "en": "Sandi 'jalur puncak'?", "id": "Situasi lalu lintas di Jalur Puncak." },
  { "en": "Sandi 'jalur wisata'?", "id": "Situasi lalu lintas di lokasi wisata." },
  { "en": "Sandi 'kendaraan mogok'?", "id": "Ada kendaraan mogok di jalan." },
  { "en": "Sandi 'ban pecah'?", "id": "Kendaraan mengalami pecah ban." },
  { "en": "Sandi 'muatan tumpah'?", "id": "Muatan truk tumpah ke jalan." },
  { "en": "Sandi 'kecelakaan tunggal'?", "id": "Kecelakaan hanya satu kendaraan." },
  { "en": "Sandi 'tabrak lari'?", "id": "Terjadi kecelakaan tabrak lari." },
  { "en": "Sandi 'korban dievakuasi'?", "id": "Korban sudah dibawa ke RS." },
  { "en": "Sandi 'kendaraan diderek'?", "id": "Kendaraan dipindahkan dengan mobil derek." },
  { "en": "Sandi 'TKP bersih'?", "id": "TKP sudah bersih, lalin normal." },
  { "en": "Sandi 'pemulangan jenazah'?", "id": "Proses pemulangan jenazah ke keluarga." },
  { "en": "Sandi 'rumah duka'?", "id": "Pengamanan di lokasi rumah duka." },
  { "en": "Sandi 'Bhabinkamtibmas'?", "id": "Bhayangkara Pembina Keamanan dan Ketertiban." },
  { "en": "Sandi 'patroli dialogis'?", "id": "Patroli sambil berkomunikasi dengan warga." },
  { "en": "Sandi 'laporan warga'?", "id": "Menerima laporan dari masyarakat." },
  { "en": "Sandi 'quick response'?", "id": "Tim reaksi cepat." },
  { "en": "Sandi 'hotline'?", "id": "Laporan dari telepon darurat." },
  { "en": "SOP komunikasi saat silent approach?", "id": "Gunakan earpiece, bicara mode berbisik." },
  { "en": "Sandi untuk 'bom ransel'?", "id": "Handak terdeteksi dalam tas ransel." },
  { "en": "Sandi untuk 'bom panci'?", "id": "Handak terdeteksi dalam panci presto." },
  { "en": "Protokol kehilangan kontak tim undercover?", "id": "Lapor pimpinan, aktifkan prosedur darurat." },
  { "en": "Penggunaan radio oleh Paminal?", "id": "Monitoring dan pengawasan internal." },
  { "en": "Sandi 'pelanggaran disiplin'?", "id": "Laporan dugaan pelanggaran oleh anggota." },
  { "en": "SOP anggota tidak merespon?", "id": "Cek via rekan, lapor pimpinan." },
  { "en": "Sandi untuk 'Surat Perintah'?", "id": "Sprin atau surat perintah tugas." },
  { "en": "Sandi untuk 'Telegram Rahasia'?", "id": "TR atau telegram rahasia." },
  { "en": "Sandi untuk laporan harian?", "id": "Laphar atau laporan giat harian." },
  { "en": "Sandi 'peralatan tidak lengkap'?", "id": "Kekurangan kelengkapan atau alutsista." },
  { "en": "Sandi 'imbauan kamtibmas'?", "id": "Memberikan imbauan keamanan lewat pengeras suara." },
  { "en": "Sandi 'seruan menyerah'?", "id": "Imbauan agar tersangka menyerahkan diri." },
  { "en": "Tantangan radio di gunung?", "id": "Sinyal terhalang, banyak dead spot." },
  { "en": "Tantangan radio di hutan?", "id": "Sinyal terserap oleh vegetasi lebat." },
  { "en": "Backup komunikasi area terpencil?", "id": "Telepon satelit atau repeater portabel." },
  { "en": "Sandi 'jaringan seluler hilang'?", "id": "Blank spot, tidak ada sinyal HP." },
  { "en": "Sandi 'listrik padam total'?", "id": "Blackout, listrik padam di wilayah." },
  { "en": "SOP perubahan frekuensi operasi?", "id": "Atas perintah pengendali operasi." },
  { "en": "Sandi 'frekuensi utama terganggu'?", "id": "Frekuensi utama di-jamming atau terganggu." },
  { "en": "Sandi 'pindah ke frekuensi alternatif'?", "id": "Ganti ke gelombang cadangan." },
  { "en": "Sandi 'radio check berantai'?", "id": "Tes radio berantai antar pos." },
  { "en": "Sandi 'identitas tidak dikenal'?", "id": "Orang atau mayat tanpa identitas." },
  { "en": "Sandi 'situasi membeku'?", "id": "Situasi tidak ada pergerakan." },
  { "en": "Sandi 'target bergerak ke utara'?", "id": "TO bergerak menuju arah utara." },
  { "en": "Sandi 'target ganti kendaraan'?", "id": "TO pindah ke kendaraan lain." },
  { "en": "Sandi 'target masuk tol'?", "id": "TO masuk ke jalan tol." },
  { "en": "Sandi 'target keluar tol'?", "id": "TO keluar dari jalan tol." },
  { "en": "Sandi 'tim kepung'?", "id": "Tim melakukan pengepungan dari segala arah." },
  { "en": "Sandi 'tim tindak'?", "id": "Tim yang melakukan penindakan langsung." },
  { "en": "Sandi 'tim evakuasi'?", "id": "Tim yang mengevakuasi korban." },
  { "en": "Sandi 'tim negosiasi buntu'?", "id": "Negosiasi tidak mencapai kesepakatan." },
  { "en": "Sandi 'ultimatum'?", "id": "Memberikan batas waktu kepada target." },
  { "en": "Sandi 'serangan fajar'?", "id": "Penyerbuan dilakukan pada waktu subuh." },
  { "en": "Sandi 'senyap'?", "id": "Operasi dilakukan secara diam-diam." },
  { "en": "Sandi 'data intelijen A1'?", "id": "Informasi intelijen sangat valid." },
  { "en": "Sandi 'data mentah'?", "id": "Informasi belum diolah atau dianalisa." },
  { "en": "Sandi 'perlu validasi'?", "id": "Informasi perlu dicek kebenarannya." },
  { "en": "Sandi 'sumber informasi'?", "id": "Orang yang memberikan informasi." },
  { "en": "Sandi 'informan'?", "id": "Sumber informasi dari jaringan kepolisian." },
  { "en": "Sandi 'pertemuan rahasia'?", "id": "Ada pertemuan rahasia di lokasi." },
  { "en": "Sandi 'transaksi narkoba'?", "id": "Terpantau transaksi serah terima narkoba." },
  { "en": "Sandi 'transaksi mencurigakan'?", "id": "Terpantau aktivitas atau transaksi aneh." },
  { "en": "Sandi 'kumpul kebo'?", "id": "Laporan pasangan bukan suami istri." },
  { "en": "Sandi 'pembalakan liar'?", "id": "Ilegal logging." },
  { "en": "Sandi 'penambangan liar'?", "id": "Ilegal mining." },
  { "en": "Sandi 'limbah beracun'?", "id": "Pembuangan limbah B3 ilegal." },
  { "en": "Sandi 'satwa dilindungi'?", "id": "Perdagangan atau perburuan satwa dilindungi." },
  { "en": "Sandi 'cagar budaya'?", "id": "Objek pengamanan situs cagar budaya." },
  { "en": "Sandi 'pencurian artefak'?", "id": "Kasus pencurian benda purbakala." },
  { "en": "Sandi 'tumpahan minyak'?", "id": "Laporan tumpahan minyak di laut." },
  { "en": "Sandi 'kapal tenggelam'?", "id": "Laporan kapal tenggelam di perairan." },
  { "en": "Sandi 'orang tenggelam'?", "id": "Laporan orang tenggelam." },
  { "en": "Sandi 'serangan hiu'?", "id": "Laporan serangan hiu pada manusia." },
  { "en": "Sandi 'pendaki tersesat'?", "id": "Laporan pendaki gunung hilang." },
  { "en": "Sandi 'penerbangan ilegal'?", "id": "Aktivitas penerbangan tanpa izin." },
  { "en": "Sandi 'drone liar'?", "id": "Drone tak dikenal di area terlarang." },
  { "en": "Sandi 'serangan siber'?", "id": "Serangan pada sistem komputer pemerintah." },
  { "en": "Sandi 'website di-deface'?", "id": "Situs web pemerintah diretas." },
  { "en": "Sandi 'kebocoran data'?", "id": "Dugaan kebocoran data sensitif." },
  { "en": "Sandi 'skimming ATM'?", "id": "Kasus skimming pada mesin ATM." },
  { "en": "Sandi 'pinjaman online ilegal'?", "id": "Penindakan pinjol ilegal." },
  { "en": "Sandi 'penipuan online shop'?", "id": "Kasus penipuan toko online." },
  { "en": "Sandi 'romance scam'?", "id": "Kasus penipuan asmara dunia maya." },
  { "en": "Sandi 'konvoi moge'?", "id": "Pengawalan konvoi motor gede." },
  { "en": "Sandi 'konvoi pejabat'?", "id": "Pengawalan konvoi kendaraan pejabat." },
  { "en": "Sandi 'iringan jenazah'?", "id": "Pengawalan iring-iringan pengantar jenazah." },
  { "en": "Sandi 'aksi damai'?", "id": "Unjuk rasa berjalan dengan damai." },
  { "en": "Sandi 'aksi anarkis'?", "id": "Unjuk rasa berubah menjadi anarkis." },
  { "en": "Sandi 'pembakaran ban'?", "id": "Massa melakukan aksi bakar ban." },
  { "en": "Sandi 'pelemparan batu'?", "id": "Massa melempari petugas atau fasilitas." },
  { "en": "Sandi 'gas air mata'?", "id": "Penembakan gas air mata." },
  { "en": "Sandi 'mundur teratur'?", "id": "Pasukan mundur secara teratur." },
  { "en": "Sandi 'formasi bertahan'?", "id": "Pasukan membentuk formasi bertahan." },
  { "en": "Sandi 'personel terluka'?", "id": "Ada anggota yang mengalami luka." },
  { "en": "Sandi 'warga terluka'?", "id": "Ada warga sipil terluka." },
  { "en": "Sandi 'jurnalis di TKP'?", "id": "Ada awak media meliput." },
  { "en": "Sandi 'area media'?", "id": "Lokasi yang ditentukan untuk media." },
  { "en": "Sandi 'wawancara pers'?", "id": "Pejabat memberikan keterangan pada pers." },
  { "en": "Sandi 'data korban'?", "id": "Pendataan jumlah dan identitas korban." },
  { "en": "Sandi 'data kerugian'?", "id": "Pendataan kerugian materiil." },
  { "en": "Sandi 'logistik terhambat'?", "id": "Pengiriman logistik bantuan terhambat." },
  { "en": "Sandi 'dapur umum'?", "id": "Mendirikan dapur umum untuk korban." },
  { "en": "Sandi 'posko kesehatan'?", "id": "Mendirikan posko kesehatan darurat." },
  { "en": "Sandi 'posko pengungsian'?", "id": "Lokasi pengungsian para korban." },
  { "en": "Sandi 'bantuan psikologis'?", "id": "Butuh tim trauma healing." },
  { "en": "Sandi 'pencarian selesai'?", "id": "Operasi pencarian korban dihentikan." },
  { "en": "Sandi 'pencarian dilanjutkan'?", "id": "Operasi pencarian dilanjutkan besok." },
  { "en": "Sandi 'kondisi aman'?", "id": "Situasi di wilayah aman terkendali." },
  { "en": "Sandi 'selamat bertugas'?", "id": "Ucapan semangat di awal tugas." },
  { "en": "Sandi 'terima kasih'?", "id": "Ucapan terima kasih atas bantuan." },
  { "en": "Sandi 'selamat jalan'?", "id": "Ucapan perpisahan setelah tugas." },
  { "en": "Prinsip utama komunikasi radio?", "id": "Cepat, Tepat, Akurat, Aman." },
  { "en": "Kenapa harus singkat (Brevity)?", "id": "Efisiensi waktu dan penggunaan kanal." },
  { "en": "Kenapa harus jelas (Clarity)?", "id": "Menghindari salah tafsir perintah." },
  { "en": "Apa itu prinsip keamanan (Security)?", "id": "Menjaga kerahasiaan informasi operasi." },
  { "en": "Tujuan utama disiplin radio?", "id": "Menjamin kelancaran dan efektivitas komunikasi." },
  { "en": "Mengapa harus tenang saat berkomunikasi?", "id": "Agar informasi tersampaikan dengan akurat." },
  { "en": "Apa yang harus dihindari?", "id": "Emosi, opini pribadi, informasi tak perlu." },
  { "en": "Tujuan utama penggunaan sandi?", "id": "Kecepatan dan kerahasiaan pesan." },
  { "en": "Prinsip dalam menyebut angka?", "id": "Sebutkan satu per satu angkanya." },
  { "en": "Konsekuensi pelanggaran disiplin radio?", "id": "Mengacaukan operasi dan membahayakan tim." },
  { "en": "Mengapa dilarang menyela musik?", "id": "Mengganggu dan tidak etis." },
  { "en": "Sikap utama seorang operator?", "id": "Profesional, bertanggung jawab, dan waspada." }




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
